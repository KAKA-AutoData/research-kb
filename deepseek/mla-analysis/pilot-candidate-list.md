# DeepSeek/MLA 试点运行 - 候选入库列表

**试点主题**: DeepSeek Multi-Head Latent Attention (MLA)  
**检索日期**: 2026-02-28  
**检索源**: arXiv + DeepSeek 官方  
**试点目的**: 验证元数据模板和工作流程

---

## 候选论文列表

### 【P0 - 必收】DeepSeek 官方技术报告

| 序号 | 论文 | arXiv ID | 发表日期 | 相关性 | 优先级 | 决策 |
|------|------|----------|----------|--------|--------|------|
| 1 | DeepSeek-V3 Technical Report | 2412.19437 | 2024-12 | P0 | 最高 | ✅ ACCEPT |
| 2 | DeepSeek LLM: Scaling Open-Source Language Models with Longtermism | 2401.02954 | 2024-01 | P0 | 最高 | ✅ ACCEPT |
| 3 | DeepSeekMoE: A Strong, Economical, and Efficient Mixture-of-Experts Language Model | 2405.04434 | 2024-05 | P0 | 最高 | ✅ ACCEPT |

### 【P1 - 高相关】MLA/KV 压缩相关

| 序号 | 论文 | arXiv ID | 发表日期 | 相关性 | 优先级 | 决策 |
|------|------|----------|----------|--------|--------|------|
| 4 | InnerQ: Hardware-aware Tuning-free Quantization of KV Cache for Large Language Models | 2602.23200 | 2026-02 | P1 | 高 | ⏳ REVIEW |
| 5 | Fine-Tuning Without Forgetting In-Context Learning: A Theoretical Analysis of Linear Attention Models | 2602.23197 | 2026-02 | P1 | 高 | ⏳ REVIEW |

---

## 深度分析样例选择

**选择**: DeepSeek-V3 Technical Report (arxiv:2412.19437)

**理由**:
1. **最新官方报告** - 2024 年 12 月发布，包含最完整的 MLA 描述
2. **核心机制覆盖** - 详细描述了 MLA、MoE、KV 压缩等核心技术
3. **高引用潜力** - 后续研究必引用的基准工作
4. **适合模板验证** - 包含理论分析 + 实验验证，可测试多个模板字段

---

## 下一步

1. 完成 DeepSeek-V3 的标准化深度分析样例
2. 验证 metadata-schema.yaml 的字段是否够用
3. 根据分析体验调整模板
