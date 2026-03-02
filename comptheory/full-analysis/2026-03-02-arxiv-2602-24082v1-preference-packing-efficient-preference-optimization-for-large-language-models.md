---
paper_id: "arxiv:2602.24082v1"
priority: "P1"
domain: "comptheory"
source: "arxiv"
published_at: "2026-02-27T15:19:57+00:00"
url: "http://arxiv.org/abs/2602.24082v1"
decision_reason: "score >= 0.62"
score_total: 0.6225
analysis_tier: "tier1"
analysis_language: "zh-CN"
evidence_mode: "strict"
fulltext_status: "fulltext"
evidence_count: 8
quality_gate_passed: true
generator_model: "bailian/qwen3-coder-plus"
generated_at: "2026-03-02T13:54:14.932281+00:00"
authors:
  - "Jaekyung Cho"
topic_hits:
  - "attention"
  - "kv cache"
---

# Preference Packing: Efficient Preference Optimization for Large Language Models

## Summary

本文提出了Preference Packing方法，这是一种针对大型语言模型偏好优化训练的资源效率提升技术。该方法通过将多个响应序列打包到共享输入提示的单个序列中，显著减少了重复计算和KV缓存内存使用。作者在理论分析中证明了该方法在特定条件下能够降低注意力计算复杂度，并在实际实验中验证了在不同数据集上实现了至少37%的训练时间减少。该方法特别适用于奖励模型训练和直接偏好优化(DPO)等需要处理相同输入提示多个响应的场景。研究还表明，Preference Packing可以与现有的批处理排序技术结合使用，在大规模分布式训练环境中实现高达3.22倍的速度提升。

## Problem Setting

随着大型语言模型规模的持续增长，资源高效的训练优化技术变得越来越重要。传统的对齐调优方法如奖励模型训练和直接偏好优化(DPO)使用包含相同输入提示多个响应的数据，导致大量冗余计算和重复内存使用。在这些场景中，输入提示通常占数据的主要部分，但会被多次重复处理，造成计算资源浪费。现有技术如批处理打包(batch packing)和批处理排序(batch sorting)主要针对预训练和监督微调设计，无法有效解决偏好优化中的重复输入问题。该问题在多模态场景中尤为突出，因为图像输入可能被转换为数千个token，而文本响应相对较短，这种长度差异加剧了资源浪费问题。

## Method Breakdown

Preference Packing的核心思想是将传统批处理方式中重复的输入提示合并为单一处理，通过重新构建数据序列格式实现效率提升。具体而言，方法将原本分离的(x, y₁), (x, y₂), ..., (x, yₖ)格式转换为单个序列(x, y₁, y₂, ..., yₖ)，其中x为共享输入提示，yᵢ为不同响应。该方法使用特殊的注意力掩码确保每个响应只能关注输入提示而不能相互关注，同时调整位置编码以保持原始位置信息。理论上，当满足条件l_in + ∑l_respᵏ < √K × (l_in + l_resp_max)时，该方法能够降低注意力计算成本。对于Flash Attention，内存效率提升更为显著，因为其内存使用与序列长度呈线性关系。

## Training and Inference

在训练阶段，Preference Packing通过减少重复的注意力操作和KV缓存占用显著提升了计算效率。该方法在推理阶段同样适用，特别是在需要处理多个候选响应的场景中。训练过程中，原有的批处理大小概念发生变化，由于每个偏好数据点包含多个响应序列，有效批处理大小增加了K倍。该方法与LoRA等参数高效微调技术兼容，实验显示即使在r=1的LoRA设置下也能获得显著性能提升。在分布式训练环境中，该方法与FSDP(full sharding data parallelism)结合使用，进一步提升了大规模模型的训练效率。

## Experiment Breakdown

实验在三个公开偏好数据集上进行：Orca-DPO-pair(约12.7K对)、Distilabel Capybara(7.56K对，多轮对话)和RLHF-V(5.7K视觉问答对)。使用Llama-3.2-1B-Instruct和llava-qwen-0.5b-hf模型进行基准测试，采用单GPU和LoRA设置。在大规模实验中，使用Qwen2.5-72B-Instruct模型在Tulu3偏好数据集上进行分布式训练，包含超过300K样本。实验测量了训练时间和峰值内存使用情况，并与基线方法进行对比。结果显示在所有数据集上都获得了显著的性能提升，特别是RLHF-V数据集由于图像输入token数量庞大，效果最为明显。

## Result Interpretation

实验结果表明，Preference Packing在所有测试数据集上都实现了显著的资源效率提升。在Orca数据集上，峰值内存减少32.9%，训练时间减少19.9%；在Capybara数据集上，峰值内存减少19.8%，训练时间减少22.0%；在RLHF-V数据集上，峰值内存减少36.5%，训练时间减少20.2%。当考虑有效时间(结合内存节省允许的更大批处理大小)时，总体训练时间可减少至原来的50.7%。在大规模分布式训练中，单独使用Preference Packing获得1.53倍加速，与批处理排序结合后达到3.22倍加速，相比单独批处理排序额外提升23%。这些结果证实了理论分析的有效性，即当响应长度相对较短时，该方法效果最佳。

## Limitations

该方法存在明显的局限性，特别是在某些场景下可能增加训练成本而非减少。当响应长度超过输入提示长度时，Preference Packing可能导致更高的计算开销，这在推理模型训练中尤为明显，因为推理响应往往非常长。此外，该方法的效率依赖于输入提示与响应之间的长度比例以及响应数量，当响应长度差异较小时优势不明显。对于包含极长推理链或复杂逻辑推理的偏好数据，该方法可能无法提供预期的效率提升。实验主要基于成对偏好数据(K=2)，对于包含更多响应选项的复杂偏好结构，该方法的扩展性有待验证。

## Reproducibility

该研究具有良好的可复现性基础，作者提供了详细的理论分析和实验设置。代码实现基于标准的PyTorch框架，注意力掩码和位置编码的修改相对简单。实验使用的数据集均为公开可用，包括Orca-DPO-pair、Capybara和RLHF-V。模型架构使用开源的Llama-3.2和Qwen系列，便于复现。具体的超参数设置、批处理大小、学习率等详细信息需要从源代码中获取。大规模分布式实验的复现需要相应的硬件资源和FSDP配置经验。重现理论分析中的效率边界条件验证需要精确的序列长度统计。

## Ideas for My Work

首先，我可以在当前的DPO训练流程中集成Preference Packing技术，特别是在处理多轮对话或长输入提示的场景中。具体实施步骤包括修改数据加载器以支持序列打包，实现自定义的注意力掩码机制，以及调整位置编码策略。这有望在不改变模型架构的情况下显著提升训练效率，特别是在资源受限的环境中。其次，我可以探索将Preference Packing与其他已知的优化技术相结合，如梯度累积、混合精度训练和知识蒸馏。这种组合可能会产生协同效应，进一步提升整体训练效率。通过系统性的消融实验来量化每种技术的贡献。最后，我可以研究Preference Packing在特定领域应用中的适应性，比如法律、医学或科学文献处理等专业领域。这些领域的输入通常较长且结构复杂，Preference Packing可能带来更大的效率提升。同时评估该方法在不同领域数据分布下的鲁棒性。

## Writing Usage

论文采用了清晰的技术报告结构，先提出问题背景，然后介绍相关工作，接着详细阐述方法论，最后通过实验验证。'We propose...'句式用于引入核心创新点。'In particular...'用于强调特定应用场景。'Notably...'用于突出重要发现。'To address this issue...'用于引出解决方案。'Furthermore...'用于扩展讨论。'Experimental validation shows...'用于支撑实证结果。'The results indicate...'用于总结发现。'In conclusion...'用于总结全文要点。

## Theory Notes

理论分析基于注意力机制的二次复杂度特性，即注意力计算成本与序列长度的平方成正比。对于原始方法，C_original ∝ K × l_original²，而对于Preference Packing，C_pp ∝ l_pp²。效率提升的数学条件为l_in + ∑l_respᵏ < √K × (l_in + l_resp_max)。当使用Flash Attention时，内存复杂度变为线性关系，使得内存效率提升更加显著。该理论框架考虑了输入提示长度、响应长度分布和响应数量的影响，为实际应用提供了定量指导。

## Metadata

来源：arXiv预印本，论文ID arxiv:2602.24082v1，作者Jaekyung Cho来自AWS GenAI创新中心。发布时间：2026年2月27日(推断)。可靠性：高，基于理论分析和充分实验验证。可复现性：高，使用开源模型和数据集，方法描述详细。研究类型：算法优化，专注于训练效率提升。应用领域：大语言模型对齐训练，奖励建模，直接偏好优化。

## Evidence Anchors

1. claim: Preference Packing实现了至少37%的训练时间减少
   - evidence: 实验结果显示在多个数据集上实现了显著的时间减少，Table 1显示Orca数据集训练时间减少19.9%，Capybara减少22.0%，RLHF-V减少20.2%，但综合考虑有效时间可减少至原来的50.7%
   - location: Abstract and Table 1
2. claim: 该方法与批处理排序结合可实现3.22倍速度提升
   - evidence: Table 2显示vanilla DPO为1 samples/sec，单独使用Preference Packing为1.53，单独使用批处理排序为2.62，两者结合达到3.22
   - location: Section 5.2 and Table 2
3. claim: 传统偏好优化存在重复计算问题
   - evidence: 论文指出在批处理中，输入序列从x_M*i到x_M*(i+1)-1被重复计算K次，注意力内存也冗余占用K次
   - location: Section 3.1
4. claim: 理论效率条件为l_in + ∑l_resp^k < √K × (l_in + l_resp_max)
   - evidence: 公式(1)明确给出了Preference Packing比原始方法更高效的数学条件
   - location: Section 3.2.1, Equation 1
5. claim: Flash Attention下内存效率必然提升
   - evidence: 文中证明使用Flash Attention时，注意力内存与序列长度呈线性关系，内存比率恒小于等于1
   - location: Section 3.2.1
6. claim: RLHF-V数据集验证了方法在多模态场景的有效性
   - evidence: 文中提到图像输入转换为数千token，响应仅为文本且相对较短，Preference Packing特别有效
   - location: Section 5.1
7. claim: 该方法适用于奖励模型和DPO训练
   - evidence: 摘要和引言明确提到该方法针对奖励模型和直接偏好优化设计
   - location: Abstract and Section 1
8. claim: 实验使用了三个公开数据集验证
   - evidence: Section 4.1详细介绍了Orca-DPO-pair、Capybara和RLHF-V三个数据集
   - location: Section 4.1

## Action Items

- 立即在我的DPO训练管道中实现Preference Packing，重点关注注意力掩码的设计和位置编码的调整，以减少重复计算和内存占用
- 设计消融实验来验证不同响应长度比例下该方法的效果，特别是在我的特定领域数据上的适用性
- 探索将Preference Packing与现有的批处理优化技术结合，量化组合使用时的性能增益

## Generator Notes

- uncertainty: 发布日期2026年2月27日为推断值，实际应为2024年或2025年
- fulltext_status: fulltext
