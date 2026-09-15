# 从表示到行为控制：LP4FM 长期研究纲领

**版本**：2.0
**更新日期**：2026-09-15
**适用周期**：4–5 年
**文档角色**：唯一 canonical 长期计划
**当前旗舰状态**：设计已冻结，经验结果 `not_evaluated`

## 0. 如何阅读这份计划

这份文件不是论文结果汇总，也不是把历史提案拼接成一个“大理论”。它区分四类内容：

| 标签 | 含义 |
|---|---|
| `validated` | 已由当前仓库中的可追溯实验支持，可在边界内写入论文 |
| `partial` / `negative` | 有真实结果，但覆盖、迁移或因果解释受限；负结果必须保留 |
| `exploratory` / `not_evaluated` | 研究设计或理论假设，尚无足够经验支持 |
| `superseded` / `historical` | 对研究演化有价值，但已被更晚决策替代 |

任何跨分支统一命题在至少两个独立研究对象通过各自门控以前，都只能是 `program hypothesis`，不能写成“已经发现的统一规律”。

## 1. 当前结论先行

### 1.1 已有研究在整套体系中的位置

当前已投稿的 LP4FM 稿件是这套计划的 **Paper A0 / 经验地基**：

> 在固定语义世界中，语言描述与 typed conflict topology 会改变 belief、reality、other-agent 与 distractor 之间的来源裁决；这种组合行为具有 checkpoint-sensitive 边界。

它已经把普通 ToM accuracy 重写为 **typed source arbitration**，并提供：

- `validated`：Qwen2.5-7B-Instruct 上受限的来源可达性组合效应；
- `validated`：pooled typed-topology effect 与局部 answer-interface geometry；
- `partial`：部分构式与控制相对形状可跨 checkpoint 保留；
- `negative`：item-level law、方向性组合、跨构式和跨 checkpoint transport 不稳定；
- `negative`：probe-readable、几何可对齐或 final-token steering 不足以支持 universal causal controller；
- `blocked claim`：不能据此声称普适 belief controller、人类式 ToM、construction-general law 或广义神经机制。

因此 LP4FM 的角色不是“终极理论”，而是发现了一个后续所有研究都必须解释的事实：

> **信息存在、关系可读与关系真正控制输出不是同一个问题。**

### 1.2 下一篇论文的冻结对象

当前唯一优先的旗舰研究是：

```text
context-conditioned rule updating
```

核心现象候选是：

```text
rule-change without fact-change
```

研究问题是：

> 当上下文改变时，模型是在更新当前事实，还是在更新“冲突时哪个来源有资格决定答案”的裁决规则？

这是 `not_evaluated` 的研究设计，不是对 LP4FM 旧结果的重新解释。冻结协议见 `research/frozen_oral_research_design_20260912_zh.md`。

### 1.3 长期统一命题的证据状态

长期可以用下列链条组织问题：

```text
Linguistic Experience
→ Representation
→ Construction
→ Recovery
→ Identification
→ Control
→ Action
```

但该链条目前只是 **program hypothesis**。现阶段只在 `source arbitration` 一段拥有边界明确的实证底座；其余箭头必须由独立论文分别识别。

## 2. 一个母问题，而不是一堆关键词

长期母问题是：

> **任务相关的结构如何被语言与表示构造出来，被模型从表面形式中恢复出来，被上下文证据识别出来，并最终取得对行为的控制权？长期语言经验又如何改变这条过程？**

这不是以下问题的简单并集：

- LLM 有没有 Theory of Mind；
- 自然语言、JSON 或 DSL 哪个准确率更高；
- 中文和英文会不会产生不同答案；
- probe 能否读出 belief 或 task vector；
- 某种 steering 能否提高一个 benchmark。

这些只是母问题在不同层级上的观测窗口。

## 3. 推理失败的五层诊断框架

| 层级 | 核心问题 | 可观察失败 | 当前状态 |
|---|---|---|---|
| Construction | 所需约束、实体或规则是否由表示直接构造？ | 只被逻辑蕴涵，不能直接使用 | `exploratory` |
| Recovery | 已存在的结构能否从序列化表面恢复？ | latent graph 未恢复 | `exploratory` |
| Identification | 模型能否从证据识别 governing relation/rule？ | 候选规则无法消歧 | `not_evaluated` |
| Control | 已识别的关系是否改变原生答案/行动？ | readout 正确但行为不随规则变 | `not_evaluated`；LP4FM 提供动机 |
| Execution | 前四层正确后，具体搜索、规划或计算是否完成？ | 算法执行失败 | 下游对象 |

这套分解的用途是生成互斥诊断，不是预设模型内部存在五个离散模块。

## 4. 四个核心 Program

### Program A — Relational Control

**对象**：evidence、candidate relations、source authority、prior pressure 与 native action。
**问题**：什么证据足以改变裁决规则？识别出的规则何时真正控制行为？
**状态**：唯一已有经验地基且当前应集中资源的 Program。

### Program B — Constructed Representation

**对象**：types、witnesses、rules、proofs。
**问题**：哪些结构必须被表示构造，而不能只在经典意义上被蕴涵？
**状态**：形式化设计较成熟，经验结果尚未建立。

### Program C — Structure Recovery

**对象**：latent task graph、serialization、structure decodability 与 geometry recovery。
**问题**：语义相同的表示为何诱发不同计算？差异来自结构恢复还是后续推理？
**状态**：高价值生死 pilot，尚未成为结果。

### Program D — Linguistic Experience

**对象**：language medium、culture、tokenization、training exposure 与 obligatory distinctions。
**问题**：语言条件作用于关系识别还是行为控制？受控语言经验能否因果改变跨任务推理？
**状态**：自然语言研究为 observational；因果结论必须等待人工语言或 matched continued training。

机制研究、人类研究和 representation synthesis 是跨 Program 的验证层或终局层，不是当前平行主线。

## 5. 论文序列与唯一“because”

| ID | 暂定论文 | 唯一核心解释 | 当前状态 |
|---|---|---|---|
| A0 | LP4FM / Source Arbitration | 描述与 typed conflict 改变来源裁决 | `submitted`, evidence-bounded |
| A1 | When Context Changes the Rule | 行为随 context 改变，因为裁决规则而非事实发生更新 | `not_evaluated`, design frozen |
| A2 | Training-Induced Composition Laws | checkpoint 差异来自训练阶段对组合规律的重写 | `exploratory`, conditional |
| B0 | Formal Capability Certificates | 形式能力判断来自可验证正负例、证明项与已知下界，而非接受倾向 | `incubation`, conditional |
| B1 | Where Chains of Thought Choose | 困难随非确定性选择复杂度而非仅随链长变化 | `not_evaluated` |
| B2 | Classically Equivalent, Dynamically Distinct | 指称使用随 proof-relevant witness accessibility 变化 | `not_evaluated` |
| B3 | Schemas Are Types | 类型化收益来自约束迁入构造层，而非形式外观 | `not_evaluated` |
| B4 | Constructed, Not Entailed | 多类结构共享“构造优于仅蕴涵”的规律 | `conditional synthesis` |
| C1 | Same Semantics, Different Computations | 表示改变结构恢复难度，进而改变推理 | `not_evaluated`, pilot first |
| D1 | Linguistic Relativity of Relational Control | 跨语言差异落在 prior、identification 或 control 的特定阶段 | `conditional observational` |
| D2 | Controlled Linguistic Experience | 随机操纵的义务性语言经验改变跨任务关系推理 | `conditional causal` |
| M1 | When Is a Controller Identifiable? | probe、control、specificity 与 transport 是不同证据层 | `conditional methods paper` |
| H1 | Artificial Language Learning in Humans and LLMs | 同一经验操纵在两类学习系统中产生结构同型效应 | `conditional human study` |
| X1 | Machine-Designed Reasoning Languages | 先验结构量能够指导表示合成并跨任务迁移 | `capstone` |

一篇论文若需要三个互不依赖的“because”才能成立，应当拆分而不是扩张。

## 6. 当前旗舰 A1：Context-Conditioned Rule Updating

### 6.1 决定性对照

```text
same latent world
same source values
same entities and query
same answer interface
different demonstration-induced governing rule
```

只有当原生答案或行动在新世界中随 governing rule 改变，才能支持 rule updating。仅接受一个新 value 属于 fact updating；仅能报告规则属于 readout without use。

### 6.2 三个实验

1. **Policy identifiability**：比较 consensus、single diagnostic、balanced diagnostic、redundant 与 corrupted contexts；检验 version-space reduction 是否优于 example count、similarity、diversity 和 coverage 预测 held-out execution。
2. **Paired rule swap**：固定事实，只交换 demonstrations 诱导的规则；native answer 为主终点，source choice、rationale 与 first action 为分层诊断。
3. **Rule transport**：分别测试新 content、新 encoding 和未见 conflict topology；不把 same-item replay 当作泛化。

### 6.3 最强竞争解释

- static source preference；
- fact updating only；
- answer-label mapping；
- recency / first-position heuristic；
- natural source-word semantic prior；
- explicit instruction following；
- global calibration、temperature 或 response-interface effect；
- rule report/readout without native use。

### 6.4 经验晋级门

A1 只有同时满足以下条件，才从 `oral_shaped by design` 晋级：

1. 预注册 policy pairs 上存在 paired native rule-swap effect；
2. effect 在新 content 上保留；
3. label、source-name、order 与 position controls 排除主要捷径；
4. diagnostic information 比 raw count 更接近执行结果；
5. 至少一个独立模型或 matched checkpoint 复现；
6. confirmation split、provenance、主图和 adversarial audit 完整。

若 control dissociation 不稳定，论文收缩为 policy identifiability / fact-vs-rule boundary，不延续未经支持的 identification→commitment 叙事。

## 7. 五个既有分支如何保留而不抢主线

| 既有分支 | 新位置 | 启动条件 |
|---|---|---|
| 训练如何改变语言组合定律 | A2，与“语言约束组合代数”合并 | 同家族 Base→SFT→alignment checkpoints 可得，且能冻结 stimuli/scorer |
| 语言约束的组合代数 | A2 的预测层；三元与跨构式组合 | 二元效应先独立复现，候选算子可被三元条件区分 |
| 机制解释的可识别性 | M1 方法论文 | 至少两个任务出现“可读但不可控”或“同子空间不同轨迹”反例 |
| Conflict-Topology Kernel | A1 Experiment 3 的 transfer/control；强时再独立 | topology 规律能预测未见 family，而非事后拟合 |
| Source-Accessibility Training Objective | A2/D2 后的 repair paper | 稳定失败已被识别，训练目标能选择性修复并跨格式迁移 |

原先“前两条合并”的方案保留为 A2：研究训练如何改变来源可达性组合规律。它不能取代 A1，因为 A2 问 training trajectory，A1 问 context-induced rule change。

## 8. 长上下文分支的准确位置

`Task-description informativeness × context length` 保留为独立的 **context robustness study**，不并入 A1 的核心贡献。

可保留的问题：

- 描述信息量与 token inflation 是否交互；
- instruction following 与 belief-representation protection 是否可分离；
- effect 是否来自位置、长度、示例利用或 state recovery。

必须降级为假设的旧叙事：

- `Effective Information` 是已建立的统一量；
- `Task-Belief Bottleneck` 已被验证；
- 海报中的 `66→82%`、特定层、attention dilution 或跨语言恢复是本项目证据。

只有公开正文、可复现实验或本地独立结果存在时，才可升级上述主张。

## 9. Workshop 审稿材料带来的方法纪律与形式能力支线

补充文件 `chatgpt-export_09_12 00_18 LP4FM 工作坊审稿参考.md` 包含两部分，必须分开吸收。

### 9.1 可直接升级为全项目规范的审稿教训

1. **正例接受率不是能力**：所有判别任务必须同时有最小配对负例，并报告 hit、false alarm、AUC 或与任务匹配的判别量。
2. **形式类别必须先证明**：不能因为生成器使用了某种文法写法，就声称其语言不属于更弱类别；最弱生成类和定理前提必须由形式证明或权威结果支持。
3. **独立单位不能伪造**：同一 item 的重复采样只估计解码噪声，不能把样本量从 50 扩成 5000；主统计单位必须是独立 item/world/proof graph。
4. **模型分组必须预先固定**：不能按结果把模型划为 conventional/augmented，再用该分组解释结果。
5. **跨学科类比不承担实证**：神经科学、认知科学或复杂度理论类比若无直接接口，只能进入 discussion。

这些规则直接适用于 A1 的 policy-equivalent support、B1–B3 的形式分类、C1 的结构干预和 D2 的训练效应。

### 9.2 B0 — Formal Capability Certificates

该文件进一步提出 `Type-Meaning Calculus`、closure-property probes、proof-term elicitation、reduction-based instances 与 causal-abstraction alignment。长期保留，但定位为 **Program B 的 incubation branch**：

- 可保留：类型检查器作为客观 verifier，而不是把 Lean pass@k 当论文终点；
- 可保留：正负最小对立项、闭包性质和规约式实例替代表层“更长更难”；
- 可保留：proof-term elicitation 用于降低猜测与反应偏好；
- 可保留：`PARITY ∉ AC0` 等已知分离结果可用于受限、明确前提下的经验检验；
- 条件保留：DisCoCat、Lambek grammar 与 ICC 只有在同一证明对象和可验证映射真正实现后才可统一；
- 降级：`Generalization-as-Lower-Bound` 不是通用定理，`TC0` 与 `NC1` 相关结论仍有公开理论边界；
- 降级：causal abstraction alignment 在没有 specificity、bidirectionality 和 transport 前只是 exploratory mechanism。

B0 的启动门是：先用一个经过证明的受限对象完成 generator→minimal pair→proof verifier→pre-registered statistics 的闭环。若只能重现 PARITY 难度而没有新的证书协议或迁移结果，则不形成独立论文。

### 9.3 与 B1–B4 的关系

B0 问“如何可靠判定一个系统完成了什么形式计算”；B1–B3 问“哪些证明结构或表示性质预测模型行为”。前者提供验证工具和反捷径纪律，后者提供本研究纲领的实质科学对象。不能把 Type-Meaning Cube 直接当成 `Constructed, Not Entailed` 的证明。

## 10. Program B：形式基础的依赖顺序

### B1 — Choice complexity 与 harmony/focusing

先比较 step count、rule count、token length 与多个候选 `κ` 定义。在步数匹配条件下操纵有效选择点，并用 focused scaffold 做选择性干预。若 `κ` 不优于 chain length，则放弃普适 choice-complexity law。

### B2 — Witness accessibility

比较经典等价但 proof relevance 不同的表示，例如 `∃x.P(x)` 与 `¬¬∃x.P(x)`。同时测 under-use、leakage 与 global accommodation。`context window = DRS` 只能是待检验机制假设。

### B3 — Constraint migration

在 Implicit→Propositional→Type-level 条件中操纵约束承担层级，并保留“形式外观相同但 `σ,κ` 不变”的 placebo。若 propositional→type-level 无额外收益，结论应是显式化足够，而不是硬讲类型认知优势。

### B4 — 综合理论

只有 B1–B3 至少两个对象形成独立、跨模型且可预测的共同规律，才启动 `Constructed, Not Entailed`。在此之前，它是 umbrella hypothesis，不是 Paper B 或整个项目的已验证理论。

## 11. Program C：Structure Recovery

C1 的主干是：

```text
Representation Distortion
→ Structure Recovery
→ Reasoning
```

Primary intervention 必须是 semantics、token multiset、长度和词汇频率匹配的 `aligned / random / anti-aligned` serialization，而不是简单比较 NL、JSON、DSL。

证据依次要求：

1. behavioral distortion effect；
2. structure decodability 超过 format decodability；
3. recovery metric 对未见实例具有预测力；
4. structure-specific 双向干预改变行为且不泄漏答案。

如果 token-matched permutation 没有稳定效应，停止 structure-recovery law；自然格式差异只能写作 format sensitivity。

## 12. Program D：从语言条件到语言经验

### D1 — Natural-language stage decomposition

设计：

```text
Language_of_evidence × Language_of_query × Diagnosticity × Rule
```

区分：

- 语言改变 relation prior：强 diagnostic evidence 后差异消失；
- 语言改变 identification：规则读出本身跨语言不同；
- 语言改变 control：读出一致，但 native behavior 不同。

自然语言切换同时改变 tokenization、频率、文化共现与 instruction-tuning coverage，因此只能支持 observational stage decomposition，不能写作 `do(Language)`。

### D2 — Controlled linguistic experience

使用同一 base checkpoint 和 matched corpora，随机操纵义务性 evidential/source marking；control marker 编码与来源无关但频率、长度和语义匹配的类别。主要终点必须迁移到未见 ToM/source conflict、symbolic/JSON interface 与 partner task，而不是只测训练格式。

只有出现 dose-response、difference-in-differences 和跨任务/接口迁移，才能声称 controlled linguistic experience causally alters relational reasoning in the tested artificial learner。

### Human extension

人类研究采用 miniature artificial language 的随机分配，不直接比较中文母语者与英文母语者。问题是两类学习系统是否呈现结构同型 treatment effect，不是 LLM 是否模拟人脑。

## 13. Related Work：已有研究占据了什么

| 文献簇 | 已有工作已经回答 | 本计划必须做出的区别 |
|---|---|---|
| Context faithfulness / knowledge conflict | 外部事实、证据样式、重复、位置和 parametric memory 会改变答案 | 正交分离 fact update 与 adjudication-rule update |
| ICL task/rule induction | demonstrations 可识别任务；similarity、diversity、coverage 和 conflict 影响泛化 | 以 same-world rule swap、policy-equivalent support 和 native execution 为测量单位 |
| Task/belief representations | task、belief 或 PK/CK interaction 可被读出或干预 | readout、single-position intervention 与 native use 严格分层 |
| Functional ToM | partner adaptation、regret 和行动评估已存在 | first action 只是 rule execution 的下游 assay，不承担 novelty |
| Imperfective Paradox / fine-grained semantics | 中间语义判断可正确而最终标签被先验改写 | 借用“关系有效性→中间判断→最终行为”的分层纪律，不声称同一机制 |
| Shaping Schema / representation effects | 表示形式影响激活、组织与任务表现 | 用 token-matched distortion、proof-relevant equivalence 和预先计算的结构变量区分 |
| Multilingual reasoning / language-culture | 同一模型跨语言会改变表示、价值和推理结果 | 区分 evidence language 与 query language，并把 culture/tokenization 当控制，不把差异当因果 |
| Artificial/counterfactual languages | 可受控研究语言先验与 typological learnability | 从“语法是否易学”推进到“义务性区分是否迁移改变 relation control” |

完整 exact-neighbor audit 以 `research/literature_evidence_map_new_study_20260912_zh.md` 为准。当前 gap verdict 是 `partly holds, candidate only`。

## 14. 共享基础设施

长期只维护一个 semantic source 与多 renderer 系统：

```text
G(W, G_task, R, D, ρ, L, E, A)
```

- `W`：latent world；
- `G_task`：task/proof/constraint graph；
- `R`：governing relation；
- `D`：diagnostic evidence；
- `ρ`：representation/serialization；
- `L`：natural or artificial language；
- `E`：training experience；
- `A`：answer/action interface。

基础设施原则：

1. semantic source 与 surface renderer 分离；
2. 每个 decisive pair 共享 latent world；
3. content、encoding、topology、language 和 checkpoint holdout 分开；
4. scorer、prompt、generator、seed 与 model snapshot 全部记录；
5. Lean/形式 checker 负责等价与证明项验证，自写 prover 负责结构复杂度；
6. 主终点优先 native answer/action，probe 与 judge 只作次级诊断。

`ctorkit` 的长期模块可以包括 `core`、`equiv`、`focus`、`harmony`、`dts`、`sigma`、`regeo`、`routing`、`multilingual` 与 `intervention`，但只按通过 gate 的研究增量实现。

## 15. 证据与统计契约

每项结果必须记录：

```text
claim
evidence bucket
independent sampling unit
source artifact
generator/scorer version
uncertainty
nearest rival
promotion gate
non-claim
```

禁止做法：

- 把 replay 当 independent replication；
- 把 pooled effect 写成 item-level law；
- 把 probe accuracy 写成 causal use；
- 把 calibration gain写成 relation-specific mechanism；
- 把跨语言差异写成语言结构的因果效应；
- 把 proposal、海报摘要或聊天综合写成本项目证据；
- 在结果出现后修改 policy family、主要终点或 equivalence criterion。

主要 bootstrap 单元必须对应真正独立的 `latent world × policy pair`、proof graph 或 discourse item，而不是 token、option 或同一 item 的多个渲染。

## 16. 12–18 个月执行顺序

### P0：A1 冻结与 pilot

1. 冻结 source ontology、policy family、prompts、seed 与 native scorer；
2. 先生成 paired rule-swap pilot；
3. 完成独立 validity screen；
4. Experiment 2 通过后再扩 Experiment 1 与 topology holdout；
5. 使用新 worlds、templates 和 confirmation split，不复用 LP4FM 作为确认集。

### P1：两个低成本生死 pilot

- B1：step-matched `κ` pilot；
- C1：token-matched Program-DAG aligned/random/anti-aligned pilot。

两者只决定是否重仓，不在 A1 完成前扩成全量项目。

### P2：条件启动

- A2 需要同家族训练 checkpoints 与可控 SFT；
- D1 需要 A1 的 identification/control 测量先稳定；
- B2/B3 需要形式 checker 与 equivalence 审计先通过；
- B0 只允许做经过证明的受限 capability-certificate pilot；
- M1 需要跨至少两个非同构任务的可识别性反例。

## 17. 五年阶段门

| 阶段 | 目标 | 晋级条件 |
|---|---|---|
| Year 1 | 建立一个稳定 behavioral law | A1 或其收缩版通过独立复现；两个 pilot 作出生死决策 |
| Year 2 | 分离 construction、recovery、identification | B1/C1 至少一条形成预测性结果；B2 validity 完成 |
| Year 3 | 建立表示理论与语言条件桥梁 | B2/B3 有独立结果；满足条件才做 B4 与 D1 |
| Year 4 | 操纵 linguistic experience | D1 存在 stage-specific effect，D2 完成 matched exposure 与 transfer |
| Year 5 | 机制、人类或表示合成闭环 | 前序行为规律稳定且因果/伦理/资源门通过 |

## 18. 全项目停止条件

| 分支 | Kill / pivot criterion |
|---|---|
| A1 Rule Updating | 无 native rule-swap effect → fact updating / identifiability boundary |
| A2 Composition Training | 无 checkpoint-stage reversal 或可控诱导 → 停止 training-causal claim |
| B0 Capability Certificates | 只有已知任务难度、无新 verifier/anti-shortcut contribution → 不独立成文 |
| B1 Focusing | `κ` 不优于 chain length → 放弃 universal choice law |
| B2 Accessibility | truth-conditional model 胜出 → 报告其边界，不强行 DRT 化 |
| B3 Constraint Migration | propositional→type-level 无增益 → 显式化已足够 |
| C1 Recovery | token-matched permutation 无效 → 降为 natural-format sensitivity |
| D1 Natural Language | effect 被 competence/tokenization/calibration 解释 → 停止 causal branch |
| D2 Controlled Experience | 只在训练格式内有效 → 不声称 reasoning transfer |
| M1 Mechanism | 无 specificity、bidirectionality 或 transport → 留在 appendix |
| H1 Humans | 模型 treatment effect 未稳定 → 不启动人类实验 |
| X1 Synthesis | 理论量不能 out-of-sample 预测 → 不做自动语言设计主张 |

## 19. 文档真值与治理

阅读顺序：

1. `MASTER_RESEARCH_PLAN.md`：长期方向与依赖；
2. `research/frozen_oral_research_design_20260912_zh.md`：当前 A1 冻结协议；
3. `research/literature_evidence_map_new_study_20260912_zh.md`：当前 exact-neighbor audit；
4. `evidence/PROJECT_REPORT_zh.md`：LP4FM 实验历史和边界；
5. `evidence/EVIDENCE_LEDGER.md` / `evidence/EVIDENCE_LEDGER_zh.md`：外部材料与本地证据边界；
6. `docs/program/DOCUMENT_GOVERNANCE_zh.md`：其余文件应保留、修订还是归档；
7. `docs/program/long_term_research_roadmap.json`：机器可读依赖、门控与停止条件。

聊天导出和新增综合材料是 **idea provenance / historical inputs**。它们可以解释研究如何演化，但不能覆盖冻结设计、证据账本或实验产物。

## 20. 最窄可守的长期表述

当前可以说：

> LP4FM 已在特定模型与受控 typed-source 环境中发现描述、冲突拓扑与 checkpoint 共同塑造来源裁决，并明确显示可读性、组合行为与因果控制不能互换。下一阶段将检验 context 是否能在事实不变时更新裁决规则。

当前不能说：

> 我们已经证明智能依赖“构造而非蕴涵”，已经建立从语言经验到行为控制的统一机制，或已经证明语言塑造了模型认知。

长期研究纲领的工作假设是：

> **推理不仅取决于一个表示蕴涵什么，也取决于它把什么构造为可恢复、可识别、可访问并最终具有行为控制力的结构。**

这句话必须随着每个独立分支的结果被保留、收紧或推翻。
