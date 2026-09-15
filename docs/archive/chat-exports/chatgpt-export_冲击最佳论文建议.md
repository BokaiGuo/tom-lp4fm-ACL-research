# 冲击最佳论文建议

> https://github.com/BokaiGuo/tom-lp4fm-research请你查阅相关文献，进行对比分析，给我提供建议，我该怎么样才能做出best paper级别的研究

我先把这个仓库的研究问题、方法路线和已有结果拆开，再对照近几年的相关论文看“别人已经做到哪一步、真正还缺什么”。我会尽量把结论收束成一套能冲 best paper 的研究路线，而不只是列一堆论文。

目前最重要的判断已经比较清楚：你仓库里最新的 **“From Relation to Commitment”** 比单独做 ACG 更有冲击力，因为它把问题从“模型能不能学到规则”升级成“模型已经读出了关系，为什么最终仍然不按它行动”。这个问题天然连接 ToM、ICL、context faithfulness、semantic reasoning，而且有机会形成一个能被其他方向复用的诊断框架。

我看到一个需要你非常警惕的“撞车点”：**“内部知道/可读，但外部行为没用上”本身已经不是新结论**。ICML 2024 的 belief representation、EMNLP 2025 的 brittle belief representations、ACL 2026 Best Theme 的 CoSToM，以及 ACL 2026 Best Paper 的 Imperfective Paradox 都已经占据了这一叙事空间。你的机会不在于再证明一次 dissociation，而在于把 **“为什么、何时发生 dissociation”变成一个可检验的关系识别—答案承诺定律**。

我把你这个仓库的当前稿件、`Conflict Is Supervision`、ACG、`From Relation to Commitment`，以及 ToM、ICL、context faithfulness、belief representation、ACL 2026 的相关工作对照看了一遍。先给结论：

**你现在最有机会冲 Best Paper 的，不是继续把 LP4FM 的 source-routing / saturation 做得更复杂，也不是单独做 ACG，而是把下一篇收敛成一个非常锋利的科学命题：**

> **模型是否“识别出正确关系”，和模型是否“允许这个关系控制最终答案”，是两个不同的计算问题。诊断性冲突解决前者，而预训练/默认先验可能在最终 commitment 阶段重新夺回控制权。**

但这里有一个关键变化：**不要把论文 headline 写成“LLM 内部知道但没用出来”。这个 novelty 已经不够了。** 你真正应该抢的是一个更强的 **Identification → Commitment theory**。

---

## 1. 你目前处在什么位置

你现有 LP4FM Paper A 其实已经相当扎实：固定 latent world，通过 typed competitors 把“答案到底由 belief、reality、other-agent、distractor 中哪一个 source 控制”变成可识别的行为对象；Qwen2.5-7B-Instruct 上又有独立 worlds 的 subadditive composition confirmation。但 Qwen3 和 Base checkpoint 没有复制方向性的 residual，所以你很克制地把 claim 限定成 **checkpoint-sensitive behavioral theory**，没有硬说 universal mechanism。这个判断是对的。turn259955view0

它的问题也正如你仓库自己判断的：ACG 即使做得漂亮，也很容易被 reviewer 解释成“一个新的 compositional generalization task family”。你的 `Conflict Is Supervision` 文档已经抓到了关键突破口——正确 demonstration 只有在 competing policies 给出不同答案时，才真正提供关于 governing relation 的监督。turn777472view2

而最新的 `From Relation to Commitment` 又比它前进了一步：**即使 relation 已经可以被正确 read out，也未必能赢得 final answer。** 这是你现在最值得押注的方向。

---

## 2. 为什么仅仅“Relation → Commitment dissociation”还不够新

这是你接下来最需要防守的 related-work 区域：

| 方向 | 现有工作已经占掉什么 | 你不能再把什么当主 novelty |
|---|---|---|
| ToM benchmark | ToMBench 已系统覆盖 8 类任务、31 种能力；OpenToM 已做更长、更丰富的 mental-state narratives。 | “我们提出一个更好的 ToM benchmark” |
| ToM benchmark critique | ICML 2025 的 Riemer 等人直接主张大部分静态 ToM benchmark 测的是 literal ToM，真正重要的是能否根据 partner behavior **adapt in context**。 | “静态 false-belief accuracy 不代表 ToM” |
| belief representation | ICML 2024 已证明 belief 可从 hidden activations 线性 decode，且 intervention 会影响 ToM；EMNLP 2025 又显示这种 representation structured 但 brittle。 | “模型内部有 belief representation” |
| internal→behavior gap | ACL 2026 Best Theme Paper CoSToM 的问题本身就是 internal ToM knowledge 与 external behavior misalignment，并进一步做 causal tracing + steering。 | “内部知道但外部表现不好” |
| task ambiguity | AmbiBench 已经说明 demonstrations 可能不足以唯一确定 task；Pan 等也区分 task recognition 与 task learning。 | “很多 examples 其实没确定任务” |
| critical example | Ye et al. 已经专门构造“一堆 ambiguous examples + 一个 critical disambiguating example”。 | “一个关键 example 可以超过很多普通 examples” |
| conflicting demos | Jiao et al. 2026 已研究单个 corrupted demonstration 如何引发 rule conflict，还找到了两阶段 processing 和相关 attention heads。 | “conflict 会影响 ICL” |
| context vs prior | Context-faithful Prompting 已经展示 counterfactual demonstrations 可以帮助 context 战胜 parametric knowledge。 | “context 和 parametric prior 会竞争” |
| representation vs decision | ACL 2026 **Best Paper** Imperfective Paradox 已报告 representation 能区分结构，但最终 inference 仍被 teleological prior 支配。 | “relation 可表示但 decision 跟着 prior” |

尤其最后一点非常关键。ACL 2026 给 *The Imperfective Paradox in Large Language Models* 的 Best Paper 理由明确强调了：它用一个非常锋利的 linguistic theory 暴露模型的 substantive limitation，同时展示了 **representation 与 reasoning 的 dissociation**。

而且这篇 Best Paper 两个月不到就出现了一篇针对 benchmark validity 的直接反驳：作者指出大量 stimulus 本身不能唯一支持原先的 semantic interpretation，并区分了 intermediate semantic decision 与 final prediction。

这件事对你特别重要：

**Best-Paper 级工作首先要经得住“是不是 stimulus 本身就没定义清楚？”这一刀。**

你自己的最新方案把 validity audit 放在 Study 0 是完全正确的。

---

# 3. 我会把你的核心 claim 改成什么

我不建议最后用：

> LLMs can represent a relation but fail to use it.

这个已经太接近 Imperfective / CoSToM / RepBelief。

我会升级成：

> **Relational in-context learning contains two separable bottlenecks: identification and commitment. Demonstrations identify a governing relation only where competing policies make different predictions; even after the relation becomes identifiable and behaviorally accessible, a competing prior can still override it at final commitment.**

中文就是：

> **上下文示例并不是因为“正确”而具有监督信息，而是因为它们能排除竞争关系才具有监督信息；而完成关系识别以后，最终答案还要经历一次独立的先验竞争。**

这样你的论文贡献就不再是一个现象，而是一套 **解释框架**：

`demonstrations`
→ **policy identification**
→ **relation accessibility**
→ **commitment competition**
→ **execution/action**

这比单纯的 ToM、ICL 或 probe paper 更有机会成为 field-level result。

更重要的是，它能同时解释几件过去看起来没有联系的现象：为什么 random labels 有时没那么伤、为什么一个 critical example 很重要、为什么 corrupted demo 极其有害、为什么 counterfactual demo 能压过 parametric prior，以及为什么 true-belief success 并不能充分证明 belief tracking。Min 等人的经典工作就发现，在不少分类任务上随机替换 demonstration labels 居然只造成很小损失；你的理论有机会解释其中一部分现象——因为很多 demos 提供的是 format/task cues，而不一定提供 competing-policy separation。

这里的关键是：**不要声称你发明了 task ambiguity、critical examples 或 Bayesian task inference。你是在给它们找一个更精确的 relational explanatory variable。**

---

# 4. Best-Paper 版本应该只剩三个主实验

你现在 `From Relation to Commitment` 里 Study 0–6 很完整，但如果全部做进主文，反而容易显得像 research program，而不是一篇有“一击必杀”结果的论文。

我会把主文压缩成三个 experiment。

### Experiment 1：Information, not example count

这是整篇最应该漂亮的一张图。

构造 candidate source policies，例如：

`belief / reality / other-agent / reported-source`

然后让 demonstration 分成：

**source-consensus demos**：所有 competing policies 都预测同一个 label，因此 label 正确，但不区分 policy。

**source-diagnostic demos**：sources 对答案产生冲突，而 label 唯一支持目标 policy。

**policy-corrupted demos**：不同 demonstrations 分别支持互相冲突的 policy。

核心 comparison 不应该只是 C1 vs D1，而应该做漂亮的 dose-response：

`C1, C2, C4, C8, C16`
对比
`D1`
以及
`D1 + incompatible D1`

如果最终出现：

> **1 个 diagnostic example > 16 个完全正确的 consensus examples**

同时：

> **第二个支持 rival policy 的 diagnostic example 又把性能打下来**

那么这个图非常有记忆点。

但仅有这个结果还不够，因为 Ye et al. 已经有 critical example。真正的新东西应该是：

**模型 performance 随 `policy-separation coverage / version-space reduction` 变化，比随 demo 数量、semantic similarity、lexical diversity 更稳定。**

你仓库里的 `C_sep` 和 demonstration distinguishability graph 就应该在这里发挥作用。

数学 proposition 本身不要卖成理论突破——“两个 hypotheses 对所有 observations 预测完全相同就无法区分”太接近 definition。真正的理论贡献是：**这个量对实际 LLM 的 demonstration effectiveness 有预测力。**

---

### Experiment 2：Identification × Prior Pressure phase diagram

你现在的 2×2 是对的，但我建议把它升级成全篇的视觉中心，而不是另一个实验：

| | prior aligned | prior opposed |
|---|---:|---:|
| consensus / non-identifying demos | prior wins，通常看起来“答对” | prior wins，暴露 failure |
| diagnostic demos | relation identification + correct answer | **critical commitment cell** |

关键不是 target accuracy。

关键是同时画两个 surface：

**Relation Readout**
和
**Native Final Commitment**

如果你找到一个稳定区域：

`relation readout ≈ high`
但
`native final answer ≈ prior`

那么你才真正拥有 identification–commitment dissociation。

你的当前方案里 RR、FC、PO、CD、DG 的定义已经很好。

但我要特别建议：**primary evidence 不要依赖 chain-of-thought，也不要把“先问它规则，再让它回答”当成 native behavior。**

应该至少有三个 completely separated arms：

native answer only；
independent relation readout on cloned/held-out instances；
explicit-rule oracle。

这样才能防止 reviewer 说“你在 measurement 过程中已经给模型做了 prompt repair”。

如果资源够，再加一个 continuous dose-response：逐步增加 diagnostic coverage 和 prior strength，看是否出现一个 **identification–commitment phase diagram**。如果不同模型、不同 domain 的数据能大致 collapse 到类似的曲线，这会比单纯 2×2 强很多。

---

### Experiment 3：从 QA 走向真正的 functional consequence

这是我认为你当前计划里最值得增加的一块。

ICML 2025 已经明确批评，大量 LLM ToM benchmark 只是让模型“回答一个关于别人 mental state 的问题”，而没有检验它是否会 **根据对方的 policy 调整自己的行为**。

而 2026 年的工作已经开始把 ToM 往 planning/action 推，而不是 passive QA。citeturn160776academia24

所以我会设计一个极简 **partner-adaptation game**：

模型先观察几个 interaction。每个 interaction 里，partner 面前同时存在 reality、belief、report 等信息源，而 partner 的行动由某一个 latent policy 决定。

有些 demonstrations 所有 source 都建议同一行动——non-diagnostic。

某一个 demonstration 中 sources 分裂——它才能识别 partner 的 policy。

然后给全新的人物、物体、lexical form 和 conflict topology，要求模型选择：

**它应该告诉 partner 什么 / 采取哪个动作，才能与 partner 成功协调。**

这时候就不是：

> “Alice believes where?”

而是：

> **你是否真的利用你推断出来的 Alice policy 改变了自己的行为？**

这一步会把你的工作直接从普通 ToM benchmark paper 拉向 **functional social reasoning / agent adaptation**。

我甚至会把它作为外部 validation，而不是再用 Imperfective event semantics 当主要 cross-domain replication。Event domain 太靠近刚刚得 Best Paper、又马上遭到 validity challenge 的那条线，容易让论文显得“借来的第二域”。turn362906view1

---

# 5. 最值得加的一个“实用 consequence”

如果前三个结果漂亮，我会追加一个很小、但很有 impact 的 section：

**Demonstration selection by policy separation.**

不要声称 active learning 或 demo selection 是新问题。只做一个极简单的 greedy selector：

选择最大化 competing-policy separation / version-space reduction 的 demonstration。

然后和：

random、semantic similarity、diversity、more demonstrations

比较。

如果结果是：

> **2 个 policy-separating demos > 16 个 similarity-retrieved demos**

那么你的 theory 就不再只是解释现象，而产生了一个实际方法。

这类“理论对象 → 可执行方法”的闭环，对 Best Paper narrative 很加分。

---

# 6. 我反而建议你砍掉什么

你现在最容易犯的错误，是因为已有实验基础很丰富，就想把所有好东西都塞进去。

**mechanistic interpretability 不应该再作为下一篇的主贡献。**

你现有 evidence 很诚实：belief 可读，但 causal portability 很有限，跨 checkpoint、跨 layer、跨模型的结果构成的是一个 **boundary map**，不是 universal belief controller。

而 CoSToM 已经是 ACL 2026 Best Theme Paper，并且走的是“causal tracing → critical layers → steering → downstream improvement”这条完整路线。

除非你后面突然得到非常强的 causal specificity + transportability，否则不要跟它正面竞争。

更好的做法是：

**behavioral law 是主文；mechanism 是 boundary/optional final section。**

同样，LP4FM 的 saturation、affine transport、checkpoint geometry 不要重新变成另一条主线。它们的历史作用是让你发现 source arbitration 这个对象。下一篇必须站到更高一级的问题。

---

# 7. 你的 kill criteria 应该非常残酷

如果目标真的是 Best Paper，而不是“想办法把实验写成论文”，我建议提前冻结下面这些 gate：

1. **Diagnosticity gate**：一个或少数 diagnostic demos 必须在 held-out worlds 上稳定优于大量 matched consensus demos，而不是只在训练模板上更好。
2. **Commitment gate**：prior-opposed 条件必须出现可复现的“relation readout 正确、native answer 跟 prior”的 separation；如果 readout 本身失败，只能叫 identification/access failure，不能叫 commitment gap。
3. **Validity gate**：人工 blind annotation、paraphrase、source swap、label shuffle、answer frequency、position、lexical matching 全部先过，再解释模型 failure。ACL 2026 Imperfective paper遭遇的后续争论已经充分说明这一点。
4. **Transfer gate**：不能只换名字。至少跨 unseen content、answer encoding 或 conflict topology；最好再过 functional partner adaptation。
5. **Model-family gate**：如果只有 Qwen2.5-7B-Instruct 成立，就不要写 universal law。至少 behavioral headline 要在多个独立 model families 上看到同方向 interaction。
6. **Repair-specificity gate**：如果 prompt repair 只是把所有答案从 A 推向 B，邻近有效关系同时受损，就不能说模型“学会了关系”，只能说 decision bias 被移动。
7. **External consequence gate**：至少有一个非原始 false-belief QA setting 重现结构，否则论文应收缩成 ToM-specific result。

你仓库现在其实已经有这种科学纪律，这是明显优势；下一步不是增加 experiment 数量，而是让每个实验都在关闭一个**致命 alternative explanation**。

---

# 8. 如果结果真跑出来，我会怎样讲这个 paper

我甚至不建议主标题继续强调 ToM。

更强的是：

**Conflict Identifies, Priors Decide: Separating Relation Identification from Answer Commitment in Language Models**

或者更直接：

**When Evidence Identifies the Rule but Language Models Follow the Prior**

整篇论文第一页应该只讲一个反直觉案例：

模型看了 16 个全部正确的 demonstrations，依然不知道该遵循 belief 还是 reality；
加入 **一个** source-conflicting example，它马上能准确指出 governing relation；
但测试题一旦与一个强 prior 冲突，模型又在最终答案处背叛刚刚识别出的 relation。

然后才告诉读者：

**accuracy 把三个东西混在一起了：是否识别任务、是否还能访问关系、以及是否允许它控制答案。**

这就是一个 Best-Paper 级的开头。

---

## 9. 我对你几条路线的排序

如果现在必须下注，我会这样排：

**第一名：Identification → Commitment + functional consequence。**
这条最有机会成为“别人以后引用你的术语和 diagnostic framework”的工作。

**第二名：Conflict Is Supervision。**
如果 commitment dissociation 不稳定，就退回这条。它仍然有很强的 main-conference potential，但必须把 novelty 放在 **within-example source conflict ≠ across-example policy conflict**，以及 policy-separation coverage，而不是“critical example”。turn506766view4turn506766view0

**第三名：ACG。**
如果前两个最惊喜的效应都没有出现，ACG 仍然可以是一篇干净的 compositional-generalization paper，但我同意你仓库里的判断：它更像 strong main paper，而不是自然的 Best-Paper headline。

---

最后给你一句我认为最重要的判断：**你现在缺的已经不是更多实验，而是一个能让别人重新解释已有结果的“科学变量”。**

你已经非常接近这个变量了，它不是 `ToM accuracy`，不是 `belief probe score`，甚至不只是 `source routing`。

它是：

> **How much does the evidence distinguish the governing relation, and once distinguished, does that relation survive commitment?**

如果最后数据真的支持这两阶段分解，而且能从静态 ToM 走到 held-out action/adaptation，我会认为这才是真正值得以 Best Paper 标准来设计的版本。Awards 本身当然不可规划，但这条路线的科学辨识度明显高于“再做一个更完整 benchmark”或者“再找一个更强 probe”。

genui{"suggest_automation":{"label":"每周检查是否出现直接撞车的 ToM/ICL/关系承诺新论文"}}

-----

> https://github.com/BokaiGuo/tom-lp4fm-research请你看看现在的项目仓库查阅相关文献，进行对比分析，给我提供建议，我该怎么样才能做出best paper级别的研究

我会重新按“当前仓库状态”来判断，不沿用上次结论：先看你最近新增/修改的研究文档和证据，再把它们放到 2024–2026 的 ToM、ICL、context faithfulness、representation–behavior gap、benchmark validity 这几条文献线上逐一对照。最后我会给你一个**主线选择 + 关键实验 + kill criteria + 论文叙事**的版本，目标是尽量接近 Best Paper 的评审标准。

这次我发现了一篇你仓库目前还没有纳入、而且**非常接近你核心命题的新工作**：Nature Machine Intelligence 2026 年 9 月 7 日的 *Causal evidence that language models use confidence to drive behaviour*。它同样把“内部信号是否存在/准确”和“这个信号是否真正驱动行为”拆开，并用干预去证明 signal→behavior。

这对你不是坏消息，反而把门槛说得更清楚了：你的论文不能停在“readout 对、final answer 错”，而要进一步证明**被识别的 relation 对最终 commitment 的控制权如何随 diagnostic evidence 与 prior pressure 改变**。这会成为我下面建议里最重要的一处升级。

还有一个很关键的“反例解释”也要提前纳入：9 月 4 日的新预印本表明，一些“内部信息正确、行为却失败”的现象可能主要只是**输出阈值/读出校准错误**，一个标量修正就能大幅恢复行为。 所以你如果能证明 relation→action 的失败**无法被全局阈值、confidence 或 elicitation format 解释**，反而会把你的结果从“又一个 dissociation”提升成真正的 relation-specific control 现象。

我重新按 **2026-09-14 当前仓库状态**和最近几个月的新文献看了一遍。和你上次让我看的版本相比，我的判断有一个明显变化：

**你的项目现在已经有了“Best-Paper-shaped”的研究骨架，但当前的 `Identification → Commitment` 本身还不能作为最终 novelty。** 最近出现的几篇工作把门槛又往前推了一截。真正值得你下注的版本应该从：

> “模型识别了关系，但最终没有使用它”

进一步升级成：

> **什么信息使关系变得可识别？被识别的关系在什么条件下真正获得对行为的控制权？这种控制失败能否排除普通的输出校准、置信度和读出界面解释？**

也就是说，我建议把最终科学对象从 **relation–commitment dissociation** 升级成 **relation-specific control / evidence-to-control**。

---

# 一、你现在的仓库其实已经进化得相当正确

你现在把项目明确拆成了 Paper A 和 Paper B。Paper A 是已经比较成熟的 bounded empirical foundation：typed source routing、Qwen2.5-7B-Instruct 上的 subadditive composition，以及明确的 checkpoint-sensitive boundary；Paper B 则把 Conflict Is Supervision、ACG、final commitment 和 partner adaptation 合并到一个更高层的问题里。turn505101view6

尤其我赞同你 9 月 11 日 canonical plan 做的三个决定：

**只保留三个主实验：**

`Information, not example count`
→ `Identification × Prior Pressure`
→ `Functional partner adaptation`

而不是把 probe、ACG、steering、跨 checkpoint、event semantics 全部包装成并列贡献。turn505101view2

而且你已经非常正确地规定了 human/expert blind validation、lexically matched pairs、relation/source swap、label shuffle、unknown、非重叠 world/template 等控制。这个部分千万不要削弱——最近的文献反而说明它应该更严格。

所以现在的问题已经不是“方向不对”。

问题是：

**2026 年 9 月这个时间点，related work 又前进了。你的理论必须再升一级。**

---

# 二、现在真正与你“撞车”的文献有哪些

| 文献 | 它已经占掉的空间 | 你必须比它多做什么 |
|---|---|---|
| **Language Models Struggle to Use Representations Learned In-Context**, ACL 2026 | 已经直接证明：模型可以在 context 中形成 novel semantic representation，但下游并不能可靠使用它。 | 不能再以“representation learned but not used”为 novelty；你要解释 **什么时候能用、什么时候不能用** |
| **The Imperfective Paradox in LLMs**, ACL 2026 Best Paper | representation 与最终 inference 可以 dissociate，default/teleological bias 可以覆盖语义区别 | 你的故事不能只是“relation readout 对、answer 错” |
| 对 Imperfective 的 2026-08 benchmark critique | 指出 stimulus 自身的语义有效性就可能产生错误结论。 | validity audit 必须成为正文实验设计，而不是 appendix hygiene |
| **RepBelief**, ICML 2024；**CoSToM**, ACL 2026 Best Theme | belief 可以内部读取；internal ToM–external behavior misalignment；还可以 steering。turn505101view12 | probe、activation steering、internal/external gap 都不能作为 headline novelty |
| Critical-example ICL, ACL Findings 2025 | 一个关键 example 能消除很多 ambiguous examples 的规则歧义。 | “1 个 diagnostic > 很多个普通 examples”本身也不够 |
| Demonstration Conflict, 2026 | corrupted demonstrations、竞争规则和阶段性 processing 已经被系统研究。 | 必须区分 **within-example source separation** 与 **across-demo policy corruption** |
| Functional ToM critique, ICML 2025；EnactToM 2026 | 已经明确从 literal QA 转向“根据他人的 policy 调整自己的行动”；EnactToM 更进一步做 embodied coordination。turn505101view17 | partner adaptation 不能卖成“我们首次做 functional ToM benchmark” |
| **Causal evidence that LMs use confidence to drive behaviour**, Nature MI，2026-09-07 | 非常关键：已经提出内部 confidence → decision policy 的两阶段框架，并分别通过 representation steering 和 threshold manipulation 做因果验证。 | “two-stage internal signal → decision”也不能单独声称新；你要建立 **relation-specific control law** |
| **When Do Internal Probes Beat Reading the Answer?**, 2026-09-04 preprint | 某些“知道但没答出来”主要只是一个 scalar decision threshold 偏移；单参数校准就可显著恢复行为。citeturn744224academia4 | 必须排除“你的 commitment gap 其实只是 output calibration” |

其中我认为**对你威胁最大的不是 Imperfective，而是最后三篇**。

特别是 ACL 2026 的 Lepori et al. 已经把“context 中学出了 representation，但是无法灵活部署”写得非常直接。

所以你的论文不能再问：

> Can LLMs identify a relation but fail to use it?

答案已经基本是 yes。

你要问的是：

> **What determines whether an identified relation obtains control over behavior?**

这一下就不一样了。

---

# 三、我建议把核心理论改成：Identification → Control

你目前的：

> `identification → commitment`

已经非常接近了。

但我会再做一个术语上的变化：

> **relation identification → relation control**

因为 `commitment` 很容易被 reviewer 理解成“最终 token 输出”“confidence threshold”“answer calibration”。

而你真正感兴趣的其实不是模型有没有“commit”。

你真正关心的是：

> **到底哪个 relation/source policy 在控制行为。**

这恰好又和你 Paper A 的 `source-indexed answer control` 自然接起来。Paper A 已经证明单纯 accuracy 无法告诉我们到底是 belief、reality、other agent 还是 distractor 在控制答案。

于是两篇论文形成非常漂亮的升级：

**Paper A**

`Which source controls the answer?`

↓

**Paper B**

`How does a source relation acquire behavioral control?`

这比现在的：

`Paper A = source routing`
`Paper B = relation–commitment gap`

更加统一。

---

# 四、理论上不要只用 C_sep，最好正式定义“Relation Information”

你现在的 `V(D)`、`C_sep(D)`、`VR(D)` 是很好的起点。

但如果想冲非常高的 level，我会进一步把 deterministic version-space 写成一个 information-theoretic quantity。

假设候选关系集合是：

$$

\Pi=\{\pi_{\text{belief}},
\pi_{\text{reality}},
\pi_{\text{report}},
\pi_{\text{other}},...\}

$$

则 demonstrations 提供的 relation information 可以定义成类似：

$$

I_R(D)
=
H(\Pi)-H(\Pi\mid D)

$$

你的 `|V(D)|` reduction 就是它的一个 deterministic 特例。

这样：

**consensus demo：**

$$

I_R(D)\approx0

$$

因为几个 candidate policy 都预测相同答案。

**diagnostic demo：**

$$

I_R(D)>0

$$

因为 competing policies 给出了不同预测。

这会把你的核心论点从：

> conflict examples 比普通 examples 好

提升为：

> **Demonstration value is determined by information over competing relations, not correctness or count.**

这就是真正有机会被别人引用的科学变量。

---

# 五、然后定义第二个量：Relation Control

这是我认为你项目现在最缺的一步。

不要只看 final accuracy。

做 **paired policy swap**。

同一个 latent world、同样的 surface、同样的答案 token、同样的人物，只改变：

> 哪个 source relation 被指定为 governing policy。

比如：

Condition A：

$$

R=\text{Alice's belief}

$$

Condition B：

$$

R=\text{reality}

$$

如果 Alice belief=A，reality=B，

那么真正遵守 relation 的模型必须发生：

$$

A \rightarrow B

$$

的答案翻转。

于是你可以定义一个很直观的 **Relation Control Effect**：

$$

RCE =
P(\text{appropriate answer flip}\mid\text{policy swap})
-
P(\text{flip}\mid\text{sham swap})

$$

这比：

> readout 对，但 final answer 错

强得多。

因为它测的不是“知道什么”，而是：

> **行为对 governing relation intervention 的因果敏感性。**

我认为这个量甚至可能成为整篇论文最值得留下来的 methodological contribution。

---

# 六、主实验 1：不要只证明 1 > 16，要证明“信息量预测行为”

你仓库现在想做：

`C1/C2/C4/C8/C16`

vs

`D1`

vs

`D1 + C7`

vs

`D1 + incompatible D1`

这个设计非常好，保留。

如果最终出现：

> **1 diagnostic example > 16 matched consensus examples**

当然会是一张非常漂亮的 teaser figure。

但 reviewer 很可能马上说：

> “Critical example literature already showed this.”

所以真正主结果必须是：

$$

I_R(D)
\rightarrow
\text{relation identification}
\rightarrow
\text{held-out transfer}

$$

而且 `I_R(D)` 应该比下面这些变量更好地预测 performance：

demo 数量、embedding similarity、semantic diversity、position、length、label balance。

你仓库已经准备比较这些，这是完全正确的。

如果最终你能得到：

> **Across models, worlds and demonstration sets, relation-information explains performance substantially better than example count or semantic similarity.**

那就不再是一张“有趣的 demo-selection 图”。

而是一条经验规律。

---

# 七、主实验 2 应该从 2×2 升级成“control surface”

你现在的：

$$

Diagnosticity \times Prior\ Pressure

$$

2×2 是很好的起点。

但 Best-Paper 版本最好不要止步于四个 bar。

我建议将两个变量连续化。

横轴：

$$

\text{Relation Information } I_R

$$

纵轴：

$$

\text{Competing-policy pressure } B

$$

这里的 $B$ 不要只是“我觉得这个 prompt 更强调 reality”。

应该用独立 calibration pool 测：

$$

B(x)
=
\log\frac{P(a_{\text{rival}}\mid x,\text{no diagnostic evidence})}
{P(a_{\text{relation}}\mid x,\text{no diagnostic evidence})}

$$

然后冻结这些 item strata。

这样可以画两张 surface：

**Relation identification surface**

$$

P(\hat R=R)

$$

和

**Relation control surface**

$$

P(Y=Y_R\mid\hat R=R)

$$

如果数据呈现：

当 $I_R$ 上升时，relation identification 很快饱和；

但是在高 $B$ 区域，relation control 仍然崩塌；

那你得到的就不是：

> “一些模型有 commitment gap。”

而是：

> **Identification and behavioral control exhibit distinct psychometric regimes.**

如果不同 model family 经过 baseline-odds normalization 后还能大致落到相似的 control curve 上，这会非常漂亮。

这才是我认为真正接近 **Best Paper signature result** 的东西。

---

# 八、最重要：一定要主动击杀“这只是 calibration”的解释

这一步现在比你 9 月 11 日写计划时重要得多。

那篇 9 月 4 日的新预印本发现，一个模型虽然 free-form 行为完全 collapse，但正确 verdict 其实一直保留到了 output logits；主要问题只是 decision threshold 偏了 **+4.6σ**，一个简单的 scalar correction 就可以把行为从约 50% 修到 81%。citeturn744224academia4

所以 reviewer 完全可能对你说：

> “你的 relation–commitment dissociation 也是这样而已。”

这反而给了你一个极好的 decisive null。

在 held-out calibration set 上拟合：

$$

z'=az+b

$$

或者只允许一个 global bias $b$。

然后重新评估。

如果一个 global scalar adjustment 就能恢复全部 relation-following：

**不要写 relation-control theory。**

它只是 answer calibration。

但如果：

global threshold correction 恢复不了；

temperature scaling 恢复不了；

confidence 也解释不了；

而 relation-policy swap 仍然产生 source-specific failure pattern；

那么你就拥有非常强的结论：

> **The failure is relation-specific, not merely readout miscalibration.**

这个 negative control 我建议直接进入正文，不要放 appendix。

---

# 九、Nature MI 那篇 confidence paper反而给了你一个非常好的模板

这篇 9 月 7 日刚发表的工作为什么漂亮？

因为它没有满足于：

> confidence 可以 decode。

它做了完整链条：

内部 confidence 与 abstention 相关；

然后 activation steering confidence；

abstention 随之变化；

再独立操纵 decision threshold；

证明 Stage 1 representation 和 Stage 2 policy 可以被分开操纵。

你的 Best-Paper 版本也应该追求类似结构：

$$

\text{diagnostic evidence}
\rightarrow
\text{relation state}
\rightarrow
\text{control policy}
\rightarrow
\text{action}

$$

但你的 novelty 是：

**它不是 generic confidence control，而是 content-specific relational control。**

如果 behavioral law 已经非常稳定，再在两个 open-weight model families 上做一次 relation representation intervention，会非常强。

不过我还是建议：

**mechanism 最后做，而不是先做。**

因为你 Paper A 已经给了一个重要警告：Qwen2.5-7B-Instruct 的 directional residual 没有在 Qwen3-8B 和 matched Base 上复制。

所以 Paper B 一定要先保证 behavioral phenomenon 跨 family 稳。

---

# 十、主实验 3：partner adaptation 保留，但改变定位

你现在设计 functional partner adaptation 是对的。

但不要写：

> “Current ToM benchmarks only test QA, and we propose functional ToM.”

这个空间已经被 ICML 2025 的 position paper 和 2026 EnactToM 明确占掉了。前者直接主张真正的 functional ToM 应该表现为根据其他 agent 的行为进行适应；后者已经把它推进到了 embodied multi-agent partial-observability coordination。turn505101view17

你的 partner game 应该被定位为：

> **a causal assay of relational control**

而不是新 benchmark。

最漂亮的设计仍然是：

Partner 有 latent policy：

$$

\pi_A = belief

$$

vs

$$

\pi_B = reality

$$

模型先观察几个 interaction。

consensus interactions 无法区分它们；

diagnostic interaction 可以区分它们。

然后在新人物、新物体、新 answer encoding、新 topology 上要求模型选择一个行动或 communication。

关键不是 absolute score。

关键是：

> **当 partner policy 从 belief-policy swap 到 reality-policy 时，模型自己的最优行动是否随之翻转？**

也就是再次测 `Relation Control Effect`。

这样三个实验用的是**同一个 estimand**。

这非常重要。

---

# 十一、这样三个实验会形成非常漂亮的逻辑闭环

你的论文就不再是：

Experiment 1 一个 ICL effect；
Experiment 2 一个 ToM effect；
Experiment 3 一个 agent game。

而是：

$$

\boxed{
\text{Information}
\rightarrow
\text{Identification}
\rightarrow
\text{Control}
\rightarrow
\text{Action}
}

$$

Experiment 1 回答：

> **什么 evidence 能确定 relation？**

Experiment 2 回答：

> **已确定 relation 什么时候获得 answer control？**

Experiment 3 回答：

> **这种 control 是否真的改变新的行为？**

这就是我觉得你当前项目最可能形成的“大论文形状”。

---

# 十二、我会特别增加三个 reviewer-proof controls

这三个可能决定论文是“很有趣”还是“非常难攻击”。

第一是 **readout-interface control**。不要用“先让模型解释它遵循什么规则”来证明 relation identification，再用 direct answer 来证明 commitment failure。这两个任务的 elicitation interface 本身不同。最好至少同时有 forced source choice、scored source alternatives、native final answer 三种测量，而且 relation-readout 用独立 replicated instances，不污染 native answer。

第二是上面说的 **scalar calibration null**。任何 commitment gap 都必须尝试被一个简单 global threshold/bias/temperature 模型解释。9 月 4 日的新结果让这个 control 变得非常必要。citeturn744224academia4

第三是 **confidence null**。Nature MI 已经表明 confidence 能非常强地预测并因果控制 answer/abstain policy。 所以要问：

$$

RelationControl
\overset{?}{=}
f(confidence)

$$

如果只需要 answer confidence 就能解释你的所有现象，那 relation-control 理论不需要存在。

反过来，如果在 confidence-matched trials 上仍存在 relation-specific policy-swap effect，这会非常有说服力。

---

# 十三、你的跨模型 replication 应该提前，而不是最后再做

这一点我会修改你当前的执行顺序。

因为 Paper A 最值得吸取的经验就是：

**reference checkpoint 上特别漂亮的 law，可能到了另一个 checkpoint 就只剩一部分 shape。**

所以 Paper B 的最小 pilot 一开始就应该覆盖几个独立训练谱系，而不是先在一个 Qwen checkpoint 上把整个 story 做满。

不需要一开始做很多模型。

但至少应该看到：

$$

\text{diagnosticity interaction}

$$

和

$$

\text{policy-swap relation control}

$$

在多个独立 family 上方向一致。

否则就应该尽早收缩 claim。

---

# 十四、我现在会用下面五个 Gate 决定是否继续冲 Best Paper

1. **Information Gate**：relation-information / version-space reduction 必须明显比 example count、similarity 等变量更好地预测 held-out relation identification；否则退回普通 critical-example/ICL story。

2. **Control Gate**：在 relation readout 正确的 trials 中，必须存在系统性的、可重复的 relation-control failure，而且它随 independently calibrated rival-policy pressure 变化；否则没有真正的第二阶段。

3. **Alternative-Explanation Gate**：global threshold、temperature scaling、confidence、response format 不能解释掉主要效应；如果一个 scalar calibration 就解决了问题，就不要叫 relation-control failure。

4. **Swap + Transfer Gate**：改变 governing relation 时行为必须 appropriately flip，而且这个 effect 要跨新 world、answer encoding 或 conflict topology；partner adaptation 里也应出现同一结构。

5. **Replication Gate**：核心 interaction 至少在多个独立 model family 上成立。如果只在一个 checkpoint 成立，就像 Paper A 一样诚实写成 checkpoint-sensitive phenomenon，不要包装成 general law。

这几个 Gate 比再加五个 benchmark 更重要。

---

# 十五、如果这些 Gate 都过了，我认为标题也应该再升级

我不太建议最后真的用：

**Conflict Identifies, Priors Decide**

它很有记忆点，但是“Priors Decide”有点过早假设第二阶段的本质就是 prior。

实际数据可能最后显示第二阶段更像：

decision criterion、default source、output policy、confidence-dependent control，或者它们的组合。

我目前更喜欢：

### **From Evidence to Control: How Language Models Identify and Deploy Relations In Context**

或者，如果 dissociation 特别漂亮：

### **Identifying a Relation Is Not Enough: How In-Context Evidence Gains Control Over Language-Model Behavior**

甚至可以把理论对象直接命名为：

> **Relational Control**

但只有在多个模型、多个 manipulation 上都稳定以后再命名。

---

# 十六、如果数据特别漂亮，你的摘要核心应该只有这一件事

不是：

> We introduce a new ToM benchmark.

不是：

> We find LLMs have a commitment gap.

也不是：

> We probe representations.

而应该接近：

> **Correct demonstrations are informative only insofar as they distinguish competing relations. Across models, relation information predicts which policy is identified, but identification alone does not guarantee behavioral control: under calibrated competing-policy pressure, models can correctly read out the governing relation while acting according to a rival source. This failure survives global readout calibration and confidence controls, and predicts whether models adapt their actions to partners under held-out policy swaps.**

如果你最终真的有数据支撑这段话，我会认为这已经不是“一个更好的 ToM paper”。

它会变成一个关于：

**ICL information → internal relation → decision control → action**

的通用研究。

---

## 我的最终路线判断

和上一次相比，我现在反而**更支持你继续走当前 canonical Paper B**，但我要把 novelty 再收紧一次：

> ~~Identification → Commitment~~
> **Relation Information → Identification → Relation Control → Action**

其中真正最值得抢的两个科学对象是：

$$

\boxed{I_R(D)}

$$

——上下文究竟提供了多少“关系信息”，以及

$$

\boxed{RCE}

$$

——被识别的关系究竟获得了多少行为控制权。

**如果你能证明第一项预测 learning，第二项预测 action，而且第二项不能被 confidence / scalar calibration / response-interface artifact 解释，同时跨 model family 和 policy-swap transfer 成立，我会认为这才是目前这个项目真正有 Best-Paper 潜力的版本。**

相比之下，我现在会明确降低三个方向的优先级：**单独扩展 ACG、以 mechanistic steering 为主线、再造一个更复杂 ToM benchmark**。它们都可以成为支撑材料，但不应该抢走这一条主线。

而且这次最大的好消息是：你当前仓库已经不需要推倒重来。你 9 月 11 日定下的三个主实验基本可以原样保留；真正需要做的是把 **Experiment 2 从“发现 dissociation”升级为“测量并解释 relation-specific control”，并在正文加入 calibration/confidence null**。这一个变化，会显著提高整篇论文的科学辨识度。

如果你接下来要实际开始跑实验，我建议下一步直接做 **最小可证伪 pilot 的完整实验矩阵**，而不是继续扩写研究 proposal：具体冻结哪些 conditions、多少 world、怎样算 $I_R$、怎样校准 prior pressure、怎样定义 RCE、哪些 statistical models，以及出现什么结果立即 kill / pivot。这已经到了应该让数据决定理论能不能成立的阶段。

genui{"suggest_automation":{"label":"每周检查是否出现直接撞车的 ICL、ToM 与 relation-control 新论文"}}

-----

> 这篇文章不是严格意义上的实证论文，而是一篇 **Scientific American 综述型科普论文**：Lera Boroditsky 的 **《How Language Shapes Thought》**。它讨论的是语言学、认知心理学和文化心理学中的经典问题：**我们说的语言，会不会影响我们怎么思考、记忆、感知世界？**
>
> ## 核心观点
>
> 作者的主张可以概括为：
>
> **语言并不只是表达思想的工具，它也会塑造思想本身。**
>
> 但这不是说语言“决定”思想，作者反对那种强硬的“语言决定论”。她的观点更接近现代版本的 **语言相对论**：不同语言会让说话者经常关注不同的信息，因此长期下来，会影响他们的空间感、时间观、记忆方式、因果归因、分类能力，甚至社会态度。
>
> ## 文章主要讲了什么
>
> 文章开头用了一个很有冲击力的例子：作者在澳大利亚 Pormpuraaw 的原住民社区问一个五岁女孩“北方在哪里”，女孩可以立刻准确指出；但她在斯坦福、哈佛、普林斯顿等地方问许多学者同样的问题，很多人却答不上来。
>
> 原因在于当地 Kuuk Thaayorre 语言不常用“左、右”这类相对方向，而是大量使用“东南西北”这样的绝对方向。比如他们会说“杯子在盘子的东南方”，而不是“杯子在盘子左边”。因此，要想正常说话，他们必须随时保持方向感。作者借此说明：**语言的表达习惯会训练人的认知能力**。
>
> ## 第一组证据：语言影响空间认知
>
> 英语等语言常用“左/右/前/后”，这是以人的身体为中心的相对坐标。
>
> 但 Kuuk Thaayorre 等语言使用“东/西/南/北”这样的绝对坐标。结果是，这些说话者即使在陌生环境中，也能更好地保持方向感。
>
> 语言在这里不是简单“命名”空间，而是迫使说话者持续追踪方向。久而久之，这种语言要求变成了一种认知训练。
>
> ## 第二组证据：语言影响时间观
>
> 作者接着说明，空间表达方式还会影响时间想象。
>
> 英语使用者通常把时间从左到右排列；希伯来语使用者更倾向于从右到左排列，这与书写方向有关。
>
> 但 Kuuk Thaayorre 使用者会把时间从 **东向西** 排列。比如面对南方时，时间卡片可能从左到右；面对北方时，则从右到左。也就是说，他们不是用身体左右来组织时间，而是用地理方向组织时间。
>
> 这说明：**时间这种抽象概念，常常借助语言中的空间框架来理解。**
>
> 文章还提到英语中“未来在前、过去在后”，而安第斯地区的 Aymara 语言中则相反：过去在前，因为过去是“看得见、已知的”；未来在后，因为未来不可见。这进一步说明，不同语言可以把时间建构成不同的心理图式。
>
> ## 第三组证据：语言影响因果归因和记忆
>
> 文章讨论了英语、西班牙语、日语在描述事故时的差异。
>
> 英语倾向于说：
>
> > John broke the vase.
> > 约翰打碎了花瓶。
>
> 即使是意外，也容易把行为者说出来。
>
> 而西班牙语、日语在描述意外事件时，更常使用非施事表达，比如“花瓶碎了”，不一定强调“是谁弄碎的”。
>
> 作者和学生做了实验：让英语、西班牙语、日语使用者观看一些人故意或意外打破东西、弄洒饮料的视频，然后测试他们记不记得是谁做的。
>
> 结果是：对于故意行为，三组人都记得差不多；但对于意外事件，英语使用者更容易记住“是谁造成的”，而西班牙语和日语使用者记得较差。
>
> 这不是因为后两组记忆力差，而是因为他们的语言在描述事故时不那么强调施事者。也就是说：
>
> **语言影响我们注意什么，也影响我们之后记住什么。**
>
> ## 第四组证据：语言影响学习和分类
>
> 文章还提到数字系统。中文、普通话等语言中的数字结构更透明，例如“十一”就是“十加一”，“十二”就是“十加二”。相比之下，英语的 eleven、twelve、thirteen 等形式不那么规则。
>
> 因此，使用更透明数字系统的儿童，可能更早理解十进制结构。
>
> 作者还提到性别标记。希伯来语大量标记性别，芬兰语几乎没有语法性别，英语居中。研究发现，希伯来语儿童更早意识到自己的性别身份，芬兰语儿童较晚，英语儿童介于两者之间。
>
> 这说明语言的语法结构不仅影响成年人，也可能影响儿童概念发展的速度。
>
> ## 第五组证据：双语者会随语言切换认知框架
>
> 文章还提到双语者研究。双语者在使用不同语言时，对世界的反应也会发生变化。
>
> 例如阿拉伯语—希伯来语双语者，在希伯来语环境下测试时，对犹太名字的隐性态度更积极；在阿拉伯语环境下测试时则不同。
>
> 这说明语言不仅影响空间、时间、记忆，也可能影响社会联想、群体态度和情感反应。
>
> ## 作者最后的结论
>
> 文章最后提出一个非常重要的判断：
>
> **思想不是纯粹非语言的，也不是完全由语言构成的；成人的许多思维过程，是语言过程和非语言过程交织在一起的。**
>
> 作者认为，每一种语言都像一种“认知工具箱”。它包含了一个文化长期积累下来的分类方式、注意方式和世界观。学习一种语言，不只是学习词汇和语法，也是在学习一种看世界的方法。
>
> ## 用语言学角度看，这篇文章的意义
>
> 这篇文章实际上是在为现代语言相对论辩护。
>
> 传统的 Sapir-Whorf 假说常被理解为“语言决定思想”，所以后来受到很多批评。但 Boroditsky 的文章并没有说语言绝对限制思想，而是说：
>
> **语言会提高某些认知路径的可用性，让某些区分更显眼，让某些记忆更容易形成。**
>
> 比如：
>
> 中文亲属称谓迫使说话者区分舅舅、叔叔、伯伯、姑父等关系；
> 英语时态迫使说话者经常标记时间；
> 俄语动词可能要求标记说话者性别；
> 某些语言要求说话者明确证据来源、方向、数量或亲属关系。
>
> 这些语言要求会反复训练注意力。久而久之，它们就可能影响思维习惯。
>
> ## 一句话总结
>
> 这篇文章讲的是：**不同语言会让人以不同方式关注世界、组织经验、记忆事件和理解抽象概念；语言不是思想的牢笼，但它确实是塑造思想的重要工具。**
> 从语言学专家角度说，**现代语言相对论**可以理解为：
>
> > **语言不会决定人能不能思考某件事，但会影响人更容易注意什么、怎样分类经验、如何记忆事件，以及倾向于用什么方式理解世界。**
>
> 它是对传统 **Sapir-Whorf 假说** 的修正和发展。
>
> ## 一、它不是“语言决定论”
>
> 早期语言相对论常被误解为：
>
> > 你说什么语言，就只能怎么想。
>
> 这叫 **强语言决定论**。现代语言学一般不接受这种说法。
>
> 比如，英语没有像中文那样区分“叔叔、伯伯、舅舅、姑父、姨父”的日常基本词，但英语使用者当然能理解这些亲属关系；只是他们在日常表达中不一定必须区分得那么细。
>
> 所以现代语言相对论强调的不是“不能想”，而是：
>
> > **语言会让某些区分变得更自然、更显眼、更常被注意到。**
>
> ## 二、现代语言相对论的核心观点
>
> 现代语言相对论主要关心几个问题：
>
> **第一，语言影响注意力。**
> 如果一种语言在说话时必须标记方向、性别、证据来源、时间、亲属关系等，说话者就会经常注意这些信息。
>
> 例如某些澳大利亚原住民语言不用“左、右”，而常用“东、西、南、北”。说这种语言的人为了正常交流，必须持续知道自己面向哪里。久而久之，方向感可能会特别强。
>
> **第二，语言影响分类方式。**
> 不同语言会把世界切分成不同类别。
>
> 比如颜色词、亲属称谓、空间关系、动作类型，在不同语言中并不完全对应。语言中的分类系统会影响说话者在判断相似性、记忆对象、归类经验时的倾向。
>
> **第三，语言影响记忆。**
> 如果一种语言描述事件时经常强调“谁做了什么”，说话者可能更容易记住施事者。
>
> 例如英语常说：
>
> > John broke the vase.
> > 约翰打碎了花瓶。
>
> 而西班牙语或日语在意外事件中更可能说类似“花瓶碎了”。这种表达差异可能影响人们事后是否记得“是谁造成的”。
>
> **第四，语言影响抽象概念。**
> 时间、空间、因果、责任、性别、数量等抽象概念，常常通过语言中的隐喻和语法结构来组织。
>
> 比如有些语言把时间理解为从左到右，有些语言受书写方向影响会从右到左排列时间；还有些语言会用东西南北来排列时间。
>
> ## 三、现代语言相对论和传统版本的区别
>
> 可以这样区分：
>
> | 类型      | 主张                        | 现代语言学态度    |
> | ------- | ------------------------- | ---------- |
> | 强语言决定论  | 语言决定思想；没有某个词就不能思考某个概念     | 基本不被接受     |
> | 弱语言相对论  | 语言影响思维习惯、注意力和记忆方式         | 现代研究较支持    |
> | 现代语言相对论 | 语言是认知工具，会塑造某些认知倾向，但不是思想牢笼 | 当前较有影响力的观点 |
>
> 现代语言相对论的关键词不是“决定”，而是：
>
> **影响、引导、偏向、训练、凸显。**
>
> ## 四、它关注的是“语言使用”而不仅是“语言结构”
>
> 现代研究不只看某种语言有没有某个词，还看人在实际说话时反复做什么。
>
> 比如英语说话者常常必须标记时态：
>
> > I walked.
> > I am walking.
> > I will walk.
>
> 这会让时间关系在英语表达中非常显眼。
>
> 中文当然也能表达过去、现在、未来，但中文不一定每个动词都强制标记时态。因此，中英文说话者并不是谁能不能理解时间，而是语言系统对时间信息的“强制关注程度”不同。
>
> ## 五、从语言学角度看，它的理论意义
>
> 现代语言相对论的重要意义在于，它反对两个极端。
>
> 一个极端是：
>
> > 思想完全独立于语言，语言只是给已有思想贴标签。
>
> 另一个极端是：
>
> > 思想完全被语言决定，人无法超出母语思考。
>
> 现代观点介于二者之间：
>
> > **语言既不是思想的全部，也不是思想的外壳；语言是人类认知系统的一部分，会参与塑造我们理解世界的方式。**
>
> 也就是说，说话者并不是被母语“困住”，但母语确实会反复训练他们的注意模式和表达习惯。
>
> ## 六、用一句话概括
>
> **现代语言相对论认为：不同语言不会决定人能不能思考什么，但会影响人更习惯注意什么、怎样组织经验、如何记忆事件，以及倾向于用什么方式理解世界。**
>
> 放在你前面读的 Boroditsky 那篇文章中，它的核心就是：**语言不是思想的牢笼，而是一套认知工具；不同语言提供不同的工具，因此也会塑造不同的思维习惯。**
> 如果从你前面读的 Boroditsky 那条“现代语言相对论”路线继续往下看，到 **2024–2026 年的计算语言学**，研究已经发生了一个很重要的变化：
>
> > **问题已经不只是“不同语言会不会让人思考得不一样”，而是开始试图计算、定位、操纵这种“语言所带来的认知/概念差异”。**
>
> 但有一个非常重要的边界：**计算语言学目前已经相当擅长证明“不同语言对应不同的语义表征、文化关联和推理偏向”，却还不能仅凭语言模型证明“语言导致了人类认知差异”。**
>
> 换句话说，现在大约走到了 **“大规模量化 → 模型内部机制 → 初步因果干预”** 这一步，而“对人类认知的完整因果解释”仍然没有完成。
>
> ---
>
> ## 一、第一步已经基本实现：把“语言切分世界的方式”计算出来
>
> 传统语言相对论研究最大的问题之一，是规模很小。
>
> Boroditsky 的经典研究可能比较：
>
> * 英语 vs 西班牙语；
> * 英语 vs 俄语；
> * Kuuk Thaayorre vs 英语；
> * 几十到几百个实验参与者。
>
> 计算语言学改变了这一点。
>
> 现在可以把几十、几百种语言放进同一个计算框架，问：
>
> > **同一个概念，在不同语言的“语义空间”里是不是位于不同的位置？**
>
> 核心技术就是：
>
> **word embeddings → contextual embeddings → multilingual LLM representations**
>
> 例如假设有：
>
> > English: uncle
> > Chinese: 叔叔、伯伯、舅舅、姑父、姨父
>
> 传统上我们会说：
>
> > 中文亲属系统做了更多词汇区分。
>
> 现在计算语言学会进一步问：
>
> > 这些概念在中文语义空间里的距离结构，与英语中的距离结构到底差多少？
>
> 也就是说，研究对象从：
>
> **“有没有这个词”**
>
> 推进到：
>
> **“整个概念空间是怎样被组织的”。**
>
> 2024 年 Karidi、Grossman 和 Abend 就利用 contextual embeddings 去测量不同语言中翻译对应词究竟有多大程度真正“语义对齐”，并专门使用亲属词的 lexical gaps 作为验证对象。([ACL Anthology][1])
>
> 更进一步，2026 年 Jung、Cohen 和 Bergen 已经利用多语 LLM 的 embedding **自动发现 lexical gaps（词汇空缺）**。他们发现，在绝大多数构造出的语义空间中，真正存在词汇空缺的概念，其跨语言表示确实表现出更弱的语义对齐。([ACL Anthology][2])
>
> 所以今天已经可以在很大程度上计算：
>
> > **语言A怎样划分概念空间，与语言B怎样划分概念空间。**
>
> 这其实就是传统语言相对论最核心的问题之一的计算版本。
>
> ---
>
> # 二、第二步：已经从“词不同”推进到“概念结构不同”
>
> 这一步理论上更加重要。
>
> 早期语言相对论很容易被误解成：
>
> > “某种语言有这个词，所以说这种语言的人有这个概念。”
>
> 现代计算语言学已经基本不这么简单处理问题。
>
> 现在研究的是所谓：
>
> ### cross-lingual semantic alignment
>
> 即：
>
> > **不同语言里的“意义空间”到底是不是同构的？**
>
> 过去很多 multilingual NLP 模型实际上有一个隐含假设：
>
> > dog = 狗 = chien = perro
>
> 因此只要把各种语言映射到一个共同 semantic space 就可以。
>
> 可是近年的研究越来越发现：
>
> > **这种“完全共享的语言无关语义空间”只是近似，而不是事实。**
>
> 例如 2024 年对 mBERT、XLM-R、mT5 等模型的分析显示，它们确实会在较深层形成更跨语言共享的概念表示，但不同语言并没有因此完全变成同一个语义系统。([ACL Anthology][3])
>
> 2026 年针对 10 种东南亚语言的大规模 embedding benchmark 更直接指出：所谓 perspective-independent、language-independent semantic space 的假设，在实践中并不成立；不同语言、不同任务形成明显不同的表示结构。([ACL Anthology][4])
>
> 这件事情和现代语言相对论其实高度吻合：
>
> > **人类也许共享大量概念，但语言会改变这些概念之间的边界、距离和显著性。**
>
> 所以现在研究重点已经从：
>
> > Does language give us concepts?
>
> 转向：
>
> > **How does language reshape conceptual geometry?**
>
> 我认为这是计算语言学对语言相对论最重要的推进之一。
>
> ---
>
> # 三、第三步：开始研究“换一种语言，推理本身会不会变化”
>
> 这一步就非常接近你读的 Boroditsky 了。
>
> 现在研究者可以做一个以前很难大规模完成的实验：
>
> 让完全相同的模型：
>
> > 用英语思考一次
> > 用中文思考一次
> > 用日语再思考一次
>
> 然后比较输出。
>
> 注意这里模型参数不变。
>
> 改变的只是：
>
> **language context。**
>
> 于是便出现了一个非常有意思的问题：
>
> > 如果“同一个大脑”只是换一种语言，推理习惯发生改变，这是不是一种计算版本的语言相对论？
>
> 近年的答案是：
>
> **确实经常会改变。**
>
> 例如 2025 年发表在 *Nature Human Behaviour* 的研究比较生成式 AI 在中文和英文条件下的表现，发现同一个 GPT 系统使用中文时表现得更偏向 **interdependent social orientation（相互依存型社会取向）** 和 **holistic cognitive style（整体型认知风格）**；使用英语时则更偏向独立型和分析型倾向。([Nature][5])
>
> 这和人类文化心理学里经典的：
>
> > East Asian holistic cognition
> > vs.
> > Western analytic cognition
>
> 出现了相似结构。
>
> 这当然还不能说明“中文使人变得整体主义”，因为语言、文化和训练数据纠缠在一起。
>
> 但它至少说明：
>
> > **输入语言本身可以成为激活不同认知/文化表征的条件变量。**
>
> ---
>
> # 四、甚至已经开始研究：语言是否改变“因果推理”
>
> 这和你之前看到 Boroditsky 关于：
>
> > John broke the vase
> > vs
> > The vase broke
>
> 那个实验尤其有关。
>
> 2025 年 EMNLP 的一项研究直接构造了一个中英双语因果推理数据集，尽量保持语义内容一致，只改变语言和因果结构。
>
> 研究发现，同一个 LLM 在中文和英文中出现不同的注意模式和因果成分顺序偏好：
>
> * 中文条件下，对“原因”和句首因果连接成分的关注更强；
> * 英文表现得相对均衡；
> * 模型还会把某种语言中习得的典型因果顺序强加到非典型句子上。
>
> 也就是说：
>
> > **语言形式不仅改变生成表面形式，也可能改变模型处理因果结构的路径。** ([ACL Anthology][6])
>
> 这已经非常接近传统语言相对论提出的问题：
>
> > 不同语言会不会使同一个事件得到不同的注意和因果分析？
>
> 只不过实验对象从 human subjects 变成了 computational models。
>
> ---
>
> # 五、更前沿的一步：开始在神经网络内部寻找“语言/文化在哪里”
>
> 这几年真正的新东西在这里。
>
> 过去语言相对论主要只能观察：
>
> > 输入 → 人的行为
>
> 现在有 LLM以后，可以研究：
>
> > 输入 → 内部神经表征 → 输出。
>
> 于是研究开始问：
>
> > 中文和英文到底激活了哪些不同 neuron / representation？
>
> 2025 年 ACL 的研究就开始明确拆分：
>
> **Language**
>
> 和
>
> **Culture**
>
> 两个变量。
>
> 研究者发现一种所谓：
>
> ### Cultural-Linguistic Synergy
>
> 也就是：
>
> > 当问题的文化背景与提问语言匹配时，模型往往表现得更好。
>
> 比如某种文化知识如果用与该文化高度相关的语言表达，内部会出现更明显的相关神经元激活。([ACL Anthology][7])
>
> 另外还有研究试图进一步区分：
>
> * language-specific neurons
> * culture-specific neurons
>
> 2025 年的一项研究甚至报告，不同文化信息可以对应相对不同的神经元群体，而且可以通过干预这些神经元来改变模型的文化输出，而不必完全改变语言能力。([ACL Anthology][8])
>
> 这代表研究问题发生了非常大的变化。
>
> 以前我们只能说：
>
> > “语言和文化可能影响认知。”
>
> 现在开始问：
>
> > **这种影响在一个人工神经认知系统中究竟以什么表征形式存在？**
>
> ---
>
> # 六、这反而暴露出了现代语言相对论最大的老问题：语言还是文化？
>
> 这可能是今天最重要的问题。
>
> 比如发现：
>
> > ChatGPT 中文回答更整体主义，英文回答更个人主义。
>
> 你不能马上得出：
>
> > 中文语法 → 整体主义思维。
>
> 因为还有另一种解释：
>
> 中文训练语料主要来自中文文化环境；
>
> 英文训练语料主要来自英语文化环境。
>
> 所以你真正观察到的可能是：
>
> [
> Language + Culture + Corpus + Training\ Distribution
> ]
>
> 而不只是：
>
> [
> Language
> ]
>
> 这正是当下计算语言学最努力解决的问题。
>
> 所以近年的论文标题已经很直接：
>
> > **Disentangling Language and Culture**
>
> 即“把语言和文化拆开”。([ACL Anthology][7])
>
> 这其实也是对 Boroditsky 路线非常重要的修正。
>
> 因为现代研究越来越意识到：
>
> > **语言并不是独立漂浮在文化之外的变量。**
>
> 它和：
>
> * 社会环境
> * 教育
> * 历史
> * 价值系统
> * 写作传统
> * 语料分布
>
> 高度共变。
>
> ---
>
> # 七、还有一个非常有趣的新发现：多语言模型会发生“认知串味”
>
> 假设一个人会中文和英文。
>
> 传统双语语言相对论会问：
>
> > 使用中文的时候是不是激活中文认知框架？
> > 使用英文是不是激活英文认知框架？
>
> 现在 multilingual LLM 给出了一个计算版本。
>
> 研究发现，多语言模型里面不同语言的文化表征并不是完全隔离的，而会发生：
>
> ### cross-lingual cultural transfer
>
> 也就是一种语言的数据会改变另一个语言条件下的价值表示。
>
> 2024 年 ACL 的研究把这种现象称作不同语言之间的 **cultural value shifts**：微调一种语言的数据后，其他语言条件下表现出的价值倾向也可能跟着发生变化。([ACL Anthology][9])
>
> 所以今天可能需要把现代语言相对论升级成一个更动态的模型：
>
> 不是简单：
>
> [
> Language \rightarrow Thought
> ]
>
> 而更像：
>
> [
> Language\ Experience
> \rightarrow
> Conceptual\ Representation
> \rightarrow
> Attention / Retrieval / Reasoning
> ]
>
> 而双语或多语经验又会使这些表征互相影响。
>
> ---
>
> # 八、因此，今天的计算语言学实际上把问题推进到了三个层次
>
> 可以这样看它的演进：
>
> | 阶段        | 问题                    | 当前状态     |
> | --------- | --------------------- | -------- |
> | 传统语言相对论   | 不同语言的人想法是否不同？         | 大量行为实验   |
> | 早期计算研究    | 不同语言的词义结构是否不同？        | 已可大规模测量  |
> | 当前 LLM 研究 | 换一种语言是否改变模型推理、价值、注意？  | 已发现大量效应  |
> | 最新机制研究    | 差异在模型内部怎样编码？          | 正快速发展    |
> | 最终问题      | 语言是否**因果性地**改变人类认知机制？ | **仍未解决** |
>
> 所以如果让我给一句学术上的判断：
>
> > **计算语言学已经很好地建立了“语言特定的概念表征确实存在”这一层证据，也越来越能够证明语言上下文会改变人工模型的检索、价值判断和推理行为；但它尚未解决“语言本身是否造成了人类认知差异”这个经典因果问题。**
>
> ---
>
> # 九、这其实让现代语言相对论发生了一次理论升级
>
> Boroditsky 那一代实验最典型的理论链条是：
>
> [
> \text{语言结构}
> \rightarrow
> \text{注意习惯}
> \rightarrow
> \text{认知差异}
> ]
>
> 而今天计算语言学逐渐提出一个更细的版本：
>
> [
> \text{Language}
> \rightarrow
> \text{statistical experience}
> \rightarrow
> \text{representation geometry}
> \rightarrow
> \text{retrieval / attention}
> \rightarrow
> \text{reasoning}
> \rightarrow
> \text{behavior}
> ]
>
> 也就是说：
>
> **语言可能不是直接“改变思想”。**
>
> 更可能发生的是：
>
> > 一个人反复生活在某种语言的统计结构中，于是某些概念关系被反复强化；
> > 这些关系逐渐形成不同的 semantic representation；
> > 当人进行判断时，不同的信息因此具有不同的可访问性和显著性；
> > 最终形成不同的认知倾向。
>
> 这个解释比简单的：
>
> > “中文让中国人这样想，英语让英国人那样想”
>
> 科学得多。
>
> ---
>
> ## 十、如果把目前整个领域压成一句话
>
> **20 世纪语言相对论问：**
>
> > Language shapes thought?
>
> **Boroditsky 那一代问：**
>
> > Language influences attention and cognition in measurable ways?
>
> 而今天的计算语言学正在问：
>
> > **Can we measure, model, localize and intervene on the language-specific representations that may give rise to those cognitive differences?**
>
> 也就是：
>
> > **语言塑造思维 → 语言如何塑造表征 → 表征如何改变推理。**
>
> 我觉得这才是这几年真正发生的理论进步。
>
> 而且这里有一个特别值得继续追下去的点：**LLM 本身其实成了现代语言相对论一个非常有意思的“计算实验动物”。** 因为我们第一次拥有一个可以保持“同一个神经网络”不变、只切换中文/英文/日文，然后观察其内部表征和推理过程如何变化的系统。这使得很多在人类实验里几乎无法严格控制的变量，现在至少可以在模型里做干预实验。([Nature][10])
>
> 如果你准备真正系统研读这个领域，我下一步会建议把它分成 **“词汇相对论 → 语义空间 → 多语模型 → LLM reasoning → language/culture disentanglement”** 五条文献线来看。这样就能非常清楚地看到，从 Sapir–Whorf 到 Boroditsky，再到现在 LLM 时代，理论究竟是怎么一步一步演化过来的。
>
> [1]: https://aclanthology.org/2024.findings-emnlp.932/?utm_source=chatgpt.com "Locally Measuring Cross-lingual Lexical Alignment: A Domain and Word Level Perspective - ACL Anthology"
> [2]: https://aclanthology.org/2026.conll-main.39/?utm_source=chatgpt.com "Discovering Lexical Gaps Using Embeddings from Multilingual LLMs - ACL Anthology"
> [3]: https://aclanthology.org/2024.acl-long.344/?utm_source=chatgpt.com "Exploring Alignment in Shared Cross-lingual Spaces - ACL Anthology"
> [4]: https://aclanthology.org/2026.acl-long.397/?utm_source=chatgpt.com "SEA-BED: How Do Embedding Models Represent Southeast Asian Languages? - ACL Anthology"
> [5]: https://www.nature.com/articles/s41562-025-02242-1?utm_source=chatgpt.com "Cultural tendencies in generative AI | Nature Human Behaviour"
> [6]: https://aclanthology.org/2025.findings-emnlp.1321/?utm_source=chatgpt.com "Under the Shadow of Babel: How Language Shapes Reasoning in LLMs - ACL Anthology"
> [7]: https://aclanthology.org/2025.acl-long.1082/?utm_source=chatgpt.com "Disentangling Language and Culture for Evaluating Multilingual Large Language Models - ACL Anthology"
> [8]: https://aclanthology.org/2025.findings-ijcnlp.45/?utm_source=chatgpt.com "Isolating Culture Neurons in Multilingual Large Language Models - ACL Anthology"
> [9]: https://aclanthology.org/2024.acl-long.803/?utm_source=chatgpt.com "The Echoes of Multilinguality: Tracing Cultural Value Shifts during Language Model Fine-tuning - ACL Anthology"
> [10]: https://www.nature.com/articles/s44159-026-00582-x?utm_source=chatgpt.com "Machine ‘culture’ beyond language | Nature Reviews Psychology"

你这段梳理其实已经碰到了一个很有潜力的交叉点：**语言相对论关注“语言经验如何改变表征与注意”，而你现在的 LP4FM / relation-control 路线关注“关系表征如何获得行为控制权”。** 我先把你列出的几篇近年计算语言学工作核一下，然后直接告诉你：这个交叉点哪些是真正新问题，哪些只是换个语言包装已有结论。

你这段梳理的总体方向是对的，但如果把它用于严肃论文或用来启发你现在的 LP4FM 项目，我会做一个非常重要的收紧：

> **当前计算语言学真正能支持的，不是“LLM 证明了语言相对论”，而是：语言条件能够系统性改变同一模型的表征、检索和决策行为；我们现在开始有能力把这种变化定位到计算链条的不同阶段。**

这个区别非常重要。而且它恰好能和你现在仓库的新主线产生一个我认为相当漂亮的研究问题。

### 先说你这段里哪些判断很稳

你引用的主干文献基本成立。Karidi et al. 2024 确实把跨语言 lexical alignment 从“全局空间对齐”推进到 domain/word-level，并用亲属词 lexical gaps 做自然验证；Jung et al. 2026 又进一步显示 lexical gaps 在多语 LLM embedding 中往往对应更弱的跨语言对齐。

“共享但并不完全 language-independent 的 semantic space”这个判断也合理。ACL 2024 的工作发现深层 representation 的跨语言 alignment 增强，但不是简单地说所有语言最后都变成同一个空间；2026 SEA-BED 更明确说明 embedding 模型在不同语言×任务组合上的表现高度不均一。

你提到的“换语言后，同一个模型会表现出不同 cognitive/cultural tendency”也有比较扎实的直接证据。Nature Human Behaviour 2025 的工作报告 GPT 和 ERNIE 在中文条件下相较英文表现出更 interdependent、holistic 的倾向。 ACL 2025 的 *Under the Shadow of Babel* 则更直接研究了中英语境中的 causal reasoning，并报告 language-specific causal-order preference、attention pattern 等差异。

语言和文化必须拆开的判断也完全正确。ACL 2025 的 *Disentangling Language and Culture* 本身就是在分别操纵 linguistic medium 与 cultural context，并报告所谓 Cultural-Linguistic Synergy。 Fine-tuning 中的跨语言 cultural-value bleed-over 也已有直接研究。

---

## 但有三个地方，我会比你现在写得更加谨慎

第一个是：

> **“同一个神经网络，只切换语言，因此像一个特别干净的计算实验动物。”**

这个说法非常诱人，但还不够严格。

参数确实不变，但“英语 prompt → 中文 prompt”并不是一个只改变抽象变量 `LANGUAGE` 的纯 intervention。它同时改变了：

token sequence、tokenization、training-frequency regime、lexical statistics、syntactic distribution、文化共现、alignment 数据分布，甚至 instruction-tuning coverage。

所以：

$$

\text{switch language}
\neq
do(\text{Language only})

$$

它比跨人群实验更可控，但还远不是完美 causal identification。

第二个是：

> embedding geometry 不同 ≠ cognition 不同。

Lexical-gap、cross-lingual alignment 工作证明的是**模型表示结构并非完全语言无关**。 这与语言相对论非常契合，但不能直接推导：

> “这就是不同语言造成不同 conceptual systems 的计算证明。”

因为 embedding geometry 还同时吸收 corpus frequency、tokenization 和训练目标。

第三个是所谓“culture neurons”。相关工作确实报告文化信息在不同 neuron populations 上具有一定可定位性，并进行 intervention。 但论文里最好写：

> **culture-related model representations can be functionally localized and manipulated**

而不要写成：

> “我们找到了模型里的文化神经元，因此文化就是这样储存的。”

尤其不能从它跳到人脑。

---

# 真正让我觉得有意思的是：它与你现在的项目可以产生一个非常锋利的交叉问题

我刚重新看了你当前仓库。

你现在其实已经进一步把 Paper B 的母对象冻结成了：

> **context-conditioned rule updating**

并且强调：

> **rule-change without fact-change**

也就是固定 latent world，只改变 governing rule / source-authority relation，看模型行为是否发生正确变化。

这个思路和语言相对论放在一起后，可以出现一个非常漂亮的新 decomposition：

$$

\boxed{
\text{fact}
\times
\text{rule}
\times
\text{language}
}

$$

固定 fact。

分别改变 rule 和 language。

然后问：

> **语言改变的是模型认为“规则是什么”，还是改变已经识别出的规则最终获得多少行为控制权？**

这比：

> 中文 ToM 比英文 ToM 好还是差？

高级很多。

因为 **multilingual ToM benchmark 这个空间已经有人做了**。ACL 2026 的 XToM 已经覆盖五种语言，并发现模型虽然 multilingual comprehension 很强，但 ToM 表现仍会随语言变化。

跨语言 reasoning consistency 也不是空白：已有工作发现同一个问题在不同语言下会产生相当大的答案冲突。

所以千万不要做：

> “We find ToM differs across languages.”

这已经不够新。

---

# 我认为真正有潜力的问题是：**Language-conditioned relational control**

把你的模型写成：

$$

D,L,W
\rightarrow
\hat R
\rightarrow
Y

$$

其中：

- $W$：latent world/facts；
- $D$：demonstrations；
- $L$：语言；
- $R$：真正 governing relation；
- $\hat R$：模型识别出的 relation；
- $Y$：最终行动。

然后进行严格 counterfactual。

同一个 world：

> Alice believes object=A
> Reality=B

同一个 governing policy：

> answer according to Alice's belief.

只切：

English → Chinese → Spanish → Japanese …

这时候你不只是问 accuracy 是否变。

你可以问两个完全不同的问题。

### 第一层：语言改变 relation identification 吗？

测：

$$

P(\hat R=R\mid L)

$$

如果中文和英文不同，那么语言影响的是：

> **模型怎样从 evidence 推断 governing relation。**

这是 induction/identification effect。

### 第二层：relation 已经识别正确后，语言还改变行为吗？

测：

$$

P(Y=Y_R\mid\hat R=R,L)

$$

如果 relation readout 在中英文中都正确，但最终答案仍随语言变化：

这就是一个非常漂亮的：

> **language-conditioned control effect**

也就是说：

> 语言并没有改变模型“知道应该遵循什么关系”，却改变了这个关系能否压过 competing source/default policy。

这就直接把 Boroditsky 式的问题从：

$$

Language\rightarrow Thought

$$

变成一个可以实验分解的：

$$

Language
\rightarrow
\begin{cases}
Relation\ identification\\
Relation\ control
\end{cases}
\rightarrow
Behavior

$$

我觉得这里是真正有味道的。

---

# 更漂亮的是，它可以直接利用你现在的 Conflict Is Supervision

你现在已经非常强调一个概念：

**diagnostic examples 提供的是 competing-policy separation，而不是单纯正确示例。**

你的 related-work map 也明确区分：

`within-example source conflict`

和

`across-example policy conflict`。

那么你可以问一个非常漂亮的问题：

> **Diagnostic relational evidence 能否消除 language-conditioned cognitive bias？**

假设没有 diagnostic evidence 时：

Chinese context：

$$

P(\text{belief source})=.7

$$

English context：

$$

P(\text{reality source})=.7

$$

这可以理解成不同 language context 激活了不同默认 prior。

然后逐渐增加 relation-information $I_R(D)$。

你最终可能观察到三种完全不同的理论结果。

**结果 A：语言效应随着 diagnostic information 增加而消失。**

那么解释非常漂亮：

> Language primarily alters relational priors; sufficiently diagnostic evidence overrides them.

即：

$$

Language\rightarrow Prior

$$

而不是语言重写了 abstract reasoning machinery。

**结果 B：identification 相同，但 control gap 仍然存在。**

那么得到的是：

> Language changes how much control an identified relation obtains over behavior.

这其实比一般“language shapes reasoning”更精确。

**结果 C：即使极强 diagnostic evidence，relation identification 本身仍跨语言系统不同。**

这才比较接近：

> language-specific reasoning induction.

这三个理论是可以被实验区分的。

这就是我特别喜欢这条线的地方。

---

# 甚至可以形成你项目里非常漂亮的一对 counterfactual

你当前最重要的对象是：

> **rule-change without fact-change**。

加入 multilingual 后，再有：

> **language-change without fact-change or rule-change**

于是你拥有：

$$

\begin{array}{lll}
\textbf{World fixed} & \textbf{Language fixed} & \textbf{Rule changes}\\
\textbf{World fixed} & \textbf{Rule fixed} & \textbf{Language changes}
\end{array}

$$

第一种检测：

> 模型是否真正更新 rule。

第二种检测：

> 模型是否把 surface linguistic context 错当成 rule/prior evidence。

然后还有最强的第三种：

> **rule 和 language 反向操纵。**

例如：

英文 context 根据模型 baseline 更倾向 reality；

但 demonstrations 明确识别 belief rule。

中文 context baseline 更倾向 belief；

但 demonstrations 明确识别 reality rule。

这样就能真正把：

$$

Language\ prior

$$

和

$$

Context-induced\ rule

$$

正交化。

这比简单测 multilingual consistency 强太多。

---

# 我甚至觉得它提供了一种更现代的“计算语言相对论”定义

不要定义为：

> 语言 A 的模型回答与语言 B 不一样。

那个太宽。

可以定义成一个 counterfactual quantity：

$$

LCE
=
P(Y_R\mid L_1,R,W,D)
-
P(Y_R\mid L_2,R,W,D)

$$

其中 $R,W,D$ 都被语义等价地冻结。

然后再分解为：

$$

LCE =
LIE + LCE_{\text{control}}

$$

概念上分别代表：

**Language-induced Identification Effect**

和

**Language-conditioned Control Effect**。

你甚至可以问：

$$

L
\rightarrow \hat R \rightarrow Y

$$

是不是主要 mediation path，

还是：

$$

L\rightarrow Y

$$

在控制 $\hat R$ 后仍然存在。

这已经是非常具体、可证伪的科学问题。

---

# 这条线和现有文献的真正区别会在哪里

现有工作大致已经证明：

**语言改变输出。**
Nature Human Behaviour 2025。

**语言改变因果推理 pattern。**
BICAUSE。

**语言和文化可以部分分解。**
ACL 2025。

**不同语言的 ToM accuracy 不一致。**
XToM 2026。

**同一事实、关系在不同语言中可能回答不一致。**
NAACL 2025 等。

而你的问题可以是：

> **When language changes reasoning behavior, which computational stage actually changed—the inferred governing relation or the control exerted by that relation?**

我目前没有看到你刚列的这些工作把这个 distinction 做成核心 estimand。

这才是潜在 novelty。

---

# 但我不会现在把 multilingual 变成你 Paper B 的新主线

这一点非常重要。

你现在仓库的设计其实比几天前更加成熟：它已经冻结了母对象，明确写着当前新研究的核心是 `context-conditioned rule updating`，并且 proposal 仍是 `not_evaluated`，先过 native swap、validity、held-out transfer 和 functional action gates。

**不要因为语言相对论很有趣，就把正在收敛的 Paper B 又扩成一个超级大项目。**

我的建议是：

> 把 multilingual / linguistic relativity 暂时当作一个极强的 **external generalization test**，而不是第四条主 contribution。

先把你的 English / canonical-language 核心现象跑出来。

如果：

`relation information → identification → control → action`

成立，

再加入：

$$

L_1 \leftrightarrow L_2

$$

作为一个非常漂亮的 robustness/theory experiment：

> 同一个 relational computation 是否保持 language-invariant？

如果发现不保持，再升级成独立研究。

---

## 如果真的发现强效应，它甚至可能成为下一篇比“ToM multilingual benchmark”更有意思的论文

我会把问题命名为：

### **Linguistic Relativity of Control**

不是：

> Does language change what LLMs think?

而是：

> **Does language change which represented relation controls what LLMs do?**

这句话其实把 Boroditsky 和你现在的研究非常自然地接上了。

Boroditsky 的理论语言是：

$$

Language
\rightarrow
Attention
\rightarrow
Cognition

$$

你现在有机会把它在人工模型里精确成：

$$

\boxed{
Language
\rightarrow
Relational\ Prior
\rightarrow
Identification
\rightarrow
Control
\rightarrow
Action
}

$$

而且每一箭头都可以设计 counterfactual intervention 去拆。

**这会比泛泛地研究“中文模型和英文模型想法不一样”科学价值高很多。**

所以，我对你这段文献总结最大的反馈不是“哪里需要补一篇论文”，而是：**你已经抓到了一个可以与当前 LP4FM 形成非常深连接的理论传统；但最有价值的连接点不是 semantic geometry，而是“语言是否改变关系的推断，还是改变关系对行为的控制权”。**

这条问题，我会认真保留下来。

-----

> 根据这个文献总结继续往下推演研究方案，也及时做出研究方案的切割与划分，我们可以多阶段做出多篇研究

可以，而且我建议这次不要再把它当成“一篇大论文怎么塞更多实验”，而是把整个方向拆成一个**连续的研究计划**：前一篇给后一篇提供可复用的任务、estimand 和理论对象，每篇都能独立成立，同时又逐步把问题从行为规律推进到跨语言、因果机制和人机认知对照。我先按当前文献版图和你现有 LP4FM / relation-control 基础，把哪些问题应该合并、哪些必须切开重新划界。

我已经把切割原则先定下来了：**“自然语言切换”与“语言结构的因果来源”必须分成两篇**。前者能回答 *where does the effect enter the computation?*，但不能干净回答 *language itself caused it?*；后者必须用受控训练或人工语言去做，否则永远绕不开 culture/corpus/tokenization 混杂。这个切口会让整个项目从“一个 multilingual extension”变成真正的多阶段研究计划。

有一个新近邻值得特别加入你的路线图：ACL 2026 的 **User-Assistant Bias** 已经证明“训练中的来源标签不对称”会形成可测的 source preference，而且 post-training recipe 能系统性改变这种偏好。 这说明你后面的因果训练论文不能只证明“训练后 source bias 变了”；它必须更进一步证明：**语言结构/强制标记改变了对抽象关系的识别样本效率或跨任务控制，而且能迁移到未见过的内容/接口。** 这实际上帮我们把第三阶段的 novelty 切得更干净。

可以。我会把现在这条线正式看成一个 **research program**，而不是继续往 Paper B 里叠实验。重新对照你当前仓库和最新文献后，我认为最合理的结构是：

$$

\boxed{
\text{Source arbitration}
\rightarrow
\text{Rule learning}
\rightarrow
\text{Linguistic conditioning}
\rightarrow
\text{Causal linguistic experience}
\rightarrow
\text{Mechanism}
\rightarrow
\text{Human comparison}
}

$$

你当前仓库已经把 Paper A 定义为 static source-arbitration foundation，把下一阶段母对象冻结为 `context-conditioned rule updating`，而且明确区分 `rule-change without fact-change`；这个基础非常适合向下面几篇自然递进。turn389963view2

我建议规划成 **A 已有 + B/C/D 三篇核心 + E/F 两篇条件性论文**。其中 B、C、D 每一篇都应该有完全不同的 causal question，绝对不要混在一起。

---

## 整个研究计划怎么切

| 阶段 | 核心问题 | 主要干预 | 核心 estimand | 应该成为独立论文吗 |
|---|---|---|---|---|
| Paper A | 当前到底哪个 source 在控制答案？ | source conflict | source routing / arbitration | 已经是 |
| **Paper B** | 什么 evidence 能让模型识别规则，并让规则控制行为？ | diagnostic evidence × rule swap × prior pressure | Relation Information + Relation Control | **是，当前旗舰** |
| **Paper C** | 改变语言时，变化发生在 rule identification 还是 behavioral control？ | language-of-evidence × language-of-query | Language Identification Effect + Language Control Effect | **是** |
| **Paper D** | 语言结构本身能否因果性地产生这种 control bias？ | controlled linguistic exposure / artificial grammar | training-treatment causal effect | **是，而且理论价值很高** |
| **Paper E** | language/rule/control 在网络内部如何组合？ | activation / representation interventions | causal mediation / transport | 条件性 |
| **Paper F** | 人类是否出现相同的 information→control 结构？ | bilingual / artificial-language human study | human–model structural correspondence | 条件性 |

我尤其建议：**Paper B 不要为了多发一篇而拆成两个小 paper。**

你现在仓库里的三个主实验——policy-separation information、identification × prior pressure、functional partner adaptation——放在一起才有完整的：

$$

Evidence\rightarrow Identification\rightarrow Control\rightarrow Action

$$

这恰恰是最有 Best Paper 形状的版本。仓库当前也是这么冻结的。

---

# Paper B：先把“Evidence → Control”做成母定律

这一篇仍然应该是当前最高优先级。

我会把它的科学命题冻结成：

> **Demonstrations teach a governing relation only insofar as they distinguish competing policies; identifying that relation is necessary but not sufficient for the relation to control behavior.**

这里其实有两个量。

第一个是 **Relation Information**。

如果候选规则集合为 $\Pi$，定义：

$$

I_R(D)=H(\Pi)-H(\Pi\mid D)

$$

deterministic 条件下也可以直接使用：

$$

I_R(D)=\log |\Pi|-\log |V(D)|

$$

其中 $V(D)$ 是 demonstrations 后仍然没有被排除的 candidate policies。

于是：

consensus examples 即使全部正确，也可能有

$$

I_R\approx0

$$

而一个 source-conflicting example 可以有很高的 $I_R$。

这一点与你现在仓库里 `within-example source conflict` 和 `across-example policy conflict` 的严格区分完全一致。

第二个量是 **Relation Control Effect**。

固定同一个 world：

$$

belief=A,\quad reality=B

$$

只把 governing rule 从：

$$

R=belief

$$

换成：

$$

R=reality

$$

模型行为应该从 A 翻到 B。

因此可以把：

$$

RCE=
P(\text{appropriate behavioral flip}\mid rule\ swap)
-
P(\text{flip}\mid sham\ swap)

$$

作为全项目长期复用的核心 estimand。

这会比单纯 accuracy 强很多，因为它问的是：

> **行为究竟有没有因 governing relation 的改变而改变。**

### Paper B 最漂亮的三个结果

第一张 hero figure 应该是：

$$

1\ diagnostic\ demo > 16\ consensus\ demos

$$

但 headline 不要停在 1 > 16，因为 critical-example literature 已经很接近。

真正应该证明：

$$

I_R(D)

$$

比 example count、semantic similarity、diversity、length、position 更好地预测 held-out rule learning。

第二张 hero figure 是：

$$

I_R\times PriorPressure

$$

的二维 control surface。

你会得到两个 surface：

$$

P(\hat R=R)

$$

以及

$$

P(Y=Y_R\mid \hat R=R)

$$

如果第一个已经饱和，而第二个仍然随着 competing prior 崩塌，就有真正的 identification/control separation。

第三个实验就是你现有 partner adaptation，但定位要改成：

> **functional assay of relation control**

而不是“另一个 ToM benchmark”。

---

## Paper B 有一个现在必须新增的强 negative control

2026 年最新工作已经把“内部信号→行为”的门槛抬高了。

Nature Machine Intelligence 刚刚用 confidence 做出了完整的 representation → threshold policy → behavior 因果链。

因此你必须主动测试：

$$

\text{commitment gap}
\overset{?}{=}
\text{simple output calibration problem}

$$

至少要拟合一个 held-out global：

$$

z'=az+b

$$

以及 temperature / response-interface controls。

如果 scalar calibration 能恢复所有行为，Paper B 不应该声称 relation-specific control。

如果恢复不了，而 rule swap effect 仍然 source-specific，那么：

> **the failure is relation-specific rather than merely output miscalibration**

会非常强。

---

# Paper C：Linguistic Relativity of Relational Control

这是我认为与你刚才 Boroditsky 文献线连接得最自然的一篇。

但它绝对不要做成：

> Chinese ToM < English ToM

因为 XToM 已经直接证明 LLM 的 ToM 表现在五种语言间有明显差异。

BICAUSE 也已经证明语言条件会对应不同的 causal reasoning preference，并观察到跨语言的表示差异和收敛。

NAACL 2025 更发现同一个 equivalence/inheritance reasoning question 跨语言可以出现 17.3%–57.5% 的冲突答案。

所以 Paper C 必须问一个别人还没有精准回答的问题：

> **When language changes reasoning, does it change what relation the model infers, or how much control an already identified relation obtains over behavior?**

这就把语言相对论变成一个 computation-stage question。

---

## Paper C 最关键的设计，不是 Language × Accuracy

而是：

$$

\boxed{
Language_{demo}
\times
Language_{query}
\times
Rule
\times
Diagnosticity
}

$$

这是我目前最推荐你的实验。

举例，同一个 latent world：

Alice believes A，reality B。

target rule 始终是 belief。

做四种条件：

English demos → English query

English demos → Chinese query

Chinese demos → English query

Chinese demos → Chinese query

这一步特别重要，因为它把“语言”拆成了两个不同干预：

**language of evidence**

和

**language of commitment**。

如果只改变 demo language 就改变：

$$

P(\hat R=R)

$$

说明语言影响 **relation induction**。

如果 relation readout 不变，但改变 query language 就改变：

$$

P(Y=Y_R\mid\hat R=R)

$$

说明语言影响 **relation control**。

这比单纯 bilingual accuracy 差异高级很多。

---

# Paper C 可以产生三个理论结果

这是我很喜欢它的地方，因为三个结果都能明确解释。

如果 diagnostic evidence 足够多之后语言差异消失：

$$

Language\rightarrow relational\ prior

$$

也就是说，语言主要改变 default prior，强证据可以覆盖它。

这其实是一个非常现代的弱语言相对论：

> language biases which relation is initially accessible, but does not constrain eventual reasoning.

如果 identification 在语言间相同，但 final control 仍然不同：

$$

Language\rightarrow control\ policy

$$

这会是更令人惊讶的发现：

> 语言没有改变模型认为规则是什么，却改变规则能否赢得行为控制权。

如果强 diagnostic evidence 下 identification 仍然系统性跨语言不同：

$$

Language\rightarrow rule\ induction

$$

这才是一个更深的 reasoning relativity effect。

---

# Paper C 里 culture 应该是 control，不要抢 headline

这一点要切得非常狠。

ACL 2025 已经专门把 language medium 和 cultural context 正交化，并报告 Cultural-Linguistic Synergy。

Nature Human Behaviour 2025 也已经报告中文/英文条件会改变 GPT 和 ERNIE 的 social orientation 与 cognitive style。

所以不要另起一篇：

> Language vs Culture in ToM.

容易撞车。

在 Paper C 里面，你只需要保证：

文化中性物体/名字；

专业翻译；

translation equivalence 人工 validation；

opaque answer symbols；

token-frequency controls；

script/tokenization diagnostics；

小规模 cultural-context crossed condition。

你的 novelty 不是：

> language and culture are different.

而是：

> **language effects enter different stages of relational computation.**

---

# Paper D：这可能是整个长期计划理论价值最高的一篇

Paper C 仍然只能证明：

$$

L_1\neq L_2
\Rightarrow
behavior\ differs

$$

但不能真正证明：

> 是语言结构本身造成了这种差异。

因为：

$$

Language
+
Culture
+
Corpus
+
Tokenizer
+
TrainingFrequency

$$

始终共变。

所以第三篇必须切到：

# **Controlled Linguistic Experience**

这里不要再用 natural languages。

用 **人工语言 / counterfactual grammar / controlled continued training**。

这条方法已有很好的基础：White & Cotterell 早在 ACL 2021 就提出用只在某个 typological feature 上不同的人工语言训练模型，从而建立 fully controlled causal framework；2025–2026 又有工作继续用 synthetic/counterfactual languages 研究 linguistic priors 和 typological learnability。

但现有工作主要问：

> 某种 grammar 好不好学？

你可以把问题推进到：

> **Does repeated exposure to a grammatical distinction causally reshape downstream relational control?**

这才真正接上 Boroditsky。

---

## Paper D 我建议只做一种 linguistic feature

不要一开始做：

空间、时间、gender、agency、evidentiality……

会散掉。

你最自然的是：

# **evidentiality / information-source marking**

因为它和整个 LP4FM 的 source ontology 天然一致。

构造完全 parallel 的两套 corpus。

Treatment language：

每个 proposition 都必须强制标记：

`VIS` = direct perception
`REP` = reported
`INF` = inferred

或者更贴合你的 ontology：

`SELF-BELIEF`
`OTHER-REPORT`
`REALITY`

Control language：

语义完全相同、token 数量和 marker frequency 也匹配，但这些 marker 编码一个与信息源无关的 random noun-class / irrelevant feature。

于是：

$$

Corpus_{treatment}

$$

和

$$

Corpus_{control}

$$

具有相同 facts、相同长度、相同 frequency、相同 lexical content。

唯一关键区别是：

> 是否习惯性地强制编码 information source。

然后复制同一 base checkpoint，做 matched continued exposure。

最后**不要在训练格式上测试**。

要测新的：

ToM source conflict；

report-vs-reality conflict；

partner adaptation；

甚至 symbolic/JSON source arbitration。

如果 treatment model 在这些未见格式上表现出：

更低的 diagnostic sample complexity；

更强的 rule-swap RCE；

更低 wrong-source capture；

那么你可以开始说：

> **controlled linguistic experience causally alters relational reasoning in an artificial neural system.**

这已经比“中文和英文输出不一样”强很多。

---

# 为什么 Paper D 不能只是“训练后 source preference 变了”

因为 ACL 2026 的 **User-Assistant Bias** 已经做得很接近了。

他们在 52 个模型上发现 instruction-tuned models 常有强 user bias，并通过 controlled fine-tuning 发现 human-preference alignment 增强 user bias、reasoning fine-tuning 减弱它，还能用 DPO 双向改变这种 bias。

所以 Paper D 的 novelty 必须明确比它多一步：

不是：

$$

training\rightarrow source\ preference

$$

而是：

$$

\boxed{
obligatory\ linguistic\ distinction
\rightarrow
abstract\ relational\ learning/control
\rightarrow
cross-task\ transfer
}

$$

也就是说，**训练时只是语言习惯，测试时是新的 reasoning task。**

这会非常关键。

---

# Paper D 最漂亮的因果 estimand

可以用 difference-in-differences：

$$

\Delta_{\text{ling}}
=
(RCE_{\text{post}}-RCE_{\text{pre}})_{\text{source-marking}}
-
(RCE_{\text{post}}-RCE_{\text{pre}})_{\text{matched-control}}

$$

再做 exposure dose：

$$

0,\quad low,\quad medium,\quad high

$$

如果出现稳定剂量效应：

$$

linguistic\ exposure
\uparrow
\Rightarrow
source\ sensitivity
\uparrow

$$

而且迁移到未见格式，那是一张非常强的 causal figure。

---

# Paper E：机制论文，不要现在就承诺

如果 B、C、D 中至少两个得到稳定结果，再做这一篇。

核心问题是：

> **Language-conditioned prior、identified rule、behavioral control 在内部到底是共享表示还是独立变量？**

可以形成三个 probe：

$$

z_L=\text{language state}

$$

$$

z_R=\text{relation state}

$$

$$

z_C=\text{control state}

$$

但 probe 不应该是 headline。

真正应该做的是 intervention。

例如：

保持：

$$

R,W,D

$$

不变，

把 language-conditioned activation 从 $L_1$ condition transplant 到 $L_2$，问：

$$

RCE

$$

是否随之移动。

或者针对已经识别出来的 source relation 做 targeted steering，看：

是否只改变对应 conflict items；

是否跨 content transfer；

是否不改变 unrelated relations；

是否不只是改变 output language。

你现在 Paper A 的一个重要教训就是：mechanism 可能 checkpoint-sensitive，所以不要过早把“某个 direction”写成 universal controller。你自己的仓库已经非常清楚地把 readability、control、specificity、transport 分开了。

所以 Paper E 只有在：

**跨 content + 跨语言 + specificity + transport**

都比较漂亮时，才值得独立成文。

否则放 Paper C/D appendix。

---

# Paper F：人类 × LLM，真正回到 Boroditsky

这一篇最适合等模型侧理论已经稳定后再做，而且最好和 psycholinguistics / cognitive science 合作者一起。

我不建议直接比较：

Chinese native speakers vs English native speakers。

因为又重新掉回 culture confound。

真正漂亮的是 **miniature artificial-language learning**。

随机把人类参与者分成：

source-obligatory language；

matched-control language。

训练一段人工语言之后，不再测语言本身，而测试新的 source-conflict / memory / partner adaptation task。

然后比较：

$$

human:\quad
LinguisticExposure\rightarrow RelationalControl

$$

与

$$

LLM:\quad
LinguisticExposure\rightarrow RelationalControl

$$

注意这里不要声称：

> LLM models human cognition.

而是问：

> **相同的 controlled linguistic experience 是否在两种学习系统中产生同方向的 relational bias？**

如果答案 yes，这就真正从 NLP 开始触及 cognitive science。

已有 2026 年土耳其 evidentiality 工作已经直接比较 humans 和 10 个 LLM，发现人类 source-trust effect 稳定，而模型高度 prompt/model-dependent。citeturn550245academia12

因此你的 human paper 必须比 natural-language evidentiality comparison 更进一步：

**随机分配 linguistic experience。**

这才有真正的 causal leverage。

---

# 最重要的战略：每篇论文只允许自己回答一个“because”

这可以避免整个研究计划再次膨胀。

Paper B：

> Behavior changes **because evidence distinguishes a governing rule**.

Paper C：

> Cross-language behavior changes **because language acts on identification and/or control**.

Paper D：

> The language effect arises **because repeated obligatory linguistic experience changes relational priors/control**.

Paper E：

> That behavioral change happens **because a particular internal state causally mediates it**.

Paper F：

> The phenomenon matters cognitively **because a structurally analogous effect can be induced in humans under controlled learning**.

这是我觉得最健康的切法。

---

# 同时建立一个“共享实验底座”

以后不要每篇重新造 benchmark。

整个项目共享一个 latent generator：

$$

G(W,R,D,L,C,A,E)

$$

其中：

$W$ = latent world；

$R$ = governing relation；

$D$ = diagnostic evidence；

$L$ = linguistic medium；

$C$ = cultural context；

$A$ = answer/action interface；

$E$ = linguistic training experience。

Paper B 只动：

$$

R,D,Prior

$$

Paper C 只额外动：

$$

L

$$

Paper D 只额外动：

$$

E

$$

Paper E 不动 stimulus，直接动：

$$

internal\ state

$$

Paper F 把：

$$

Population=\{LLM,Human\}

$$

加进来。

这样每篇论文都在回答同一个 program 的不同 causal edge。

---

# 我建议现在就画成这棵决策树

当前先跑 Paper B。

如果：

$$

I_R\rightarrow Identification

$$

很强，但 identification→control gap 不稳定，

那么直接收缩为：

**Conflict Is Supervision / Relation Information in ICL**

这是完整 paper，不要硬造 commitment story。

如果：

$$

I_R\rightarrow Identification

$$

以及

$$

Identification\not\Rightarrow Control

$$

都强，

就做完整旗舰 Paper B。

然后才启动 Paper C。

如果 Paper C 发现没有稳定 language effect：

**停止 linguistic-relativity branch。**

这不是失败；说明 relation-control 可能比普通 multilingual reasoning 更 language-invariant。

如果 Paper C 有强 effect，但全部被 tokenization / model competence / global calibration 解释：

也不要启动 Paper D。

只有当发现：

$$

Language
\rightarrow
stage\text{-}specific\ relational\ effect

$$

而不是 generic accuracy drop，

才值得进入 causal linguistic exposure。

如果 Paper D 再证明：

$$

controlled\ linguistic\ experience
\rightarrow
out\text{-}of\text{-}distribution\ relational\ control

$$

那时候整个 research program 的理论地位就完全不同了。

---

# 哪一篇最有 Best Paper 潜力？

目前我的排序会是：

**Paper B：最高的近期成功概率。**
因为任务、ontology、验证工具几乎都已经在你的仓库里，且与你 Paper A 连续。

**Paper D：最高的长期理论上限。**
因为它有机会真正回答“语言经验是否因果改变 reasoning policy”，而不是只做 observational multilingual comparison。人工语言方法本身已有严格 causal precedent，但目前的主流工作主要研究 grammar learnability，而不是 grammar experience 如何改变后续 relation control。

**Paper C：最自然的桥梁论文。**
它生态有效、容易被理解，也能直接连接 linguistic relativity、多语 reasoning 和你的 relation-control framework，但必须以 stage decomposition 赢，而不是 accuracy benchmark。

**Paper E：高风险高收益。**

**Paper F：如果找到强 cognitive collaborator，会非常有学术辨识度，但不应该阻塞前面几篇。**

---

## 最终我会把整个项目的长期理论命题定成这一句

不是：

> LLMs have Theory of Mind.

也不是：

> Language shapes LLM thought.

而是：

> **Reasoning behavior emerges from competition among candidate relations; contextual evidence determines which relation is identified, linguistic experience shapes the priors and accessibility of those relations, and a distinct control process determines which identified relation is ultimately enacted.**

对应一个统一链条：

$$

\boxed{
Linguistic\ Experience
\rightarrow
Relational\ Prior
\rightarrow
Diagnostic\ Evidence
\rightarrow
Relation\ Identification
\rightarrow
Relational\ Control
\rightarrow
Action
}

$$

Paper A 已经从中间的 **source arbitration** 开了口子。

Paper B 建立 `Evidence → Identification → Control`。

Paper C 把 `Language` 加进来。

Paper D 真正操纵 `Linguistic Experience`。

Paper E 打开内部机制。

Paper F 才有资格重新回到 Boroditsky / Sapir–Whorf 的人类认知问题。

**如果按这个切法推进，你就不需要每次发现一篇新文献便重写整个项目。新文献只会告诉我们它占掉了链条中的哪一条边，而我们继续沿尚未被因果识别的边往前推。**

这会比“不断找一个新的 Best Paper idea”稳定得多。