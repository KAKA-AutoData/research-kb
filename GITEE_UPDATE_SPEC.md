# Gitee 持续更新与写入规则说明

> **版本**: v1.0  
> **日期**: 2026-02-28  
> **阶段**: Phase 1 - Gitee 持续更新机制标准化  
> **性质**: 知识库从"规则制定"到"长期运行"的落地规范

---

## 一、总体目标

### 1.1 为什么需要 Gitee 持续更新机制

| 问题 | 无持续更新的局限 | Gitee 持续更新的解决 |
|------|-----------------|---------------------|
| 成果停留在本地 | 临时结果，易丢失 | 结构化积累，长期保存 |
| 目录混乱 | 随意存放，难以检索 | 清晰结构，一致命名 |
| 仓库臃肿 | 堆积大文件和残留 | 轻量设计，避免冗余 |
| 质量下降 | 自动过程失控 | 门槛控制，质量保证 |
| 无法复用 | 零散碎片 | 支持检索、导航、写作、选题 |

### 1.2 核心作用

1. **高质量写入**: 将结构化、正式化的研究成果持续入库
2. **长期积累**: 使知识库能够长期稳定运行
3. **结构稳定**: 保持目录清晰、命名一致
4. **轻量维护**: 避免堆积大文件和中间残留
5. **功能支持**: 支持检索、导航、写作、选题支持和路线回顾
6. **质量控制**: 防止自动更新导致目录失控或质量下降

### 1.3 黄金标准

> **一个合格的 Gitee 持续更新流程，应该确保：**
> - 每一条入库内容都经过质量门槛检查
> - 目录结构始终清晰，命名始终一致
> - 仓库体积控制在合理范围 (<50MB)
> - 索引系统始终有效，支持快速检索
> - 历史演化痕迹可追溯，无重复版本
> - 自动化不牺牲质量，便利不破坏规则

---

## 二、写入对象范围

### 2.1 应写入的内容 (白名单)

| 类别 | 具体内容 | 格式 | 存储位置 |
|------|---------|------|---------|
| **单篇分析** | 完整深度分析 | `.md` | `biomed/papers/full-analysis/` |
| | 中等深度分析 | `.md` | `biomed/papers/medium-analysis/` |
| | 轻量归档 | `.yaml` | `biomed/papers/minimal-archive/` |
| **横向比较** | 轻量比较笔记 | `.md` | `_comparisons/lightweight/` |
| | 中等比较报告 | `.md` | `_comparisons/medium/` |
| | 深度比较报告 | `.md` | `_comparisons/deep/` |
| **趋势分析** | 小型趋势总结 | `.md` | `_trends/small/` |
| | 中型专题趋势 | `.md` | `_trends/medium/` |
| | 阶段性全局报告 | `.md` | `_trends/comprehensive/` |
| **空白分析** | 研究空白分析 | `.md` | `_gaps/` |
| **作者档案** | 郑双佳完整档案 | `.md/.yaml` | `biomed/authors/zheng-shuangjia/` |
| **索引文件** | 全局论文索引 | `.yaml` | `_indices/global-paper-index.yaml` |
| | 主题索引 | `.yaml` | `_indices/topic-*.yaml` |
| | 作者索引 | `.yaml` | `_indices/author-*.yaml` |
| | DeepSeek索引 | `.yaml` | `_indices/deepseek-*.yaml` |
| **模板Schema** | 8个YAML模板 | `.yaml` | `_meta/*.yaml` |
| **规范文档** | 所有SPEC文档 | `.md` | 根目录或`_meta/` |
| **阶段性报告** | 月度/季度/年度报告 | `.md` | `_reports/` |

### 2.2 不应直接写入的内容 (黑名单)

| 类别 | 示例 | 处理方式 |
|------|------|---------|
| **PDF原文** | `paper.pdf` | 存外部云盘，metadata中记录链接 |
| **大体积压缩包** | `dataset.zip` | 存外部云盘，metadata中记录链接 |
| **原始日志** | `training.log` | 本地保留，关键信息提取后入正文 |
| **下载缓存** | `arxiv_cache/` | 定期清理，不入仓库 |
| **中间临时文件** | `draft_v1.md`, `temp.yaml` | 本地处理，正式版才入库 |
| **重复草稿** | 多个版本的草稿 | 合并为正式版后入库 |
| **低质量未验收** | 未完成分析 | 完成后再入库 |
| **碎片化半成品** | 零散笔记 | 整理成正式条目后入库 |

### 2.3 外部存储策略

```
大文件/二进制文件:
├── PDF原文 → 阿里云盘/坚果云
├── 数据集 → Kaggle/Google Drive
├── 代码仓库 → GitHub
└── 图表大图 → 图床/OSS

metadata中记录:
external_resources:
  pdf_url: "https://..."
  code_repo: "github.com/..."
  dataset_url: "https://..."
```

---

## 三、写入前的质量门槛

### 3.1 八项检查清单

每一条内容在写入 Gitee 前必须通过以下检查：

| # | 检查项 | 检查方法 | 失败处理 |
|---|--------|---------|---------|
| 1 | **符合研究边界** | 对照 `RESEARCH_SCOPE_DEFINITION.md` | 拒绝写入 |
| 2 | **符合优先级体系** | P0/P1/P2/P3 判定正确 | 降级或拒绝 |
| 3 | **按模板完成** | 对照对应 SPEC 模板检查 | 补充完善 |
| 4 | **Metadata完整** | 检查必填字段 | 补充缺失 |
| 5 | **命名符合规则** | 对照命名规范 | 重命名 |
| 6 | **正式版本** | 非草稿状态 | 标记为正式 |
| 7 | **无重复条目** | 查重检查 | 合并或跳过 |
| 8 | **值得长期保留** | 质量评估 | 本地保留或丢弃 |

### 3.2 自动检查脚本框架

```bash
#!/bin/bash
# pre-commit-check.sh

entry_file=$1
entry_type=$2  # paper/comparison/trend/etc.

echo "=== Gitee 写入前检查 ==="

# 1. 研究边界检查
echo "[1/8] 检查研究边界..."
if ! check_scope "$entry_file"; then
    echo "❌ 不符合研究边界"
    exit 1
fi

# 2. 优先级检查
echo "[2/8] 检查优先级..."
if ! check_priority "$entry_file"; then
    echo "❌ 优先级判定错误"
    exit 1
fi

# 3. 模板合规检查
echo "[3/8] 检查模板合规性..."
if ! check_template "$entry_file" "$entry_type"; then
    echo "❌ 不符合模板要求"
    exit 1
fi

# 4. Metadata完整性检查
echo "[4/8] 检查 metadata..."
missing_fields=$(check_metadata "$entry_file")
if [ -n "$missing_fields" ]; then
    echo "❌ Metadata 缺失: $missing_fields"
    exit 1
fi

# 5. 命名规范检查
echo "[5/8] 检查命名规范..."
if ! check_naming "$entry_file"; then
    echo "❌ 命名不符合规范"
    exit 1
fi

# 6. 草稿状态检查
echo "[6/8] 检查是否为正式版本..."
if is_draft "$entry_file"; then
    echo "⚠️  仍为草稿状态，是否继续? (y/n)"
    read confirm
    if [ "$confirm" != "y" ]; then
        exit 1
    fi
fi

# 7. 重复检查
echo "[7/8] 检查重复条目..."
duplicate=$(check_duplicate "$entry_file")
if [ -n "$duplicate" ]; then
    echo "⚠️  发现潜在重复: $duplicate"
    echo "处理方式: (s)kip/(m)erge/(r)eplace?"
    read action
    case $action in
        s) exit 0 ;;
        m) merge_entries "$entry_file" "$duplicate" ;;
        r) replace_entry "$entry_file" "$duplicate" ;;
    esac
fi

# 8. 质量评估
echo "[8/8] 质量评估..."
quality_score=$(assess_quality "$entry_file")
if [ "$quality_score" -lt 6 ]; then
    echo "⚠️  质量评分较低 ($quality_score/10)，建议本地保留"
    exit 1
fi

echo "✅ 所有检查通过，可以写入 Gitee"
```

### 3.3 质量评分标准

| 维度 | 权重 | 评分标准 |
|------|------|---------|
| 完整性 | 25% | 必填章节是否齐全 |
| 深度 | 25% | 分析是否触及本质 |
| 结构化 | 20% | 是否符合模板 |
| 可检索 | 15% | metadata是否完整 |
| 价值 | 15% | 对我研究的实际帮助 |

**评分等级**:
- 9-10分: 优秀，立即入库
- 7-8分: 良好，可以入库
- 5-6分: 及格，建议完善后入库
- <5分: 不合格，本地保留或丢弃

---

## 四、写入策略

### 4.1 三种写入方式

#### 方式1: 单条写入 (Single Entry)

**适用场景**:
- 新增单篇高优先级论文 (P0/P1)
- 新增单个重要比较报告
- 新增紧急趋势报告
- 修正关键错误

**执行流程**:
```bash
# 1. 质量检查
./pre-commit-check.sh new-paper.md paper

# 2. 确定存储位置
# 根据 metadata 中的 priority 和 topic

# 3. 写入文件
cp new-paper.md research-kb/biomed/papers/full-analysis/

# 4. 更新索引
python update_indices.py --add new-paper.md

# 5. Git提交
git add .
git commit -m "Add: 2025-zheng-latent-diffusion-molecules (P0)"
git push origin main
```

**注意事项**:
- 每次只处理一个条目
- 必须同步更新相关索引
- Commit message 必须包含条目标识

#### 方式2: 批量写入 (Batch Update)

**适用场景**:
- 一轮补档完成后的集中更新 (5-10篇)
- 一轮横向比较后的集中入库
- 一轮阶段性总结后的批量更新

**执行流程**:
```bash
# 1. 准备批量清单
cat > batch-list.txt << EOF
paper1.md
paper2.md
paper3.md
EOF

# 2. 批量质量检查
for file in $(cat batch-list.txt); do
    ./pre-commit-check.sh "$file" paper || exit 1
done

# 3. 批量写入
python batch_import.py --list batch-list.txt --target biomed/papers/

# 4. 批量更新索引
python update_indices.py --batch batch-list.txt

# 5. Git提交
git add .
git commit -m "Batch: Add 8 papers from drug-discovery theme (Week 3)"
git push origin main
```

**注意事项**:
- 同一批次的条目应有内在关联 (同主题/同作者/同期)
- Commit message 应概括批次特征
- 批次大小控制在 10-20 个条目以内

#### 方式3: 增量更新 (Incremental Update)

**适用场景**:
- 已有条目的补充分析
- Metadata 修订
- 分析深度升级
- 标签补充

**执行流程**:
```bash
# 场景: 将轻量归档升级为中等分析

# 1. 创建新版本
cp minimal-archive/old-entry.yaml medium-analysis/old-entry.md
# 编辑扩充内容...

# 2. 标记关系
# 在新文件中添加:
# predecessor: "minimal-archive/old-entry.yaml"
# upgrade_reason: "Found direct relevance to my work"

# 3. 更新原文件
# 在原文件中添加:
# superseded_by: "medium-analysis/old-entry.md"
# status: "upgraded"

# 4. 更新索引
python update_indices.py --update old-entry

# 5. Git提交
git add .
git commit -m "Upgrade: 2024-smith-baseline-method (light→medium, relevance 0.3→0.6)"
git push origin main
```

**注意事项**:
- 保留历史版本，建立演进关系
- 明确标注升级原因
- 同步更新所有相关索引

### 4.2 写入方式选择决策树

```
新内容准备就绪
    ↓
数量?
├── 1篇 ──► 是否P0/P1或紧急? ──Yes──► 单条写入
│           └── No ──► 暂存，等待批量
├── 2-10篇 ──► 是否有共同特征? ──Yes──► 批量写入
│              └── No ──► 拆分为多个单条
└── 10+篇 ──► 分批处理，每批5-10篇

已有内容修改
    ↓
类型?
├── 补充分析 ──► 增量更新
├── Metadata修订 ──► 增量更新
├── 深度升级 ──► 增量更新 (保留历史)
└── 错误修正 ──► 视情况单条或批量
```

---

## 五、更新频率与节奏

### 5.1 推荐节奏

| 频率 | 动作 | 触发条件 | 最大条目数 |
|------|------|---------|-----------|
| **即时** | 单条写入 | P0论文/紧急修正 | 1 |
| **每日** | 小批量写入 | 当日完成2-5篇 | 5 |
| **每周** | 周度批量 | 本周完成5-15篇 | 15 |
| **每月** | 月度汇总 | 月度总结时 | 30 |
| **每季** | 季度归档 | 季度报告后 | 100 |

### 5.2 平衡策略

**高频 vs 稳定的平衡**:
```
原则: 质量优先，稳定次之，频率最后

具体策略:
├── P0/P1 高优先级: 即时单条写入
├── P2 中优先级: 日度或周度批量
├── P3 辅助级: 周度或月度批量
├── 索引更新: 随内容更新同步
├── 规范文档: 月度或按需更新
└── 大规模重构: 季度规划后执行
```

**避免碎片更新**:
- 单次 Commit 至少包含一个有意义的变更
- 避免 "fix typo" 类的单独 Commit
- 相关变更应合并为一个 Commit

### 5.3 固定节奏示例

```markdown
## 周一
- 审查周末完成的分析
- 批量写入上周积累的 P2/P3 条目
- 更新主题索引

## 周三
- 检查是否有达到比较阈值的主题
- 如有，执行横向比较
- 比较报告写入

## 周五
- 周度总结
- 更新周度报告
- 推送本周所有变更

## 月末
- 月度统计
- 更新月度趋势跟踪
- 检查是否需要触发趋势报告

## 季末
- 季度全面回顾
- 更新所有全局索引
- 生成季度趋势报告
```

---

## 六、重复与版本控制规则

### 6.1 重复检测机制

#### 检测维度

| 维度 | 检测方法 | 工具 |
|------|---------|------|
| **DOI/arXiv ID** | 精确匹配 | `grep` |
| **标题相似度** | Levenshtein距离 | Python `difflib` |
| **作者+年份** | 组合匹配 | SQL查询 |
| **内容指纹** | MinHash/ SimHash | `datasketch` |

#### 检测流程

```python
def check_duplicate(new_entry):
    """检查新条目是否与已有条目重复"""
    
    # 1. ID精确匹配
    if new_entry.doi in existing_dois:
        return DuplicateResult(
            type="exact",
            existing=find_by_doi(new_entry.doi),
            action="skip"
        )
    
    # 2. 标题相似度
    for existing in all_entries:
        similarity = title_similarity(new_entry.title, existing.title)
        if similarity > 0.9:
            return DuplicateResult(
                type="high_similarity",
                existing=existing,
                similarity=similarity,
                action="manual_review"
            )
    
    # 3. 作者+年份匹配
    matches = find_by_authors_year(
        new_entry.authors, 
        new_entry.year
    )
    if len(matches) > 0:
        return DuplicateResult(
            type="potential",
            candidates=matches,
            action="manual_review"
        )
    
    return DuplicateResult(type="new", action="proceed")
```

### 6.2 已有条目补充分析

当需要对已有条目添加补充分析时：

```markdown
## 方案A: 追加模式 (推荐用于小幅补充)

原文件: 2024-author-title.md

追加内容到文件末尾:
```markdown
---

## 补充分析 ([日期])

### 新增发现
[补充内容]

### 更新原因
[为什么现在补充]
```

Git记录:
```bash
git commit -m "Update: 2024-author-title (add experiment analysis)"
```
```

```markdown
## 方案B: 分离模式 (推荐用于大幅补充)

创建补充文件: 2024-author-title-supplement.md

原文件添加引用:
```markdown
supplements:
  - "2024-author-title-supplement.md"
```

Git记录:
```bash
git commit -m "Add: supplement to 2024-author-title (theory notes)"
```
```

### 6.3 分析深度升级

从轻量 → 中等 → 完整的升级路径：

```
升级前: minimal-archive/2024-author-title.yaml
    ↓
升级后: medium-analysis/2024-author-title.md
    ↓
(可选) 进一步升级: full-analysis/2024-author-title.md
```

**升级规则**:
1. 保留原文件，标记为 `status: upgraded`
2. 新文件添加 `predecessor` 指向原文件
3. 更新索引，确保指向最新版本
4. Commit message 明确说明升级路径

```yaml
# 原文件 (minimal-archive/2024-author-title.yaml)
paper_id: "doi:10.xxx"
status: "upgraded"
superseded_by: "medium-analysis/2024-author-title.md"
upgrade_date: "2026-03-15"
upgrade_reason: "Relevance increased from 0.3 to 0.6 after deeper reading"

# 新文件 (medium-analysis/2024-author-title.md)
---
predecessor: "minimal-archive/2024-author-title.yaml"
upgrade_date: "2026-03-15"
upgrade_trigger: "Found direct application to my DTI prediction problem"
---

[完整分析内容...]
```

### 6.4 Metadata 修订

Metadata 修订应采用 **追加模式**，保留历史：

```yaml
# metadata 历史记录
metadata_history:
  - date: "2026-02-28"
    revision: "initial"
    fields:
      relevance_score: 0.5
      
  - date: "2026-03-15"
    revision: "updated"
    reason: "After re-reading method section"
    changes:
      relevance_score: 0.7
      tags_added: ["latent-diffusion"]
```

### 6.5 正式主文件 vs 增量补充文件

| 类型 | 定义 | 命名 | 存储 |
|------|------|------|------|
| **正式主文件** | 当前有效版本 | `YYYY-author-title.md` | 主目录 |
| **历史版本** | 被升级的版本 | 保留原名，标记status | 原位置 |
| **补充文件** | 追加的分析 | `YYYY-author-title-supplement-N.md` | 同级目录 |
| **勘误文件** | 错误修正记录 | `YYYY-author-title-errata.md` | 同级目录 |

---

## 七、作者线与主题线的写入规则

### 7.1 写入原则

```
核心原则: 单一主存储 + 多维度索引

├── 作者线: 郑双佳论文全部进入 author 目录
├── 主题线: 通过索引引用，不重复存储正文
└── 保证: 作者线完整、主题线清晰、仓库不冗余
```

### 7.2 郑双佳作者线写入规则

```markdown
## 规则1: 全收录
所有郑双佳论文必须进入:
biomed/authors/zheng-shuangjia/papers/

## 规则2: 分层存储
根据相关性分层:
├── full-analysis/     # relevance ≥ 0.7
├── medium-analysis/   # relevance 0.4-0.7
└── minimal-archive/   # relevance < 0.4

## 规则3: 额外字段
郑双佳论文必须包含特定字段:
```yaml
zheng_shuangjia_specific:
  is_core_work: true/false
  research_phase: "phase-3-generative"
  related_papers: ["doi:xxx", "arxiv:xxx"]
  roadmap_entry: true
```

## 规则4: 主题索引关联
在主题索引中只存引用:
```yaml
# _indices/topic-drug-discovery.yaml
papers:
  - ref: "biomed/authors/zheng-shuangjia/papers/full-analysis/2025-zheng-xxx.md"
    relevance_to_topic: 0.9
```
```

### 7.3 主题线写入规则

```markdown
## 规则1: 正文不重复
主题目录下不存储论文正文，只存:
├── indices/          # 索引文件
├── comparisons/      # 横向比较报告
├── trends/           # 趋势报告
└── gaps/             # 空白分析

## 规则2: 索引引用
索引使用相对路径引用:
```yaml
# _indices/topic-rna-therapeutics.yaml
papers:
  - path: "../../authors/zheng-shuangjia/papers/full-analysis/2025-zheng-rna.md"
    topic_relevance: 0.85
    notes: "Key paper on RNA structure prediction"
```

## 规则3: 聚合信息
主题索引可存储聚合分析:
```yaml
topic_summary:
  total_papers: 45
  dominant_method: "Transformer-based"
  emerging_trend: "3D structure modeling"
  key_gaps: ["long RNA modeling", "delivery optimization"]
```
```

### 7.4 防冗余检查

```python
def check_redundancy_before_write(entry, target_path):
    """写入前检查是否会造成冗余"""
    
    # 1. 检查正文是否已存在
    if entry.has_full_text() and target_path.startswith("_indices/"):
        raise RedundancyError(
            "索引目录不应存储完整正文"
        )
    
    # 2. 检查是否重复拷贝
    existing_locations = find_all_copies(entry.paper_id)
    if len(existing_locations) > 0:
        raise RedundancyError(
            f"该论文已存在于: {existing_locations}"
        )
    
    # 3. 检查引用是否正确
    if target_path.startswith("biomed/authors/"):
        # 作者目录可以存正文
        pass
    elif target_path.startswith("_indices/"):
        # 索引目录只能存引用
        if entry.content_size > 1000:  # 假设阈值
            raise RedundancyError(
                "索引条目过大，应使用引用而非全文"
            )
    
    return True
```

---

## 八、索引与导航更新规则

### 8.1 索引体系架构

```
_indices/
├── global/                    # 全局索引
│   ├── all-papers.yaml       # 全部论文总索引
│   ├── by-year.yaml          # 按年份索引
│   └── by-priority.yaml      # 按优先级索引
├── topics/                    # 主题索引
│   ├── topic-drug-discovery.yaml
│   ├── topic-rna-therapeutics.yaml
│   └── topic-attention-mechanisms.yaml
├── authors/                   # 作者索引
│   ├── author-zheng-shuangjia.yaml
│   └── author-deepseek-team.yaml
├── deepseek/                  # DeepSeek专项
│   ├── deepseek-mla.yaml
│   ├── deepseek-moe.yaml
│   └── deepseek-all.yaml
├── usage/                     # 用途索引
│   ├── for-introduction.yaml
│   ├── for-related-work.yaml
│   └── for-methods.yaml
└── theory/                    # 理论索引
    ├── theory-representation.yaml
    ├── theory-low-rank.yaml
    └── theory-relation.yaml
```

### 8.2 索引更新触发条件

| 操作 | 必须更新的索引 | 可选更新 |
|------|---------------|---------|
| 新增论文 | 全局、主题、作者、年份、优先级 | 用途、理论 |
| 新增比较 | 全局、主题 | 作者 |
| 新增趋势 | 全局、主题 | - |
| 升级分析深度 | 全局、主题、作者 | 优先级 |
| 修订metadata | 全局、主题、作者、优先级、用途、理论 | - |
| 删除条目 | 所有包含该条目的索引 | - |

### 8.3 索引更新脚本

```python
# update_indices.py

def update_indices(entry_file, operation="add"):
    """更新所有相关索引"""
    
    entry = load_entry(entry_file)
    
    # 1. 更新全局索引
    update_global_index(entry, operation)
    
    # 2. 更新主题索引
    for topic in entry.topics:
        update_topic_index(topic, entry, operation)
    
    # 3. 更新作者索引
    for author in entry.authors:
        update_author_index(author, entry, operation)
    
    # 4. 更新年份索引
    update_year_index(entry.year, entry, operation)
    
    # 5. 更新优先级索引
    update_priority_index(entry.priority, entry, operation)
    
    # 6. 更新用途索引 (如果指定了写作用途)
    if entry.writing_usage:
        for usage in entry.writing_usage:
            update_usage_index(usage, entry, operation)
    
    # 7. 更新理论索引 (如果涉及理论)
    if entry.theory_tags:
        for theory in entry.theory_tags:
            update_theory_index(theory, entry, operation)
    
    print(f"✅ 索引更新完成: {entry.paper_id}")


def update_global_index(entry, operation):
    """更新全局论文索引"""
    index_file = "_indices/global/all-papers.yaml"
    index = load_yaml(index_file)
    
    if operation == "add":
        index["papers"][entry.paper_id] = {
            "path": entry.relative_path,
            "title": entry.title,
            "authors": entry.authors,
            "year": entry.year,
            "priority": entry.priority,
            "last_updated": datetime.now().isoformat()
        }
    elif operation == "remove":
        del index["papers"][entry.paper_id]
    
    save_yaml(index_file, index)
```

### 8.4 索引有效性检查

```bash
#!/bin/bash
# check-index-validity.sh

echo "=== 索引有效性检查 ==="

# 1. 检查死链
for index in _indices/**/*.yaml; do
    echo "检查: $index"
    python check_dead_links.py "$index"
done

# 2. 检查重复条目
python check_duplicate_entries.py _indices/

# 3. 检查metadata一致性
python check_metadata_consistency.py _indices/

# 4. 生成索引健康报告
echo "生成健康报告..."
python generate_index_report.py > _indices/health-report.md
```

---

## 九、轻量化与空间控制

### 9.1 体积限制策略

| 层级 | 限制 | 措施 |
|------|------|------|
| **单文件** | <100KB | 正文精简，图表外链 |
| **单目录** | <100个文件 | 按年份/子主题细分 |
| **总仓库** | <50MB | 严格控制大文件 |

### 9.2 避免冗余拷贝

```python
def deduplication_strategy():
    """去重策略"""
    
    # 1. 正文唯一存储
    rules = {
        "paper_full_analysis": "只在 authors/ 或 papers/ 存一次",
        "paper_medium": "同上",
        "paper_minimal": "YAML格式，极简存储",
        "comparison_report": "只在 _comparisons/ 存一次",
        "trend_report": "只在 _trends/ 存一次",
    }
    
    # 2. 其他位置使用引用
    reference_format = {
        "topic_index": "path + relevance + notes",
        "author_index": "path + author_relevance + relation",
        "global_index": "path + basic_metadata",
    }
    
    # 3. 禁止行为
    forbidden = [
        "在多个主题目录复制同一篇正文",
        "在索引中存储完整分析内容",
        "保留多个版本的完整副本",
    ]
```

### 9.3 外部资源引用

```yaml
# 在 metadata 中引用外部资源
external_resources:
  pdf:
    url: "https://aliyundrive.com/xxx"
    size: "2.3MB"
    md5: "abc123..."
  
  code:
    repo: "https://github.com/author/project"
    commit: "a1b2c3d"
    license: "MIT"
  
  data:
    url: "https://huggingface.co/datasets/xxx"
    description: "Processed version used in paper"
  
  figures:
    - name: "Figure1-architecture.png"
      url: "https://imgur.com/xxx"
      caption: "Model architecture"
```

### 9.4 本地清理策略

```bash
#!/bin/bash
# local-cleanup.sh

echo "=== 本地清理 ==="

# 1. 清理已入库的临时文件
echo "清理已入库的临时文件..."
find inbox/ -name "*.md" -o -name "*.yaml" | while read file; do
    if is_in_repository "$file"; then
        rm "$file"
        echo "  删除: $file"
    fi
done

# 2. 清理旧缓存
echo "清理缓存..."
rm -rf cache/downloads/*
rm -rf cache/temp/*

# 3. 压缩旧日志
echo "压缩日志..."
gzip logs/*.log

# 4. 生成清理报告
echo "生成清理报告..."
du -sh research-kb/
du -sh inbox/
du -sh cache/
```

---

## 十、写入后的标准产出

### 10.1 更新记录模板

每次写入完成后自动生成：

```markdown
# Gitee 更新记录

**更新时间**: 2026-03-15 14:30  
**更新者**: AI Assistant  
**Commit**: a1b2c3d

## 统计摘要

| 指标 | 数值 |
|------|------|
| 新增条目 | 8 |
| 更新条目 | 3 |
| 删除条目 | 0 |
| 升级条目 | 2 |

## 详细清单

### 新增条目

| # | 类型 | 标识 | 位置 | 优先级 |
|---|------|------|------|--------|
| 1 | 论文 | 2025-zheng-latent-diffusion | biomed/papers/full-analysis/ | P0 |
| 2 | 论文 | 2024-smith-baseline | biomed/papers/medium-analysis/ | P1 |
| ... | ... | ... | ... | ... |

### 更新条目

| # | 标识 | 更新内容 | 原因 |
|---|------|---------|------|
| 1 | 2024-author-title | 补充实验分析 | 重新阅读后发现遗漏 |
| ... | ... | ... | ... |

### 升级条目

| # | 标识 | 升级路径 | 触发原因 |
|---|------|---------|---------|
| 1 | 2024-old-paper | minimal → medium | 相关性提升 |
| ... | ... | ... | ... |

## 涉及范围

### 主题
- drug-discovery (+3)
- rna-therapeutics (+2)
- attention-mechanisms (+3)

### 作者线
- zheng-shuangjia (+5)
- smith-lab (+2)
- deepseek-team (+1)

### 索引更新
- [x] 全局索引
- [x] 主题索引 (3个)
- [x] 作者索引 (2个)
- [x] 年份索引
- [x] 优先级索引

## 需关注事项

1. **主题接近阈值**: drug-discovery 已达 18篇，再增2篇触发中型比较
2. **作者线进展**: 郑双佳论文达 32篇，建议启动趋势分析
3. **待升级条目**: 有 5 篇轻量归档可考虑升级

## 下一步建议

- [ ] 继续补档 drug-discovery 方向 (目标: 20篇触发比较)
- [ ] 启动郑双佳作者线趋势分析
- [ ] 审查并升级 5 篇候选轻量归档
```

### 10.2 自动生成命令

```bash
# 写入后自动生成更新记录
generate_update_report() {
    local commit_hash=$1
    local prev_commit=$2
    
    cat > _updates/update-$(date +%Y%m%d-%H%M).md << EOF
# Gitee 更新记录

**更新时间**: $(date '+%Y-%m-%d %H:%M')  
**Commit**: $commit_hash

## 变更统计
$(git diff --stat $prev_commit $commit_hash)

## 详细变更
$(git log --oneline $prev_commit..$commit_hash)

## 索引状态
$(python check_index_status.py)
EOF
}
```

---

## 十一、失败与异常处理

### 11.1 常见问题处理

| 问题 | 检测方法 | 处理方案 |
|------|---------|---------|
| **命名冲突** | 预提交检查 | 自动重命名或人工确认 |
| **重复条目** | DOI/标题匹配 | 跳过/合并/替换 |
| **Metadata缺失** | Schema验证 | 返回补充 |
| **分析不完整** | 模板检查 | 标记为草稿，不入库 |
| **目录路径错误** | 路径验证 | 自动纠正或报错 |
| **索引未同步** | 索引一致性检查 | 自动补更新 |
| **空间不足** | 磁盘检查 | 暂停写入，清理后重试 |
| **写入中断** | 事务日志 | 回滚或断点续传 |

### 11.2 异常处理流程

```python
def safe_write(entry, target_path):
    """安全写入，带异常处理"""
    
    try:
        # 1. 预检查
        validate_before_write(entry, target_path)
        
        # 2. 创建备份
        backup_if_exists(target_path)
        
        # 3. 执行写入
        write_entry(entry, target_path)
        
        # 4. 更新索引
        update_indices(entry, "add")
        
        # 5. 验证写入
        verify_write(target_path)
        
        # 6. Git提交
        git_commit(entry)
        
        return Success(target_path)
        
    except ValidationError as e:
        logger.error(f"验证失败: {e}")
        return Failure("validation", e)
        
    except DuplicateError as e:
        logger.warning(f"发现重复: {e}")
        return handle_duplicate(entry, e.existing)
        
    except DiskFullError:
        logger.error("磁盘空间不足")
        notify_admin("Disk full, write paused")
        return Failure("disk_full", None)
        
    except Exception as e:
        logger.exception("未知错误")
        rollback_backup(target_path)
        return Failure("unknown", e)
```

---

## 十二、长期维护原则

### 12.1 核心原则

```
1. 清晰 > 便利
   目录结构始终优先于临时方便

2. 质量 > 数量
   自动化不能牺牲条目质量

3. 稳定 > 频繁
   避免过度频繁的碎片更新

4. 高价值优先
   P0/P1内容优先保障入库

5. 轻量持续
   严格控制仓库体积，及时清理
```

### 12.2 定期维护任务

| 频率 | 任务 | 负责人 |
|------|------|--------|
| **每周** | 索引一致性检查 | 自动化 |
| **每月** | 死链清理 | 自动化 |
| **每季** | 仓库体积审计 | 人工+自动化 |
| **每年** | 规范文档更新 | 人工 |
| **按需** | 大规模重构 | 人工规划 |

### 12.3 质量红线

以下内容**不得**进入 Gitee:
- ❌ 未经质量检查的条目
- ❌ 低质量、低相关的P3条目堆积
- ❌ 重复拷贝的正文
- ❌ 大体积二进制文件
- ❌ 临时草稿和半成品
- ❌ 与研究边界无关的内容

---

## 十三、下一步建议

### 13.1 Phase 1 圆满收官

**已完成全部规则层文档** (~280KB):
```
research-kb/
├── RESEARCH_SCOPE_DEFINITION.md      # 研究范围定义
├── PAPER_ANALYSIS_SPEC.md            # 单篇分析规范
├── COMPARISON_RULES_SPEC.md          # 横向比较机制
├── TREND_GAP_ANALYSIS_SPEC.md        # 趋势与空白分析
├── GITEE_UPDATE_SPEC.md              # Gitee持续更新 ⭐ 新增
├── WORKFLOW_SPEC.md                   # 工作规范说明
├── GITEE_REPO_DESIGN.md               # 仓库结构设计
├── _meta/                             # 8个模板文件
└── biomed/authors/zheng-shuangjia/    # 郑双佳档案
```

### 13.2 首轮试运行方案

> **建议的首轮试运行方式**

**Step 1: 仓库初始化** (Day 1)
```bash
cd /root/openclaw-workspace/research-kb
git init
git add .
git commit -m "Phase 1: Complete research knowledge base framework v1.0"
# 推送到 Gitee
```

**Step 2: 首批内容入库** (Day 1-3)
- 选择 5-10 篇已分析的论文
- 覆盖不同优先级 (P0, P1, P2)
- 覆盖不同主题 (BioMed, DeepSeek)
- 使用批量写入方式

**Step 3: 索引验证** (Day 3)
- 运行索引有效性检查
- 验证检索功能
- 修复发现的问题

**Step 4: 流程调优** (Day 4-7)
- 根据首批经验调整写入脚本
- 优化质量检查流程
- 完善异常处理

**Step 5: 正式运行** (Week 2+)
- 按既定节奏持续更新
- 每周回顾，每月总结
- 季度全面审计

### 13.3 Phase 2 展望

**Month 1**:
- 完成首批 20-30 篇论文入库
- 第一次轻量横向比较
- 第一次小型趋势总结

**Month 2-3**:
- 累计 60+ 篇论文
- 第一份中型专题趋势报告
- 第一份正式研究空白分析

**Month 6**:
- 累计 150+ 篇论文
- 第一份阶段性全局趋势报告
- 支持第一个正式选题论证

---

## 附录: 快速参考卡

### A.1 写入检查速查

```
✅ 符合研究边界
✅ 优先级判定正确
✅ 模板合规
✅ Metadata完整
✅ 命名规范
✅ 正式版本
✅ 无重复
✅ 质量达标
```

### A.2 写入方式速查

```
单条: P0/P1/紧急/修正
批量: 5-10篇有关联的条目
增量: 补充/修订/升级
```

### A.3 索引更新速查

```
新增论文 → 全局+主题+作者+年份+优先级
新增比较 → 全局+主题
修订metadata → 所有相关索引
```

### A.4 质量红线

```
❌ 未经检查直接写入
❌ 重复存储正文
❌ 索引不同步
❌ 大文件入库
❌ 低质量P3堆积
```

---

**文档编制**: AI Assistant  
**审核状态**: Phase 1 全部完成  
**生效日期**: 2026-02-28  
**里程碑**: 研究知识基线框架 v1.0 正式发布
