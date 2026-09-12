# 文献证据地图：Context 中的来源策略学习

更新日期：2026-09-12

## Review question

已有研究是否已经完成了下面这个问题：

> 模型从上下文示例中推断一个隐藏规则/任务/来源优先级，并在同一潜在世界的反事实
> policy swap、原生输出和未见结构上执行它？

调研目的不是证明“source policy”这个词新，而是判断完整的研究协议是否已有直接先例。

## Scope and source base

检索和核验覆盖四类：

1. ICL 任务/规则识别与示例选择；
2. compositional generalization 与未见结构；
3. task representation、可读性和因果干预；
4. ToM、belief representation、动态/功能性伙伴适应，以及语义表示与最终判断分离。

关键来源优先读取作者原始摘要或正文；正式出版信息通过 ACL Anthology、PMLR 或 arXiv
原始页面核验。宽泛 OpenAlex 关键词检索噪声较大，不能单独作为缺口证据。

## Gap verdict

**Partly holds.**

“从 demonstrations 识别一个隐变量”并不新；“示例覆盖影响组合泛化”也不新；“probe
可读但不等于 causal use”已有直接近邻。尚未在本次核验中找到把以下四件事作为同一
中心对象并同时完成的直接工作：

```text
source-authority policy as the latent object
-> paired counterfactual policy interchange on the same world
-> native answer/action as the primary execution test
-> unseen conflict topology as compositional transfer
```

但这只能支持“协议组合和对象收紧后的候选缺口”，不能直接支持 Best Paper 新颖性。必须
进一步做 exact-task、exact-construct 和 exact-metric 对照；尤其要检查 2026 年新近的
policy、task-vector 和 functional-ToM 工作是否已经覆盖其中某一环。

## Second-pass audit: direct recent neighbors

第二轮不再用宽泛关键词判断缺口，而是逐篇检查最接近的四类工作：来源冲突、上下文
忠实性、规则/策略学习，以及表示到行为的迁移。新增近邻的共同特点是，它们已经把
“冲突”从普通 accuracy 问题提升为一个可测量对象；因此新论文不能再以“模型会被
先验、位置或重复影响”作为中心发现。

| Source | What the paper actually studies | What remains outside its protocol | Consequence for the new study |
|---|---|---|---|
| [Whose Facts Win?](https://arxiv.org/abs/2601.03746) | 在 synthetic factual conflicts 中测量不同 source type 的静态 credibility preference；source attribution 改变 MCQA 概率，低可信来源的重复可以翻转偏好 | source preference 是被测属性，不是由 demonstrations 反事实诱导的 governing policy；没有 same-world policy swap、unseen topology 或 native action transfer | 放弃“首次研究 LLM source preference”；把 source preference 作为 baseline/混淆因素，主对象改为 policy identifiability |
| [Investigating Context-Faithfulness](https://arxiv.org/abs/2409.10955) | 研究 parametric memory strength 与 evidence presentation style 如何影响模型接受外部证据 | 主要是 context receptiveness，不区分“相信这条事实”与“学会了冲突时谁有资格决定” | 必须把 content update 与 policy update factorial 分离 |
| [Context-DPO](https://arxiv.org/abs/2412.15280) / [ContextFocus](https://arxiv.org/abs/2601.04131) | 通过 preference optimization 或 activation steering 提高 context-faithfulness | 目标是让模型更忠实于给定 context，不检验一个上下文规则能否被交换并迁移到新世界 | steering 只能作为 intervention baseline，不能作为新论文中心贡献 |
| [Large Language Models in Resolving Contextual Knowledge Conflicts](https://arxiv.org/abs/2609.03148) | 建立 contextual-conflict taxonomy，研究事实、推理、时间、粒度、视角和歧义冲突；发现 earlier-evidence bias 并提出 steering | 冲突类型是数据集维度，模型不需要从 demonstrations 归纳 source-authority policy；没有 paired policy interchange | 研究“冲突中的 governing rule 如何被教会”，而不是再建 conflict taxonomy |
| [Understanding the Dynamics of Demonstration Conflict in ICL](https://arxiv.org/abs/2603.04464) | 研究 corrupted rule demonstrations 如何影响规则推断，以及中间层同时编码正确/错误规则、后层形成决策 | 关注错误示例造成的 rule conflict，不把 policy-equivalent contexts、native swap 和未见 topology 作为统一测量单位 | 将 corrupted context 作为竞争解释与 negative control，而非主问题 |
| [In-Context Learning as Implicit Policy Gradient](https://arxiv.org/abs/2607.23153) | 对 score-conditioned ICL 给出与 policy-gradient 的结构对应，并验证 reward-weighted exemplar aggregation | policy 是优化/奖励意义上的 action policy；不是多来源语义冲突中的 authority policy，且理论对应依赖简化条件 | “policy”一词必须限定为 source-authority policy |
| [Policy Iteration with Human Feedback](https://arxiv.org/abs/2608.16831) | 将持久化自然语言 policy、critic、专家审阅和 rollback 组成外部策略迭代流程 | policy 在上下文外被显式版本化和人工批准，不是模型从 demonstrations 隐式归纳 | 不能声称首次研究 language-model policy adaptation；差异是 latent policy 的被动识别与原生执行 |
| [Rift](https://arxiv.org/abs/2606.17229) | 用控制 wrongness 的方法区分“知道真相但撒谎”和“只会输出错误答案”，寻找 conflict signature | 研究内部 deception signature，不测试 context-induced policy、source authority 或 native policy swap | 支持“行为相同不代表内部状态相同”的问题意识，但不能替代本研究的行为协议 |
| [Multi-Step Knowledge Interaction Analysis](https://arxiv.org/abs/2511.01706) | 用 rank-2 subspace 分离 parametric knowledge 与 context knowledge 的多步生成贡献 | 仍然是 PK/CK interaction readout；没有 policy induction，也没有证明 readout 被最终答案使用 | 表示分析只能作为 secondary diagnostic |
| [Vision-Default, Prior-Override](https://arxiv.org/abs/2606.28273) | 在 VLM 中用 patching/ablation 定位视觉证据与先验冲突的稀疏因果回路 | modality-specific prior override；不涉及语言 demonstrations 诱导 authority rule | 若做机制，必须先有行为上的 policy swap |
| [Analogical Structure, Minimal Contextual Cues and Contrastive Distractors](https://arxiv.org/abs/2511.10441) | 研究输入组织、类比结构和对比干扰如何提高语言规则归纳样本效率 | 规则是 verb alternation，不是 conflicting sources 的资格政策 | 可借鉴 diagnostic example 设计，但不能把 analogical organization 包装成新贡献 |
| [Skill-Based Few-Shot Selection](https://arxiv.org/abs/2305.14210) | 用 skill description 改善跨域 few-shot example selection，减少 surface-feature bias | 选择的是给定任务的示例，不是学习 source-authority policy | example selection 只能是实验因素 |
| [Breaking through the learning plateaus of ICL](https://arxiv.org/abs/2309.06054) | 将 Transformer 内部表示拆成受 weights 影响的 component 与受 context 影响的 component，并研究训练 plateau 与 ICL 能力的关系 | 研究训练效率与表示分解，不定义可交换的 governing policy，也不做 same-world native policy execution | 支持把“模型已有能力”与“上下文当下诱导的能力”分开，但不能直接解释 source arbitration |
| [Does Deep Learning Learn to Abstract?](https://arxiv.org/abs/2302.11978) | 用 transferability probing 研究从具体实例到抽象概念的迁移，并报告 memorize-then-abstract、概念变异鲁棒性和中间层集中 | abstraction 的 transfer metric 不是冲突来源资格；probe/transfer 也不等于最终决策按抽象规则执行 | 可借鉴 concept mutation 与 source-side/target-side holdout，但主终点仍必须是 native policy execution |
| [Make Your LLM Fully Utilize the Context](https://arxiv.org/abs/2404.16811) | 用 information-intensive training 改善长上下文不同位置的信息利用与多片段整合 | 目标是位置与信息整合，不区分 context 中的事实更新与 governing policy induction | position/length 只能作为 matched nuisance controls |

### What the second pass rules out

以下命题已经被近邻工作实质性占据，不能再作为新论文 headline：

1. 模型在知识冲突中有 source preference；
2. 重复、位置、记忆强度或 evidence style 会改变 context faithfulness；
3. context conflict 可以被 taxonomy、steering 或 DPO 改善；
4. probe/readout 能看到 task、belief 或 PK/CK interaction；
5. demonstrations 的 diversity、similarity、coverage 或 analogical structure 影响泛化；
6. policy、partner adaptation 或 functional ToM 比静态 accuracy 更接近真实行为。

这些工作并不互相冲突，但它们的测量单位不同。新研究必须以一个更窄的单位连接它们，
而不是把它们的术语并列堆叠。

## Revised candidate gap

经过第二轮审计，最稳妥的候选缺口不再是 `source-policy induction` 这个宽泛名称，
而是：

> **policy identifiability under contextual conflict：给定若干可能产生相同已观测答案的
> contexts，模型能否识别真正的 governing source policy，并在同一潜在世界的反事实
> policy swap 后，于新内容、新编码和未见冲突拓扑上改变原生答案或行动？**

这个对象有三个必要的收紧点：

- **identifiability**：先构造 policy-equivalent support，检验 diagnostic example 是否
  真正缩小候选 policy version space，而不是仅增加事实或词面相似度；
- **counterfactual execution**：固定事实、实体、query、source values 和 output format，
  只交换 demonstrations 指示的 governing policy；原生 answer/action 是主要终点；
- **transport**：把 content、label/encoding 和 conflict topology 分开做 held-out transfer，
  不把 same-item replay 当作泛化。

因此新的研究问题应写成：

```text
Can a context teach a model not only what is true,
but which source is entitled to decide what is true?
```

这句话是研究动机，不是结论。若 exact-neighbor audit 发现已有工作完成了同样的
policy-equivalent support 与 same-world swap，则本项目应降级为 source-specific extension，
不能继续使用 Best Paper 叙事。

## Closest prior work

| Source | What it actually establishes | Boundary | Relation to proposed study |
|---|---|---|---|
| [AmbiBench / Task Ambiguity in Humans and Language Models](https://arxiv.org/abs/2212.10711) | 通过模糊任务说明和不同数量的 labeled examples 测量模型识别 intended task；规模与 human-feedback 共同影响表现 | latent object 是 task identity，不是同一世界中 conflicting sources 的 authority policy；没有 paired source-policy interchange | 直接关闭“我们首次研究从示例识别隐藏任务”的宽泛说法；保留 source-authority 作为更窄对象 |
| [How Do In-Context Examples Affect Compositional Generalization?](https://arxiv.org/abs/2305.04835) | 系统比较 similarity、diversity、complexity，并发现结构覆盖和示例选择影响未见组合 | 组合单位是 semantic parsing primitives/structures；不测试来源资格或反事实 policy swap | 迫使新研究把 `policy information` 与 similarity/coverage 做 matched comparison |
| [Diverse Demonstrations Improve In-context Compositional Generalization](https://arxiv.org/abs/2212.06800) | 通过覆盖测试程序所需结构的 diverse demonstrations 改善组合泛化 | 结构 coverage 已是明确先例；没有 source policy latent variable 和 native policy swap | 不能把“coverage-based selection”作为新贡献；只能检验 source-conflict coverage 是否增加额外预测力 |
| [Active Example Selection for In-Context Learning](https://arxiv.org/abs/2211.04486) | 把示例选择建模为 sequential decision，学习能泛化到新任务的 selection policy | 优化的是选哪些 examples，不是模型在 examples 中学到哪一个 governing source policy | selector 只能是 consequence，不能是论文 identity |
| [A Theory of Emergent In-Context Learning as Implicit Structure Induction](https://arxiv.org/abs/2303.07971) | 从信息论角度解释 compositional structure induction 与 intermediate steps；小模型受控设置支持理论预测 | 不是 typed source arbitration，也不以 counterfactual source-policy execution 为 outcome | 提供“结构归纳”理论邻近，但不能替代本文的来源竞争对象 |
| [Understanding the Dynamics of Demonstration Conflict in ICL](https://arxiv.org/abs/2603.04464) | 单个 corrupted rule demonstration 会伤害规则推断；中间层可同时编码正确/错误规则，后层形成决策置信度；报告 attention-head 干预 | 研究 across-example corrupted rule conflict 和错误恢复；不是 within-world source authority，也没有 policy-swap native execution | 直接关闭“冲突示例导致错误/存在两阶段计算”的宽泛 novelty；新研究必须研究不同类型的 conflict |
| [Learning Task Representations from ICL](https://arxiv.org/abs/2502.05390) | 用 attention-head 加权形成 task vector，并以功能任务表现和跨模态测试评估 representation fidelity | latent object 是 task representation；representation extraction/causal weighting 不能直接等于 source-policy execution | 新研究若用 probe，只能作补充；primary outcome 必须是原生答案/行动 |
| [Just-in-time and distributed task representations](https://arxiv.org/abs/2509.04466) | task identity 可持续解码，但可迁移 task representation 具有时间、语义和分布式局部性 | 研究 representation 何时形成及能否 transfer，不研究 source authority 的反事实替换 | 支持把 readout 与 transfer 分开，但不占据 policy-swap gap |
| [Single-Position Intervention Fails](https://arxiv.org/abs/2605.04061) | probing accuracy 与单点 causal importance 可完全分离；多位置干预可恢复 task transfer，并跨模型复现 | 研究 task identity 的分布式表示和干预位置，不研究语义 source arbitration 或 native policy swap | 关闭“probe 高准确率证明模型会使用 policy”；支持主研究不把 probe 当终点 |
| [Standards for Belief Representations in LLMs](https://arxiv.org/abs/2405.21030) | 提出 accuracy、coherence、uniformity、use 四个 belief-like representation 条件 | 是规范性框架，不是一个已完成的 source-policy induction 实验 | 提供 measurement precedent；新研究需把 `use` 具体化为 counterfactual execution |
| [Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html) | 研究模型内部是否表示 self/other beliefs，并用表示分析支持 belief attribution | belief representation 不等于 model 在冲突中按该 source 作答；任务与实验对象不同 | 关闭“belief 可读/可解码就是行为使用”；新研究需 native output |
| [Clever Hans or Neural Theory of Mind?](https://aclanthology.org/2024.eacl-long.138/) | stress-test social reasoning，揭示 ToM 行为可能由表面线索或 shortcut 驱动 | 主要是 benchmark/shortcut audit，不定义 source policy 或 policy swap | 提供反捷径设计先例 |
| [FANToM](https://arxiv.org/abs/2310.15421) | 在信息不对称互动情境中评测 ToM，比被动叙事更接近 interaction | 仍是 QA benchmark；没有显式 governing policy interchange | 支持功能性输出，但不直接覆盖 policy induction |
| [Dynamic ToM](https://aclanthology.org/2025.acl-long.1171/) | 评估模型对人类状态随时间变化的适应 | temporal state evolution ≠ source-authority policy；不等于同世界 counterfactual swap | 说明“动态”也不是我们的独占缺口 |
| [Theory of Mind Benchmarks are Broken](https://proceedings.mlr.press/v267/riemer25a.html) | 区分 literal ToM 与 functional ToM，要求模型根据 partner behavior 在上下文中适应，并用 regret 评估 | 直接提出 functional ToM 和 partner policy adaptation；因此 partner game 不能作为新颖性本身 | 新研究不能声称首次把 ToM 推到行动；只能把 partner adaptation 作为 source-policy execution 的下游验证 |
| [The Imperfective Paradox in LLMs](https://aclanthology.org/2026.acl-long.689/) | 用语言学上精确的 aspectual contrast，把中间语义区分与最终 NLI 判断分开，观察 teleological prior 和 calibration trade-off | 研究 event semantics 的 premise-hypothesis entailment，不研究 demonstrations 诱导 policy 或未见 topology | 提供最重要的实验纪律：先保证关系有效，再分离 representation/intermediate judgment/final label |
| [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271) | 主张语言表示的结构和符号复杂度影响知识激活、组织与任务表现，并报告表示/激活变化 | schema 是宽泛对象；没有把变化定义为 source authority，也没有完整 native policy-swap protocol | 关闭“语言表示改变模型内部组织”作为新颖性；新研究必须更窄、更可操作 |
| [Context-faithful Prompting](https://aclanthology.org/2023.findings-emnlp.968/) | 反事实 demonstrations 可提升对上下文的忠实性 | 研究 context faithfulness intervention，不分解 source policy identification 和 unseen topology execution | 是重要 baseline/alternative explanation |
| [The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) | 显式训练不同优先级 instructions 冲突时遵守 privileged instructions，并测试未见攻击 | 预设显式 system/user priority hierarchy，目标是 safety robustness | 新研究不能把“优先级冲突”本身宣称新；差异只能是无显式 hierarchy 的 semantic source policy induction |

## Evidence clusters

### Cluster 1：ICL 识别与示例信息

Settled：模型可以从 instructions/examples 识别任务或结构；示例的 similarity、diversity、
coverage 和 complexity 会改变组合泛化；一个 corrupted example 可以显著影响规则推断。

Contested：模型到底学到的是任务/规则、表面模板、结构 coverage，还是某种可迁移表示；
不同方法的 task vector 是否代表同一对象也未统一。

Missing：在一个固定 latent world 中，多个 sources 给出冲突值时，demonstrations 是否
诱导了一个“谁有资格决定答案”的 policy；这个 policy 是否能被 counterfactually swapped
并改变 native output。

### Cluster 2：表示、可读性与使用

Settled：可读性、task-vector 相似度和 causal intervention 不是同一层级；单点 probe/patch
不能自动支持分布式或功能性机制。

Contested：何种 representation 足以构成 belief-like state，何种干预能跨 content/format
迁移，任务向量是否具有统一身份。

Missing：把 source-policy 的 use 定义为原生 answer/action 对 paired policy swap 的
响应，而不是 probe accuracy 或 same-item reconstruction。

### Cluster 3：ToM 与功能适应

Settled：静态 false-belief accuracy 容易被 shortcut；动态、交互和 partner adaptation
比单一 QA 更有意义；functional ToM 已被提出并量化。

Contested：功能适应是否需要显式 belief representation、是否能跨 partner、任务和 horizon
迁移。

Missing：将 partner policy 作为 source-policy induction 的下游 consequence，而不是把
functional ToM 重新命名为新贡献。

### Cluster 4：语言语义与最终判断

Settled：精细语义关系可以在表示层被区分，但最终判断会受到先验/叙事偏置；刺激有效性
和 premise 的多重解释是关键风险。

Contested：prompt intervention 是修复了语义推理，还是改变了 calibration/label mapping。

Missing：在 demonstrations 诱导的 source policy 场景下，最小语义关系是否能被读出、执行，
以及未见 topology 是否暴露 composition boundary。

## Contribution diagnosis

### 过宽、不能用的 gap

- “我们首次研究 ICL 中的 task/policy induction。”已有 AmbiBench、task-vector、rule-conflict 工作。
- “我们首次研究 demonstration selection/coverage。”已有 CoFe、Diverse Demonstrations、Active Example Selection。
- “我们首次发现 representation 与 final behavior dissociate。”已有 belief representation、task intervention 和 Imperfective Paradox 近邻。
- “我们首次将 ToM 推向功能行动。”已有 functional ToM 立场工作。

### 保留下来的候选 gap

> **已有研究分别研究任务识别、示例结构、规则冲突、belief 可读性和功能适应；尚缺一个
> 以 typed source-authority policy 为唯一 latent object，使用 same-world paired policy
> interchange、native answer/action 和 unseen conflict topology 来验证 policy use 的完整协议。**

Gap verdict：**Partly holds, candidate only**。在开始写 Best Paper 叙事前，必须完成 exact
nearest-neighbor audit；若发现已有工作包含 same-world policy interchange，则将贡献收缩为
source-specific extension或复现/边界研究。

## New study design after literature audit

### Research object

```text
policy identifiability under contextual conflict
```

### Decisive contrast

```text
same latent world
same facts and source values
same query and surface form
different demonstration-induced governing policy
```

### Primary outcome

```text
native answer/action follows the induced policy on a new world
```

### Three studies

1. **Policy identifiability**：在 matched token budget 下构造 policy-equivalent support，
   比较 consensus、diagnostic、balanced diagnostic 和 corrupted contexts，检验真正减少
   policy version space 的 information gain 是否比数量、相似度、diversity、coverage 更能
   预测 held-out policy execution。
2. **Counterfactual policy interchange**：同一 world 仅改变 demonstrations 所指定的
   governing policy，测量 source-choice、native answer 和 action 的 paired swap effect；
   加入 label/source/order/fact permutation controls。
3. **Unseen topology composition**：训练只见二元 source conflicts，测试未见三元冲突，
   比较 pairwise、recency、majority、explicit-rule 和 direct-label baselines。

partner adaptation 只有在 Study 2 的 native policy execution 通过后才加入；它不再承担
独立 novelty。

## Required claim boundary

若成功，最多可以写：

> 在所测试的 synthetic typed-source environments 中，模型可以从 diagnostic demonstrations
> 归纳一个 source-authority policy，并在 matched counterfactual policy swaps 与未见
> conflict topologies 上表现出 native policy-sensitive execution。

不能写：

- 模型拥有 intrinsic ToM；
- 模型内部有一个 source-policy module；
- 该协议证明了人类式语义理解；
- 所有 ICL 都遵循同一 policy-learning law；
- probe/steering 成功就等于 policy 被 native execution 使用。

## Literature review architecture for the eventual paper

1. **Why accuracy is not enough**：用 ToM shortcut、belief representation 和 Imperfective
   Paradox 说明最终答案不唯一指向中间关系。
2. **What demonstrations already do**：用 AmbiBench、CoFe、coverage selection 和 rule
   conflict 说明任务/结构识别已有研究。
3. **What remains unmeasured**：明确 source-authority policy、same-world swap、native
   execution 和 unseen topology 的组合缺口。
4. **Our protocol**：定义 sources、policies、paired swaps、controls 和 execution outcome。
5. **Boundary**：区分 source-choice/readout、native execution、composition 和 functional
   partner consequence。

## Remaining verification before design freeze

| Source class | Why another pass is needed |
|---|---|
| 2025–2026 task-vector/function-vector papers | 仍需逐篇检查全文是否包含 source-specific policy swap；目前摘要级证据未显示覆盖 |
| 2025–2026 policy/agent ICL papers | 需区分 reward/action policy 与 semantic source-authority policy；目前未发现同一对象 |
| functional ToM follow-up benchmarks | 需确认是否有显式 partner-policy swap 而非 regret/adaptation；当前主近邻只占据后者 |
| source-aware/context-faithful prompting | 需把 context adherence 与 authority-rule induction 做直接 matched baseline |
| Chinese-language work by Fu/Jingwen and Nanning Zheng | 已核对 ICL plateau、抽象迁移、示例选择和 context utilization；仍需按具体全文补齐引用细节 |

## Exact-neighbor audit matrix

本矩阵采用同一套判据逐篇核对，而不是按关键词相似度判断：

```text
L = latent rule/policy is induced by context
C = conflict is inside the task world, not only parametric-vs-context
E = policy-equivalent support is explicitly constructed
S = same-world rule/policy swap is the decisive contrast
N = native answer/action is the primary outcome
T = transfer to unseen content, encoding, or topology is tested
```

| Work | L | C | E | S | N | T | Exact-audit verdict |
|---|---:|---:|---:|---:|---:|---:|---|
| AmbiBench | yes | no | no | no | partial | partial | task-intent identification precedent; not rule-change under source conflict |
| CoFe / Diverse Demonstrations | yes | no | no | no | yes | yes | compositional-generalization precedent; not authority-rule induction |
| Whose Facts Win? | no | yes | no | no | probability/MCQA | no | occupies static source preference and repetition effects |
| Context-Faithfulness / Context-DPO | no | parametric-context | no | no | yes | limited | occupies evidence adherence and mitigation |
| Contextual Knowledge Conflicts | no | yes | no | no | yes | no | occupies conflict taxonomy and positional bias |
| Demonstration Conflict Dynamics | partial | across examples | no | no | yes | limited | occupies corrupted-rule vulnerability and internal two-phase account |
| Task Representations / JIT Task Representations | yes | no | no | no | transfer metric | yes | occupies representation readability and transfer dissociation |
| Single-Position Intervention Fails | yes | no | no | no | intervention transfer | yes | occupies distributed task-template causality |
| Functional ToM / EnactToM | no | epistemic interaction | no | no | action | partial | occupies action-level ToM evaluation, not policy induction |
| Memory Trust Gap / DACSI | no | authority conflict | no | no | yes | partial | occupies persistent-memory/RAG authority boundary, not context-taught rule change |
| Instruction Hierarchy | explicit | yes | no | no | yes | yes | occupies pre-specified priority hierarchy, not latent semantic policy induction |
| Imperfective Paradox | no | semantic entailment | no | no | yes | lexical transfer | occupies representation-vs-judgment dissociation |
| Fu/Zheng ICL and abstraction work | yes | no | no | no | transfer/probe | yes | supplies structure-coverage and abstraction controls |

在已核验的来源中，没有一篇同时满足 `L+C+E+S+N+T`。这仍然不是“没有人做过”的
证明，而是一个明确的 candidate gap：它必须在实验前继续以 exact-task、exact-metric
和 exact-source audit 维护。

## Freeze decision

旧版 `source-policy induction` 不再作为最终论文标题，因为它会直接撞上 source
preference、context-faithfulness 和 policy-learning 文献。冻结后的母对象是：

> **context-conditioned rule updating：上下文是否改变了模型用于决定答案的规则，还是
> 只改变了模型对当前事实的接受程度？**

source authority 是该母对象的第一个、也是与 LP4FM 最连续的实例；它不是论文的全部
理论。论文的决定性现象应命名为：

```text
rule-change without fact-change
```

操作性判据为：固定 latent world、source values、query、answer interface 和表面编码，
只交换 demonstrations 所指示的 governing rule；如果模型真正更新了规则，native
answer/action 应随 rule swap 改变，并在新内容上保持该改变。

冻结后的文档与执行协议见：

`docs/research/frozen_oral_research_design_20260912_zh.md`

## Final status

```text
exact-neighbor audit = complete at metadata/abstract + selected full-text level
literature gap       = partly holds, candidate only
frozen object        = context-conditioned rule updating
new study            = not_evaluated
old source-policy induction = superseded by the frozen rule-update design
old identification -> commitment story = conditional downstream branch
remaining gate       = execute exact-task/metric audit before novelty claim
```
