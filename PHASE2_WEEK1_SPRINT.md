# Phase 2 Week 1 冲刺计划

> **目标**: 一周内完成首批入库 + 第一次横向比较 + 小型趋势总结
> **时间**: 2026-03-01 至 2026-03-07
> **强度**: 最大速度执行

---

## Day 1 (周一): 论文检索与筛选

### Morning - ZSJ论文批量检索
- [ ] Google Scholar 检索: "Shuangjia Zheng" + drug discovery
- [ ] PubMed 检索: "Zheng S"[Author] + AI
- [ ] arXiv 检索: author:"Zheng, Shuangjia"
- [ ] 初步筛选: 20-30 篇候选

### Afternoon - DeepSeek 核心论文
- [ ] DeepSeek-V2/V3 技术报告
- [ ] MLA 相关分析论文 (≥5篇)
- [ ] DeepSeekMoE 相关论文
- [ ] 初步筛选: 10-15 篇候选

### Output
- `inbox/week1-candidates-zheng.yaml` (20-30篇)
- `inbox/week1-candidates-deepseek.yaml` (10-15篇)

---

## Day 2 (周二): 优先级判定与快速筛选

### Morning - ZSJ论文分级
- [ ] P0 (≥0.7 relevance): 预计 5-8 篇
- [ ] P1 (0.4-0.7): 预计 8-12 篇  
- [ ] P2/P3 (<0.4): 剩余

### Afternoon - DeepSeek 论文分级
- [ ] P0 (官方/核心机制): 3-5 篇
- [ ] P1 (紧密关联): 5-8 篇
- [ ] P2 (松散关联): 剩余

### Output
- 分级后的候选清单
- 确定首批深度分析目标: 10-15 篇

---

## Day 3 (周三): 完整深度分析 × 5

### Target Papers
- ZSJ P0: 3 篇
- DeepSeek P0: 2 篇

### Output
- `biomed/papers/full-analysis/` +3
- `deepseek/papers/full-analysis/` +2

---

## Day 4 (周四): 中等深度分析 × 5

### Target Papers
- ZSJ P1: 3 篇
- DeepSeek P1: 2 篇

### Output
- `biomed/papers/medium-analysis/` +3
- `deepseek/papers/medium-analysis/` +2

---

## Day 5 (周五): 轻量归档 + 索引更新

### Morning - 轻量归档
- [ ] ZSJ P2/P3: 5-10 篇 → `minimal-archive/`
- [ ] DeepSeek P2: 3-5 篇 → `minimal-archive/`

### Afternoon - 索引构建
- [ ] 全局论文索引
- [ ] 主题索引 (drug-discovery, deepseek-mla)
- [ ] 作者索引 (ZSJ)

### Git Push
```bash
git add .
git commit -m "Week 1: Add 15-20 papers (5 full + 5 medium + 5-10 minimal)"
git push origin main
```

---

## Day 6 (周六): 第一次轻量横向比较

### Trigger
- drug-discovery 主题: ≥10 篇 → 触发轻量比较
- deepseek-mla 主题: ≥5 篇 → 触发轻量比较

### Output
- `_comparisons/lightweight/drug-discovery-20260306.md`
- `_comparisons/lightweight/deepseek-mla-20260306.md`

### Content
- 论文清单表格
- 快速分类 (代表/增量/待深入)
- 关键差异对比
- 最值得深挖的条目

---

## Day 7 (周日): 小型趋势总结 + 复盘

### Morning - 趋势总结
- [ ] Drug Discovery 方向: 小型趋势总结
- [ ] DeepSeek MLA 方向: 小型趋势总结

### Output
- `_trends/small/drug-discovery-trend-20260307.md`
- `_trends/small/deepseek-mla-trend-20260307.md`

### Afternoon - 复盘与规划
- [ ] 统计: 本周入库总数
- [ ] 识别: 研究空白初步判断
- [ ] 规划: Week 2 重点方向

### Final Push
```bash
git add .
git commit -m "Week 1 complete: 20-30 papers, 2 comparisons, 2 trend summaries"
git push origin main
```

---

## 预期产出

| 指标 | 目标 | 实际 |
|------|------|------|
| **完整深度分析** | 5 篇 | ___ |
| **中等深度分析** | 5 篇 | ___ |
| **轻量归档** | 10-15 篇 | ___ |
| **总计入库** | 20-30 篇 | ___ |
| **横向比较** | 2 份 | ___ |
| **趋势总结** | 2 份 | ___ |
| **Git Commits** | 3-5 次 | ___ |

---

## 成功标准

- [ ] 20+ 篇论文正式入库
- [ ] 2 个主题达到比较阈值
- [ ] 完成 2 份轻量横向比较
- [ ] 完成 2 份小型趋势总结
- [ ] 所有变更推送到 GitHub

---

**开始时间**: 2026-03-01 08:00  
**结束时间**: 2026-03-07 18:00  
**总时长**: 7 天  
**状态**: 🚀 准备就绪
