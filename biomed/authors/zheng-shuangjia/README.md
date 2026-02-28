# 郑双佳作者档案 - 目录结构说明

```
research-kb/biomed/authors/zheng-shuangjia/
│
├── profile/                    # 作者基本信息
│   ├── profile.md              # 研究者主档案 (当前文件)
│   ├── keywords.yaml           # 研究方向关键词集合
│   └── publications.bib        # [待生成] BibTeX 文献库
│
├── papers/                     # 论文详细分析
│   ├── full-analysis/          # 完整深度分析 (高度相关)
│   │   └── YYYY-title-keywords.md
│   ├── medium-analysis/        # 中等深度分析 (中度相关)
│   │   └── YYYY-title-keywords.md
│   └── minimal-archive/        # 简要归档 (弱相关)
│       └── YYYY-title-keywords.md
│
├── timeline/                   # 研究演变时间线
│   ├── research-timeline.md    # 分期研究路线
│   ├── yearly-summary/         # 年度总结
│   │   ├── 2023-summary.md
│   │   ├── 2024-summary.md
│   │   └── 2025-summary.md
│   └── milestone-papers.md     # 里程碑论文
│
├── topic-map/                  # 主题分布图
│   ├── topic-distribution.md   # 主题分布统计
│   ├── method-evolution.md     # 方法演进图
│   └── keyword-cloud.yaml      # 关键词云数据
│
├── comparisons/                # 横向比较
│   ├── method-comparisons/     # 方法对比
│   ├── performance-benchmarks/ # 性能基准对比
│   └── with-other-researchers/ # 与其他研究者对比
│
└── indices/                    # 各类索引
    ├── all-papers-index.yaml   # 全部论文索引
    ├── by-year-index.yaml      # 按年份索引
    ├── by-topic-index.yaml     # 按主题索引
    ├── by-collaborator-index.yaml  # 按合作者索引
    └── citation-network.yaml   # 引用网络
```

## 使用说明

### 新增论文流程
1. **识别** → 通过名称+机构+主题确认是郑双佳论文
2. **评估相关性** → 计算与我研究的相关性得分
3. **分层处理**:
   - 相关性 ≥ 0.7 → `full-analysis/` + 主题知识库
   - 相关性 [0.4, 0.7) → `medium-analysis/` + 主题知识库索引
   - 相关性 < 0.4 → `minimal-archive/` (仅作者档案)
4. **更新索引** → 更新 `indices/` 下的所有相关索引

### 维护频率
- **新论文**: 即时处理
- **索引更新**: 每周一次
- **时间线更新**: 每季度一次
- **全面 review**: 每年一次

### 关联规则
- 主存放位置: `papers/` (本目录)
- 主题线索引: `research-kb/biomed/{topic}/analyses/` (软链接或引用)
- 避免重复存储，使用相对路径引用
