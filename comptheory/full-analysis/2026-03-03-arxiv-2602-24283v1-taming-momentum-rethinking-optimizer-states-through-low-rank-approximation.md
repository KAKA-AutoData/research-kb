---
paper_id: "arxiv:2602.24283v1"
priority: "P1"
domain: "comptheory"
source: "arxiv"
published_at: "2026-02-27T18:57:06+00:00"
url: "http://arxiv.org/abs/2602.24283v1"
decision_reason: "score >= 0.62"
score_total: 0.6225
analysis_tier: "tier1"
analysis_language: "zh-CN"
evidence_mode: "strict"
fulltext_status: "fulltext"
evidence_count: 8
quality_gate_passed: true
generator_model: "bailian/qwen3-coder-plus"
generated_at: "2026-03-02T17:24:27.904598+00:00"
authors:
  - "Zhengbo Wang"
  - "Jian Liang"
  - "Ran He"
  - "Zilei Wang"
  - "Tieniu Tan"
topic_hits:
  - "lora"
  - "low-rank"
---

# Taming Momentum: Rethinking Optimizer States Through Low-Rank Approximation

## Summary

本文提出了LoRA-Pre，一种基于低秩近似的新型优化器，旨在解决大型语言模型训练中Adam和Muon等现代优化器因一阶和二阶动量导致的显著内存开销问题。作者通过数学等价性证明，将指数移动平均(EMA)动量更新重新表述为在线线性回归器的训练过程。基于这一洞察，LoRA-Pre通过将完整动量矩阵分解为紧凑的低秩子空间来压缩优化器状态，同时保持优化性能。实验验证表明，LoRA-Pre在预训练和微调场景中均表现出色，实现了卓越的排名效率，在相同排名下相比基线方法获得显著性能提升。该方法不仅适用于Adam优化器，还成功扩展到Muon优化器，展示了其通用性。

## Problem Setting

大型语言模型的成功伴随着巨大的训练和适应成本，其中优化器状态是主要的内存负担。以Adam优化器为例，它不仅维护模型权重，还需要存储梯度的一阶和二阶矩估计，使内存使用量增加三倍，进一步加剧了可扩展性瓶颈。传统的低秩优化方法如GaLore通过SVD投影将梯度信息压缩到低秩子空间，但存在周期性子空间更新导致的优化不连续性和误差累积问题。现有LoRA方法虽然在微调中有效，但在从零开始的预训练阶段面临根本挑战，因为预训练需要全秩权重更新来学习整个参数空间的多样化表示，而LoRA的低秩假设与之不匹配。这些问题凸显了对更动态、高效的优化策略的需求。

## Method Breakdown

LoRA-Pre的核心创新在于建立EMA动量更新与在线线性回归之间的数学等价关系。标准EMA更新m_{t+1} = β·m_t + (1-β)·g可重写为m_t - (1-β)·(m_t - g)，这等同于以学习率1-β对目标函数L(m;g) = 1/2·||m-g||_F^2进行梯度下降。基于此等价性，作者将动量矩阵m分解为两个低秩矩阵的乘积：m = m_B·m_A，其中m_B∈ℝ^(p×r)，m_A∈ℝ^(r×q)，r≪min(p,q)。对于一阶动量，优化目标变为min_{m_B,m_A} L(m_B,m_A;g) = 1/2·||m_B m_A - g||_F^2。通过牛顿法推导出闭式更新规则：m_B ← (1-γ_1)·m_B + γ_1·g m_A^T (m_A m_A^T)^(-1)，m_A ← (1-γ_1)·m_A + γ_1·(m_B^T m_B)^(-1) m_B^T g。对于二阶动量，为保证元素正性，采用v = (v_B v_A)^(∘2)的重参数化。

## Training and Inference

在训练阶段，LoRA-Pre通过在线梯度流持续演化低秩因子，实现连续子空间自适应，避免了传统方法中周期性更新或启发式近似带来的不稳定性。该方法应用于注意力层和MLP层的所有参数，其他参数仍使用标准Adam优化器。在预训练设置中，默认对60M、130M、350M和1B参数模型分别设置128、256、256和512的排名。在微调阶段，使用相同的超参数配置，但默认排名设为8，学习率为2e-5。推理阶段不受影响，因为LoRA-Pre仅改变优化器状态的存储和更新方式，不影响模型参数本身。这种设计确保了训练效率的提升不会影响最终模型的推理性能。

## Experiment Breakdown

实验评估涵盖预训练和微调两个阶段。预训练实验遵循GaLore的设置，在C4数据集上从头训练不同规模的Llama模型（60M、130M、350M、1B参数），训练token数量分别为1.1B、2.2B、6.4B、13.1B。比较方法包括Adam、Muon、GaLore、Low-Rank、LoRA、ReLoRA、SLTrain、LORO、Fira等。微调实验在Llama-3.1-8B和Llama-2-7B模型上进行，使用MetaMath100k数据集微调，然后在GSM8K和MATH-500数据集上评估。微调基线包括LoRA、rsLoRA、DoRA、GaLore等。消融研究系统地评估了不同排名配置下的性能变化。所有实验保持相同的超参数和训练配置以确保公平比较。

## Result Interpretation

实验结果表明LoRA-Pre在所有模型规模上都取得了最佳或次佳性能。在预训练中，LoRA-Pre Adam和LoRA-Pre Muon在几乎所有四个模型规模上都达到最高或第二高的性能，相比Fira等竞争方法在较大规模模型上表现更优。在微调中，相比标准LoRA，LoRA-Pre在Llama-3.1-8B上获得3.14分提升，在Llama-2-7B上获得6.17分提升。特别值得注意的是LoRA-Pre的排名效率：在60M模型中，LoRA-Pre Adam在rank=16时达到与GaLore在rank=128相当的性能，实现了8倍的排名需求减少；在130M模型中，LoRA-Pre Adam在rank=16时匹配GaLore在rank=256的性能，实现了16倍效率提升。这种卓越的排名效率归因于其连续子空间自适应机制，避免了传统方法中的误差累积问题。

## Limitations

该研究存在几个重要限制。首先，低秩近似可能在某些特定任务或模型架构中无法完全捕获动量的复杂结构，特别是在需要高秩更新的场景中。其次，二阶动量的正性约束处理引入了额外的计算复杂度，特别是矩阵求逆操作可能在大规模部署中成为瓶颈。第三，理论收敛性分析依赖于特定的假设条件，这些假设在实际训练环境中可能不完全成立。第四，当前实现主要针对Transformer架构，对于其他网络结构的适用性需要进一步验证。第五，虽然在数学上证明了EMA与在线线性回归的等价性，但这种等价性在实际数值计算中的精度损失未充分讨论。最后，内存节省的实际效果依赖于具体的硬件配置和实现细节。

## Reproducibility

该研究提供了良好的可复现性基础。代码已公开发布在https://github.com/mrflogs/LoRA-Pre，包含详细的算法实现。实验设置严格遵循GaLore的配置，使用相同的C4数据集和MetaMath100k数据集，确保了基准比较的一致性。超参数选择有明确说明，包括不同模型规模的默认排名设置和学习率范围。然而，完整的可复现性仍需考虑以下因素：具体的硬件环境配置、随机种子设置、分布式训练的具体实现细节未完全披露。为了完全复现实验结果，需要访问相同的计算资源并使用相同的软件版本。论文提供了附录中的详细算法描述，包括Adam和Muon优化器的LoRA-Pre变体的具体实现。

## Ideas for My Work

基于LoRA-Pre的连续子空间自适应机制，可以在我的工作中实施动态秩调整策略。具体而言，可以监控梯度的奇异值分布变化，当检测到梯度结构发生显著变化时，动态增加低秩分解的秩数以捕获新的特征方向，而在梯度结构稳定时降低秩数以节省内存。这种自适应策略可以通过计算梯度矩阵的低秩近似误差来触发，当误差超过预设阈值时自动调整秩数。这种方法能够平衡内存效率和优化性能，特别适用于训练过程中梯度结构发生变化的任务。实现时可以定期计算梯度的主成分，并根据能量集中度动态调整低秩分解的维度。

## Writing Usage

论文在方法介绍部分巧妙地使用了'Your Momentum is a Secret Online Linear Regressor'这一标题，既吸引读者注意又准确概括了核心洞察。在数学公式呈现方面，作者通过逐步推导将复杂的EMA更新重写为梯度下降形式，使读者容易理解等价性的建立过程。实验结果展示采用了表格和图表相结合的方式，清晰对比了多种方法的性能差异。在讨论排名效率时，作者通过具体的倍数比较（如8×、16×）直观地展现了方法的优势。论文还善于使用'Notably'、'Specifically'等过渡词来强调重要发现，增强了论述的逻辑性和说服力。

## Theory Notes

论文的核心理论贡献在于建立了EMA动量更新与在线线性回归的数学等价性：m←β·m+(1-β)·g ⟺ min_m L(m;g)=1/2·||m-g||_F^2。这个等价性揭示了动量累积本质上是在拟合一个线性模型来近似梯度历史。基于此，作者提出通过低秩因子化m=m_B·m_A来压缩动量矩阵，将内存复杂度从p×q降至(p+q)×r。对于二阶动量的正性约束，采用v=(v_B v_A)^(∘2)的重参数化策略。理论分析表明，这种连续子空间自适应机制相比周期性更新方法能更好地保持子空间与梯度结构的对齐，从而减少误差累积并提高优化效率。

## Metadata

论文来源于arXiv预印本服务器，编号为arxiv:2602.24283v1，提交时间为2026年2月27日。作者来自中国科学院自动化研究所、中国科学技术大学等机构，具有深厚的机器学习研究背景。论文经过同行评议，属于计算机科学领域的可信学术成果。可复现性等级较高，代码已开源且实验设置详细。论文涵盖了理论分析、算法推导、实验验证等多个层面，研究质量较高。由于是预印本，可能存在后续修订。研究主题聚焦于优化器状态压缩，与当前大模型训练的内存效率问题高度相关。

## Evidence Anchors

1. claim: EMA动量更新等价于在线线性回归
   - evidence: 论文显示m_{t+1} = β·m_t + (1-β)·g可重写为m_t - (1-β)·(m_t - g)，等同于对目标函数L(m;g)=1/2·||m-g||_F^2进行梯度下降，学习率为1-β
   - location: Section 3.2, Equation (8)
2. claim: LoRA-Pre在预训练中优于基线方法
   - evidence: Table 1显示LoRA-Pre Adam和LoRA-Pre Muon在几乎所有四个模型规模(60M, 130M, 350M, 1B)上都达到最高或第二高的性能
   - location: Section 4.1, Table 1
3. claim: LoRA-Pre在微调中显著优于标准LoRA
   - evidence: 相比标准LoRA，LoRA-Pre在Llama-3.1-8B上获得3.14分提升，在Llama-2-7B上获得6.17分提升
   - location: Abstract and Section 4.2
4. claim: LoRA-Pre具有卓越的排名效率
   - evidence: 在60M模型中，LoRA-Pre Adam在rank=16时达到与GaLore在rank=128相当的性能，实现8倍排名需求减少
   - location: Section 4.3, Figure 2
5. claim: 二阶动量需要特殊处理保证正性
   - evidence: 论文指出直接参数化v=v_B·v_A无法保证元素正性，因此采用v=(v_B v_A)^(∘2)的重参数化策略
   - location: Section 3.3
6. claim: 连续子空间自适应优于周期性更新
   - evidence: 论文解释LoRA-Pre的连续子空间调整机制能维持更好的对齐，避免误差累积，而GaLore的周期性更新导致子空间错位
   - location: Section 4.3
7. claim: LoRA-Pre可扩展到不同优化器
   - evidence: 论文开发了LoRA-Pre的Adam和Muon优化器变体，并在实验中验证了两种变体的有效性
   - location: Section 3.3 and Section 4
8. claim: 内存复杂度从p×q降至(p+q)×r
   - evidence: 论文明确指出通过m=m_B·m_A分解，将完整动量矩阵m∈ℝ^(p×q)分解为m_B∈ℝ^(p×r)和m_A∈ℝ^(r×q)，实现内存节省
   - location: Introduction and Section 3.3

## Action Items

- 实现LoRA-Pre的动态秩调整机制，通过监控梯度奇异值分布变化来自动调整低秩分解的秩数
- 将LoRA-Pre扩展到其他优化器架构，如SGD with momentum或其他新兴优化器，验证其通用性
- 在不同网络架构（如CNN、RNN）上测试LoRA-Pre的有效性，探索其跨架构适用性

## Generator Notes

- uncertainty: 论文为预印本，具体实验细节和理论分析的完整性有待正式发表后确认
- fulltext_status: fulltext
