# Phase 1 规则层总览与验收说明

> **版本**: v1.0 Final  
> **日期**: 2026-02-28  
> **状态**: Phase 1 正式收尾  
> **用途**: Phase 2 启动基准文档

---

## 一、统一核对结果

### 1.1 研究边界一致性检查 ✅

| 维度 | 定义来源 | 状态 |
|------|---------|------|
| **BioMed 方向** | WORKFLOW_SPEC.md §1.1 | ✅ 明确：药物/RNA/医学机制 |
| **CompTheory 方向** | WORKFLOW_SPEC.md §1.2 | ✅ 明确：Attention/Low-rank/LoRA/关系评估 |
| **DeepSeek 专项** | WORKFLOW_SPEC.md §1.3 | ✅ 明确：MLA/MoE/KV压缩官方+外部研究 |
| **排除项** | WORKFLOW_SPEC.md §1.x | ✅ 明确：纯应用/无理论分析/简单套用 |

**核对结论**: 三大方向边界清晰，无重叠冲突。

### 1.2 优先级体系一致性检查 ✅

| 层级 | 定义 | 规则位置 | 状态 |
|------|------|---------|------|
| **P0 (最高)** | 郑双佳 + DeepSeek官方 + 核心主题 | institution-priority.yaml + topic-scope.yaml | ✅ 一致 |
| **P1 (高)** | 高质量期刊/会议 + 理论贡献 | journal-tiers.yaml tier_1-3 | ✅ 一致 |
| **P2 (中)** | 领域知名机构 + 方法创新 | journal-tiers.yaml tier_3 | ✅ 一致 |
| **P3 (低)** | 其他可靠来源 | journal-tiers.yaml tier_4 | ✅ 一致 |

**核对结论**: 四层优先级体系完整，自动接受规则已配置。

### 1.3 作者线规则完整性检查 ✅

| 规则项 | 定义位置 | 实现状态 |
|--------|---------|---------|
| **最高优先级作者** | institution-priority.yaml tier_1_core | ✅ 郑双佳 |
| **名称变体** | institution-priority.yaml name_variants | ✅ 6种变体 |
| **机构变体** | institution-priority.yaml institution_variants | ✅ 5种变体 |
| **分层分析规则** | topic-scope.yaml zheng_shuangjia_analysis_tiers | ✅ 高/中/低三层 |
| **专属目录** | GITEE_REPO_DESIGN.md §5 | ✅ authors/zheng-shuangjia/ |
| **关键词集合** | keywords.yaml | ✅ 14核心方向+扩展 |

**核对结论**: 郑双佳作者线已完整纳入规则体系。

### 1.4 仓库结构与模板字段一致性检查 ⚠️

| 检查项 | 仓库结构支持 | 模板字段覆盖 | 状态 |
|--------|-------------|-------------|------|
| **单篇论文存储** | papers/{tier}/ 或 authors/{author}/papers/ | metadata-schema.yaml core_metadata | ✅ 一致 |
| **主题索引** | topics/{domain}/{topic}/indices/ | metadata-schema.yaml classification | ✅ 一致 |
| **年份索引** | indices/by-year/ | metadata-schema.yaml year | ✅ 一致 |
| **作者索引** | indices/by-author/ + authors/{author}/indices/ | metadata-schema.yaml authors | ✅ 一致 |
| **深度分级** | full-analysis/medium-analysis/minimal-archive | metadata-schema.yaml analysis_depth | ✅ 一致 |
| **写作标签** | writing/ 目录 | writing-usage-tags.yaml | ✅ 一致 |
| **理论标签** | theory/ 目录 | theory-tags.yaml | ✅ 一致 |

**发现缺口**: 
- `resource_status` (code_status/data_status/reproduction_status) 已添加到 metadata-schema.yaml，但 Gitee 设计方案中未明确对应存储位置
- 建议：在 `papers/` 下的元数据文件中直接包含这些字段

### 1.5 Metadata 与目录结构承接性检查 ✅

```
metadata.json ──┬──► 决定存储路径 (direction/topic/analysis_depth)
                ├──► 生成文件名 (year-author-slug)
                ├──► 创建索引条目 (indices/)
                └──► 关联作者档案 (authors/)
```

**核对结论**: metadata 字段完整覆盖目录路由需求。

### 1.6 Gitee 轻量化要求落实检查 ✅

| 要求 | 设计方案 | 状态 |
|------|---------|------|
| **不存 PDF** | 只存链接，外部平台存放 | ✅ GITEE_REPO_DESIGN.md §8 |
| **不存大附件** | >100KB 图片转外部 | ✅ GITEE_REPO_DESIGN.md §8 |
| **不存原始日志** | 本地/缓存定期清理 | ✅ GITEE_REPO_DESIGN.md §8 |
| **分层存储** | L1核心(Gitee)+L2扩展(外部)+L3临时(本地) | ✅ GITEE_REPO_DESIGN.md §8 |
| **体积控制** | 目标 <50MB，预估 500篇=5MB | ✅ GITEE_REPO_DESIGN.md §8 |

**核对结论**: 轻量化原则已全面落实。

---

## 二、冲突与缺口识别

### 2.1 发现的冲突 ❌→✅

| 冲突描述 | 位置 | 严重程度 | 修正方案 |
|---------|------|---------|---------|
| ~~institution-priority.yaml 中上交/郑大优先级表述模糊~~ | §1.1 | 中等 | ✅ 已修正为"郑双佳个人最高优先级" |
| ~~journal-tiers.yaml 中 auto_accept 规则与 topic-scope.yaml 决策规则重复~~ | 多处 | 低 | ✅ 已统一以 topic-scope.yaml 为准 |

**当前状态**: 无重大冲突。

### 2.2 识别的缺口 ⚠️

| 缺口描述 | 影响 | 补充优先级 | 建议方案 |
|---------|------|-----------|---------|
| **缺少自动化脚本设计** | Phase 2 效率 | P1 | 补充 `scripts/` 目录设计和核心脚本规范 |
| **缺少 Git 工作流规范** | 协作与版本 | P2 | 补充 `.gitignore` 策略和分支策略 |
| **缺少数据备份策略** | 长期安全 | P2 | 补充 Gitee + 本地双备份方案 |
| **缺少质量门禁规则** | 入库质量 | P1 | 补充 metadata 校验脚本和人工 review 触发条件 |
| **缺少与其他工具的集成** | 工作流完整 | P3 | 预留 Zotero/Obsidian/Notion 集成接口 |

### 2.3 需要明确的边界 📋

| 问题 | 当前状态 | 建议明确 |
|------|---------|---------|
| **简版分析与详版分析的触发条件** | 有分值阈值 | 补充字数/章节数参考标准 |
| **横向比较的触发条件** | "≥5篇" | 补充时间跨度要求 |
| **趋势报告的更新频率** | "每季度" | 补充具体月份 |
| **废弃主题的归档策略** | 提及但未详细 | 补充 archived/ 目录规范 |

---

## 三、Phase 1 最终规则清单

### 3.1 研究边界摘要

```
┌─────────────────────────────────────────────────────────────┐
│                     研究知识库边界                           │
├─────────────────────────────────────────────────────────────┤
│ BioMed (生物信息/医学)                                       │
│   • 药物发现: discovery, repositioning, DTI, response       │
│   • RNA相关: miRNA, lncRNA, circRNA, therapeutics           │
│   • 医学机制: disease mechanism, precision medicine         │
│   ⚠️ 排除: 纯临床观察、无机制研究、低水平期刊               │
├─────────────────────────────────────────────────────────────┤
│ CompTheory (计算机理论)                                      │
│   • Attention: 结构分析、KV压缩、表示能力                    │
│   • Low-rank: 近似、参数化、适配机制                         │
│   • LoRA: 学习机制、rank分析、泛化特性                       │
│   • 关系评估: 内部表示、模块交互、token关系                  │
│   ⚠️ 排除: 纯应用部署、性能提升无理论、简单套用              │
├─────────────────────────────────────────────────────────────┤
│ DeepSeek 专项 (最高优先级)                                   │
│   • 官方报告: V2/V3/Coder 技术报告                          │
│   • 核心机制: MLA, MoE, KV compression                      │
│   • 外部研究: MLA理论分析、架构推广                          │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 优先级体系摘要

| 优先级 | 对象 | 自动接受 | 分析深度 | 存储位置 |
|--------|------|---------|---------|---------|
| **P0** | 郑双佳所有论文 + DeepSeek官方 | ✅ 是 | 按相关性分层 | authors/zheng-shuangjia/papers/ |
| **P1** | Tier 1-2 期刊 + 顶会 + 高相关 | 否 | 完整分析 | topics/{topic}/analyses/ |
| **P2** | Tier 3 期刊 + 方法创新 | 否 | 中等分析 | topics/{topic}/analyses/ |
| **P3** | Tier 4 + 预印本 + 弱相关 | 否 | 简要归档 | inbox/rejected/ 或 minimal-archive |

**相关性分层 (郑双佳专属)**:
- ≥0.7: full-analysis (完整10+章节)
- 0.4-0.7: medium-analysis (核心5-7章节)
- <0.4: minimal-archive (metadata+摘要)

### 3.3 仓库结构摘要

```
research-kb-gitee/                    # Gitee 主仓库 (<50MB)
├── schemas/                          # 模板与规范
├── indices/                          # 全局索引 (轻量)
│   ├── by-year/
│   ├── by-topic/
│   └── by-author/
├── topics/                           # 主题线 (引用为主)
│   ├── biomed/
│   ├── cs-theory/
│   └── deepseek/
├── authors/                          # 作者线 (主存储)
│   └── zheng-shuangjia/              # 最高优先级作者
│       ├── profile/
│       ├── papers/{full,medium,minimal}/
│       ├── timeline/
│       ├── topic-map/
│       └── indices/
├── papers/                           # 非郑双佳论文主存储
├── comparisons/                      # 横向比较
├── trends/                           # 趋势报告
├── gaps/                             # 空白分析
├── theory/                           # 理论笔记
├── writing/                          # 写作素材
└── reports/                          # 阶段总结
```

### 3.4 模板与 Metadata 体系摘要

**核心模板文件** (8个):
| 文件 | 用途 | 关键字段 |
|------|------|---------|
| `journal-tiers.yaml` | 期刊分级 | tier, auto_accept, match_score |
| `keyword-taxonomy.yaml` | 关键词分类 | domain/category/keyword, priority |
| `institution-priority.yaml` | 机构/作者优先级 | priority, auto_accept, track_new_pub |
| `topic-scope.yaml` | 主题范围 | include/exclude rules, relevance_threshold |
| `metadata-schema.yaml` | 论文元数据 | 30+ 字段，覆盖核心/分类/分析/资源状态 |
| `writing-usage-tags.yaml` | 写作用途 | intro/rw/method/discussion tags |
| `theory-tags.yaml` | 理论分析 | expressivity/optimization/generalization |
| `reproducibility-rubric.yaml` | 可复现性评估 | 5维度评分标准 |

**Metadata 核心字段**:
```yaml
# 标识
paper_id, slug, title, authors[], year, venue

# 分类
direction, topic, subtopic, tags[], journal_tier, quality_score

# 优先级
institution_priority, author_priority, is_zheng_shuangjia, deepseek_related

# 分析
relevance_score, analysis_depth, analysis_status, theoretical_contribution_types[]

# 资源
code_status, data_status, reproduction_status, reading_status

# 使用
writing_usage[], theory_relevance[], cited_in[]

# 路径
main_storage, topic_indices[]
```

### 3.5 作者线与主题线组织原则

**核心原则: 单点存储 + 多维度索引**

```
作者线 (主存储)
================
authors/zheng-shuangjia/papers/full-analysis/2025-zheng-xxx.md
    ↑
    │ 自动生成/更新
    ↓
主题线 (索引引用)
================
topics/biomed/drug-discovery/index.yaml:
  - paper_id: "arxiv:2501.12345"
    ref: "authors/zheng-shuangjia/papers/full-analysis/2025-zheng-xxx.md"
    relevance: 0.85

年份线 (索引引用)
================
indices/by-year/2025.md:
  - [2025-zheng-xxx](authors/zheng-shuangjia/papers/...)
```

**避免重复**: 主题目录下只存 YAML/JSON 索引或 Markdown 引用，不存完整分析内容。

### 3.6 轻量化与空间控制原则

| 层级 | 存储位置 | 内容 | 体积限制 |
|------|---------|------|---------|
| **L1 核心** | Gitee 仓库 | 分析文档、索引、报告 | <50MB |
| **L2 扩展** | 外部平台 (OSS/Drive) | PDF、代码、数据集 | 链接引用 |
| **L3 临时** | 本地/缓存 | 原始日志、中间文件 | 定期清理 |

**控制策略**:
- 单文件大小: Markdown 无限制，图片 <100KB
- 目录深度: 最大 4 级
- 单目录文件数: 建议 <100，超过则细分
- 命名规范: `{YYYY}-{lastname}-{slug}.md`，小写+连字符

---

## 四、验收结论

### 4.1 Phase 1 完成状态

| 交付物 | 状态 | 位置 |
|--------|------|------|
| 研究知识库工作规范说明 | ✅ 完成 | `WORKFLOW_SPEC.md` |
| Gitee 仓库结构设计方案 | ✅ 完成 | `GITEE_REPO_DESIGN.md` |
| 8个核心模板文件 | ✅ 完成 | `_meta/*.yaml` |
| 郑双佳作者档案 | ✅ 完成 | `biomed/authors/zheng-shuangjia/` |
| DeepSeek 试点运行 | ✅ 完成 | `deepseek/mla-analysis/` |

### 4.2 是否视为完成？

**✅ Phase 1 可以视为完成。**

理由:
1. 研究边界清晰且完整
2. 优先级体系已建立
3. 仓库结构已设计
4. 模板体系已完善
5. 作者线已完整纳入
6. 轻量化原则已落实
7. 试点运行验证了可行性

### 4.3 尚未完成的非关键项

以下缺口不影响 Phase 2 启动，可在进行中补充:

| 缺口 | 优先级 | 计划补充时机 |
|------|--------|-------------|
| 自动化脚本设计 | P1 | Phase 2 第一周 |
| Git 工作流规范 | P2 | Phase 2 第二周 |
| 质量门禁规则 | P1 | 首批 10 篇入库后 |
| 数据备份策略 | P2 | Gitee 仓库创建后 |

### 4.4 Phase 2 最合理的启动方式

**推荐启动方式: "主题驱动 + 作者并行"**

```
Week 1: 基础设施
  ├── 创建 Gitee 仓库并推送 Phase 1 成果
  ├── 设置自动化检索脚本 (arXiv每日 + PubMed每周)
  └── 入库首批 5-10 篇论文 (郑双佳 + DeepSeek)

Week 2-4: 批量建设
  ├── 每周完成 3-5 篇深度分析
  ├── 建立 inbox → review → analyze → index 工作流
  └── 完成首批横向比较 (DeepSeek MLA vs 其他 KV压缩)

Month 2: 综合产出
  ├── 第一份趋势报告 (DeepSeek 专项)
  ├── 第一份研究空白分析 (AI for Drug Discovery)
  └── 支持第一个选题论证
```

**最关键的下一步动作**:

> **创建 Gitee 仓库并初始化**
> 
> 这是 Phase 2 的唯一前置依赖。完成后即可开始批量论文入库。

---

## 五、附录: 快速参考

### 5.1 核心文件索引

| 文件 | 用途 |
|------|------|
| `WORKFLOW_SPEC.md` | 完整工作规范 |
| `GITEE_REPO_DESIGN.md` | 仓库结构设计 |
| `_meta/*.yaml` | 8个模板文件 |
| `biomed/authors/zheng-shuangjia/profile/` | 郑双佳档案 |
| `deepseek/mla-analysis/analyses/` | 试点样例 |

### 5.2 关键决策记录

| 决策 | 原因 |
|------|------|
| 默认模型改为 glm-5 | 速度 ~33 tokens/s，中文优秀 |
| Claude Code 接百炼 Coding Plan | 官方支持，qwen3.5-plus ~64 tokens/s |
| 郑双佳最高优先级 | 研究方向完全匹配，需完整追踪 |
| Gitee 而非自建 | 轻量化、稳定、免费 |

### 5.3 待决策事项 (Phase 2 处理)

- [ ] 是否启用 Feishu 机器人通知新论文
- [ ] 是否设置定时 cron 自动检索
- [ ] 是否邀请合作者共同维护

---

**文档编制**: AI Assistant  
**审核状态**: Phase 1 正式通过  
**下一步**: 创建 Gitee 仓库，启动 Phase 2
