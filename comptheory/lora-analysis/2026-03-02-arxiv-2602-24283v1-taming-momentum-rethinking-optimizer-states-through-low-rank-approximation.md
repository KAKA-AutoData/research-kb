---
paper_id: "arxiv:2602.24283v1"
priority: "P1"
domain: "comptheory"
source: "arxiv"
published_at: "2026-02-27T18:57:06+00:00"
url: "http://arxiv.org/abs/2602.24283v1"
decision_reason: "daily historical upgrade"
score_total: 0
analysis_tier: "tier1"
analysis_language: "zh-CN"
evidence_mode: "strict"
fulltext_status: "fulltext"
evidence_count: 8
quality_gate_passed: true
generator_model: "bailian/qwen3-coder-plus"
generated_at: "2026-03-02T13:56:37.083801+00:00"
authors:
  - ""
topic_hits:
  - ""
---

# Taming Momentum: Rethinking Optimizer States Through Low-Rank Approximation

## Summary

本文提出了LoRA-Pre，一种基于低秩近似的新型优化器，旨在解决现代大语言模型训练中Adam和Muon等优化器因一阶和二阶动量而产生的显著内存开销问题。核心创新在于将指数移动平均(EMA)动量更新重新表述为通过在线梯度流训练线性回归器的过程。基于这一理论等价性，LoRA-Pre通过将完整的动量矩阵分解为在线线性学习器中的紧凑低秩子空间来减少优化器的内存占用。实验验证表明，LoRA-Pre在Llama架构系列模型的预训练中实现了最高性能，从60M到1B参数规模均表现出色。特别值得注意的是，LoRA-Pre展现出卓越的秩效率，仅使用基线方法1/8的秩就能达到相当或更优的结果。在微调场景中，LoRA-Pre同样优于所有高效微调基线，在相同秩条件下相比标准LoRA在Llama-3.1-8B上提升3.14分，在Llama-2-7B上提升6.17分。

## Problem Setting

大型语言模型的成功伴随着巨大的训练和适应成本，其中优化器状态是主要负担之一。以Adam为例，该优化器不仅维护模型权重，还维护梯度的一阶和二阶矩估计，使内存使用量增加三倍并加剧扩展性瓶颈。现有低秩优化方法如GaLore、Flora、Fira等采用投影梯度下降策略，通过SVD或随机映射初始化投影矩阵，将梯度投影到较小子空间进行优化器状态计算，然后投影回原空间实现参数更新。然而，这些方法需要定期子空间更新，由于无法即时更新子空间，导致优化器状态计算中误差累积，产生次优性能。本研究针对预训练阶段的全秩权重更新需求与现有低秩方法假设不匹配的问题，提出了一种动态适应变化梯度子空间的新方法。

## Method Breakdown

LoRA-Pre的核心思想是建立EMA动量更新与在线线性回归器之间的数学等价性：m←β·m+(1-β)·g⟺min_m L(m;g)=1/2·||m-g||_F^2。基于此等价性，作者将动量矩阵m分解为两个低秩矩阵的乘积：m=m_B·m_A，其中m_B∈ℝ^(p×r)，m_A∈ℝ^(r×q)，r≪min(p,q)。这种分解将内存复杂度从p×q降低到(p+q)×r，对大规模模型产生巨大内存节省。对于一阶动量压缩，目标函数为min_(m_B,m_A) L(m_B,m_A;g)=1/2·||m_B m_A-g||_F^2，通过牛顿法得到闭式更新规则。对于二阶动量压缩，由于Adam参数更新规则需要平方根运算，作者重新参数化为v=(v_B v_A)^∘2以确保元素级正性。整个方法兼容任何基于动量的优化器，包括Adam和Muon。

## Training and Inference

在训练过程中，LoRA-Pre通过在线梯度流连续更新低秩因子，避免了传统方法中周期性子空间更新带来的不稳定性。对于一阶动量，更新规则为：m_B←(1-γ_1)·m_B+γ_1·g m_A^T(m_A m_A^T)^(-1)，m_A←(1-γ_1)·m_A+γ_1·(m_B^T m_B)^(-1) m_B^T g。对于二阶动量，由于需要保持元素级正性，采用v=(v_B v_A)^∘2的重新参数化，更新规则相应调整为处理|g|而非g^2。推理阶段保持不变，因为低秩分解仅影响优化器状态的存储和更新，不影响最终模型参数的前向传播。这种方法实现了真正的在线子空间适应，每个步骤都调整低秩因子以匹配当前梯度结构。

## Experiment Breakdown

实验设计涵盖预训练和微调两个阶段。预训练实验遵循GaLore设置，使用C4数据集训练不同规模的Llama模型（60M、130M、350M、1B参数），训练token数量分别为1.1B、2.2B、6.4B、13.1B。微调实验在Llama-3.1-8B和Llama-2-7B模型上进行，使用MetaMath100k数据集微调，然后在GSM8K和MATH-500数据集上评估。对比方法包括Adam、Muon、GaLore、Low-Rank、LoRA、ReLoRA、SLTrain、LORO、Fira等。预训练中默认rank设置为：60M模型128，130M和350M模型256，1B模型512。微调中默认rank为8，学习率为2e-5。所有实验保持相同的缩放因子0.25以确保公平比较。

## Result Interpretation

预训练结果显示，LoRA-Pre在几乎所有模型规模上都达到了最高或第二高的性能。LoRA-Pre Adam和LoRA-Pre Muon在四个模型尺寸上都取得了优异表现，相比之前的最佳高效基线在130M、350M和1B模型上分别提升了0.81、2.45和1.6困惑度点数。微调结果表明，LoRA-Pre在所有实验配置中持续取得最高分数，特别是在Llama-3.1-8B上相比LoRA提升3.14点，Llama-2-7B上提升6.17点。消融研究表明LoRA-Pre具有卓越的秩效率：60M模型中LoRA-Pre Adam在rank=16时达到GaLore在rank=128时的性能，实现8倍秩需求减少；130M模型中LoRA-Pre Adam在rank=16时匹配GaLore在rank=256时的表现，实现16倍效率提升。这种效率归因于LoRA-Pre的连续子空间适应机制，避免了周期性更新导致的误差累积。

## Limitations

LoRA-Pre方法存在几个具体限制。首先，低秩分解引入了额外的矩阵逆运算，如(m_A m_A^T)^(-1)和(m_B^T m_B)^(-1)，这些操作在数值不稳定时可能导致训练崩溃，特别是在低精度训练环境中。其次，理论分析依赖于矩阵满秩假设，实际应用中可能不总是满足，影响算法收敛性。第三，虽然方法声称适用于任何基于动量的优化器，但实验仅验证了Adam和Muon两种优化器，缺乏对其他优化器的广泛测试。第四，秩的选择仍然需要经验性调整，尽管方法对秩选择相对鲁棒，但最优秩值与模型大小和任务类型的关系尚未完全量化。第五，计算复杂度分析显示，虽然内存使用减少，但每次迭代的计算开销可能增加，特别是在小批量训练时。

## Reproducibility

论文提供了详细的算法描述和理论证明，附录B中包含了LoRA-Pre用于Adam和Muon优化器的完整伪代码，附录A提供了定理3.1的完整证明。代码已开源在https://github.com/mrflogs/LoRA-Pre。实验设置详细说明了超参数选择，包括学习率范围{0.01, 0.005, 0.001, 0.0005, 0.0001}，默认rank设置，以及相同的缩放因子0.25。数据集使用C4用于预训练，MetaMath100k用于微调，评估使用GSM8K和MATH-500。为了完全复现结果，需要：1)精确的随机种子设置；2)硬件配置信息以确保数值精度一致性；3)完整的训练日志以验证收敛行为；4)不同批次大小下的性能验证；5)不同精度格式下的数值稳定性测试。

## Ideas for My Work

首先，将LoRA-Pre的在线线性回归框架扩展到其他优化器状态压缩场景。具体执行动作：分析你当前使用的优化器（如SGD with momentum、RMSprop等）的动量更新机制，将其重新表述为在线回归问题，然后应用低秩分解技术。你可以选择特定的网络层（如注意力机制或FFN层）作为试点，逐步验证这种方法在你任务上的有效性，并监控内存使用和训练速度的变化。其次，探索LoRA-Pre在不同模型架构中的适用性。具体执行动作：将LoRA-Pre应用于你正在研究的特定模型架构（如Vision Transformer、Diffusion Model等），不仅仅是Transformer语言模型。你需要调整低秩分解的维度参数以适应不同架构的参数形状，并设计相应的消融实验来确定最优的rank设置和学习率配置。最后，结合LoRA-Pre与其他参数高效微调方法。具体执行动作：将LoRA-Pre的动量压缩与LoRA的权重分解相结合，在同一训练过程中同时压缩优化器状态和模型参数。你可以设计混合训练策略，在训练的不同阶段动态调整压缩比例，或者开发自适应的秩选择机制，根据梯度的低秩特性自动调整压缩强度。

## Writing Usage

论文采用了清晰的数学表述和严谨的理论推导，特别是公式(1)中EMA动量更新与线性回归的等价性展示，为后续方法设计提供了坚实的理论基础。引言部分有效地建立了问题背景，从LLM成功带来的挑战到具体的技术瓶颈，逻辑链条清晰。相关工作部分系统地分类了低秩适配和低秩预训练方法，并准确指出了现有方法的局限性。方法部分按照预备知识、理论洞察、具体实现的顺序组织，便于读者理解。实验部分提供了详尽的对比和消融分析，结果呈现清晰。写作技巧包括：使用数学公式精确表达核心思想，通过图表直观展示方法原理，提供详细的实验设置确保可复现性，以及通过消融研究深入分析方法特性。

## Theory Notes

论文的核心理论贡献是揭示了EMA动量更新与在线线性回归之间的数学等价性：m←β·m+(1-β)·g等价于min_m L(m;g)=1/2·||m-g||_F^2。这个发现将传统的动量维护重新解释为连续的线性模型拟合过程。基于此等价性，作者推导出低秩动量的更新规则，其中一阶动量的分解m=m_B·m_A通过牛顿法得到闭式解。对于二阶动量，由于需要保持元素级正性，采用v=(v_B v_A)^∘2的特殊参数化。理论分析（附录C）涵盖了近似误差和收敛性的详细讨论，包括有效动量的定义和有界性分析。这种方法将优化器状态压缩转化为标准的机器学习问题，为后续的低秩近似提供了理论支撑。

## Metadata

论文ID: arxiv:2602.24283v1，标题: Taming Momentum: Rethinking Optimizer States Through Low-Rank Approximation，来源: arxiv，发布日期: 2026年2月27日，作者: Zhengbo Wang等，机构: 中科院自动化所、中科大等。可靠性等级: 高，基于严格的数学推导和广泛的实验验证。可复现性等级: 高，提供了开源代码、详细算法描述和完整的实验设置。论文属于机器学习领域的优化算法研究，专注于大语言模型训练的内存效率问题。研究类型: 方法论创新，提出了新的优化器设计范式。技术领域: 低秩优化、在线学习、动量方法。

## Evidence Anchors

1. claim: EMA动量更新与在线线性回归等价
   - evidence: 论文公式(1)明确展示了m←β·m+(1-β)·g⟺min_m L(m;g)=1/2·||m-g||_F^2的数学等价性
   - location: Section 3.2, Equation (1)
2. claim: LoRA-Pre在预训练中达到最高性能
   - evidence: 表1显示LoRA-Pre Adam和LoRA-Pre Muon在几乎所有模型尺寸上都获得最高或第二高性能
   - location: Table 1, Section 4.1
3. claim: LoRA-Pre实现8倍秩效率提升
   - evidence: 图2显示60M模型中LoRA-Pre Adam在rank=16时达到GaLore在rank=128时的性能
   - location: Figure 2, Section 4.3
4. claim: 微调中LoRA-Pre相比LoRA显著提升
   - evidence: 表2显示相比标准LoRA，LoRA-Pre在Llama-3.1-8B上提升3.14点，在Llama-2-7B上提升6.17点
   - location: Table 2, Section 4.2
5. claim: 低秩分解减少内存复杂度
   - evidence: 论文指出将内存复杂度从p×q降低到(p+q)×r，其中r≪min(p,q)
   - location: Section 3.3
6. claim: 二阶动量需保持元素级正性
   - evidence: 论文提到由于Adam参数更新需要√v，必须保证v_i,j>0,∀i,j，因此采用v=(v_B v_A)^∘2重新参数化
   - location: Section 3.3
7. claim: 连续子空间适应避免误差累积
   - evidence: 论文指出LoRA-Pre通过直接演化低秩因子实现连续子空间适应，消除周期更新的不稳定性
   - location: Section 1 and 2 Related Works
8. claim: 方法兼容多种优化器
   - evidence: 论文开发了LoRA-Pre的Adam和Muon变体，并在实验中验证了跨优化器的有效性
   - location: Section 3.3 and 4.1

## Action Items

- 实现LoRA-Pre的Adam变体，重点关注一阶和二阶动量的低秩分解，验证其在你当前项目中的内存节省效果
- 设计消融实验测试不同rank设置对模型性能的影响，找到内存效率和性能的最佳平衡点
- 将LoRA-Pre扩展到其他基于动量的优化器，验证其通用性和跨优化器的性能表现

## Generator Notes

- uncertainty: 论文中某些具体的数值结果需要通过复现实验验证，理论分析中的假设条件在实际应用中的满足程度需要进一步确认
- fulltext_status: fulltext
