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
generated_at: "2026-03-02T13:49:37.445138+00:00"
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

本文提出了LoRA-Pre，一种基于低秩近似的新型优化器，旨在解决现代大语言模型训练中Adam和Muon等优化器因一阶和二阶动量而产生的显著内存开销问题。核心创新在于将指数移动平均(EMA)动量更新重新表述为通过在线梯度流训练线性回归器的过程。基于这一理论等价性，LoRA-Pre通过将完整动量矩阵分解为在线线性学习器中的紧凑低秩子空间来减少优化器内存占用。实验验证表明，LoRA-Pre在Llama架构系列模型的预训练中实现了最高性能，从60M参数扩展到1B参数。特别值得注意的是，LoRA-Pre展现出卓越的秩效率，在相同秩设置下优于所有基线方法，并且在微调场景中也表现出色，相比标准LoRA在Llama-3.1-8B上提升了3.14分，在Llama-2-7B上提升了6.17分。

## Problem Setting

大型语言模型的成功伴随着巨大的训练和适应成本，其中优化器状态是主要负担之一。以Adam为例，该优化器不仅维护模型权重，还维护梯度的一阶和二阶矩估计，使内存使用量增加三倍并加剧了可扩展性瓶颈。传统的低秩优化方法如GaLore采用SVD投影将梯度信息压缩到低秩子空间，但存在周期性子空间更新导致的优化不连续性和误差累积问题。现有LoRA方法在微调中有效，但在从零开始的预训练中面临根本挑战，因为预训练需要全秩权重更新来学习整个参数空间的多样化表示，而LoRA的低秩假设与之不匹配。因此，需要一种能够动态适应变化梯度子空间的更高效优化策略。

## Method Breakdown

LoRA-Pre的核心思想是建立EMA动量更新与在线线性回归之间的数学等价性：m←β·m+(1-β)·g ⟺ min_m L(m;g)=1/2·||m-g||_F^2。基于此等价性，作者提出通过低秩分解压缩动量矩阵：m=m_B·m_A，其中m_B∈ℝ^(p×r)，m_A∈ℝ^(r×q)，r≪min(p,q)。对于一阶动量压缩，目标函数为min_{m_B,m_A} L(m_B,m_A;g)=1/2·||m_B m_A-g||_F^2，通过牛顿法得到闭式更新规则：m_B←(1-γ_1)·m_B+γ_1·g m_A^T(m_A m_A^T)^(-1)，m_A←(1-γ_1)·m_A+γ_1·(m_B^T m_B)^(-1) m_B^T g。对于二阶动量压缩，为保证元素正性，重新参数化为v=(v_B v_A)∘2，确保√v可计算。

## Training and Inference

在训练阶段，LoRA-Pre通过在线梯度流持续演化低秩因子，实现连续子空间自适应，避免了周期性更新或启发式近似相关的不稳定性。该方法适用于任何基于动量的优化器，包括Adam和Muon。在推理阶段，由于优化器状态被压缩为低秩形式，显著减少了内存占用，同时保持了优化性能。训练过程中，LoRA-Pre应用于注意力和MLP层的所有参数，其他参数仍使用标准Adam优化器。默认情况下，60M、130M、350M和1B参数模型的秩分别设置为128、256、256和512。在微调任务中，默认秩设为8，学习率设为2e-5。

## Experiment Breakdown

实验评估涵盖内存高效预训练和内存高效微调两个方面。预训练实验遵循GaLore的设置，在C4数据集上从头训练不同规模的Llama模型（60M、130M、350M、1B参数），训练token数量分别为1.1B、2.2B、6.4B、13.1B。微调实验在MetaMath100k数据集上对Llama-3.1-8B和Llama-2-7B进行微调，然后在GSM8K和MATH-500数据集上评估。比较方法包括Adam、Muon、GaLore、Low-Rank、LoRA、ReLoRA、SLTrain、LORO、Fira等。消融研究测试了不同秩配置（4、16、64、128、256）对性能的影响，验证了LoRA-Pre的秩效率优势。

## Result Interpretation

预训练结果显示，LoRA-Pre Adam和LoRA-Pre Muon在几乎所有四个模型规模上都达到了最高或第二高的性能。LoRA-Pre Adam在130M、350M和1B模型上分别比之前最佳高效基线高出0.81、2.45和1.6困惑度点数。微调结果表明，LoRA-Pre在所有实验配置中始终获得最高分数，当训练Llama-3.1-8B与Adam时平均提升2.59分，Llama-2-7B与Adam时提升4.88分。消融研究表明，LoRA-Pre在较低秩下就能达到与其他方法在高秩下的相当性能，例如60M模型中LoRA-Pre Adam在rank=16时达到GaLore在rank=128时的性能，实现了8倍的秩需求减少。这种秩效率归因于LoRA-Pre的连续子空间自适应机制，避免了周期性更新导致的误差累积。

## Limitations

该方法存在几个具体限制：首先，低秩分解引入了额外的矩阵乘法运算，可能增加计算复杂度，尽管内存使用减少；其次，秩的选择需要仔细调整，虽然LoRA-Pre对秩选择相对鲁棒，但最优秩仍依赖于具体任务和模型规模；第三，对于某些特定的梯度结构，低秩近似可能无法完全捕获复杂的动量模式；第四，该方法主要针对基于动量的优化器，对其他类型的优化器适用性有限；第五，理论收敛性分析基于特定假设条件，实际应用中可能存在偏差；第六，逆矩阵计算（如(m_A m_A^T)^(-1)）在数值不稳定时可能导致训练困难。

## Reproducibility

该研究提供了完整的可复现性保障：代码已公开发布在https://github.com/mrflogs/LoRA-Pre；详细的算法伪代码在附录B中提供，包括LoRA-Pre用于Adam和Muon优化器的具体实现；实验设置与GaLore保持一致，确保公平比较；提供了具体的超参数配置，包括不同模型规模的默认秩设置和学习率选择范围；详细的理论分析在附录C中提供，包括近似误差和收敛性的数学证明；附录D提供了额外的实验结果和超参数敏感性分析；所有比较基线都有明确的引用和实现细节描述，使得实验可以精确重现。

## Ideas for My Work

基于LoRA-Pre的理论框架，我可以在自己的工作中实施三个具体改进：第一，将LoRA-Pre的在线线性回归等价性原理应用到我的优化器设计中，特别是针对特定领域的大模型训练，通过实现连续子空间自适应机制来减少内存占用，同时保持优化性能。具体操作是重构现有动量更新公式为线性回归问题，并应用低秩分解技术。第二，利用LoRA-Pre展现的跨优化器兼容性，开发适用于我当前使用的特定优化器（如SGD变体或其他自定义优化器）的低秩版本，通过将该优化器的动量项转换为低秩形式来实现内存效率提升。第三，借鉴LoRA-Pre的秩效率特性，在资源受限环境中部署我的模型训练流程，通过系统性地降低优化器状态的秩来减少GPU内存使用，同时监控性能指标以找到内存效率和模型性能的最佳平衡点。

## Writing Usage

论文写作中值得借鉴的表达方式包括：'我们建立了新颖的理论连接'用于介绍核心贡献；'我们的关键洞察是'用于突出重要发现；'通过广泛的实验验证'用于强调实证支撑；'具体而言'用于详细阐述方法；'值得注意的是'用于强调重要结果；'为了确保公平比较'用于说明实验设计原则；'消融研究表明'用于呈现控制变量分析；'我们将其归因于'用于解释现象原因；'这证实了'用于总结验证结果；'在附录中提供'用于处理补充材料；'我们的代码公开可用'用于增强可复现性。

## Theory Notes

理论基础的核心是EMA动量更新与在线线性回归的等价性：m←β·m+(1-β)·g等价于min_m L(m;g)=1/2·||m-g||_F^2。这个等价性揭示了动量累积可以视为拟合线性模型来逼近梯度历史。通过低秩分解m=m_B·m_A，内存复杂度从p×q降至(p+q)×r，实现显著内存节省。牛顿法导出的闭式更新规则避免了反向传播需求。二阶动量的Hadamard积重新参数化v=(v_B v_A)∘2确保元素正性。理论分析表明，连续子空间自适应机制优于周期性更新方法，因为后者存在子空间错配和误差累积问题。

## Metadata

论文来源：arXiv预印本，ID为arxiv:2602.24283v1，发表时间为2026年2月27日。作者团队来自中国科学院自动化研究所、中国科学院大学人工智能学院和中国科学技术大学。研究可靠性较高，提供了完整的理论推导、详细的实验验证和消融分析。可复现性等级为高，代码已开源，算法细节完整，实验设置明确。论文属于机器学习优化器领域的前沿研究，专注于大语言模型训练的内存效率问题，具有重要的实用价值和理论意义。

## Evidence Anchors

1. claim: EMA动量更新等价于在线线性回归
   - evidence: m←β·m+(1-β)·g ⟺ min_m L(m;g)=1/2·||m-g||_F^2，这个等价性揭示了动量累积可以视为拟合线性模型来逼近梯度历史
   - location: Section 3.2, Equation (1)
2. claim: LoRA-Pre在预训练中表现最优
   - evidence: Table 1显示LoRA-Pre Adam和LoRA-Pre Muon在几乎所有四个模型规模(60M, 130M, 350M, 1B)上都达到了最高或第二高的性能
   - location: Section 4.1, Table 1
3. claim: LoRA-Pre在微调中显著优于标准LoRA
   - evidence: 相比标准LoRA，LoRA-Pre在Llama-3.1-8B上提升了3.14分，在Llama-2-7B上提升了6.17分
   - location: Abstract and Section 4.2
4. claim: LoRA-Pre具有卓越的秩效率
   - evidence: 60M模型中LoRA-Pre Adam在rank=16时达到GaLore在rank=128时的性能，实现了8倍的秩需求减少
   - location: Section 4.3, Figure 2
5. claim: 低秩分解显著减少内存复杂度
   - evidence: 通过低秩分解m=m_B·m_A，内存复杂度从p×q降至(p+q)×r，其中r≪min(p,q)
   - location: Section 3.3
6. claim: 二阶动量需特殊处理保证正性
   - evidence: 为保证√v可计算，二阶动量重新参数化为v=(v_B v_A)∘2，使用Hadamard积确保元素正性
   - location: Section 3.3, Second-Order Momentum Compression
7. claim: 连续子空间自适应优于周期性更新
   - evidence: LoRA-Pre通过在线梯度流持续演化低秩因子，避免了周期性更新导致的误差累积
   - location: Section 3.3 and 4.3
8. claim: LoRA-Pre兼容多种优化器
   - evidence: 论文展示了LoRA-Pre在Adam和Muon优化器上的实现，证明其适用于任何基于动量的优化器
   - location: Section 3.3 and 4.1-4.2

## Action Items

- 实现LoRA-Pre的低秩动量更新算法，重点关注一阶和二阶动量的分解及闭式更新规则的正确实现
- 在现有训练流程中集成LoRA-Pre优化器，对比标准Adam优化器的内存使用和训练性能差异
- 进行秩敏感性分析，系统测试不同秩设置对模型收敛性和最终性能的影响，找到最优配置

## Generator Notes

- uncertainty: 部分理论分析细节和收敛性证明位于附录中，正文未详述具体数学推导过程
- fulltext_status: fulltext
