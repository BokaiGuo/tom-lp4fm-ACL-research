# Best Paper 级下一阶段候选：当冲突成为监督

英文对应稿：[Best-Paper Candidate: When Conflict Becomes Supervision](best_paper_conflict_as_supervision_20260909.md)。

上位升级方案见：[从关系到答案承诺：先验改写](best_paper_relation_to_commitment_20260911_zh.md)。本文件不再单独承担最高层标题；它是新主线中的
**relation identification** 半部，负责说明哪些 demonstrations 真正区分候选
source policies。

更新日期：2026-09-09

当前状态：`argument_shaped`。本文档给出一个具有 Best Paper 潜力的研究候选，不把
奖项、oral 或经验结论写成已经实现的事实。

## 0. 直接判断

当前 ACG 方案已经有主会论文形状，但还没有 Best Paper 级的中心发现。它的问题不是
实验不够多，而是“demonstrations 学习 source-authority relation”仍像一个新的任务家族，
尚未改变领域对 ICL 的基本认识。

最近邻检索后，不能再把以下说法当作 novelty：

- demonstrations 可能不能唯一确定任务；AmbiBench 已直接研究 task ambiguity；
- 一个关键示例可以消除多个规则之间的歧义；Ye et al. (2025) 已把它作为 attribution
  研究的受控对象；
- 示例之间发生冲突会伤害 rule inference；Jiao et al. (2026) 已研究 corrupted
  demonstration conflict；
- counterfactual demonstrations 可以提高 context faithfulness；Zhou et al. (2023)
  已给出方法结果。

因此，下一阶段真正值得押注的命题应收紧为：

> **在关系型任务中，正确示例是否提供监督，不取决于它是否“正确”，而取决于候选
> 来源策略在该示例上是否产生不同答案。来源一致的正确示例可以无限增加而几乎不
> 识别任务；一个来源冲突但标签一致的示例，反而可能提供决定性的关系监督。**

更短的一句话是：

> **Conflict is supervision when it separates candidate source policies; conflict is noise
> when demonstrations disagree about the policy itself.**

这条命题把两种常被混写的“冲突”分开：

1. **示例内部的来源冲突**：belief、reality、report、context 或 result 对同一问题给出
   不同候选答案，而 gold label 揭示谁应控制答案。这是正监督。
2. **示例之间的规则冲突**：不同 demonstrations 支持互不相容的控制规则。这是噪声、
   corruption 或真正的规范不确定性。

如果该规律跨 ToM、context--knowledge conflict 和事件语义成立，并且能前瞻性预测或
选择高价值 demonstrations，它才可能从“一个新 benchmark”上升为改变 ICL 数据观的
研究：**有效样本量不是 correct-example count，而是被 demonstrations 区分开的候选关系
数量及其拓扑。**

## 1. 为什么这比当前 ACG 更接近 Best Paper

### 1.1 当前 ACG 还缺什么

当前 ACG 已经提出：模型能否从示例学习 source-authority relation，并迁移到新内容、
新编码和未见拓扑。这个对象是清楚的，但仍可能被审稿人归入：

- task induction 的一个新实例；
- prompt sensitivity 的精细评估；
- compositional generalization 的一个 ToM domain；
- context-faithfulness 或 instruction hierarchy 的变体。

Best Paper 级工作需要一个可以反过来解释既有矛盾的规律，而不是只开辟一个新任务。
“冲突作为监督”提供了这个可能性：

```text
为什么随机标签有时影响很小？
  因为许多示例主要提供格式、标签空间或输入分布，未区分目标关系。

为什么一个 aha example 影响巨大？
  因为它第一次把候选规则分开。

为什么 corrupted conflict 会伤害模型？
  因为它在 demonstrations 之间制造了不相容的规则证据。

为什么 counterfactual demonstrations 提高 context faithfulness？
  因为 context 与模型先验第一次给出不同答案，标签才识别“应跟随 context”。

为什么 true-belief examples 不能证明模型学会 belief tracking？
  因为 belief 与 reality 一致时，follow-belief 与 follow-reality 行为等价。
```

### 1.2 Best Paper 候选所需的四个同时成立条件

1. **一个反直觉但必然的规律**：agreement 可以让大量正确标签变成关系层面的零信息，
   而受控 source conflict 可以成为最有价值的监督。
2. **一个精确对象**：不是宽泛 schema，而是 demonstrations 对候选 source policies 的
   distinguishability。
3. **一个决定性经验签名**：八个来源一致示例几乎不如一个 matched diagnostic example；
   但一个 inter-demonstration corrupted example 产生相反方向的作用。
4. **一个领域后果**：示例选择、benchmark 设计、ToM 训练和 ICL 评估都应报告
   relation-diagnostic coverage，而不能只报告示例数、标签正确率和结构覆盖。

少一个条件，都更像一篇扎实 oral，而不是 Best Paper 候选。

## 2. 形式对象：正确标签何时真的识别了关系

### 2.1 来源状态与候选策略

对一个输入 `x`，令：

```text
S = {s_1, ..., s_m}             candidate information sources
z_s(x) in Y                     answer implied by source s
Pi = {pi_1, ..., pi_K}          candidate source-selection policies
pi(x, T) in S                   source selected under conflict topology T
h_pi(x, T) = z_{pi(x,T)}(x)     answer implied by policy pi
```

例子：

```text
ToM       S = {queried belief, reality, other agent, distractor}
factual   S = {local context, parametric/default knowledge}
event     S = {process occurrence, culmination/result state}
reporting S = {reporter commitment, narrator/world state}
```

demonstration `d=(x,T,y)` 对一个策略 `pi` 一致，当且仅当
`h_pi(x,T)=y`。

### 2.2 三类示例必须严格分开

**来源一致示例（source-consensus demonstration）**

对当前候选策略集合中的所有 `pi`，都有 `h_pi(x,T)=y`。示例是正确的，但不能告诉
学习者哪个策略产生了答案。

典型例子是真信念：agent belief 与 reality 相同。无论模型跟随 belief 还是 reality，
都得到同一答案。

**关系诊断示例（policy-diagnostic demonstration）**

至少两个仍可能的策略对该输入给出不同答案，且 gold label 排除其中一部分。示例内部
有来源冲突，但它与目标策略完全一致。

典型例子是假信念：belief 与 reality 不同，gold answer 才能说明任务要求 follow-belief
还是 follow-reality。

**规则不一致示例（policy-inconsistent demonstration）**

单个示例可以由某个替代策略解释，但与已有的 target-consistent diagnostic examples
合在一起时，没有同一个候选策略能解释全部标签。它是 inter-demonstration conflict，
不是本文所说的正监督。为了避免概念错误，不能把“一个 corrupted diagnostic item +
若干 source-consensus items”叫作示例间冲突：source-consensus items 不支持任何相反
policy。真正的 conflict condition 必须同时含有至少一个 target-consistent diagnostic
item 和至少一个支持竞争 policy 的 matched diagnostic item。

### 2.3 版本集合与可识别性

给定 demonstrations `D`，定义仍与全部标签一致的策略集合：

```text
V(D) = {pi in Pi : h_pi(x_i,T_i) = y_i for every d_i in D}
```

若目标策略为 `pi*`，则它被 demonstrations 识别，当且仅当：

```text
V(D) = {pi*}
```

这不是本文要冒充的新定理；它来自经典 concept learning / machine teaching 的 version
space 视角。本文的潜在新意是把它落实到自然语言中的**typed source policies**，并证明
source-conflict topology，而不是示例数量或普通结构覆盖，系统性决定 ICL 的关系迁移。

### 2.3.1 必要前提：selection-blinded 的示例构造

“来源一致示例在关系层面是零信息”不是对任意自然语言 prompt 的无条件断言。它只在
下面的受控干预条件下成立：给定可观测的长度、词汇、构式、标签频率、位置和 source
mentions 后，teacher / generator 选择哪个 source-consensus item 的分布不随目标 policy
改变：

```text
q(x, T, surface | pi_a) = q(x, T, surface | pi_b)
for every competing pi_a, pi_b in Pi.
```

若这一条件不成立，示例的**选择方式**本身就可能泄漏目标策略。例如，teacher 总在
follow-belief task 中选择某类人物、词汇、顺序或极端 world，模型可以不靠标签关系而
更新。因此 Study 0 必须审计并平衡 `q`，Study 1 必须随机化 selection/order 并在新的
surface pool 上确认。

在这个条件下，若 `h_pi_a(d)=h_pi_b(d)=y`，那么该示例的 observed label 对两个 policy
的 likelihood 相同。这里的“零信息”只指**对预注册候选 policy 集合的 label-likelihood
没有区分力**；它仍可提供输出格式、标签空间、任务家族、语法、实体类型和一般语用
信息。

### 2.3.2 Bayesian 只是比较语言，不是机制承诺

可以用

```text
P(pi | D) proportional to P(D | pi) P(pi)
```

说明 source-consensus labels 为什么不能改变两个等价 policy 的相对 posterior，但本文
不假设真实 LLM 精确执行 Bayesian inference。Xie et al. 的 implicit Bayesian account
与 latent-variable ICL 提供有用的规范性比较；Pan et al. 已将 task recognition 与 task
learning 分开；Falck et al. 则显示实际 LLM ICL 不满足一般 Bayesian learner 的必要行为
条件。本文测量的是 prior attraction 和 contextual update 的行为规律，不把它们解释为
模型内部真的维护一个规范 posterior。

### 2.4 诊断覆盖，而不是示例数量

定义一个不依赖模型的 target-relative 覆盖：

```text
C_sep(D, pi*) =
  sum_{pi != pi*} w(pi) * 1[exists d in D: h_pi(d) != h_pi*(d)]
  ------------------------------------------------------------
                    sum_{pi != pi*} w(pi)
```

- `w(pi)=1` 给出 prior-free coverage；
- `w(pi)` 也可由独立 zero-shot / no-diagnostic condition 估计，得到 prior-weighted
  coverage；
- 主分析必须同时报告两者，防止用被测模型行为循环定义解释变量。

`C_sep=0` 表示 demonstrations 没有排除任何竞争策略；`C_sep=1` 表示目标策略与所有
候选策略都被至少一个示例区分。

若多个策略在所有 demonstrations 上有相同输出签名：

```text
sigma_D(pi) = [h_pi(d_1), ..., h_pi(d_n)]
```

则在 selection-blinded、预注册的 `Pi` 内，仅凭 demonstrations 的 observed
input--output pairs 无法区分这些策略。模型的最终选择可以来自 checkpoint prior、额外
语言线索、随机性，或候选集合之外的假设；因此本文不能把 `V(D)` 非单例写成“任务在
所有可能解释下不可识别”。

### 2.5 Source-conflict graph

构造图 `G_D`：

```text
node = candidate policy or typed source
edge(pi_a, pi_b) exists
  iff at least one target-consistent demonstration makes them predict differently
```

该图把旧 ACG 的 conflict topology 与 ICL 的 teaching evidence 接起来：

- 没有边：所有示例均为 source consensus，无法识别 authority；
- 只覆盖部分边：只能识别一个 equivalence class；
- 目标节点与所有竞争节点被分开：目标 policy 可识别；
- 只给 `a>b`、`b>c` 而测试 `a>c`：开始检验 relation composition，而不是简单识别。

这允许把“示例 coverage”拆为两层：

```text
surface/structural coverage   = 是否看过相似形式与 primitive
policy-separation coverage    = 是否看过能区分候选来源规则的冲突
```

两者都可能重要，但不能互相替代。

### 2.6 三个有限命题

**命题 1：受控一致示例不可识别。**

若两个策略在 selection-blinded demonstrations 上给出相同答案，则 demonstrations 的
标签似然对两者相同；没有外部假设的行为学习者不能由这些标签识别哪个策略是目标。

**命题 2：目标策略的最小分离条件。**

在确定性候选策略类中，`pi*` 被唯一识别，当且仅当每个 `pi != pi*` 至少被一个
target-consistent demonstration 与 `pi*` 分开。

**命题 3：识别与执行可以独立失败。**

即使 `V(D)={pi*}`，模型仍可能无法在新内容或未见拓扑执行 `pi*`。因此总失败至少要
拆成：

```text
identification gap = explicit-policy upper bound - nondiagnostic-demonstration performance
execution gap      = task ceiling - explicit-policy upper bound
```

这里的 gap 是实验分解，不宣称 Transformer 内部存在两个独立模块。

## 3. 与最近邻工作的精确边界

### 3.1 最强近邻总表

| 最近邻 | 已经做了什么 | 本文若只做到什么就不新 | 必须新增的证据 |
|---|---|---|---|
| [Task Ambiguity in Humans and Language Models](https://arxiv.org/abs/2212.10711) | AmbiBench 控制多个 classification features 与示例数量，研究 intended task 是否被识别 | 再证明示例不足会产生任务歧义 | 把歧义具体化为 typed source-policy equivalence，操纵 source agreement，并预测跨内容/拓扑迁移 |
| [Can Input Attributions Explain Inductive Reasoning in ICL?](https://aclanthology.org/2025.findings-acl.1092/) | 用一个 disambiguating aha example 测 attribution 是否找到关键示例 | 构造“一个关键示例胜过其他示例”的 synthetic task | 以示例信息结构为主结果，覆盖自然语言来源、多个候选策略、组合与前瞻性示例选择 |
| [Understanding the Dynamics of Demonstration Conflict](https://arxiv.org/abs/2603.04464) | 注入一个 corrupted rule example，研究 inter-demonstration conflict 与 attention heads | 再做 noisy/corrupted demonstration robustness | 区分 intra-example source conflict 与 inter-example policy conflict，并预测二者效应方向相反 |
| [Context-faithful Prompting](https://aclanthology.org/2023.findings-emnlp.968/) | counterfactual demos 提升 context 对抗 parametric knowledge 时的忠实性 | 只复现 counterfactual demos 有效 | 证明效果由候选策略分离解释，跨 ToM/事件关系迁移，并导出统一 selector |
| [CoFe](https://aclanthology.org/2023.acl-long.618/) | similarity、diversity、simplicity、linguistic coverage 影响 ICL composition | 增加一种 coverage 指标并相关分析 | matched factorial 中让 policy-separation coverage 超越这些因素并预测未见 topology |
| [Rethinking Demonstrations](https://aclanthology.org/2022.emnlp-main.759/) | 某些任务中随机标签影响有限，格式、标签空间和输入分布重要 | 再报告 labels sometimes do not matter | 预测正确标签仅在 policy-separating items 上具有关系监督价值 |
| [Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/) | 支持 ICL 通过组合预训练任务处理新 structured task | 把 source relation 称为 structured task | 明确 demonstrations 是否识别了哪个 relation，并分开识别与执行 |
| [Debiasing ICL Demonstrations](https://aclanthology.org/2024.findings-acl.430/) | 将多个 input-to-label mappings 的 semantic ambiguity 与 order/selection bias 相连 | 再做 mapping ambiguity 或 instruction debiasing | 从语义 mapping 歧义推进到可计算的 source-state disagreement 和 graph composition |

### 3.2 与五篇 ACL 2026 参考论文的关系

**Imperfective Paradox。** completed telic examples 可能让 process 与 result 两种解释
行为一致；interrupted minimal pairs 才能区分它们。原论文提供 representation--decision
dissociation，后续 benchmark critique 则要求先验证中间语义。本文把这两个警告写进
Study 0 和 event-domain 的 source-agreement design，不照搬“LLM 不懂体义”的结论。

**Local Attention。** 该工作证明特定形式设定下 local/global attention 的表达力互补。
它不能支持本文的语义机制，但提醒我们：如果模型已获得 diagnostic evidence 却不能
跨距离执行 relation，access/topology 可能是边界变量。只作为后续 stress test。

**Memory Efficiency。** 它区分 retrieval constraint 与 encoded representation quality。
本文可以在主规律确认后测试 diagnostic relation 是否比内容事实更容易在干扰下丢失，
但不把记忆噪声纳入主论文的必要故事。

**CoSToM。** 它提供 causal tracing 与 steering 的比较对象。本文的主问题先于 steering：
demonstrations 是否已经提供足够信息识别 source policy。只有行为规律稳定后，才问
diagnostic example 是否改变可迁移的 relation state，而不以 probe gain 替代它。

**ImplicitMemBench。** 它强调 exposure--interference--first-attempt protocol。该协议可
测试一次 diagnostic conflict 是否在干扰后继续影响首次选择；“implicit memory”不是
本文的中心构念，也不用于意识类宣称。

### 3.3 与 Shaping Schema 的关系

`Shaping Schema via Language Representation` 提出语言表示影响知识组织和激活。本文
不竞争“schema”这个宽泛概念，而是给出一个更窄的必要条件：无论 representation
多好，如果 demonstrations 中候选 source policies 始终行为等价，label 都没有识别
authority relation 的信息。representation 可以改变可访问性，但不能创造 demonstrations
中不存在的关系证据。

### 3.4 论文 novelty 的安全写法

不能写：

> We discover that demonstrations can be ambiguous or that one critical example disambiguates a task.

可以在证据成立后写：

> We distinguish conflict within an example from conflict across examples and show that
> the former is the supervision that identifies a latent source policy, whereas the latter
> corrupts it. Across relational domains, held-out behavior is predicted by which candidate
> policies the demonstrations separate, beyond example count, correctness, similarity, and
> structural coverage.

## 4. 六个预注册预测

### P1：More-correct-examples flat curve

在所有 demonstrations 都是 source consensus 时，把正确示例从 `1` 增加到 `2/4/8/16`
不应稳定提高 source-conflict query 上的 relation accuracy。输出应主要保持 checkpoint
的 default policy prior。

反证：严格匹配后，consensus-only 数量持续、跨内容地提高正确 source selection，且
提升不能由格式熟悉、标签校准、明确关系措辞或 query similarity 解释。

### P2：Single-diagnostic-example jump

将一个 consensus example 替换为等长、等标签分布、等结构的 diagnostic example，应
产生远大于增加若干 consensus examples 的 held-out gain。

反证：效果只来自 recency、长度、罕见词、惊讶度或直接答案重叠；换位置、换编码、换
内容后消失。

### P3：Prior attraction under non-identification

在 `V(D)` 含多个策略时，模型选择应接近独立测得的 checkpoint policy prior；当
diagnostic coverage 提高时，prior 对答案的解释力下降。

这把 Base/Instruct 差异重新解释为两个可分量：default prior 与 evidence responsiveness，
而不是直接归因于 instruction tuning 的机制改变。

### P4：Graph coverage beats example count

在固定 demonstration count、结构覆盖和标签平衡下，未见拓扑表现应随
`C_sep(D,pi*)` 或剩余 `|V(D)|` 改变。对多来源任务，一组覆盖不同 policy pairs 的
示例应优于重复覆盖同一 pair 的示例。

### P5：Two conflicts, opposite signs

把一个 source-consensus item 替换为 target-consistent 的 intra-example source conflict
应提高 policy identification；把一个 target-consistent diagnostic item 替换为同状态、
同表面、但标签支持竞争 policy 的 diagnostic item，应降低或不稳定化识别。后一条件
必须与至少一个仍支持 target policy 的 diagnostic item 同时出现，才是实际的
inter-example conflict。二者必须在同一生成器、同一位置和同一 token budget 下比较。

### P6：Identification is not execution

即使 diagnostic set 唯一识别 `pi*`，模型仍可能在 cross-encoding、cross-content 或
transitive topology 上失败。显式告知 policy 的 upper bound 用来区分：

```text
无法由示例识别规则  vs.  已知规则但不会执行/组合
```

这条负结果同样有价值，因为它防止将所有 ICL failure 都归为 ambiguity。

## 5. 决定性实验程序

### Study 0：Gold relation 与候选策略审计

每个 domain 在模型实验前完成：

1. 写出有限候选策略集合，不从模型错误中事后发明候选；
2. 为每个 item 保存每个 source 的独立状态 `z_s(x)`；
3. 自动验证 source agreement/disagreement、label balance、topology 和 policy signature；
4. 独立人工标注 intended relation、中间 source states 和 alternative interpretation；
5. 审计 selection distribution 在 competing policies 之间是否泄漏词汇、人物、顺序、
   source mention、结构覆盖或 answer frequency；
6. 删除不能在严格标准下唯一解释的 item，并报告删除比例；
7. 冻结 discovery、holdout 和 confirmation pools。

事件语义域尤其要吸收 Imperfective benchmark critique：不能仅凭模板名把 predicate
标成 telic/atelic，也不能把不明确的 non-culmination 当作 gold。

### Study 1：一张图决定论文是否成立

在二来源任务中构造以下 matched support regimes：

```text
Z0        zero demonstration
C1/C2/C4/C8/C16
          only source-consensus, target-consistent demonstrations
D1+C7     one diagnostic + seven consensus demonstrations
D2+C6     two diagnostic + six consensus demonstrations
D8        eight diagnostic, target-consistent demonstrations
D1+X1+C6 one target-consistent diagnostic + one matched diagnostic labeled by a rival policy
          + six consensus demonstrations; the only true inter-demonstration conflict condition
E         explicit policy instruction upper bound
```

所有 evaluation queries 都让候选 sources 给出不同答案，否则 query 本身不能测 authority。

匹配变量：

- demonstration count 与 token budget；
- gold-label frequency；
- lexical items 与 syntactic templates；
- source mentions、position 和 order；
- query similarity 与 primitive structure coverage；
- output format 与 answer-token frequency；
- diagnostic item 的 surprisal、长度和信息密度。

主图预期：

```text
held-out relation accuracy / typed margin
^
|                           diagnostic conditions
|                       ____/
|                      /
|  consensus-only  ----
|  prior baseline  ----
+----------------------------------> number of correct examples
```

真正决定性的不是所有模型都出现同一幅漂亮曲线，而是预注册 contrast：

```text
diagnostic replacement gain
  > matched consensus-count gain

first diagnostic source-conflict effect
  performance(D1+C7) - performance(C8) > 0

inter-demo conflict penalty, holding two diagnostic slots fixed
  performance(D1+X1+C6) - performance(D2+C6) < 0
```

### Study 2：Checkpoint prior 与 contextual update

在 matched Base/Instruct 和至少一个独立模型家族上分开估计：

```text
default policy prior       = conflict queries under Z0 / consensus-only D
diagnostic responsiveness  = change after matched D1 replacement
execution ceiling          = explicit-policy condition E
```

模型比较不以平均 accuracy 排名，而以二维坐标呈现：

```text
x = prior correctness under ambiguity
y = update efficiency after diagnostic evidence
```

可能出现：

- prior 好、update 弱；
- prior 差、update 强；
- 两者都好；
- policy 已识别但执行上限仍低。

这比“instruction model better than base”更有解释力，也与 LP4FM 的 checkpoint-sensitive
composition boundary 接上。

### Study 3：多来源 conflict graph 与未见组合

将来源扩展到三至四类：

```text
queried belief / reality / other agent
context / prior / reporter
process / result / explicit endpoint evidence
```

比较等量 demonstration sets：

- 重复分离同一 policy pair；
- 覆盖目标 policy 与多个 competitors；
- 只给 primitive edges；
- 给出 transitive chain，但 hold out inferred edge；
- 给出 graph-ambiguous 或 cyclic evidence。

核心问题：

1. target-relative separation coverage 是否预测 held-out source choice；
2. 模型能否利用 `a>b, b>c` 组合出 `a>c`；
3. 失败来自仍有多个 version-space policies，还是唯一 policy 已识别但 composition 失败；
4. 旧 LP4FM 的 saturation / topology result 是否只是某些 checkpoint 的执行边界。

### Study 4：三个关系域的确认

**Domain A：ToM source arbitration。**

```text
consensus  = true-belief worlds where belief == reality
diagnostic = false-belief worlds where belief != reality
target     = follow queried agent / follow reality / specified other source
```

这是主域，因为 repo 已有 typed source ontology、latent-world generator、answer-simplex
scorer 和 conflict topology。

**Domain B：local context versus default knowledge。**

```text
consensus  = local context agrees with default/parametric answer
diagnostic = counterfactual local context disagrees with default answer
target     = follow context or follow default source under a stated task family
```

为避免模型知识是否真实的混杂，主确认应使用闭合世界或可控 fictional facts；真实世界
事实只做外部生态效度检查。

**Domain C：process versus result relation。**

```text
consensus  = process occurred and culmination independently established
diagnostic = process occurred while culmination is explicitly blocked
target     = infer occurrence, result, or the specified event relation
```

必须使用 lexical matched pairs、multi-step judgments 和独立母语者审计。若 validity
不能通过，该域降为 benchmark case study，不得充当跨域机制确认。

可选 Domain D 是 reportative/factive source routing；它与 LP4FM 更近，因此不能替代至少
一个真正外部域。

### Study 5：前瞻性 demonstration selection

对候选 pool 中每个 item 先由 gold source states 计算 signature，不看 test labels，选择：

```text
Greedy-Diagnostic
  每一步最大化 target-relative policy elimination

Prior-Weighted Diagnostic
  优先分开模型先验质量最高的竞争策略
```

对照：

- random；
- semantic similarity retrieval；
- diversity；
- CoFe-style simplicity / structure coverage；
- model uncertainty；
- cross-entropy-difference selection；
- oracle test-label selector，只作为不可部署上界。

主要结果不是“又一个 selector 提高平均分”，而是：

> 在严格 token budget 下，policy-separation objective 能否跨新内容、编码和 domain
> 前瞻性预测教学效率，并解释其他 selector 何时成功。

若只在生成器内有效、离开候选策略标注就失效，应把它保留为科学诊断，不包装成通用
prompt optimization 方法。

### Study 6：可选机制桥，不是主结果

只有 Study 1--5 通过后才做：

- 比较 consensus 与 diagnostic support 下可读的 policy representation；
- 测试关键示例 attribution 是否真的指向 policy-separating information，而非位置；
- 使用 wrong-channel、item-shuffled、norm-matched、final-token-only controls；
- 要求跨内容、跨编码和跨 topology 的 policy transport；
- 对照 CoSToM 风格 tracing/steering 和 Jiao et al. 的 conflict heads。

即使机制部分完全失败，行为规律仍可独立成文。反过来，漂亮 probe 或 head ablation
不能挽救行为规律不成立。

## 6. 主要 estimands 与统计设计

### 6.1 核心量

```text
diagnostic replacement gain
  performance(D1+C7) - performance(C8)

consensus count gain
  performance(C16) - performance(C1)

conflict sign contrast
  effect(target-consistent intra-item conflict)
  - penalty(replacing one target-consistent diagnostic with rival-policy diagnostic)

prior retention
  agreement(consensus-only prediction, independently estimated prior)

identification gap
  performance(E) - performance(best nondiagnostic D)

execution gap
  validated ceiling - performance(E)

transfer ratio
  cross-content or cross-encoding gain / same-template gain
```

主要行为量使用 deterministic source identity、target-versus-competitor logprob margin 和
off-policy leak；不把 LLM judge score 作为唯一主指标。

### 6.2 模型比较

最低确认结构：

- matched Base/Instruct checkpoints；
- 一个独立 architecture/training family；
- 一个强闭源模型可作为生态外推，但不能替代 open-weight logprob 分析；
- temperature `0` 的主评估，加独立 stochastic robustness；
- 同一 frozen manifest、scorer 和 answer encoding。

模型数量不是贡献。应先获得三个 domain 中一致的定律，再扩展规模。

### 6.3 统计单位与模型

- sampling unit 是独立 latent world / predicate / closed-world fact；
- paraphrase、answer permutation 和多 source rows 在 item 内聚类；
- 主要分析使用 mixed-effects logistic/linear model 或 item-clustered bootstrap；
- model、domain、checkpoint 和 support regime 的 interaction 预注册；
- discovery、operator/model selection 与 confirmation pools 分离；
- 由 generator 抽样而非 task policy 决定 consensus items；显式测试 policy 能否从
  surface-only prompt 被预测；
- 不用重复 prompts 当作独立样本抬高显著性。

### 6.4 解释变量竞争

在 held-out set 上比较以下预测量：

```text
example count
correct-label count
semantic similarity
lexical overlap
diversity
structural coverage
token count / position
model uncertainty
policy-separation coverage
remaining version-set size
```

Best Paper 候选要求最后两项在 matched factorial 和 out-of-domain prediction 中提供额外
解释力，而不仅是在同一个生成数据集上相关更高。

## 7. 一张主图、一个主表、一个可视化对象

### Figure 1：More correct examples can teach nothing

主图只展示 Study 1 的决定性曲线：consensus-only count、single diagnostic replacement、
two-valid-diagnostic support、真正的 inter-demo conflict 和 explicit-policy upper bound。
读者应在十秒内看到两个结论：

1. 更多正确示例不等于更多关系监督；
2. 两种“冲突”的效应方向相反。

### Figure 2：Demonstration distinguishability graph

左侧是 source state；中间是 candidate policies 对各 demo 的预测 signature；右侧是
remaining version set 与 held-out answer。图中不能画抽象神经电路，除非有因果证据。

### Figure 3：Cross-domain law

横轴使用预先计算的 policy-separation coverage，纵轴使用跨内容 relation transfer；
不同颜色为 domain，不以模型排行榜为主视觉。

### Table 1：解释量的前瞻性比较

比较 count、similarity、diversity、structural coverage、uncertainty 和 policy separation
对 held-out transfer 的预测。报告 out-of-domain / leave-one-template-out 性能，而不是
同数据拟合优度。

## 8. 什么才算 Best Paper 级证据

### Gate A：现象不是视觉错觉

- `D1+C7 > C8` 在至少两个模型家族、三个 domain 中方向稳定；
- `C16-C1` 明显小于 diagnostic replacement gain；
- 位置、长度、label balance、surprisal、lexical overlap 控制后仍成立；
- source conflict query 使用全新 latent worlds。

### Gate B：不是 AmbiBench/aha example 重做

- source-state agreement 是被直接操纵的因果因素；
- 不只在二元 synthetic feature classification 上成立；
- graph coverage 对多来源和未见 topology 有前瞻性预测；
- 识别与执行被 explicit-policy upper bound 分开。

### Gate C：解决了已有文献中的表面矛盾

同一框架同时预测：

- target-consistent source conflict 帮助学习；
- 把一个 valid diagnostic replacement 成 rival-policy diagnostic 的 inter-demo conflict
  伤害学习；
- consensus demos 的 labels 可能不重要；
- counterfactual demos 在 source-prior conflict 时特别有效。

不能只在 Discussion 中口头统一，必须用同一 generator 的 matched experiment 直接展示。

### Gate D：有领域后果

至少实现一项：

1. diagnostic selector 在未见 domain/token budget 下优于主流 selectors；
2. 发现现有 benchmark 的多数 training examples 在目标 relation 上不具诊断性，并由
   重新设计的 teaching set 改变结论；
3. 证明某类“reasoning failure”主要是 task underdetermination，而另一类在 policy
   明示后仍存在，从而修正领域的能力归因。

### Gate E：理论和经验互相约束

理论不是给直觉加公式；它必须在实验前决定：

- 哪些 demos 应为零信息；
- 哪个单一 replacement 应产生跳变；
- 哪些 policy pairs 仍无法区分；
- 哪个 held-out topology 需要 relation composition；
- 什么时候 explicit policy 也不能修复。

若只得到平均相关性，没有这些 prospective wins，应按 oral/regular paper 级别处理。

## 9. 与 LP4FM 和五个分支的重新分工

### 9.1 LP4FM 是 Paper A，不是新论文的 pilot 结果

当前已投稿 LP4FM 建立：

```text
language form
-> typed source arbitration under fixed latent worlds
-> checkpoint-sensitive composition
-> same-item reconstruction does not imply typed controller transport
```

它为本研究提供：

- typed source ontology；
- source-conflict generator；
- answer-simplex scorer；
- conflict topology；
- checkpoint prior 差异；
- 机制 claim ladder 和 transport controls。

但它没有 demonstrations，也没有操纵 demonstration-level policy distinguishability，
所以不能计入 Paper B 的主要确认数据。

### 9.2 原 ACG 是本文的执行/组合半部

ACG 现在不再作为最高层母对象，而承担：

```text
先由 diagnostic demonstrations 唯一识别 source policy
-> 再测试该 policy 能否跨内容、编码和未见 topology 组合执行
```

这使 ACG 更强，因为它终于能区分：

- demonstrations 根本没说明要学什么；
- demonstrations 已说明，但模型不会执行；
- primitive policy 会执行，但不会组合到新 topology。

### 9.3 五个分支的职责

| 原分支 | 在新主线中的角色 | 不再承担的中心 claim |
|---|---|---|
| Training-induced phase transition | 比较 checkpoint prior 与 diagnostic responsiveness | Base/Instruct endpoint 自动证明训练因果 |
| Behavioral source-access algebra | Study 3 的 graph composition 与 held-out topology | 二元 saturation 自动构成 universal algebra |
| Mechanistic identifiability | Study 6 的 claim ladder 与 transport controls | probe/readout 是主发现 |
| Conflict-topology kernel | 构造 policy-separation graph 和跨域 prediction | 单域 kernel 即共享神经路由 |
| Source-accessibility objective | 后续训练模型偏好 diagnostic evidence、拒绝 policy-inconsistent evidence | consistency gain 即机制修复 |

五条分支不再平行扩张，而是服务同一条证据链。

## 10. 最小高信息量 pilot

### 10.1 先只回答一个问题

> 在 held-out false-belief conflict query 上，一个 matched diagnostic demonstration 是否
> 稳定胜过八个 source-consensus correct demonstrations？

最小设计：

```text
models       = Qwen2.5-7B Base + Instruct; Qwen3 as frozen boundary
policies     = follow queried belief vs follow reality
support      = Z0, C1, C4, C8, D1+C7, D2+C6, D1+X1+C6, explicit-policy upper bound
worlds       = independent discovery and confirmation latent worlds
encodings    = natural source names + permuted source/answer labels
queries      = all belief--reality conflict
metrics      = source identity, typed logprob margin, off-policy leak
```

必要 falsification controls：

- 把 diagnostic demo 放在首、中、尾；
- 保持 gold label distribution 相同；
- 交换 agent/reality token 与答案标签；
- 使用 diagnostic demo 的 matched lexical twin，但让 sources 一致；
- 用同一 source-state / template pair 产生 target-policy label 与 rival-policy label，且
  rival-policy item 只与一个 valid diagnostic item 成对出现；
- 新 world、新人物、新对象、新表述上确认。

### 10.2 Pilot 的 go/no-go

**Go：** diagnostic replacement gain 在 Base/Instruct 中至少方向一致，并在 permutation
和 confirmation worlds 中保留；`D1+X1+C6` 相对 `D2+C6` 产生预注册的 conflict penalty。

**Narrow：** 只在 Instruct 成立。转为 checkpoint-prior/plasticity boundary，不立即做
跨域大论文。

**Stop：** 效应由位置、lexical cue、答案复制或 explicit relation wording 解释，或者
consensus-only examples 同样产生完整跨内容 relation transfer。此时不再把 conflict-as-
supervision 作为母对象，回到窄 ACG。

## 11. 实施顺序

### Phase 1：定义冻结

- 冻结 candidate policies、source-state schema 和 version-set calculator；
- 写 semantic validity rubric；
- 生成 matched consensus / target-diagnostic / rival-policy-diagnostic triples，并验证
  selection-blinding；
- 建立 item-level provenance 与 deterministic validators。

### Phase 2：决定性 ToM pilot

- 跑 Study 1 最小矩阵；
- 只看预注册 contrasts；
- 在 confirmation worlds 重跑；
- 根据 go/no-go 决定是否扩域。

### Phase 3：图结构与外部域

- 加入三来源 policy graph；
- 加入 closed-world context/default domain；
- 完成 event semantics validity screen 后再纳入第三域；
- 冻结跨域 analysis model。

### Phase 4：前瞻性选择与论文结果

- 比较 diagnostic selector 与 retrieval/coverage/uncertainty baselines；
- 做 leave-domain-out 和 leave-template-out prediction；
- 完成主图、主表和 benchmark audit；
- 根据证据决定 regular/oral/Best-Paper-candidate claim tier。

### Phase 5：可选机制与训练

- 只补能解释已确认规律的 attribution、steering 或 checkpoint trajectory；
- 不因机制结果漂亮而扩大主 claim；
- 不让高风险 GPU 实验阻塞行为论文完成。

## 12. 失败结果仍然能回答什么

| 结果 | 解释 | 论文动作 |
|---|---|---|
| Consensus count 有效，diagnosticity 无额外解释力 | 模型可从未建模线索、分布或 similarity 识别 task | 扩充候选策略/控制；若仍成立则停止主命题 |
| Diagnostic 只在同模板有效 | surface retrieval，不是 relation induction | 不称跨内容 supervision law |
| Diagnostic 唯一识别 policy，但 explicit upper bound 仍低 | execution/composition failure | 转为 identification--execution dissociation 论文 |
| Intra-source 与 inter-demo conflict 都伤害 | 模型将任何冲突视为异常或无法做 source arbitration | 形成 conflict processing boundary，不写正监督规律 |
| 只在 ToM 成立 | domain-specific belief/reality phenomenon | 作为 LP4FM 后继短线，不写一般 ICL law |
| Selector 只在已知 generator 有效 | gold-structure diagnostic，不具部署普适性 | 保留科学工具，删除通用优化叙事 |
| Event domain validity 不通过 | benchmark 不能识别模型能力 | 把它作为 benchmark audit，不计跨域确认 |

## 13. 论文叙事骨架

### 开场反例

给模型八个 true-belief demonstrations，每个答案都正确；然后问一个 false-belief query。
这些示例看似充分，实际上对“follow belief”与“follow reality”提供了完全相同的证据。
再只替换其中一个示例，使 belief 与 reality 分离，保持标签、长度和结构匹配。若模型
行为发生跨内容跳变，读者立即理解论文对象。

### 主论证

```text
correct demonstrations can be relationally non-identifying
-> source disagreement creates policy-separating supervision
-> inter-demonstration disagreement instead corrupts the policy
-> distinguishability graph predicts transfer beyond count and similarity
-> explicit-policy controls separate identification from execution
-> diagnostic selection changes how ICL data should be built
```

### 不要采用的叙事

```text
LLMs do not truly understand language
five cognitive papers all reveal one hidden mechanism
we propose another large benchmark and test many models
we find a new universal neural circuit for conflict
```

## 14. 暂定标题与摘要骨架

### 推荐标题

**Conflict Is Supervision: Identifying Latent Source Policies in In-Context Learning**

中文：**当冲突成为监督：上下文学习中的潜在来源策略识别**。

备选：

- **When Agreement Teaches Nothing: Diagnostic Demonstrations for Relational ICL**
- **Correct Examples Need Not Identify the Task**
- **What Makes a Demonstration Informative? Source Conflict and Policy Identification**

### 摘要骨架

1. ICL 通常按示例数量、正确率、相似度和结构覆盖评价 demonstrations。
2. 这些量忽略一个必要条件：候选关系策略是否在示例上产生不同预测。
3. 提出 source-policy distinguishability 的形式化和 matched benchmark。
4. 展示 source-consensus 正确示例几乎不改变冲突 query，而一个 diagnostic example
   产生跨内容跳变；将另一个 valid diagnostic 替换为 rival-policy diagnostic 的
   inter-demo conflict 则有相反效应。
5. 该规律跨三个关系域，并在未见 source graph 上预测 transfer。
6. 一个基于 distinguishability 的 selector 在固定 budget 下优于 count/similarity/
   diversity/coverage baselines。
7. explicit-policy upper bound 揭示 task identification 与 relation execution 是两种失败。

其中第 4--6 点在实验完成前只能作为计划，不能写进实际摘要。

## 15. 最终定位

```text
program-level question
  how relations survive from context to decision

best-paper candidate law
  demonstrations teach a latent source policy only where candidate policies disagree

decisive distinction
  intra-example source conflict = potentially diagnostic supervision
  inter-example policy conflict = corruption or ambiguity

formal object
  policy distinguishability / remaining version set / source-conflict graph

empirical burden
  one diagnostic example beats many matched consensus examples across domains

practical consequence
  select and audit demonstrations by relation-diagnostic coverage

current evidence
  LP4FM supplies the ontology and boundary, not validation of this law

status
  argument_shaped; requires a new frozen pilot and independent confirmation
```

这不是通过堆更多认知概念追求“颠覆”。它真正有机会改变默认认识的地方是：

> **语言模型从示例中学到什么，首先受示例能区分什么约束。对于来源关系，agreement
> 可能让一百个正确答案仍然没有教学信息，而一次受控 disagreement 才第一次告诉模型
> 谁拥有答案权。**

## 16. 参考入口

### 最强直接近邻

- [Task Ambiguity in Humans and Language Models](https://arxiv.org/abs/2212.10711)
- [Can Input Attributions Explain Inductive Reasoning in In-Context Learning?](https://aclanthology.org/2025.findings-acl.1092/)
- [Understanding the Dynamics of Demonstration Conflict in In-Context Learning](https://arxiv.org/abs/2603.04464)
- [Debiasing In-Context Learning by Instructing LLMs How to Follow Demonstrations](https://aclanthology.org/2024.findings-acl.430/)
- [Context-faithful Prompting for Large Language Models](https://aclanthology.org/2023.findings-emnlp.968/)
- [Rethinking the Role of Demonstrations](https://aclanthology.org/2022.emnlp-main.759/)
- [How Do In-Context Examples Affect Compositional Generalization?](https://aclanthology.org/2023.acl-long.618/)
- [What Do Language Models Learn in Context? The Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/)
- [Active Example Selection for In-Context Learning](https://aclanthology.org/2022.emnlp-main.622/)
- [The Teaching Dimension of Linear Learners](https://proceedings.mlr.press/v48/liua16.html)
- [An Explanation of In-context Learning as Implicit Bayesian Inference](https://arxiv.org/abs/2111.02080)
- [Large Language Models Are Latent Variable Models](https://arxiv.org/abs/2301.11916)
- [What In-Context Learning “Learns” In-Context](https://aclanthology.org/2023.findings-acl.527/)
- [Is In-Context Learning in Large Language Models Bayesian?](https://proceedings.mlr.press/v235/falck24a.html)

### 本项目吸收的 2026 工作

- [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/)
- [The Imperfective Paradox Is Not Necessarily in Large Language Models](https://arxiv.org/abs/2608.25005)
- [Characterizing the Expressivity of Local Attention in Transformers](https://aclanthology.org/2026.acl-long.1739/)
- [Memory efficiency and resource-rational encoding in sentence processing](https://aclanthology.org/2026.acl-long.1550/)
- [CoSToM](https://aclanthology.org/2026.acl-long.421/)
- [ImplicitMemBench](https://aclanthology.org/2026.acl-long.1301/)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
