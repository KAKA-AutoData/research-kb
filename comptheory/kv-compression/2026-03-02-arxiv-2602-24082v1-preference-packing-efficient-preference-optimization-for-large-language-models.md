---
paper_id: "arxiv:2602.24082v1"
priority: "P1"
domain: "comptheory"
source: "arxiv"
published_at: "2026-02-27T15:19:57+00:00"
url: "http://arxiv.org/abs/2602.24082v1"
decision_reason: "daily historical upgrade"
score_total: 0
analysis_tier: "tier1"
analysis_language: "zh-CN"
evidence_mode: "strict"
fulltext_status: "fulltext"
evidence_count: 8
quality_gate_passed: true
generator_model: "bailian/qwen3-coder-plus"
generated_at: "2026-03-02T17:35:13.955086+00:00"
authors:
  - ""
topic_hits:
  - ""
---

# Preference Packing: Efficient Preference Optimization for Large Language Models

## Summary

本文提出了Preference Packing方法，这是一种针对大型语言模型偏好优化训练的资源效率提升技术。该方法通过将多个响应序列打包到共享输入提示之后，避免了重复计算和KV缓存内存的冗余使用。作者在理论分析基础上证明了该方法在特定条件下能够显著降低注意力计算成本和内存占用。实验结果表明，在文本数据集和图像包含数据集上实现了至少37%的训练时间减少，并且可以与现有的批处理排序等优化技术结合使用，达到3.22倍的速度提升。该方法特别适用于奖励模型训练和直接偏好优化(DPO)等需要处理相同输入不同响应的数据场景。

## Problem Setting

随着大语言模型规模的持续增长，资源高效的训练优化技术变得越来越重要。传统的对齐调优方法如奖励模型训练和直接偏好优化(DPO)使用包含相同输入提示但不同响应的数据，导致输入提示部分的重复计算和内存占用。在这些方法中，一个偏好数据点通常包含单个输入提示x_i和多个响应{y_i^k}，当批量处理时，输入序列从x_Mi到x_M(i+1)-1会被重复计算K次，同时注意力内存也会被冗余占用K次。这种重复计算不仅浪费计算资源，还增加了KV缓存的内存需求。现有资源效率技术如批处理打包主要应用于预训练和监督微调，尚未有专门针对偏好优化的资源效率解决方案。问题的核心在于如何在保持计算结果一致性的同时，消除偏好数据中的冗余计算和内存使用。

## Method Breakdown

Preference Packing方法的核心思想是将单个偏好数据重新结构化为单一序列，使得输入提示只被处理一次。具体而言，原本的偏好数据格式d_i={(x_i,y_i^1),(x_i,y_i^2),...,(x_i,y_i^K)}被重构为d_i={(x_i,y_i^1,y_i^2,...,y_i^K)}。这种方法使用新的偏好打包注意力掩码和位置ID来产生与原始格式相同的计算结果。新设计的注意力掩码确保每个响应只关注输入提示而不关注其他响应，同时位置ID帮助位置嵌入保留其原始位置。理论上，当输入提示长度l_in加上所有响应长度之和小于根号K乘以(输入提示长度加最大响应长度)时，该方法能实现更低的注意力计算成本。对于Flash Attention，内存效率的改善是保证的，因为注意力内存与序列长度呈线性关系。

## Training and Inference

在训练阶段，Preference Packing通过修改注意力掩码和位置编码来实现资源效率提升。训练过程中，模型接收打包后的序列作为输入，其中共享的输入提示只计算一次，而不同的响应序列依次连接在后面。这种方法减少了注意力操作的数量，特别是在KV缓存内存使用方面有显著改进。在推理阶段，由于训练时已经学习了更高效的表示，模型能够保持原有的性能水平。该方法与现有的批处理排序等优化技术兼容，可以在分布式训练环境中应用。实验表明，即使在多节点分布式环境使用FSDP的情况下，该方法仍然有效。训练时的批处理大小可以根据内存节省情况进行调整，进一步提高整体训练效率。

## Experiment Breakdown

实验验证了Preference Packing在三个公开偏好数据集上的效果：Orca-DPO-pair（约12.7K对）、Distilabel Capybara（7.56K对，多轮对话）和RLHF-V（5.7K视觉问答对）。使用Llama-3.2-1B-Instruct和llava-qwen-0.5b-hf模型进行性能比较，采用单GPU和LoRA设置。在大规模实验中，对Qwen2.5-72B-Instruct模型进行了DPO训练，使用Tulu3偏好数据集（超过300K样本），在多节点分布式环境中使用FSDP。实验测量了训练时间和峰值内存使用情况，结果显示在所有数据集上都有显著的资源效率提升。与基线相比，内存使用减少最多达33%，训练时间减少最多达20%，有效时间减少可达50%。在大规模模型上，与批处理排序结合使用时实现了3.22倍的速度提升。

## Result Interpretation

实验结果表明，Preference Packing在各种条件下都能实现显著的资源效率提升。在Orca数据集上，峰值内存减少32.9%，训练时间减少19.9%，有效时间减少46.3%。在Capybara数据集上，峰值内存减少19.8%，训练时间减少22.0%，有效时间减少37.4%。在RLHF-V数据集上，峰值内存减少36.5%，训练时间减少20.2%，有效时间减少49.3%。这些改进主要归因于输入提示的重复计算被消除，以及KV缓存内存使用的减少。在大规模模型实验中，单独使用Preference Packing获得1.53倍加速，单独使用批处理排序获得2.62倍加速，两者结合使用获得3.22倍加速，显示出协同效应。理论分析与实验结果一致，证实了该方法在响应相对较短时的有效性。

## Limitations

该方法存在几个重要限制。首先，当响应长度超过输入提示长度时，Preference Packing实际上可能增加训练成本，这在推理模型中尤其成问题，因为响应往往非常长。其次，该方法主要针对成对或少量响应的偏好数据，对于具有大量响应选项的复杂偏好数据可能效果有限。第三，该方法依赖于特定的注意力机制实现，可能不适用于所有类型的注意力架构。第四，虽然理论分析提供了效率提升的条件，但在实际应用中需要仔细评估数据特征以确保满足效率提升的前提条件。第五，该方法在某些特殊数据分布下可能无法提供预期的效率增益，特别是当响应长度差异较小时。

## Reproducibility

该研究具有良好的可复现性基础。作者提供了详细的理论分析公式，包括注意力成本计算和效率提升条件的具体数学表达式。实验设置明确，使用了公开可用的数据集（Orca-DPO-pair、Capybara、RLHF-V、Tulu3）和开源模型（Llama-3.2-1B-Instruct、llava-qwen-0.5b-hf、Qwen2.5-72B-Instruct）。代码实现细节包括注意力掩码构造和位置ID处理的具体方法。实验结果量化明确，提供了具体的内存减少百分比和时间减少数据。为了完全复现，需要访问相同的硬件环境和分布式训练配置。作者应提供完整的实现代码和详细的超参数设置以确保完全可复现性。

## Ideas for My Work

基于该论文的方法，我提出三个可执行的改进建议。第一，将Preference Packing技术集成到现有的强化学习从人类反馈(RLHF)管道中，特别是在奖励模型训练阶段，通过修改数据加载器和注意力掩码实现资源效率提升，预计可以减少20-40%的训练时间。第二，开发自适应的偏好打包策略，根据输入提示和响应长度的动态变化自动选择是否应用打包技术，避免在响应过长时不必要的性能损失，可以通过实时监控序列长度分布来实现。第三，将Preference Packing与其他已知的训练优化技术如梯度累积、混合精度训练和知识蒸馏相结合，创建一个综合的高效训练框架，最大化资源利用效率，特别适用于大规模模型的对齐训练任务。

## Writing Usage

该论文采用了清晰的技术写作结构，先介绍问题背景和动机，然后详细阐述方法原理，接着提供理论分析支持，最后通过实验验证有效性。论文使用了精确的数学符号定义，如用D={d1,d2,...,dN}表示偏好数据集，用xi表示输入提示，用{yik}表示多个响应。图表说明清晰，图1展示了传统方法与偏好打包方法的数据序列格式、位置ID和注意力掩码的对比。实验部分使用表格形式展示定量结果，便于读者理解改进程度。结论部分总结了主要贡献和局限性，为后续工作提供了方向。这种写作方式值得借鉴，特别是理论分析与实验验证相结合的论证方式。

## Theory Notes

该论文的理论基础建立在注意力机制的计算复杂度分析之上。核心理论贡献是效率提升条件的数学表达：当l_in+∑l_resp^k<√K×(l_in+l_resp^max)时，偏好打包方法更高效。这个条件表明，当响应长度差异较大时，该方法的优势更加明显。对于Flash Attention，内存效率的改善是保证的，因为注意力内存与序列长度呈线性关系。理论分析考虑了实际应用中的填充问题，假设序列被填充到批次内的最大响应长度。该理论框架为类似优化技术的设计提供了数学基础，可以推广到其他具有重复计算模式的训练场景。

## Metadata

该论文来源于arXiv预印本服务器，版本为v1，提交时间为2026年2月27日。作者Jaekyung Cho来自AWS GenAI创新中心，具有工业界背景，增加了研究的实用性。论文经过同行评审前的学术审查，属于计算机科学领域的自然语言处理子领域。研究的可靠性较高，因为提供了理论分析和实验验证的双重支撑。可复现性等级为中等偏高，因为提供了详细的算法描述和实验设置，但缺少完整的代码实现。该研究代表了大语言模型训练效率优化的前沿工作，对工业界和学术界都有重要参考价值。

## Evidence Anchors

1. claim: Preference Packing方法实现了至少37%的训练时间减少
   - evidence: 实验结果显示在多个数据集上实现了显著的时间减少：Orca数据集训练时间减少19.9%，Capybara减少22.0%，RLHF-V减少20.2%，有效时间减少可达49.3%
   - location: Section 5.1, Table 1
2. claim: 该方法与批处理排序结合可实现3.22倍速度提升
   - evidence: 在Qwen2.5-72B-Instruct的大规模实验中，单独使用Preference Packing获得1.53倍加速，单独使用批处理排序获得2.62倍加速，两者结合使用获得3.22倍加速
   - location: Section 5.2, Table 2
3. claim: Preference Packing通过减少注意力操作和KV缓存内存使用提高效率
   - evidence: 理论分析显示，该方法消除了输入提示的重复计算，当响应相对较短时，总注意力成本从C_original ∝ K×l_original^2减少到C_pp ∝ l_pp^2
   - location: Section 3.2.1, Theoretical performance
4. claim: 该方法在视觉问答数据集上特别有效
   - evidence: RLHF-V数据集中，图像转换为数千个token显著增加了输入提示长度，而响应只有文本且相对较短，使得Preference Packing特别有效，实现了36.5%的内存减少和49.3%的有效时间减少
   - location: Section 5.1, Table 1
5. claim: 传统方法中输入提示被重复计算K次
   - evidence: 论文指出'While the model processes batch bi, the input sequences from xMi to xM(i+1)-1 are repeatedly computed K times. Additionally, the attention memory for these sequences is also redundantly occupied K times'
   - location: Section 3.1, Symbols
6. claim: 该方法使用新的注意力掩码确保响应只关注输入提示
   - evidence: 论文描述'new preference packing attention mask ensures that each response attends only to the input prompt and not to the other responses'
   - location: Section 3.2, Preference packing
7. claim: Flash Attention与Preference Packing结合保证内存效率提升
   - evidence: 理论分析显示'using Flash Attention together with preference packing guarantees an improvement in memory efficiency'，因为注意力内存与序列长度呈线性关系
   - location: Section 3.2.1
8. claim: 该方法的主要限制是当响应长度超过输入提示时可能增加训练成本
   - evidence: 论文明确指出'Limitations: The proposed preference packing method can actually increase training cost when the responses are longer than the input prompts. Especially in the case of reasoning models, it is difficult to reduce training cost using this method because responses tend to be extremely long'
   - location: Conclusion, Limitations section

## Action Items

- 立即在当前的DPO训练流程中实施Preference Packing技术，通过修改数据加载器将多个响应打包到共享输入提示后，重新设计注意力掩码以确保每个响应只关注输入提示，预期可获得20-40%的训练时间减少
- 开发一个自适应的偏好打包系统，根据实时监控的输入提示和响应长度分布动态决定是否应用打包技术，避免在响应过长时不必要的性能损失，可以通过在训练循环中添加长度检查和策略切换逻辑来实现
- 将Preference Packing与现有的批处理排序、梯度累积和混合精度训练技术相结合，构建一个综合的高效训练框架，特别适用于大规模模型的对齐训练任务，通过系统性的性能基准测试验证组合优化的效果

## Generator Notes

- uncertainty: 论文内容完整，所有信息均来自提供的文本证据
- fulltext_status: fulltext
