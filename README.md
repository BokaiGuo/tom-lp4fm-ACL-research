# ToM / LP4FM Research Documents

Private document repository for the Theory-of-Mind / LP4FM research program.

本仓库同步保留英文原稿与中文研究版本。英文文件是正式投稿与原始研究方案；文件名以 `_zh.md` 结尾的文档是中文阅读、讨论和执行版本。

## Contents

- `manuscript/lp4fm2026/`: current LP4FM manuscript source, figures, tables, and rendered PDF.
- `manuscript/lp4fm2026/manuscript_zh.md`: 当前 LP4FM 稿件中文阅读版。
- `proposals/`: five independent oral branches plus the combined source-access algebra proposal. Every proposal now contains an explicit nearest-neighbor, novelty-boundary, and baseline discussion, with synchronized Chinese versions.
- `research/`: stage evolution, the ACG plan, the project-level relation-preservation map, the Best-Paper candidate, and bilingual related-work positioning maps.
- `theory/`: source-accessibility saturation theory in English and Chinese.
- `evidence/`: contribution, validation, claim-boundary, and project-report documents, including Chinese counterparts.

## Scope

This repository intentionally contains manuscript and research-planning documents only. It excludes model weights, prompts with operational secrets, run logs, large experiment outputs, caches, and local environment files.

## Current manuscript position

The LP4FM manuscript is **Paper A**, the bounded empirical foundation: it identifies typed source routing, confirms a subadditive composition class in the reference checkpoint, and defines checkpoint-sensitive behavioral and transport limits.

The submitted LP4FM manuscript is **Paper A**, the bounded static source-arbitration result. The strongest proposed **Paper B** candidate is **Conflict Is Supervision**: whether target-consistent source conflict identifies a latent source policy, while across-demonstration policy conflict corrupts it. ACG is retained as Paper B's execution/composition half: after policy identification, does the relation transfer across content and encodings and compose on unseen conflict topologies? Existing Stage-53--57 results motivate these objects but are not counted as Paper B evidence.

## Research entry points

- [Stage evolution and five-branch map](research/stage_evolution_and_oral_branches_20260805.md)
- [Next-stage ACG paper plan](research/next_stage_authority_compositional_generalization_20260826.md)
- [下一阶段 ACG 中文方案](research/next_stage_icl_schema_rewriting_20260826_zh.md)
- [Best-Paper candidate: Conflict Is Supervision](research/best_paper_conflict_as_supervision_20260909.md)
- [Best-Paper 候选中文方案](research/best_paper_conflict_as_supervision_20260909_zh.md)
- [Program-level relation-preservation map](research/next_stage_relation_preservation_20260909.md)
- [关系表征到答案承诺中文总图](research/next_stage_relation_preservation_20260909_zh.md)
- [Related-work positioning](research/related_work_positioning_20260826.md)
- [Related Work 中文定位地图](research/related_work_positioning_20260826_zh.md)
- [Combined training and source-access algebra proposal](proposals/oral_branch_learning_source_access_algebra_20260805.md)
- [合并分支中文方案](proposals/oral_branch_learning_source_access_algebra_20260805_zh.md)

## Related-work discipline

The documents distinguish background, precedent, contrast, and direct
competition. They do not claim novelty for ICL, task ambiguity, critical
disambiguating examples, demonstration conflict, compositional generalization,
schema shaping, belief probing, instruction hierarchy, or activation steering
as broad topics. The candidate separation is the distinction between
target-consistent within-example source conflict and across-example policy
conflict, followed by held-out relation transfer. Until the frozen pilot and
independent confirmation are complete, the next-stage status remains
`argument_shaped`.
