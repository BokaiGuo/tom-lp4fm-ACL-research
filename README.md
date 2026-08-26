# ToM / LP4FM Research Documents

Private document repository for the Theory-of-Mind / LP4FM research program.

本仓库同步保留英文原稿与中文研究版本。英文文件是正式投稿与原始研究方案；文件名以 `_zh.md` 结尾的文档是中文阅读、讨论和执行版本。

## Contents

- `manuscript/lp4fm2026/`: current LP4FM manuscript source, figures, tables, and rendered PDF.
- `manuscript/lp4fm2026/manuscript_zh.md`: 当前 LP4FM 稿件中文阅读版。
- `proposals/`: five independent oral branches plus the combined source-access algebra proposal. Every proposal now contains an explicit nearest-neighbor, novelty-boundary, and baseline discussion, with synchronized Chinese versions.
- `research/`: stage evolution, the next-stage Authority Compositional Generalization plan, and bilingual related-work positioning maps.
- `theory/`: source-accessibility saturation theory in English and Chinese.
- `evidence/`: contribution, validation, claim-boundary, and project-report documents, including Chinese counterparts.

## Scope

This repository intentionally contains manuscript and research-planning documents only. It excludes model weights, prompts with operational secrets, run logs, large experiment outputs, caches, and local environment files.

## Current manuscript position

The LP4FM manuscript is **Paper A**, the bounded empirical foundation: it identifies typed source routing, confirms a subadditive composition class in the reference checkpoint, and defines checkpoint-sensitive behavioral and transport limits.

The proposed next paper is **Paper B**, centered on **Authority Compositional Generalization (ACG)**: whether demonstrations induce a typed source-relation program that transfers across content and answer encodings and composes on unseen conflict topologies. Existing Stage-53--57 results motivate this object but are not counted as Paper B evidence.

## Research entry points

- [Stage evolution and five-branch map](research/stage_evolution_and_oral_branches_20260805.md)
- [Next-stage ACG paper plan](research/next_stage_authority_compositional_generalization_20260826.md)
- [下一阶段 ACG 中文方案](research/next_stage_icl_schema_rewriting_20260826_zh.md)
- [Related-work positioning](research/related_work_positioning_20260826.md)
- [Related Work 中文定位地图](research/related_work_positioning_20260826_zh.md)
- [Combined training and source-access algebra proposal](proposals/oral_branch_learning_source_access_algebra_20260805.md)
- [合并分支中文方案](proposals/oral_branch_learning_source_access_algebra_20260805_zh.md)

## Related-work discipline

The documents distinguish background, precedent, contrast, and direct
competition. They do not claim novelty for ICL, compositional generalization,
schema shaping, belief probing, instruction hierarchy, or activation steering
as broad topics. The candidate separation is relation-level: typed source
authority must transfer across content and encodings and support prediction on
unseen topologies. Until that pilot is complete, the next-stage status remains
`argument_shaped`.
