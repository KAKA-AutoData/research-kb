---
paper_id: "arxiv:2602.24283v1"
priority: "P1"
domain: "comptheory"
source: "arxiv"
published_at: "2026-02-27T18:57:06+00:00"
url: "http://arxiv.org/abs/2602.24283v1"
decision_reason: "score >= 0.62"
score_total: 0.6225
authors:
  - "Zhengbo Wang"
  - "Jian Liang"
  - "Ran He"
  - "Zilei Wang"
  - "Tieniu Tan"
topic_hits:
  - "lora"
  - "low-rank"
---

# Taming Momentum: Rethinking Optimizer States Through Low-Rank Approximation

## Summary

Modern optimizers like Adam and Muon are central to training large language models, but their reliance on first- and second-order momenta introduces significant memory overhead, which constrains scalability and computational efficiency. In this work, we reframe the exponential moving average (EMA) used in these momenta as the training of a linear regressor via online gradient flow. Building on this equivalence, we introduce LoRA-Pre, a novel low-rank optimizer designed for efficient pre-training. Specifically, LoRA-Pre reduces the optimizer's memory footprint by decomposing the full momentum matrix into a compact low-rank subspace within the online linear learner, thereby maintaining optimization performance while improving memory efficiency. We empirically validate LoRA-Pre's efficacy by pre-training models from the Llama architecture family, scaling from 60M to 1B parameters. LoRA-Pre achieves the highest performance across all model sizes. Notably, LoRA-Pre demonstrates remarkable rank efficiency, achieving comparable or superior results using only 1/8 the rank of baseline methods. Beyond pre-training, we evaluate LoRA-Pre's effectiveness in fine-tuning scenarios. With the same rank, LoRA-Pre consistently outperforms all efficient fine-tuning baselines. Specifically, compared to standard LoRA, LoRA-Pre achieves substantial improvements of 3.14 points on Llama-3.1-8B and 6.17 points on Llama-2-7B, validating our approach's effectiveness across both pre-training and fine-tuning paradigms. Our code is publicly available at https://github.com/mrflogs/LoRA-Pre.

## Metadata

- Priority: P1
- Domain: comptheory
- Source: arxiv
- URL: http://arxiv.org/abs/2602.24283v1
- Published: 2026-02-27T18:57:06+00:00

## Notes for Research Pipeline

- Auto-generated from daily rulepack pipeline.
- Needs template enrichment for deep/manual analysis when required.
