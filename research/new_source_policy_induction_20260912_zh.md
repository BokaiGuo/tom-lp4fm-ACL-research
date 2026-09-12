# 历史方案：Who Gets to Be Right?

## Context 中的来源选择策略学习

更新日期：2026-09-12

研究状态：`superseded`

> 本文保留为文献调研前的候选方案，不再是冻结后的主研究设计。经过 exact-neighbor
> audit，单独研究 source-policy induction 会与 source preference、context-faithfulness
> 和 policy-learning 工作发生概念重叠。最终方案已收紧为“事实更新与裁决规则更新的
> 分离”，见 `docs/research/frozen_oral_research_design_20260912_zh.md`。

本文档是一份新的研究设计，不是对 LP4FM 已有结果的重新解释。LP4FM 只提供 typed
source topology、latent-world generator 和答案来源审计经验；下面的核心数据、任务
协议和结论都必须重新设计、重新运行。

## 1. 研究问题

### 主问题

> **语言模型能否从上下文示例中学习“在多个相互冲突的信息来源之间，应该让谁决定
> 答案”的选择策略，并把这一策略迁移到新的事实、新的表达和新的冲突结构？**

研究的不是模型是否记住某个事实，也不是是否答对一道 false-belief 题，而是一个更
小的学习对象：

```text
source values + conflict topology + contextual demonstrations
                 -> inferred source-selection policy
                 -> answer or action on a new world
```

论文暂定标题：

> **Who Gets to Be Right? Learning Source-Selection Policies from Context**

“source-selection policy”是本文对可观察任务结构的操作性定义，不预设模型内部存在
同名模块。

## 2. 为什么这是新的研究对象

已有研究分别讨论了 ICL 示例、schema/语言表示、ToM belief 表征、shortcut 和
partner adaptation，以及细粒度语言关系与最终判断的分离。本方案不把这些方向简单
拼起来，而是提出一个可操作单位：

> **示例不只告诉模型“答案是什么”，还可能告诉模型“哪个来源在冲突时拥有答案
> 资格”。研究的关键是检测这种资格策略是否被归纳，并能在反事实世界中执行。**

与普通 task identification 的区别是：任务标签可以不变，变化的是同一世界中哪个
source 应控制输出。与普通 ToM benchmark 的区别是：不把 belief accuracy 当终点，
而是显式生成多个来源、多个 policy 和未见的 policy 组合。

## 3. 形式化任务

每个 latent world `w` 包含一组来源：

```text
S(w) = {belief, reality, testimony, memory, instruction, ...}
```

每个 source 对 query 给出可能不同的值 `z_s(w)`；conflict topology `T` 指明哪些
来源冲突，以及它们如何组成二元或三元竞争。

```text
pi(T, q) -> selected source
y_pi(w, T, q) = z_{pi(T,q)}(w)
D_pi = {(w_i, T_i, q_i, y_pi(w_i,T_i,q_i))}_{i=1}^n
```

目标不是复制 `w_i` 的事实，而是从 `D_pi` 归纳 `pi`，再在未见 `w*` 上输出
`y_pi(w*,T*,q*)`。

必须区分四种成功：

```text
fact recall        = 复述示例中出现过的事实
policy reading     = 说出当前应优先哪个 source
policy execution   = 在新 world 上按 policy 选择 source
policy composition = 在未见 topology 上组合已学到的 source priorities
```

只有第三种是核心行为结果，第四种是理论升级；第一、二种不能替代它。

## 4. 数据生成原则

### 4.1 同一世界，多种政策

对同一个 latent world 同时生成多套 target：

```text
belief-first
reality-first
testimony-first
recency-first
instruction-first
```

每套 target 只改变 governing policy，不改变世界事实、人物、对象、词面模板和答案
空间，形成严格的 paired policy-swap：

```text
(same world, same query, policy A) vs. (same world, same query, policy B)
```

### 4.2 五个数据池

1. `policy-identification`：估计哪些 demonstrations 能区分候选 policy；
2. `content-holdout`：新人物、新对象、新词面和新事实；
3. `encoding-holdout`：答案标签、答案顺序和输出格式置换；
4. `topology-holdout`：训练见过二元冲突，测试出现未见三元结构；
5. `policy-swap-confirmation`：完全新世界，成对交换 governing policy。

任何 item 选择、prior calibration 和模板调优必须在 confirmation pool 解盲前冻结。

### 4.3 防止伪政策学习

- source 名称、人物身份、位置和词序不能预测 policy；
- policy 与答案 label 不能固定绑定；
- policy A/B 的 demonstrations 数量、长度、相似度和情感色彩匹配；
- 同一个 source 在不同 policy 中都要被选中、被拒绝并与其他 source 冲突；
- partner identity、表面语气和答案 token 不能成为 policy 的唯一线索。

## 5. 三项核心研究

### Study 1：What do demonstrations teach?

比较四种 matched context：

```text
many-consensus       = 大量示例，但大多不能区分 policy
one-diagnostic       = 一个能区分 target policy 的示例
balanced-diagnostic  = 区分多个 policy pair 的最小集合
corrupted-policy     = 支持不一致 policy 的示例集合
```

定义：

```text
C_sep(D) = 被 D 中至少一个 example 区分的 policy pair 比例
```

在完全新 world 上测试 policy execution，比较 `C_sep/version-space reduction` 与
example count、similarity、lexical diversity、structural coverage。这里不预设
`C_sep` 是普适定律，只检验它是否比现有 example-level 描述更接近迁移行为。

### Study 2：Counterfactual policy interchange

这是整篇论文的决定性实验。对每个新 world 构造：

```text
same world + same query + same surface form + policy A
same world + same query + same surface form + policy B
```

唯一差异是 demonstrations 指示的 governing policy。输出分为：

1. `source-choice output`：明确选择哪个 source；
2. `native answer`：不要求解释的最终答案；
3. `action output`：需要协调时的第一次行动。

关键指标是 paired policy-swap effect，而不是总准确率：

```text
P(native output follows pi_A | D_A)
- P(native output follows pi_A | D_B)
```

必须加入 fact-swap、label-swap、demonstration-order、source-name permutation、
explicit-rule upper bound，以及 same-item replay/new-world transfer 分离。

如果 source-choice 会随 policy 变化而 native answer 不变，这是 policy execution
failure，不是“模型已经掌握 policy”。只有 native answer/action 也随 swap 改变，
才支持功能性 policy execution。

### Study 3：Compositional source-policy transfer

训练阶段只提供二元冲突：

```text
belief vs. reality
testimony vs. reality
belief vs. testimony
```

测试阶段出现未见三元冲突：

```text
belief vs. reality vs. testimony
```

比较 pairwise composition、majority/recency heuristic、explicit-rule、direct-label
memorization、random 和 source-order controls。核心问题是模型是否将二元 source
relations 组合成未见 policy，而不是把三元任务当作新 pattern。

## 6. 功能性扩展：Partner policy inference

partner game 不是主问题的起点，而是 Study 2 的 consequence。模型先观察 partner 在
若干冲突世界中的行为，再在新世界中选择沟通或协调行动。通过 paired partner-policy
swap，只改变 partner policy，不改变 partner 身份、语气、奖励和表面行为频率。

只有 Study 2 的 native policy-swap 通过后，partner adaptation 才进入主文；否则放到
后续工作，避免把未完成的功能链当成论文中心。

## 7. 主要假设与竞争解释

### H1：政策信息

匹配示例数量和 token budget 后，policy-separation coverage 比 example count 更能
预测 held-out policy execution。

### H2：反事实交换

只改变 governing policy 时，native answer/action 随 policy 改变，而不随 source name、
label 或 demonstration order 改变。

### H3：组合迁移

若模型学习的是 source policy 而非 pairwise pattern，它应能在未见三元 topology 上
执行预先定义的组合策略。

最强竞争解释为 surface cue following、fact memorization、source-order heuristic、
answer-label association 和 partner-identity memorization；每个解释对应独立 control。

## 8. Oral 级别的决定性 artifact

主图只保留三张：

1. **Policy information curve**：`C_sep` 相比示例数量是否预测新 world 的 policy execution；
2. **Counterfactual policy-swap matrix**：同一 world 中只换 policy，native answer/action 是否换；
3. **Compositional transfer map**：二元训练到三元测试，以及 partner action 的功能后果。

如果这三张图成立，论文改变的是 ICL 的基本测量单位：从“示例是否提高准确率”转为
“示例是否指定了来源选择策略，以及该策略是否能在反事实和未见结构中被执行”。

## 9. 近邻研究的准确分离

- **Imperfective Paradox**：吸收其最小语义对比、关系有效性审核和中间判断/最终标签分离；
  新增的是 demonstration-induced source policy 与 counterfactual policy swap。
- **Shaping Schema**：吸收语言表示改变知识组织的问题意识；本方案固定 latent world，
  直接测 policy identification、execution 和 composition，不用宽泛 schema 解释一切。
- **ToM benchmark/partner adaptation**：吸收功能性终点，但不再做更大的 static benchmark；
  partner policy 是显式 latent variable，paired swap 是识别设计。
- **ICL task identification/demonstration selection**：不把“关键示例”本身当贡献；关键是
  它是否区分 source policies，且该 policy 能否在新 world 中执行。
- **Belief probing/steering**：只作为补充诊断，主证据必须来自 native answer/action 和
  held-out transfer。

## 10. 成功、边界与失败路径

### 强成功版本

`C_sep` 预测 held-out execution；policy swap 改变 native answer；二元 policy 组合到
未见三元 topology；partner swap 改变 held-out coordination；一个独立 checkpoint 或
模型族复现主要行为签名。

### 有价值的边界版本

模型能读出 policy 但不能执行；能跨 content 但不能跨 encoding/topology；平均 policy-swap
有效但 item-level 泛化失败；partner adaptation 不超越 source-choice readout。

这些结果仍可形成论文，但标题和 claim 必须收紧为 policy identification、execution 或
composition boundary，不能用“模型真正理解”补偿。

## 11. LP4FM 的位置

LP4FM 提供：

```text
source ontology
conflict topology grammar
typed answer auditing experience
mechanism overclaim 的反例
```

LP4FM 不作为本研究 confirmation data。新研究必须使用新的 latent worlds、demonstration
generator、policy-swap pairs 和 held-out targets。

当前最窄研究对象是：

> **context-induced source-policy learning and counterfactual execution**

在真实 scoring 运行前，状态只能是 `not_evaluated`。
