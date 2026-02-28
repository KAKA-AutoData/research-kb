# 研究知识库工作规范说明

> 版本: v1.0  
> 创建日期: 2026-02-28  
> 适用范围: 生物信息/医学 + 计算机理论双方向长期研究  
> 核心原则: **宁可少收，不要泛收**

---

## 一、研究边界确认

### 1.1 生物信息与医学方向 (BioMed)

**核心主题:**
| 类别 | 关键词 | 优先级 |
|------|--------|--------|
| 药物相关 | drug discovery, drug repositioning, drug-target interaction, drug response/sensitivity, 作用机制, 小分子/RNA药物 | P0 |
| 医学相关 | disease mechanism, translational medicine, precision medicine, multi-omics, biomarker(需机制/方法深度) | P1 |
| RNA相关 | miRNA, lncRNA, circRNA, RNA regulation, RNA biology, RNA therapeutics | P0 |

**研究者/团队优先级 (降序):**
1. **ZSJ** (Shanghai Jiao Tong University) - 最高优先级，其所有论文必收并深度分析
2. 其他高水平团队 (Nature/Science/Cell主刊及子刊)

**期刊层级:**
- Tier 1: Nature, Science, Cell 主刊
- Tier 2: Nature Medicine, Nature Biotechnology, Cell Research, Cancer Cell 等子刊
- Tier 3: Nucleic Acids Research, Genome Biology, Briefings in Bioinformatics 等领域顶刊

### 1.2 计算机与理论方向 (CompTheory)

**核心主题:**
| 类别 | 关注重点 | 排除项 |
|------|---------|--------|
| Attention | 结构本身、latent attention、compressed attention、KV cache/compression、关系建模、表示能力分析 | 纯应用部署优化 |
| Low-rank | low-rank approximation/parameterization/adaptation、对表达能力/优化/泛化的影响 | 仅性能提升无理论分析 |
| LoRA | 学习机制、与full FT的差异、rank与表达能力、遗忘/泛化/稳定性、理论分析 | 简单套用LoRA的应用论文 |
| 关系评估 | relation modeling, relational reasoning, internal representation evaluation, module/layer/token interaction | 无机制理解的工程实现 |

### 1.3 DeepSeek专项 (DeepSeek Priority)

**最高优先级内容:**
- DeepSeek官方技术报告与论文
- Multi-Head Latent Attention (MLA) 结构与机制
- Low-rank key-value compression
- KV cache compression 理论与实现
- Latent representation in attention
- DeepSeekMoE 及其与attention的关系

**次级优先级:**
- 外部对DeepSeek MLA的理论分析
- DeepSeek架构向其他Transformer的推广
- DeepSeek中的关系建模、低秩机制、表达结构分析

---

## 二、知识库主题结构划分

```
research-kb/
├── _meta/                          # 元数据与配置
│   ├── journal-tiers.yaml          # 期刊分级表
│   ├── institution-priority.yaml   # 机构优先级
│   └── keyword-taxonomy.yaml       # 关键词分类体系
│
├── biomed/                         # 生物信息/医学方向
│   ├── drug-discovery/             # 药物发现
│   ├── drug-repositioning/         # 药物重定位
│   ├── drug-target-interaction/    # 药物-靶点相互作用
│   ├── rna-therapeutics/           # RNA治疗
│   ├── rna-regulation/             # RNA调控 (miRNA/lncRNA/circRNA)
│   ├── disease-mechanism/          # 疾病机制
│   └── multi-omics/                # 多组学整合
│
├── comptheory/                     # 计算机理论方向
│   ├── attention-mechanisms/       # Attention机制分析
│   ├── kv-compression/             # KV缓存压缩
│   ├── low-rank-theory/            # 低秩理论
│   ├── lora-analysis/              # LoRA机制分析
│   └── relation-modeling/          # 大模型关系评估
│
├── deepseek/                       # DeepSeek专项
│   ├── official-reports/           # 官方技术报告
│   ├── mla-analysis/               # MLA结构分析
│   ├── kv-compression/             # KV压缩机制
│   ├── moe-structure/              # MoE结构
│   └── external-studies/           # 外部研究
│
├── cross-domain/                   # 跨领域研究
│   └── ai-for-drug-discovery/      # AI药物发现 (AI+BioMed交叉)
│
├── synthesis/                      # 综合分析产出
│   ├── trend-reports/              # 趋势报告
│   ├── gap-analysis/               # 研究空白分析
│   ├── method-comparisons/         # 方法对比
│   └── writing-notes/              # 写作素材与选题笔记
│
└── inbox/                          # 待处理队列
    ├── pending-review/             # 待筛选
    └── rejected/                   # 已拒绝 (保留记录)
```

---

## 三、检索、筛选与优先级规则

### 3.1 检索策略

**定期检索源:**
| 来源 | 频率 | 检索式要点 |
|------|------|-----------|
| PubMed | 每周 | (drug discovery[Title/Abstract] OR drug repositioning[...]) AND (machine learning[...] OR deep learning[...]) |
| bioRxiv/medRxiv | 每周 | 同上 + 预印本标签 |
| arXiv cs.LG/cs.CL | 每日 | attention mechanism, low-rank, LoRA, KV cache, transformer theory |
| **Google Scholar (ZSJ)** | **每日** | **author:"Shuangjia Zheng" OR author:"ZSJ"** |
| Google Scholar Alerts | 实时 | 针对特定作者/机构设置 |
| Twitter/X Academic | 每日 | 关注关键研究者动态 |

**ZSJ专项监控:**
- 每日检查 Google Scholar 新发表
- 监控其 GitHub 仓库更新
- 跟踪其课题组网站/博客
- 设置关键词 Alert: "Shuangjia Zheng" + [生信/药物/RNA相关词]

**DeepSeek专项监控:**
- 每日检查 arXiv "DeepSeek" 关键词
- 监控 DeepSeek 官方 GitHub/博客
- 设置 Google Alert: "Multi-Head Latent Attention" OR "DeepSeek MLA"

### 3.2 初筛流程 (Inbox → Review)

每篇进入 `inbox/pending-review/` 的论文必须填写:

```yaml
# paper-checklist.yaml
paper_id: "arxiv:2501.12345"  # 或 DOI
source: "arXiv"               # 来源
retrieved_at: "2026-02-28"

# 快速判断
relevance_check:
  matches_bio_direction: false  # 是否符合生信方向
  matches_comp_direction: true  # 是否符合计科方向
  matches_deepseek_priority: true  # 是否DeepSeek相关
  
# 质量门槛
quality_gate:
  journal_tier: "arXiv"       # 期刊/会议级别
  institution_match: []        # 匹配的机构
  has_code: true              # 是否开源代码
  has_theory_contribution: true  # 是否有理论贡献

# 初步决策
decision: "ACCEPT"  # ACCEPT / REJECT / UNDECIDED
reject_reason: ""   # 如果REJECT，说明原因
assigned_to: "comptheory/deepseek/mla-analysis/"  # 目标目录
```

### 3.3 深度筛选标准

**必须通过以下至少一项:**

| 维度 | 通过标准 | 权重 |
|------|---------|------|
| 理论深度 | 提出新理论框架、证明重要性质、揭示机制本质 | 高 |
| 方法创新 | 提出可复用的新方法，有清晰的方法论贡献 | 高 |
| 实验严谨 | 大规模验证、消融实验充分、对照合理 | 中 |
| 影响力 | 被后续重要工作引用、引发新的研究方向 | 中 |
| 机构/期刊 | 顶级机构+顶刊组合，或有突破性发现 | 低(辅助) |

**自动排除项:**
- [ ] 仅在某数据集上刷分，无方法论贡献
- [ ] 简单套用现有方法，无适配创新
- [ ] 综述类文章 (除非是非常权威的系统性综述)
- [ ] 重复性工作、增量改进 (<5%性能提升)
- [ ] 缺乏可复现性 (无代码+描述不清)

---

## 四、单篇论文深度分析规范

### 4.1 文件命名规范

```
{year}-{first_author}-{short_title}.{ext}

示例:
2025-deepseek-v3-technical-report.md
2025-liu-mha-theoretical-analysis.md
2024-wang-lora-rank-generalization.md
```

### 4.2 分析文档结构

每篇论文的分析文档必须包含以下章节:

```markdown
# {完整标题}

## Metadata
- **Paper ID**: arxiv:2501.12345 / DOI:10.xxxx
- **Authors**: 主要作者及机构
- **Journal/Conference**: 发表 venue
- **Year**: 2025
- **Code**: https://github.com/...
- **Tags**: [attention, low-rank, drug-discovery, ...]
- **Added to KB**: 2026-02-28
- **Analyst**: AI Assistant

## 一句话总结
用一句话概括核心贡献 (不超过50字)

## 研究背景与动机
- 该问题为什么重要？
- 现有方法的局限性是什么？
- 本文要解决的核心问题是什么？

## 核心方法/发现
### 方法概述
- 技术路线简述
- 关键创新点 (最多3个)

### 关键细节
- 重要的公式、算法、架构图描述
- 超参数设置、训练细节 (如相关)

## 实验与验证
### 实验设计
- 数据集、评价指标、baseline

### 主要结果
- 核心结果表格/图表的文字描述
- 统计显著性 (如有)

### 消融实验
- 各组件的贡献分析

## 理论贡献 (如适用)
- 证明了什么性质？
- 揭示了哪些机制？
- 有哪些定理/引理？

## 局限性与不足
- 作者自己承认的局限
- 我观察到的潜在问题
- 可改进的方向

## 与我研究的关联
### 直接关联
- 与我的哪个研究方向直接相关？
- 可以如何应用到我的工作中？

### 启发与借鉴
- 哪些思路/方法可以迁移？
- 哪些教训需要避免？

## 关键引用追踪
- 本文引用的重要前置工作
- 后续可能跟踪的引用本文的工作

## 备注与思考
- 阅读时的即时想法
- 需要进一步查证的问题
```

### 4.3 质量自检清单

完成分析后，必须检查:

- [ ] 一句话总结是否准确抓住核心?
- [ ] 方法描述是否足够详细，能让未来的我理解?
- [ ] 实验结果是否客观呈现，包括负面结果?
- [ ] 与我研究的关联部分是否具体，而非泛泛而谈?
- [ ] 是否标注了不确定的信息和需要跟进的问题?
- [ ] 文档长度是否适中 (建议2000-5000字)?

---

## 五、多篇论文横向比较规范

### 5.1 触发条件

以下情况应启动横向比较:
- 同一主题积累 ≥5 篇高质量论文
- 出现相互矛盾的结果或观点
- 需要为选题或方法设计做决策支持

### 5.2 比较文档结构

```markdown
# {主题} 横向比较分析

## 比较范围
- **时间跨度**: 2023-2025
- **论文数量**: 8篇
- **比较维度**: 方法创新、实验规模、理论贡献、可复现性

## 论文列表
| 序号 | 论文 | 年份 | 核心方法 | 主要结论 |
|-----|------|------|---------|---------|
| 1 | Smith et al. | 2024 | Method A | Conclusion X |
| ... | ... | ... | ... | ... |

## 维度对比

### 方法层面
- 各方法的技术路线对比图
- 优缺点矩阵

### 实验层面
- 数据集覆盖对比
- 性能对比 (统一格式表格)
- 计算成本对比

### 理论层面
- 假设条件对比
- 适用范围对比
- 证明强度对比

## 共识与分歧
### 学界共识
- 各方都认可的观点

### 存在分歧
- 争议焦点1: ... (支持 vs 反对的证据)
- 争议焦点2: ...

## 趋势判断
- 方法演进方向
- 新兴的研究范式
- 可能过时的方法

## 对我研究的启示
- 应该选择哪种方法路线?
- 有哪些可以结合的互补思路?
- 可能的创新切入点?
```

---

## 六、趋势报告与研究空白分析规范

### 6.1 趋势报告 (Trend Report)

**更新频率**: 每季度一次

**内容结构:**
```markdown
# {方向} 研究趋势报告 (2026 Q1)

## 热点主题演变
- 相比上季度的变化
- 新兴关键词
- 降温的主题

## 方法进展
- 重要的新方法/新架构
- 性能基准的提升
- 效率优化的突破

## 关键论文速览
- 本季度必读Top 5
- 每篇一句话点评

## 机构动态
- 重点关注机构的产出
- 工业界vs学术界的分工变化

## 预测与展望
- 接下来6个月可能出现的热点
- 值得提前布局的方向
```

### 6.2 研究空白分析 (Gap Analysis)

**触发条件:**
- 准备开题/选题前
- 投稿前论证创新性时
- 年度研究规划时

**内容结构:**
```markdown
# {方向} 研究空白分析

## 已有工作的覆盖地图
- 用表格或图示展示现有研究覆盖的范围
- 标注高密度区域和空白区域

## 识别出的研究空白
### 空白1: {具体描述}
- **证据**: 为什么这是空白?
- **重要性**: 填补它的价值
- **可行性**: 为什么现在可以做

### 空白2: ...

## 潜在的研究问题
- 基于空白提炼的具体研究问题
- 每个问题的预期贡献

## 风险与挑战
- 为什么这个空白还没被填?
- 可能的技术障碍
- 缓解策略
```

---

## 七、目录结构与命名规范

### 7.1 目录层级规则

```
research-kb/
├── {direction}/           # 一级: 大方向 (biomed/comptheory/deepseek)
│   ├── {subtopic}/        # 二级: 子主题 (snake_case)
│   │   ├── papers/        # 三级: 论文原文或链接
│   │   ├── analyses/      # 三级: 单篇分析
│   │   ├── comparisons/   # 三级: 横向比较
│   │   └── notes/         # 三级: 零散笔记
│   └── README.md          # 子主题索引
├── synthesis/
│   ├── trend-reports/
│   ├── gap-analysis/
│   ├── method-comparisons/
│   └── writing-notes/
└── _meta/
    ├── catalog-index.yaml     # 全局索引
    ├── stats-monthly.yaml     # 月度统计
    └── workflow-spec.md       # 本文件
```

### 7.2 文件命名规范

| 类型 | 格式 | 示例 |
|------|------|------|
| 论文分析 | `{year}-{first_author}-{short_title}.md` | `2025-deepseek-v3-technical-report.md` |
| 横向比较 | `{topic}-comparison-{date}.md` | `mla-mechanisms-comparison-20260228.md` |
| 趋势报告 | `trend-report-{direction}-{year}Q{q}.md` | `trend-report-comptheory-2026Q1.md` |
| 空白分析 | `gap-analysis-{direction}-{date}.md` | `gap-analysis-biomed-20260228.md` |
| 笔记 | `note-{topic}-{date}.md` | `note-kv-cache-survey-20260228.md` |

### 7.3 Metadata 字段规范

每篇分析文档头部必须包含:

```yaml
---
paper_id: "arxiv:2501.12345"  # 唯一标识
title: "完整标题"
authors: ["Author A", "Author B"]
institutions: ["MIT", "Stanford"]
journal: "Nature Medicine"
year: 2025
url: "https://arxiv.org/abs/2501.12345"
code_url: "https://github.com/..."
tags: ["drug-discovery", "deep-learning", "graph-neural-networks"]
direction: "biomed"  # biomed / comptheory / deepseek / cross-domain
status: "analyzed"   # pending / analyzing / analyzed / reviewed
quality_score: 4     # 1-5, 基于筛选标准
added_date: "2026-02-28"
last_reviewed: "2026-02-28"
related_papers:
  - "arxiv:2501.12340"
  - "doi:10.xxxx/yyyy"
---
```

---

## 八、质量控制与自检规则

### 8.1 入库前检查 (Pre-commit Checklist)

每篇论文进入正式知识库前:

- [ ] **ZSJ优先检查**: 如果是ZSJ的论文，自动进入核心库
- [ ] **相关性检查**: 确实符合研究边界，非表面相关
- [ ] **质量检查**: 通过深度筛选标准至少一项 (ZSJ论文可适度放宽)
- [ ] **重复检查**: 未与已有论文重复 (查标题+作者+年份)
- [ ] **完整性检查**: 分析文档包含所有必需章节
- [ ] **准确性检查**: 关键事实、数据、公式已核对
- [ ] **关联性检查**: 已标注与其他论文的关联

### 8.2 定期审查 (Quarterly Review)

每季度执行:

- [ ] 抽查10%的分析文档，检查质量是否达标
- [ ] 更新 `_meta/catalog-index.yaml`
- [ ] 清理 `inbox/rejected/` (保留但归档)
- [ ] 生成 `_meta/stats-monthly.yaml` 统计报告
- [ ] 回顾并更新本规范 (如有必要)

### 8.3 避免空泛的规则

**禁止出现的表述:**
- ❌ "这篇文章很有价值"
- ❌ "该方法效果良好"
- ❌ "对未来研究有启发"

**必须替换为:**
- ✅ "该方法在X数据集上将Y指标从A提升到B，提升幅度C%"
- ✅ "相比Baseline，该方法的核心优势在于...，这体现在..."
- ✅ "对我关于Z的研究的直接帮助是..."

**信息密度检查:**
- 每段必须有≥1个具体事实/数据/观点
- 禁止连续三段都是背景介绍而无实质内容
- 分析:背景比例应 ≥ 7:3

---

## 九、知识库服务目标

整个知识库最终服务于以下产出:

### 9.1 选题支持
- 通过 Gap Analysis 识别有价值的研究问题
- 通过 Trend Report 预判热点方向
- 通过横向比较评估不同路线的优劣

### 9.2 方法设计
- 参考同类问题的解决方案
- 避免已知的方法缺陷
- 借鉴跨领域的思路迁移

### 9.3 理论切入
- 找到理论的支撑点和创新点
- 了解相关理论的发展脉络
- 识别理论的适用范围和边界

### 9.4 论文写作
- 文献综述的素材库
- Related Work 的对比基准
- 引言部分的背景铺垫
- 讨论部分的延伸方向

---

## 十、下一步建议

完成本规范说明后，最合理的下一个动作是:

### Phase 1: 基础设施搭建 (预计1-2天)

1. **初始化目录结构**
   ```bash
   mkdir -p research-kb/{biomed,comptheory,deepseek,cross-domain,synthesis,_meta,inbox}
   ```

2. **创建元数据模板文件**
   - `_meta/journal-tiers.yaml`: 定义期刊分级
   - `_meta/institution-priority.yaml`: 定义机构优先级
   - `_meta/keyword-taxonomy.yaml`: 定义关键词分类
   - `_meta/paper-template.md`: 论文分析模板

3. **建立初始索引**
   - 整理你目前已有的论文列表
   - 按规范命名并填入 `catalog-index.yaml`

### Phase 2: 试点运行 (预计1周)

选择一个小主题 (建议: **DeepSeek MLA** 或 **Drug Repositioning with GNN**):
- 检索5-10篇核心论文
- 按规范完成单篇分析
- 尝试一次横向比较
- 根据实际体验微调规范

### Phase 3: 全面展开

试点验证规范可行后，开始系统性地:
- 设置定期检索任务
- 建立持续更新的工作流
- 按季度产出趋势报告和空白分析

---

**是否现在开始 Phase 1 的基础设施搭建？** 或者你有现有的论文列表需要先整理入库？
