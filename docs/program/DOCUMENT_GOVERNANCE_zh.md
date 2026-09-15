# 文档治理索引：LP4FM 长期研究体系

**更新日期**：2026-09-15
**目的**：区分可核验事实、冻结设计、活跃方案、历史叙事与聊天材料。

## 1. 冲突解决顺序

```text
原始实验产物 / provenance
> EVIDENCE_LEDGER 与 PROJECT_REPORT
> 冻结设计与 exact-neighbor audit
> MASTER_RESEARCH_PLAN
> supporting proposals
> historical proposals
> chat exports / synthesis notes
```

聊天材料中的“发现、证明、定律”若没有对应结果 artifact，一律解释为候选命题。

## 2. Canonical 入口

| 文件 | 角色 | 状态 |
|---|---|---|
| `MASTER_RESEARCH_PLAN.md` | 4–5 年研究纲领、论文依赖与停止条件 | canonical |
| `research/frozen_oral_research_design_20260912_zh.md` | 当前旗舰 A1 的冻结协议 | `not_evaluated` |
| `research/literature_evidence_map_new_study_20260912_zh.md` | exact-neighbor audit | candidate gap only |
| `evidence/PROJECT_REPORT_zh.md` | LP4FM 实证历史与 claim 边界 | evidence canonical |
| `evidence/EVIDENCE_LEDGER.md` / `evidence/EVIDENCE_LEDGER_zh.md` | 外部材料与本地证据边界 | evidence canonical |
| `docs/program/long_term_research_roadmap.json` | 机器可读论文依赖、gate 与 stop condition | canonical manifest |

## 3. Supporting active 文件

| 文件 | 核心对象 | 保留价值 | 使用边界 |
|---|---|---|---|
| `docs/foundations/proposal_harmony_and_focusing.md` | harmony、inversion、focusing、`κ` | B1 详细形式与实验设计 | `not_evaluated`；不声称模型已遵循证明论语义 |
| `docs/foundations/research_plan_integrated.md` | L1/L2/L3 形式集群 | 44 周工具链、等价层级与门控 | Program B 内部计划，不等于整个研究纲领 |
| `docs/context-representation/tom_task_info_full_plan.md` | task informativeness × context length | 长上下文 robustness 设计 | 分离 instruction following 与 belief protection |
| `research/evidence_grounded_oral_research_20260912_zh.md` | 既有 transfer profile | 保留 negative/partial 边界 | 不覆盖 A1 新实验 |
| `research/related_work_positioning_20260826_zh.md` | 早期 ICL/ToM/authority 文献定位 | 文献 provenance | 由 2026-09-12 exact audit 更新 |

## 4. Historical / superseded 文件

| 文件 | 原对象 | 后来决策 | 处理 |
|---|---|---|---|
| `docs/archive/legacy-plans/research_roadmap_multi_stage.md` | Relation Information→Control 路线 | 条件链不再作为既成主线 | historical |
| `research/best_paper_relation_to_commitment_20260911_zh.md` | identification→commitment | joint readout/behavior evidence 未运行 | conditional branch |
| `research/best_paper_conflict_as_supervision_20260909_zh.md` | conflict as supervision | 邻近 critical-example/rule-conflict 文献 | 吸收到 A1 Experiment 1 |
| `research/new_source_policy_induction_20260912_zh.md` | source-policy induction | 对象过宽 | 被 frozen rule-updating design 取代 |
| `research/next_stage_authority_compositional_generalization_20260826.md` | ACG | authority composition 不再是唯一母对象 | topology/encoding supporting |
| `research/next_stage_icl_schema_rewriting_20260826_zh.md` | schema rewriting | schema 太宽且易把表示变化当机制 | historical precursor |
| `research/next_stage_relation_preservation_20260909_zh.md` | relation preservation | identification/control 尚未验证 | supporting precursor |
| `research/oral_shape_upgrade_20260912_zh.md` | oral-shape 装配 | 早于最终冻结设计 | historical package |

英文对应版、CSV 和 YAML 跟随其母文档的治理状态。

## 5. 五个既有 oral 分支

| 分支 | 新位置 | 启动门 |
|---|---|---|
| learning source-access algebra | 与 behavioral algebra 合并为 A2 | 同家族训练 checkpoints 与独立复现 |
| behavioral source-access algebra | A2 的三元算子预测层 | 二元效应先过 confirmation |
| mechanistic identifiability | 条件性 M1 | 至少两个非同构任务 |
| conflict-topology kernel | A1 topology transfer/control | 能预测未见 family 才独立 |
| source-accessibility training objective | A2/D2 后的 repair study | 稳定 failure law 与跨接口修复 |

`oral_branch_training_induced_composition_phase_transition_*` 是前两分支的合并 proposal，不是已验证结果。

## 6. 八份聊天导出逐篇审计

| 文档 | Core object | 可保留 insight | Evidence status | 风险 | 长期位置 | 决定 |
|---|---|---|---|---|---|---|
| `chatgpt-export_07_07 15_02 长上下文社会推理失效机制.md` | token inflation 与社会推理 | informativeness × length；instruction vs belief protection | mixed notes | 海报数值与机制未核验 | Context robustness | revise/archive |
| `chatgpt-export_08_01 12_10 形式化语言的深层意义结构.md` | types、witness、harmony | Construction Gap；L1/L2/L3 依赖 | proposal | 把统一理论写成已验证 | Program B | retain provenance |
| `chatgpt-export_冲击最佳论文建议.md` | Relation Information/Control 与语言相对论 | 每篇一个 because；kill gates | proposal synthesis | 旧 commitment 叙事过早中心化 | A1/D1/D2 | revise/archive |
| `chatgpt-export_论文查找与摘要.md` | task descriptions 与长上下文 ToM | 二维干预和位置控制 | literature notes | Effective Information 过早实体化 | Context robustness | archive |
| `chatgpt-export_论文解析 Theory of Mind.md` | Believing is Seeing 与任务信息 | intermediate state vs final answer | poster/secondary summary | 不可引用未公开数值和层位 | Evidence caution | archive |
| `chatgpt-export_论文深度解析.md` | Shaping Schema / ReGeo | distortion→recovery→reasoning | proposal | geometry/probe 误当因果 | Program C | retain precursor |
| `chatgpt-export_语言与思维关系.md` | Boroditsky 到多语 LLM | language/culture 分离；accessibility framing | review synthesis | 模型差异外推人类因果 | Program D | retain background |
| `chatgpt-export_09_12 00_18 LP4FM 工作坊审稿参考.md` | formal-language review、Type-Meaning、GLB | 正负例、类别证明、独立单位、proof verifier、受限下界测试 | review + exploratory proposals | 跨学科过度统一；GLB 非通用定理；部分文献需核验 | B0/formal methodology | retain/archive |

## 7. 五份新增综合材料

原附件保持不变；仓库副本只作为 `historical synthesis notes`。

| 材料 | 可保留内容 | 风险 | 归属 |
|---|---|---|---|
| Boroditsky 与计算语言相对论综述 | language→representation→reasoning 的文献线 | language switch ≠ `do(Language)` | Program D background |
| 语言相对论的批评性收紧 | language-of-evidence vs language-of-query | 不进入当前 A1 headline | D1 bridge |
| A–F research program 版本 | 每篇一个 causal question | 把未来链条写得像母定律 | program precursor |
| 四 Program / Constructed Relational Control 版本 | 五层 failure taxonomy | 统一链尚无实证 | program precursor |
| From Representation to Control 综合版本 | 论文序列、shared infrastructure、kill criteria | 仍需状态分层 | MASTER 直接来源 |

## 8. 补充文件的研究归属

`chatgpt-export_09_12 00_18 LP4FM 工作坊审稿参考.md` 不属于 LP4FM 证据。其前半的 review 贡献全项目方法纪律；后半的 Type-Meaning Calculus、closure probes、proof-term elicitation、causal abstraction 与 Generalization-as-Lower-Bound 进入 B0 incubation。

可直接采用：

- 正负最小对立项与 response-bias-aware metric；
- 最弱形式类别必须证明；
- 预先模型分组；
- 独立 item 作为统计单位；
- verifier、generator 和预注册采样规则。

不能直接采用：

- 把 DisCoCat、Lambek grammar、ICC 和 LLM mechanism 的并列当作统一理论；
- 把 PARITY 已知下界包装成新的复杂度发现；
- 把 `TC0`/`NC1` 未解分离写成经验可证明的结论；
- 把 causal abstraction 对齐当作 native causal use。

## 9. 更新纪律

1. 新实验先更新 artifact/provenance，再更新 `evidence/PROJECT_REPORT_zh.md`。
2. 只有 gate 状态改变才更新 MASTER 与 roadmap JSON。
3. 新 proposal 默认 `not_evaluated`，不得使用“发现、证明、建立定律”。
4. 被替代文件不删除，移动到 archive 并记录 superseding document。
5. 文献笔记不能成为 claim source；投稿前回到原文核验。
6. 合作者的新 idea 先进入 branch proposal，通过 novelty、validity 与 feasibility gate 后才进入 current study。
