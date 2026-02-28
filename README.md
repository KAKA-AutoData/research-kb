# Research Knowledge Base

Research knowledge base v1.0 - Phase 1 Complete

## Overview

This is a structured research knowledge base designed for long-term paper archiving, analysis, comparison, trend tracking, and research gap identification.

## Directory Structure

```
research-kb/
├── _meta/                     # Templates, schemas, and specifications
│   ├── institution-priority.yaml
│   ├── journal-tiers.yaml
│   ├── keyword-taxonomy.yaml
│   ├── metadata-schema.yaml
│   ├── topic-scope.yaml
│   ├── writing-usage-tags.yaml
│   ├── theory-tags.yaml
│   └── reproducibility-rubric.yaml
├── biomed/                    # Biomedical research
│   ├── authors/               # Author-specific archives
│   │   └── ZSJ/   # Zheng Shuangjia author archive
│   └── papers/                # Paper analyses (to be populated)
├── comptheory/                # Computational theory research
├── deepseek/                  # DeepSeek-specific research
├── cross-domain/              # Cross-domain research
├── synthesis/                 # Synthesis and integration
└── _comparisons/              # Cross-paper comparisons
└── _trends/                   # Trend reports
└── _gaps/                     # Research gap analyses
```

## Key Documents (Phase 1)

| Document | Description |
|----------|-------------|
| `RESEARCH_SCOPE_DEFINITION.md` | Research scope, inclusion/exclusion rules |
| `PAPER_ANALYSIS_SPEC.md` | Single paper analysis specifications (3 tiers) |
| `COMPARISON_RULES_SPEC.md` | Cross-paper comparison rules (3 levels) |
| `TREND_GAP_ANALYSIS_SPEC.md` | Trend report and gap analysis rules |
| `GITEE_UPDATE_SPEC.md` | Continuous update and write rules |
| `WORKFLOW_SPEC.md` | Full workflow specifications |
| `GITEE_REPO_DESIGN.md` | Repository structure design |

## Research Directions

1. **BioMed**: AI-driven drug discovery, drug repositioning, DTI prediction, RNA therapeutics
2. **CompTheory**: Attention mechanisms, low-rank methods, LoRA analysis, relation modeling
3. **DeepSeek**: MLA, KV compression, DeepSeekMoE, latent attention

## Priority System

- **P0**: Zheng Shuangjia high relevance, DeepSeek official papers, Tier1-2 top venues
- **P1**: Valuable methods, reference papers
- **P2**: Supplementary materials
- **P3**: Weak relevance but completeness needed

## Phase 2 (Next Steps)

1. Initialize Gitee remote repository
2. Push Phase 1 framework to remote
3. Start systematic paper ingestion (target: 20-30 papers in first month)
4. Trigger first lightweight cross-paper comparisons
5. Generate first trend reports

## Maintenance

- Weekly: Index updates and consistency checks
- Monthly: Trend tracking and summary
- Quarterly: Comprehensive review and cleanup

---

**Version**: 1.0  
**Last Updated**: 2026-02-28  
**Phase**: Phase 1 Complete - Ready for Phase 2 Deployment