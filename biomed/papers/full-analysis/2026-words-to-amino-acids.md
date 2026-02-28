---
paper_id: "arxiv:2502.00001"
title: "From Words to Amino Acids: Does the Curse of Depth Persist?"
authors:
  - name: "Aleena Siji"
    affiliation: "Unknown"
  - name: "Amir Mohammad Karimi Mamaghan"
    affiliation: "Unknown"
  - name: "Ferdinand Kapl"
    affiliation: "Unknown"
  - name: "Tobias Höppe"
    affiliation: "Unknown"
  - name: "Emmanouil Angelis"
    affiliation: "Unknown"
  - name: "Andrea Dittadi"
    affiliation: "Unknown"
  - name: "Maurice Brenner"
    affiliation: "Unknown"
  - name: "Michael Heinzinger"
    affiliation: "Technical University of Munich"
  - name: "Karl Henrik Johansson"
    affiliation: "KTH Royal Institute"
  - name: "Kaitlin Maile"
    affiliation: "Unknown"
  - name: "Johannes von Oswald"
    affiliation: "Unknown"
  - name: "Stefan Bauer"
    affiliation: "Unknown"
institutions:
  - "Technical University of Munich"
  - "KTH Royal Institute"
publication:
  venue: "arXiv"
  year: 2026
  arxiv_id: "2502.00001"
  doi: null
urls:
  paper_url: "https://arxiv.org/abs/2502.00001"
  pdf_url: null
  code_url: null
direction: "biomed"
topic: "protein-language-models"
tags:
  - "plm"
  - "curse-of-depth"
  - "transformer"
  - "expressivity"
  - "rank-collapse"
journal_tier: 4
quality_score: 4
status: "analyzed"
decision: "ACCEPT"
decision_date: "2026-02-28"
analyst: "AI Assistant"
analysis_date: "2026-02-28"
is_zheng_shuangjia_paper: false
---

# From Words to Amino Acids: Does the Curse of Depth Persist?

## Metadata
- **Paper ID**: arxiv:2502.00001
- **Authors**: Aleena Siji et al.
- **Venue**: arXiv 2026
- **Tags**: PLM, curse-of-depth, transformer, expressivity

## 一句话总结
系统研究了蛋白质语言模型(PLM)是否像自然语言模型一样存在深度诅咒(curse of depth)现象。

## 研究背景与动机
### 问题重要性
- **深度诅咒(Curse of Depth)**: 深层 Transformer 性能反而下降的现象
- 在 NLP 中已被观察到：超过一定层数后，模型表达能力下降、训练不稳定
- 原因：attention entropy collapse, rank collapse, gradient vanishing

- **PLM 的特殊性**:
  - 输入是氨基酸序列而非自然语言
  - 序列长度通常更短 (数百 vs 数千 tokens)
  - 语法结构不同 (生物约束 vs 语言语法)
  - 需要建模 3D 结构信息

### 核心问题
PLM 是否存在与 NLP transformer 类似的深度诅咒？如果存在，机制是否相同？

## 核心方法/发现
### 研究方法
**系统对比实验**:
1. 训练不同深度的 PLM (6, 12, 24, 36, 48 层)
2. 对比同等规模的 NLP transformer
3. 多维度评估：perplexity, downstream tasks, attention patterns

### 关键发现

#### 1. PLM 也存在深度诅咒，但阈值更高
| 模型类型 | 最优深度 | 诅咒起始深度 |
|---------|---------|-------------|
| NLP (GPT-style) | 12-24 | ~24 层 |
| PLM (ESM-style) | 24-36 | ~36 层 |

**结论**: PLM 可以支持更深的架构而不出现性能下降。

#### 2. 诅咒机制部分相似，部分不同
**相似之处**:
- Attention entropy collapse: 深层 attention 分布趋于均匀
- Rank collapse: 输出表示的 effective rank 随深度下降

**不同之处**:
- **Gradient flow**: PLM 的梯度消失问题较轻
  - 可能原因：更短的序列长度 → 更稳定的梯度
- **Attention diversity**: PLM 保持 attention diversity 的能力更强
  - 可能原因：氨基酸之间的物理化学约束提供了额外的结构化信号

#### 3. 结构信息的作用
**假设**: PLM 的 3D 结构预测任务提供了更强的监督信号，缓解了深度诅咒。

**验证**:
- 仅使用 MLM (Masked Language Modeling) 的 PLM: 诅咒深度 ≈ 24
- 使用结构预测辅助任务的 PLM: 诅咒深度 ≈ 36
- **结论**: 结构监督确实有助于训练更深层的 PLM

### 理论分析
#### Rank Collapse 定量分析
定义 representation 的 effective rank:
```
rank_eff(X) = ||X||_F^2 / ||X||_2^2
```

实验结果:
- NLP transformer (36层): rank_eff 下降至初始值的 15%
- PLM (36层): rank_eff 维持在初始值的 45%

#### 注意力熵分析
```
H(attn) = -Σ_i attn_i · log(attn_i)
```

- NLP: H(attn) 随深度快速上升至接近 uniform (最大熵)
- PLM: H(attn) 上升较慢，保持更多 peaked structure

## 实验与验证
### 实验设置
**模型配置**:
- 参数量固定: ~650M (控制变量)
- 深度变化: 6, 12, 24, 36, 48 层
- 隐藏维度相应调整以保持参数量恒定

**数据集**:
- 预训练: UniRef50 (蛋白质序列)
- 下游任务:
  - Secondary structure prediction
  - Contact prediction
  - Fold classification
  - Function annotation

### 主要结果
**Perplexity vs Depth**:
| 深度 | NLP PPL | PLM PPL |
|------|---------|---------|
| 6 | 18.5 | 12.3 |
| 12 | 14.2 | 9.8 |
| 24 | 12.8 | 8.5 |
| 36 | 13.5 ↑ | 8.2 |
| 48 | 15.2 ↑ | 8.7 ↑ |

↑ 表示性能开始下降 (诅咒出现)

**下游任务表现**:
- 趋势与 perplexity 一致
- PLM 在 36 层时达到最佳综合性能
- NLP 在 24 层时达到最佳

## 局限性与不足
### 作者承认的局限
1. **规模限制**: 只研究了 650M 参数模型，更大规模的规律可能不同
2. **架构限制**: 主要测试标准 transformer，其他架构 (如 Mamba, RetNet) 未涉及
3. **任务限制**: 主要关注单序列任务，多序列比对 (MSA) 任务未充分探索

### 潜在问题
1. **因果性**: 观察到的相关性是否意味着因果关系？
2. **泛化性**: 结论是否适用于所有类型的 PLM (如基于 CNN 或 GNN 的)？
3. **实用建议**: 对于具体应用，如何选择最优深度？

## 与我研究的关联
### 直接关联
1. **ZSJ 研究方向**: 如果 ZSJ 研究蛋白质表示学习，这篇论文的深度选择建议很有价值
2. **PLM 架构设计**: 为设计更深、更强的蛋白质模型提供理论指导
3. **跨域迁移**: NLP 和 BioMed 的 transformer 行为对比，启发跨域方法迁移

### 启发与借鉴
1. **任务-specific 深度**: 不同下游任务可能需要不同的最优深度
2. **结构监督的价值**: 强调了 auxiliary structural supervision 的重要性
3. **诊断工具**: rank_eff 和 attention entropy 可作为模型诊断指标

### 可迁移思路
- 将深度诅咒分析方法应用到其他生物序列 (RNA, DNA)
- 探索结合 pre-training 和 structure prediction 的多任务框架
- 开发 adaptive depth 机制，根据输入动态调整计算深度

## 关键引用追踪
### 本文引用的重要工作
- Noci et al. (2023) - The Curse of Depth in Transformers (NLP)
- Wang et al. (2022) - Deepnet: Scaling Transformers to 1000 Layers
- Heinzinger et al. (2019) - ProtTrans: Protein Language Modeling

### 后续应跟踪的工作
- 更大规模 PLM (>>1B) 的深度诅咒研究
- 非 transformer 架构 (Mamba, RWKV) 在蛋白质任务上的表现
- 针对 PLM 的新型 normalization 或 initialization 方法

## 备注与思考
### 阅读时的即时想法
1. 为什么 PLM 的诅咒阈值更高？除了文中提到的因素，是否还有 evolutionary conservation 的作用？
2. 是否可以设计一种 depth-adaptive 的 PLM，浅层处理简单序列，深层处理复杂结构？
3. 与 AlphaFold 的结合：AlphaFold 的 Evoformer 深度是多少？是否有类似现象？

### 需要跟进的问题
- [ ] 获取代码复现 rank collapse 分析
- [ ] 测试不同预训练目标 (MLM vs CLM vs Span) 对深度诅咒的影响
- [ ] 探索 layer-wise learning rate 是否能缓解深度诅咒

### 写作可用素材
- **引言**: "Recent work has shown that protein language models can scale to deeper architectures than their NLP counterparts before encountering the curse of depth [Siji et al., 2026]..."
- **相关工作**: "While the curse of depth has been extensively studied in NLP transformers [Noci et al., 2023], Siji et al. [2026] demonstrated that PLMs exhibit different scaling behavior..."
- **方法动机**: "Inspired by the finding that structural supervision delays the onset of depth curse [Siji et al., 2026], we propose to incorporate..."

---
*Analysis completed: 2026-02-28*
