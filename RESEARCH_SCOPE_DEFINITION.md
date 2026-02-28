# 研究范围定义与筛选边界说明

> **版本**: v1.0  
> **日期**: 2026-02-28  
> **阶段**: Phase 1 - 研究范围正式确定  
> **性质**: 长期研究档案系统的核心筛选准则

---

## 一、总体原则

### 1.1 核心定位

本研究知识库**不是**泛泛的文献收集系统，而是一个**高度聚焦的长期研究档案系统**。其存在目的是：

- 服务于选题论证与方法设计
- 支撑理论切入与论文写作
- 积累可复用的研究洞察与方法论

### 1.2 筛选黄金法则

| 法则 | 说明 |
|------|------|
| **高相关** | 与研究主线直接关联，非表面关键词匹配 |
| **高价值** | 具有理论贡献、方法创新或机制深度 |
| **可服务** | 能直接支持我的研究设计、理论切入或论文写作 |
| **宁可少收** | 质量 > 数量，宁缺毋滥 |

### 1.3 排除红线

以下情况**一律不纳入核心知识库**：
- ❌ 仅表面关键词相关，无实质研究价值
- ❌ 不符合主题重点、作者优先级、期刊层级或理论取向
- ❌ 纯应用部署、工程优化、行业场景落地
- ❌ 常规统计流程、无机制或方法深度

---

## 二、研究主线边界定义

### 2.1 生物信息与医学主线 (BioMed)

#### 2.1.1 核心主题 (P0 - 必须深度分析)

| 主题 | 关键词 | 纳入标准 |
|------|--------|---------|
| **Drug Discovery** | drug discovery, drug design, de novo design | AI/ML方法 + 实验验证或大规模基准 |
| **Drug Repositioning** | drug repurposing, drug rescue, therapeutic switching | 计算方法 + 生物学机制解释 |
| **Drug-Target Interaction** | DTI prediction, target identification, binding affinity | 预测方法创新 + 可解释性 |
| **Molecular Generation** | molecular generation, generative chemistry | 生成模型(VAE/GAN/Diffusion/Flow) + 化学有效性验证 |
| **RNA Therapeutics** | RNA drug, RNA therapy, mRNA/siRNA drug | 递送机制或调控机制研究 |
| **RNA Regulation** | miRNA, lncRNA, circRNA, post-transcriptional regulation | 功能机制 + 疾病关联 + 计算方法 |

**核心标准**: 必须具备以下条件中的至少两项
- 提出新的计算方法或模型架构
- 有清晰的机制解释或生物学洞察
- 发表在 Nature/Science/Cell 主刊或子刊
- ZSJ团队相关 (自动进入核心)

#### 2.1.2 扩展主题 (P1 - 中等深度分析)

| 主题 | 关键词 | 纳入条件 |
|------|--------|---------|
| **Disease Mechanism** | disease pathway, pathogenesis, omics integration | 多组学整合 + 网络分析 + 临床相关性 |
| **Translational Medicine** | bench to bedside, clinical translation | 计算预测 + 临床验证队列 |
| **Precision Medicine** | personalized medicine, stratified medicine | 分层标志物 + 治疗响应预测 |
| **Biomarker Discovery** | biomarker, predictive marker, prognostic marker | 机制深度或方法深度 (非单纯统计关联) |
| **Protein-Ligand Interaction** | docking, scoring function, binding pose | 结构预测精度提升 + 大规模基准测试 |

**扩展标准**: 满足以下之一即可考虑
- 发表在领域顶刊 (NAR, Genome Biology, Brief Bioinf)
- 方法有创新性但验证规模有限
- 可作为对比基线或方法参考

#### 2.1.3 弱相关主题 (P2 - 简要归档)

| 主题 | 处理方式 |
|------|---------|
| **General Multi-omics** | 仅保留ZSJ相关或顶级期刊 |
| **Single-cell analysis** | 除非涉及药物响应或疾病机制 |
| **Imaging-based diagnosis** | 除非结合分子机制 |
| **Clinical trial design** | 除非涉及计算优化方法 |

#### 2.1.4 明确排除

❌ **不纳入**:
- 纯差异表达分析 (DEA) 无下游机制研究
- 常规生存分析、预后模型 (无方法创新)
- 仅数据库构建或资源论文 (无方法贡献)
- 病例报告、小样本临床观察

---

### 2.2 计算机与理论主线 (CompTheory)

#### 2.2.1 核心主题 (P0 - 必须深度分析)

| 主题 | 关键方向 | 纳入标准 |
|------|---------|---------|
| **Attention Mechanisms** | attention structure, multi-head attention, attention pattern analysis | 理论分析 + 表示能力 + 复杂度研究 |
| **Latent Attention** | latent attention, compressed attention, KV cache compression | 压缩机制 + 信息保留分析 + 效率评估 |
| **Low-Rank Methods** | low-rank approximation, intrinsic dimension, rank analysis | 理论保证 + 表达能力影响 + 优化特性 |
| **LoRA Theory** | LoRA mechanism, rank-selection, generalization, forgetting | 学习动态 + 与full FT对比 + 理论解释 |
| **Relation Modeling** | internal representation, module interaction, token relation | 关系评估方法 + 可视化 + 机制解释 |

**核心标准**: 必须具备
- 理论分析 (证明、复杂度、收敛性等)
- 机制解释 (为什么有效、何时失效)
- 发表在 NeurIPS/ICML/ICLR/JMLR 或同等顶会/期刊

#### 2.2.2 扩展主题 (P1 - 中等深度分析)

| 主题 | 关键方向 | 纳入条件 |
|------|---------|---------|
| **KV Cache Optimization** | KV compression, memory efficiency, long-context handling | 效率提升 + 性能保持验证 |
| **Parameter-Efficient Fine-Tuning** | adapters, prompt tuning, prefix tuning | 与LoRA对比 + 适用场景分析 |
| **Transformer Architecture** | architecture variants, inductive biases | 结构创新 + 理论动机 |
| **Interpretability** | attention visualization, probing, feature analysis | 揭示机制 + 可复现发现 |

**扩展标准**:
- 发表在 ACL/EMNLP/AAAI/IJCAI 等主流会议
- 方法有启发性但理论分析不够深入
- 可作为实现参考或对比基线

#### 2.2.3 偏应用排除项 (明确不纳入)

❌ **不纳入核心知识库**:
| 类型 | 示例 | 原因 |
|------|------|------|
| **纯工程优化** | CUDA kernel优化、推理加速、量化部署 | 无理论贡献 |
| **下游任务套用** | 在XX任务上用LoRA微调 | 无方法创新或机制分析 |
| **行业场景应用** | 金融/法律/医疗领域的LLM应用 | 超出研究边界 |
| **性能刷榜** | 在某数据集上提X%准确率 | 无方法论贡献 |
| **Prompt Engineering** | 提示词技巧、Few-shot模板 | 过于实践导向 |

#### 2.2.4 关键词陷阱识别

⚠️ **表面相关但应排除**:
- 标题含 "attention" 但只是用Transformer做分类
- 标题含 "low-rank" 但只是简单矩阵分解应用
- 标题含 "LoRA" 但只是超参数调优经验分享

✅ **真正相关需满足**: 对方法本身的理解、分析或改进

---

### 2.3 DeepSeek 专项主线 (DeepSeek Priority)

#### 2.3.1 DeepSeek 核心线 (P0+ - 最高优先级)

| 类别 | 内容 | 处理方式 |
|------|------|---------|
| **官方技术报告** | DeepSeek-V2/V3/Coder 技术报告 | 全部收录，完整深度分析 |
| **核心架构创新** | MLA, DeepSeekMoE, aux-loss-free load balancing | 全部收录，重点关注机制 |
| **效率优化技术** | FP8 training, DualPipe, Expert Parallelism | 收录并分析与理论主线的关联 |

**特殊规则**: DeepSeek 官方工作自动接受，无论预印本或正式发表

#### 2.3.2 围绕 DeepSeek 的扩展研究 (P0 - 核心优先级)

| 方向 | 示例 | 纳入标准 |
|------|------|---------|
| **MLA 理论分析** | 低秩压缩的理论保证、信息损失分析 | 有理论证明或系统性实验 |
| **架构推广** | 将 MLA/MoE 应用到其他 Transformer 变体 | 有适配创新 + 效果验证 |
| **机制研究** | DeepSeek 内部表示分析、专家行为分析 | 有可复现的发现 |
| **效率评估** | 不同压缩策略对比、硬件友好性分析 | 有系统性基准测试 |

#### 2.3.3 热点借用降低优先级 (P1 或排除)

⚠️ **降低优先级**:
- 仅在标题/摘要提及 DeepSeek 但没有实质机制关联
- 简单复现 DeepSeek 结果而无新发现
- 将 DeepSeek 作为基线对比但无深入分析

❌ **排除**:
- 蹭热点但内容与 DeepSeek 机制无关
- 错误解读 DeepSeek 技术导致结论不可靠

---

## 三、研究者/团队优先级规则

### 3.1 优先级体系

| 层级 | 研究者/团队 | 规则 |
|------|------------|------|
| **Tier 0 (最高)** | ZSJ (Shuangjia Zheng, SJTU) | 所有论文必收，按相关性分层处理 |
| **Tier 1 (高)** | DeepSeek 官方团队 | 所有技术报告必收 |
| **Tier 2 (中)** | SJTU医学院、郑大附院相关团队 | 高质量工作优先关注 |
| **Tier 3 (一般)** | MIT、Stanford、Harvard、Google DeepMind 等 | 顶刊/顶会工作值得关注 |

### 3.2 ZSJ专属规则

#### 3.2.1 全收录原则

**所有ZSJ相关论文必须进入作者专属档案**，不漏一篇。

#### 3.2.2 分层分析规则

| 相关性得分 | 层级 | 分析深度 | 存储位置 |
|-----------|------|---------|---------|
| ≥ 0.7 | 高度相关 | 完整深度分析 (10+章节) | `authors/ZSJ/papers/full-analysis/` |
| 0.4 - 0.7 | 中度相关 | 中等深度分析 (5-7章节) | `authors/ZSJ/papers/medium-analysis/` |
| < 0.4 | 弱相关 | 简要归档 (metadata+摘要) | `authors/ZSJ/papers/minimal-archive/` |

#### 3.2.3 相关性计算维度

```
relevance_score = 
    0.4 × keyword_match (关键词匹配)
  + 0.3 × method_match (方法匹配)
  + 0.2 × institution_match (机构匹配，ZSJ此项加权)
  + 0.1 × data_type_match (数据类型匹配)

bonus:
  + 0.5 if author_is_zheng_shuangjia
```

#### 3.2.4 防止范围无限扩大的机制

**机制 1: 硬边界约束**
- 即使ZSJ论文，也必须通过基础质量门槛 (非随意涂鸦)
- 弱相关论文只存 minimal-archive，不进主题知识库索引

**机制 2: 主题关联阈值**
- 只有 relevance ≥ 0.4 才进入主题线索引
- 避免作者线与主题线过度耦合

**机制 3: 定期 review**
- 每季度回顾ZSJ论文收录情况
- 调整相关性评分标准和分层阈值

### 3.3 作者线与主题线关系

```
┌─────────────────────────────────────────────────────────────┐
│                    单点存储原则                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   作者线 (主存储)                                            │
│   authors/ZSJ/papers/full-analysis/xxx.md       │
│        ↑                                                    │
│        │ 唯一完整版本                                       │
│        ↓                                                    │
│   主题线 (引用/索引)                                         │
│   topics/biomed/drug-discovery/index.yaml                   │
│     - paper_id: xxx                                         │
│     - ref: "authors/ZSJ/..."                    │
│     - relevance: 0.85                                       │
│                                                             │
│   规则:                                                     │
│   • 主题目录下不存重复内容                                   │
│   • 只存 YAML/JSON 索引或 Markdown 引用                      │
│   • 更新时只需修改作者线文件                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 四、期刊/会议/来源层级规则

### 4.1 Tier 1 - 顶级来源 (优先深度分析)

#### 综合期刊
- **Nature**, **Science**, **Cell** (主刊)

#### 医学/生信子刊
- Nature Medicine, Nature Biotechnology, Nature Methods
- Nature Reviews Drug Discovery
- Cell Research, Cancer Cell, Cell Systems

#### 生信领域顶刊
- Nucleic Acids Research
- Genome Biology
- Briefings in Bioinformatics

#### AI/ML 顶会
- NeurIPS, ICML, ICLR
- ACL, EMNLP (NLP方向)

**规则**: Tier 1 来源自动进入候选，经主题相关性确认后深度分析。

### 4.2 Tier 2 - 高水平来源 (应关注)

#### 医学/生信
- Bioinformatics, PLOS Computational Biology
- BMC Bioinformatics, GigaScience
- Journal of Chemical Information and Modeling (JCIM)

#### AI/ML 期刊
- JMLR, TPAMI, TNNLS
- AAAI, IJCAI (会议)

#### 技术报告
- arXiv (计算机理论方向，需严格评估)
- DeepSeek 官方技术报告

**规则**: Tier 2 来源经评估后，有价值的进入中等或完整分析。

### 4.3 Tier 3 - 补充来源 (选择性收录)

- Scientific Reports, Frontiers 系列
- 国内优秀期刊 (Cell Research 等已列 Tier 1)
- 高质量预印本 (bioRxiv, medRxiv)

**规则**: 仅当内容特别相关或有独特价值时才收录。

### 4.4 Tier 4 - 通常不纳入

- 低影响因子期刊 (<3)
- 无同行评审的会议
- 个人博客、技术笔记 (除非来自顶级专家)

### 4.5 特殊来源处理

| 来源类型 | 处理方式 |
|---------|---------|
| **技术报告** | DeepSeek官方必收；其他需评估原创性 |
| **预印本** | arXiv计算机理论可收；bioRxiv/medRxiv需评估 |
| **开源项目** | 只存链接和关键文档，不存代码 |
| **附属材料** | Supplementary 数据按需下载，不存入仓库 |

---

## 五、纳入规则

### 5.1 核心知识库纳入条件

一篇论文要进入**核心知识库** (即进行深度分析)，必须满足：

#### 必要条件 (必须全部满足)

| # | 条件 | 检查方式 |
|---|------|---------|
| 1 | **主题相关性** | 属于三大主线之一，且非排除项 |
| 2 | **来源层级** | Tier 1-2，或ZSJ/Tier 0 作者 |
| 3 | **理论/方法价值** | 有理论贡献、方法创新或机制深度 |
| 4 | **可服务性** | 能支持我的研究设计、理论切入或论文写作 |

#### 充分条件 (满足任一即可优先考虑)

- ZSJ团队论文 (自动进入)
- DeepSeek 官方论文 (自动进入)
- Nature/Science/Cell 主刊 (快速通道)
- 高引用 (>100) 且主题高度相关

### 5.2 快速筛选检查清单

```markdown
□ 主题检查: 是否属于 BioMed/CompTheory/DeepSeek 主线?
□ 来源检查: 是否 Tier 1-2 或ZSJ/DeepSeek?
□ 质量检查: 是否有理论/方法/机制贡献?
□ 价值检查: 是否能服务于我的研究?
□ 排除检查: 是否触碰排除红线?
```

**4个✅ + 0个❌ = 纳入核心知识库**

---

## 六、排除规则

### 6.1 明确排除项

以下类型的内容**原则上不进入核心知识库**：

| 类别 | 具体描述 | 示例 |
|------|---------|------|
| **泛泛生信分析** | 仅有常规差异分析、聚类、富集分析 | GEO数据下载→DEA→GO富集→发文 |
| **常规统计流程** | 没有机制或方法深度的统计建模 | 简单Cox回归、LASSO预后模型 |
| **方法套用无分析** | 仅在下游任务套用LoRA/attention | "我们用LoRA微调了BERT做分类" |
| **纯工程优化** | 部署提速、内存优化、并行化 | CUDA kernel优化、推理加速 |
| **行业场景落地** | 特定领域的应用实践 | 金融风控、法律合同审查 |
| **表面相关无实质** | 关键词匹配但内容偏离 | 标题有drug但实际是材料科学 |

### 6.2 排除决策树

```
论文初筛
    ↓
是否ZSJ/DeepSeek官方? ──Yes──► 纳入 (跳过后续检查)
    ↓ No
是否Tier 1-2来源? ──No──► 大概率排除 (除非特别相关)
    ↓ Yes
是否核心主题? ──No──► 排除
    ↓ Yes
是否有理论/方法贡献? ──No──► 排除
    ↓ Yes
是否纯应用/部署/优化? ──Yes──► 排除
    ↓ No
纳入核心知识库候选
```

---

## 七、优先级分层

### 7.1 三层优先级体系

| 层级 | 名称 | 判断标准 | 处理方式 |
|------|------|---------|---------|
| **P0** | 核心优先级 | 高度相关 + 高价值 | 完整深度分析，进主题知识库 |
| **P1** | 次级优先级 | 中度相关 或 扩展价值 | 中等深度分析，进作者档案+主题索引 |
| **P2** | 辅助优先级 | 弱相关 或 完整性需要 | 简要归档，仅metadata+摘要 |

### 7.2 各层级详细标准

#### P0 - 核心优先级 (完整深度分析)

**标准** (满足任一):
- ZSJ团队 + 相关性 ≥ 0.7
- DeepSeek 官方技术报告
- Nature/Science/Cell 主刊 + 主题高度相关
- NeurIPS/ICML/ICLR + 理论贡献显著

**产出要求**:
- 完整分析文档 (10+章节)
- 包含方法拆解、实验分析、与我研究关联
- 进入主题知识库核心索引

#### P1 - 次级优先级 (中等深度分析)

**标准** (满足任一):
- ZSJ团队 + 相关性 0.4-0.7
- Tier 1-2 期刊 + 方法有启发性
- 可作为对比基线或方法参考
- 跨领域启发 (如 CompTheory 方法应用于 BioMed)

**产出要求**:
- 中等分析文档 (5-7核心章节)
- 突出关键贡献和与我研究的关联
- 进入作者档案 + 主题线索引

#### P2 - 辅助优先级 (简要归档)

**标准** (满足任一):
- ZSJ团队 + 相关性 < 0.4 (完整性需要)
- Tier 3 来源 + 有一定参考价值
- 综述类文章 (权威作者的系统性综述)

**产出要求**:
- 简要归档 (metadata + 一句话总结 + 主题标签)
- 不进入主题知识库核心，仅作者档案
- 用于完整性追踪和潜在后续深挖

### 7.3 优先级动态调整

**升级条件**:
- 后续发现该论文被高频引用
- 与我正在开展的研究产生直接关联
- 发现之前低估了其价值

**降级条件**:
- 后续出现更优的方法替代
- 发现存在严重方法缺陷
- 主题相关性随研究方向调整而降低

---

## 八、规则执行与维护

### 8.1 规则更新机制

| 触发条件 | 更新方式 |
|---------|---------|
| 新增重要期刊/会议 | 更新 `journal-tiers.yaml` |
| 研究方向调整 | 更新 `topic-scope.yaml` |
| 新增关注作者 | 更新 `institution-priority.yaml` |
| 发现规则漏洞 | 更新本文档并记录变更日志 |

### 8.2 规则冲突解决

当多条规则冲突时，优先级如下：

1. **作者优先级 > 主题优先级**: ZSJ弱相关 > 其他人高度相关
2. **来源优先级 > 主题优先级**: Nature 主刊边缘主题 > 普通期刊核心主题
3. **排除规则 > 纳入规则**: 触碰红线的论文一律排除

### 8.3 质量保证

**入库前检查**:
- 使用 `paper-checklist.yaml` 逐项核对
- 至少两人确认 (我 + AI Assistant)
- 争议论文标记为 `UNDECIDED` 待进一步评估

**定期 review**:
- 每月回顾一次收录决策
- 每季度评估规则有效性
- 每年全面修订研究范围定义

---

## 九、下一步建议

### 9.1 Phase 1 状态

**已完成**:
- ✅ 研究范围定义与筛选边界说明 (本文档)
- ✅ 工作规范说明 (`WORKFLOW_SPEC.md`)
- ✅ Gitee 仓库结构设计方案 (`GITEE_REPO_DESIGN.md`)
- ✅ 模板与 schema 体系 (8个 YAML 文件)
- ✅ ZSJ作者档案框架
- ✅ DeepSeek 试点验证

### 9.2 Phase 1 最合理的下一个动作

> **创建 Gitee 仓库并推送 Phase 1 全部成果**

这是 Phase 1 的最后一步，也是 Phase 2 的唯一前置依赖。

**执行命令**:
```bash
cd /root/openclaw-workspace/research-kb
git init
git add .
git commit -m "Phase 1 complete: research scope, workflow spec, templates, author archive"
# 然后在 Gitee 创建仓库并推送
```

### 9.3 Phase 2 启动预览

**Week 1**: 基础设施完善
- Gitee 仓库初始化
- 自动化检索脚本部署
- 首批 5-10 篇论文入库 (ZSJ + DeepSeek)

**Week 2-4**: 批量建设
- 每周 3-5 篇深度分析
- 建立稳定的工作流节奏
- 完成首批横向比较

**Month 2**: 综合产出
- 第一份趋势报告
- 第一份研究空白分析
- 支持第一个选题论证

---

## 附录: 快速参考卡

### A.1 纳入检查清单 (5秒版)

```
□ ZSJ/DeepSeek? → 自动纳入
□ Nature/Science/Cell? → 快速通道
□ 核心主题? → 继续检查
□ 理论/方法价值? → 继续检查
□ 非排除项? → 纳入
```

### A.2 排除红线 (3秒版)

```
纯应用? 工程优化? 常规统计? 套用无分析? → 排除
```

### A.3 优先级速查

```
P0: ZSJ≥0.7 / DeepSeek官方 / Nature/Science/Cell
P1: ZSJ0.4-0.7 / Tier1-2有价值 / 可对比参考
P2: ZSJ<0.4(完整性) / Tier3有价值 / 权威综述
```

---

**文档编制**: AI Assistant  
**审核状态**: Phase 1 第二部分完成  
**生效日期**: 2026-02-28  
**下次 review**: 2026-03-31
