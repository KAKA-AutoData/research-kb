---
paper_id: "arxiv:2602.24082v1"
priority: "P1"
domain: "comptheory"
source: "arxiv"
published_at: "2026-02-27T15:19:57+00:00"
url: "http://arxiv.org/abs/2602.24082v1"
decision_reason: "score >= 0.62"
score_total: 0.6225
authors:
  - "Jaekyung Cho"
topic_hits:
  - "attention"
  - "kv cache"
---

# Preference Packing: Efficient Preference Optimization for Large Language Models

## Summary

Resource-efficient training optimization techniques are becoming increasingly important as the size of large language models (LLMs) continues to grow. In particular, batch packing is commonly used in pre-training and supervised fine-tuning to achieve resource-efficient training. We propose preference packing, a method to enhance resource efficiency in training techniques that use data with different responses for the same input prompt, such as reward models or Direct Preference Optimization (DPO). Preference packing improves resource efficiency by reducing the attention operations for duplicate input prompts and decreasing KV cache memory usage. We conducted experiments on text-only datasets and image-included datasets and achieved at least 37% reduction in training time. Notably, this method can be applied alongside existing optimization techniques such as batch sorting, resulting in a 3.22x speedup.

## Metadata

- Priority: P1
- Domain: comptheory
- Source: arxiv
- URL: http://arxiv.org/abs/2602.24082v1
- Published: 2026-02-27T15:19:57+00:00

## Notes for Research Pipeline

- Auto-generated from daily rulepack pipeline.
- Needs template enrichment for deep/manual analysis when required.
