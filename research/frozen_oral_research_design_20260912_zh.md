# 冻结版 Oral 研究方案：上下文改变的是事实，还是裁决规则？

更新日期：2026-09-12  
研究状态：`not_evaluated`  
设计状态：`oral_shaped`，尚未 `oral_ready`

## 0. 冻结决定

经过 exact-neighbor audit，本项目不再把“LLM 有来源偏好”或“模型能从上下文学习
source policy”作为 headline。这些说法分别会撞上 source-preference、context-
faithfulness、rule induction、task-vector 和 functional-ToM 文献。

冻结后的论文对象是：

> **context-conditioned rule updating：当上下文改变时，模型究竟是在更新当前事实，
> 还是在更新“冲突时哪个来源有资格决定答案”的裁决规则？**

一句话标题：

> **When Context Changes the Rule: Separating Fact Updating from Adjudication Updating in Language Models**

中文标题：

> **当上下文改变规则：分离语言模型中的事实更新与冲突裁决更新**

核心现象命名为：

```text
rule-change without fact-change
```

它不是预先声称模型具有某种内部模块，而是一个行为层研究对象：在事实、实体、query、
答案接口和表面编码保持不变时，只改变上下文所诱导的 governing rule，观察模型的
原生答案或行动是否随规则改变，并能否迁移到新内容。

## 1. 为什么这是比旧方案更准确的母对象

近邻工作已经分别回答了：

| 已有问题 | 已有答案 | 本文不能再声称 |
|---|---|---|
| 模型偏好哪类来源？ | source credibility hierarchy、重复效应和位置效应 | 首次发现 source preference |
| 模型是否忠实于外部 context？ | memory strength、evidence style、DPO/steering 会改变 context faithfulness | 首次发现 context can override memory |
| demonstrations 是否教会任务/规则？ | task ambiguity、rule conflict、compositional generalization 已有系统研究 | 首次研究 latent task/rule induction |
| 表示是否可读、是否可迁移？ | task vector、belief probe、distributed template 和 representation transfer 已有近邻 | probe/readout 等于 native use |
| ToM 是否需要行动适应？ | functional ToM、partner adaptation 和 embodied evaluation 已经提出 | 首次将 ToM 推向 action |

真正尚未被同一协议回答的问题是：

```text
fact update                 = context changes which value is accepted
rule update                 = context changes which source is entitled to decide
rule execution              = the changed rule controls native answer/action
rule transport              = the same change survives new content/encoding/topology
```

过去的 context-faithfulness 实验通常改变的是外部事实；过去的 task-induction 实验通常
改变的是任务映射或标签规则。本文把二者放入同一 `2 × 2` 设计，直接估计事实效应和
规则效应是否可分离。

## 2. Research object

### 2.1 Latent world

每个 episode 的潜在世界 `w` 包含：

```text
entities          = 人物、事件、对象
source set S      = queried belief, reality, testimony, memory, instruction
source values     = z_s(w) for each s in S
conflict topology = which sources disagree and how they are connected
query             = asks for the value to be returned or action to be taken
```

来源名称、人物名称和答案 token 在数据生成时随机化，避免模型通过名称本身恢复目标。
每个 source 都必须在不同 episodes 中既被选中也被拒绝。

### 2.2 Governing adjudication rule

定义一个 episode-local adjudication rule：

```text
π : (query type, conflict topology) -> selected source
```

本文只把 `π` 当作可观察任务结构的操作性变量，不预设模型内部有一个 source-policy
module。第一版固定使用三类可组合规则：

1. **priority rule**：在冲突中按上下文指定的 source priority 选取来源；
2. **query-conditioned rule**：不同 query type 选择不同 source，例如 queried belief
   与 reality query 使用不同来源；
3. **topology-conditioned rule**：在二元和三元冲突中按预设关系选择 source，而不是
   简单 majority、recency 或 first-seen。

主实验只使用预注册的有限 policy family；不在结果出来后自由选择规则。

### 2.3 Four separable levels

```text
fact recall       = 能否复述 demonstrations 中出现过的 value
fact updating     = 是否接受当前 context 提供的 value
rule reading      = 能否外显报告当前 governing rule
rule execution    = native answer/action 是否按该 rule 选择 source
```

主终点是 `rule execution`。`rule reading` 只作为诊断，不能替代原生答案；probe、
logit lens、steering 和 activation patch 也不能单独证明 rule execution。

## 3. Central design: a factorial separation

本文所有核心 episode 都来自两个正交变量：

```text
F = current source facts / values
Π = governing adjudication rule induced by demonstrations
```

构造四类 matched condition：

| Condition | 当前事实 | governing rule | 用途 |
|---|---|---|---|
| `F0Π0` | baseline | baseline | reference |
| `F1Π0` | changed | unchanged | fact-updating effect |
| `F0Π1` | unchanged | changed | rule-updating effect |
| `F1Π1` | changed | changed | interaction / separability |

关键 estimands：

```text
FAE = E[Y(F1, Π0) - Y(F0, Π0)]
RUE = E[Y(F0, Π1) - Y(F0, Π0)]
INT = E[Y(F1, Π1) - Y(F1, Π0) - Y(F0, Π1) + Y(F0, Π0)]
```

其中 `Y` 不是简单 accuracy，而是：

1. native answer 是否等于 `z_{π}(w)`；
2. source-choice output 是否选择 `π` 指定的 source；
3. 在需要协调的 episode 中，第一次 action 是否按 `π` 采取。

这使论文可以回答一个比“模型是否 faithful”更精确的问题：

> 当外部 context 改变时，模型是更新了内容，还是更新了内容裁决规则？

## 4. The decisive counterfactual

每个确认单元都构造一对：

```text
same latent world
same source values
same entities
same query
same answer candidates
same output format
different demonstrations-induced rule
```

记为：

```text
(w, D_Π0, q)  vs.  (w, D_Π1, q)
```

只有当 native answer 或 action 随 `Π0 -> Π1` 改变，并且这种改变在新世界上复现，才
支持“上下文改变了裁决规则”。如果只观察到模型更愿意接受某个 value，则只能写
context faithfulness 或 fact updating。

## 5. Three main experiments

### Experiment 1：Can demonstrations identify a rule?

#### Research question

在 token 数量、长度、示例数量、词面相似度和答案格式匹配后，真正决定 policy
execution 的是 source-conflict information，还是普通的 example count、similarity、
diversity 和 structural coverage？

#### Context conditions

```text
consensus support       = 所有 source 给出相同 value，无法区分 policy
single diagnostic       = 一个 source conflict 足以区分两个 policy
balanced diagnostic     = 最小集合，覆盖多个 policy pair
redundant diagnostic    = 重复相同 conflict，不增加 policy information
corrupted policy        = demonstrations 对 policy 不一致
fact-only context       = 改变 value，但不改变 governing rule
```

定义 context 的可计算信息量：

```text
version-space reduction = 1 - |V(D)| / |V|
```

其中 `V` 是预注册 policy family，`V(D)` 是与 demonstrations 一致的 policy 集合。

#### Primary outcome

在新 latent worlds 上测量：

```text
held-out native policy execution accuracy
source-choice accuracy
rule-report accuracy, secondary only
```

#### Required comparisons

- matched example count；
- matched token budget；
- matched lexical similarity；
- matched diversity and structural coverage；
- source-name permutation；
- answer-label permutation；
- demonstration-order permutation；
- explicit-rule upper bound。

#### Oral-level prediction

若模型学习的是 governing rule，而不是重复、相似度或答案标签，则 `version-space
reduction` 应比 example count、lexical similarity 和 raw coverage 更能预测 held-out
native execution。

这不是把 `version-space reduction` 预先封装为定律，而是一个可被 matched controls
推翻的设计预测。

### Experiment 2：Does the rule change the answer when facts do not?

这是全篇决定性实验。

#### Main contrast

对同一个新世界执行 paired policy swap：

```text
world facts: unchanged
query: unchanged
source names: permuted and counterbalanced
answer interface: unchanged
demonstrations: Π0 -> Π1
```

#### Four readouts

1. **native answer**：只要求回答，不要求解释；
2. **source choice**：要求选择依据哪个 source；
3. **brief rationale**：用于检查是否报告了与答案一致的 rule；
4. **first action**：在协调/沟通 episode 中记录第一次行动。

native answer 是 primary；source choice 和 rationale 是 diagnostic；first action 只有
在 source-choice 和 native answer 都通过后才进入主文。

#### Decisive estimand

```text
paired rule-swap effect
= P(Y follows Π1 | D_Π1, w)
  - P(Y follows Π1 | D_Π0, w)
```

同时报告：

```text
fact-swap effect
label-swap effect
source-name effect
order effect
rule × fact interaction
```

#### Interpretation table

| 结果 | 允许的解释 |
|---|---|
| source choice 和 native answer 都随 rule swap 改变 | 支持 native rule execution |
| source choice 改变，native answer 不变 | source readout 与 final commitment dissociation |
| native answer 改变但只在原 item | pattern/answer association，未通过 rule transfer |
| 只有事实变化改变答案 | context faithfulness，不支持 rule updating |
| 只有 source names 或 labels 改变答案 | semantic/format shortcut |
| rule swap 对所有模型均无效 | 有价值的 boundary result：context 不足以更新裁决规则 |

### Experiment 3：Does the rule transport?

Experiment 2 只证明 rule swap 是否能影响行为；Experiment 3 决定它是否脱离原始示例。

#### Three held-out axes

```text
content transfer       = 新人物、新对象、新事实、新 latent worlds
encoding transfer      = 新 source labels、新答案顺序、新输出格式
topology transfer      = training 只见二元冲突，test 出现未见三元冲突
```

#### Topology construction

训练阶段提供：

```text
A vs B
A vs C
B vs C
```

测试阶段提供：

```text
A vs B vs C
```

三元测试必须区分：

```text
learned priority composition
majority heuristic
recency heuristic
first-position heuristic
direct-label memorization
```

#### Primary outcome

```text
rule-swap effect × transfer axis
```

不是只报告三元 task accuracy，而是报告 rule-induced change 是否在每个 transfer axis
上仍然存在。

#### Oral-level prediction

如果 demonstrations 诱导的是可迁移的裁决规则，rule-swap effect 应在 content 和
encoding holdout 上保留；topology holdout 可以下降，但应显著超过 majority、recency、
source-order 和 direct-label baselines。

若只在同一 source labels、同一 topology 或同一 item 上成立，结论降为局部 pattern
following。

## 6. Dataset and generator requirements

### 6.1 Synthetic but semantically valid

数据使用 synthetic latent worlds，但每个 episode 必须通过独立 validity checks：

- source values 在语言上明确且互相冲突；
- query 唯一决定需要返回的 source/value；
- policy A/B 在 target item 上确实产生不同 gold answer；
- consensus item 不被误标为 diagnostic；
- 三元 topology 不可由 source order 或 token identity 直接解码；
- 所有 policy 在不同内容、source 和 query 上均衡出现。

### 6.2 Source ontology

第一版只保留四类 typed sources，避免对象扩张：

```text
queried belief
reality
testimony/report
memory/record
```

`instruction` 只在 robustness control 中出现，不与 system/user instruction hierarchy
混淆。这样可以明确区别 semantic source authority 与显式 instruction priority。

### 6.3 Policy family

主实验不允许由结果倒推 policy。预注册 policy set：

```text
Π1: queried-belief first
Π2: reality first
Π3: testimony first
Π4: topology-conditioned mixed rule
```

其中 `Π4` 只在 topology transfer 作为组合规则出现，且必须在训练/测试 policy split
中保持严格独立。

## 7. Strongest rival explanations and controls

### Rival 1：static source preference

模型本来就偏好某个 source，不是上下文改变了 rule。

Control：在每个 source 上交叉 policy、位置、名称、value 和 conflict topology；先测
无 demonstrations 的 source preference baseline，再计算增量 rule-swap effect。

### Rival 2：fact updating only

模型只是接受了当前 context 的 value。

Control：`F1Π0` 与 `F0Π1` factorial separation；同一 facts 下只交换 rule。

### Rival 3：answer-label mapping

模型只学会了 label 与答案 token 的对应。

Control：答案 token、选项顺序、source label 和输出格式全部 permutation；native
answer 必须在新 encoding 下保持 rule effect。

### Rival 4：recency / first-position heuristic

模型只跟随最后或最先出现的 demonstration。

Control：order-balanced contexts；让同一 rule 在不同位置表达，让不同 rule 出现在
相同位置，并加入 recency/first-position predictors。

### Rival 5：surface semantic cue

模型根据 `belief`、`reality` 等词的预训练含义作答。

Control：source names 置换为 nonce labels；随后再恢复自然 source names，比较 semantic
cue gain 与 genuine rule transfer。

### Rival 6：explicit instruction following

模型不是从 demonstrations 归纳 rule，而是读取一句显式规则。

Control：explicit-rule condition 作为 upper bound 和 calibration reference，不能用来
证明 implicit rule induction。

### Rival 7：readout without use

模型可以报告 rule，但 native answer 不随 rule 变化。

Control：把 report、source choice、native answer 和 first action 分成四层，禁止用
高层输出替代低层终点。

## 8. Models and replication

最低模型矩阵：

```text
Qwen2.5-7B-Instruct
Qwen2.5-7B-Base
Qwen3-8B
Llama-3.1-8B-Instruct
```

主结论不能依赖单一 checkpoint。若资源有限，先在 Qwen2.5-7B-Instruct 完成完整三项
实验，再用一个 Base/Instruct matched pair 复现 Experiment 2 的 native rule-swap
effect；复现不是把所有模型都跑成同等深度。

LP4FM 旧结果不能作为新论文的 confirmation sample。新实验必须使用：

- 新 latent worlds；
- 新 entity/value seeds；
- 新 prompt templates；
- 新 demonstrations；
- 独立 scorer 和 held-out confirmation split。

## 9. Statistical and provenance contract

### Sampling unit

主要 bootstrap 单元为 `latent world × policy pair`，不能把同一 world 的多个 token 或
answer option 当作独立样本。

### Required reports

- paired policy-swap effect 及 bootstrap interval；
- fact-swap effect 与 rule-swap effect 的差异；
- item-level matched transfer；
- source-name、label、order 和 position controls；
- policy-wise confusion matrix；
- model/checkpoint-wise replication；
- failure regime by topology and encoding；
- full manifest、generator hash、scorer version 和 prompt provenance。

### Promotion gate

主结果只有同时满足以下条件，才能升级为 `oral_shaped` 的经验核心：

1. Experiment 2 的 native rule-swap effect 在预注册 policy pairs 上通过 paired gate；
2. effect 在新 content 上保留，而不是只在 replay 上存在；
3. label/source/order controls 排除主要 shortcut；
4. Experiment 1 显示 diagnostic information 比 raw example count 更接近执行结果；
5. 至少一个独立模型或 matched checkpoint 复现 Experiment 2。

经验上 `oral_ready` 还需要：独立 confirmation split、完整 provenance、主图重绘、
related-work citation audit 和 reviewer-style adversarial audit。

## 10. Main-paper displays

主文只保留三张图：

### Figure 1：Fact update vs. rule update

显示 `FAE`、`RUE` 和 `INT`，同一坐标系中呈现四个 factorial conditions。图标题回答：

> Context changes the accepted value, the adjudication rule, or both?

### Figure 2：Paired rule-swap matrix

每个 cell 是同一 latent world 的 `Π0 -> Π1` native answer change，旁边放 source-choice
和 label/order control。图标题回答：

> Does changing the governing rule change the model’s native commitment when facts do not?

### Figure 3：Rule transport map

横轴为 content、encoding、topology transfer；纵轴为 rule-swap effect。对比 direct-label、
majority、recency、source-order 和 explicit-rule baselines。图标题回答：

> Does a context-induced rule survive the world in which it was taught?

所有 intermediate probe、activation、judge、失败样例和完整 policy confusion matrix
进入 supplement 或 artifact，不承担主论证。

## 11. Related-work positioning for the paper

Related Work 应按“已有测量单位—本文缺失单位”组织：

1. **Context faithfulness and knowledge conflict**：已有工作测量模型是否接受外部
   value，本文加入 governing-rule update 的正交维度；
2. **ICL task/rule induction**：已有工作测量任务识别、示例选择和组合泛化，本文把
   latent object 收紧为 source-adjudication rule，并要求 same-world native swap；
3. **Task/belief representations**：已有工作研究可读性和转移，本文把 representation
   作为 secondary diagnostic，把 native execution 作为 primary outcome；
4. **Functional ToM and partner adaptation**：已有工作说明 action-level evaluation
   更严格，本文只在 rule execution 成立后把 first action 作为 downstream consequence；
5. **Semantic relation vs. final judgment**：未完成体研究说明中间语义区分与最终判断
   可以分离，本文将同样的纪律用于事实更新与规则更新的分离。

## 12. Relation to LP4FM and previous branches

### LP4FM 的准确位置

LP4FM 是 Paper A：

```text
fixed latent worlds
-> typed source arbitration under linguistic pressure
-> checkpoint-sensitive behavioral boundary
```

它提供新论文的 motivation、typed source ontology 和 conflict-grammar design precedent，
但不提供 Paper B 的 empirical confirmation。Paper B 必须重新生成 demonstrations、worlds、
queries 和 confirmation split。

### 五个旧分支的吸收方式

| 旧分支 | 在冻结方案中的位置 |
|---|---|
| learning source-access algebra | 变成 topology transfer 的候选分析，不再独立做 algebra headline |
| conflict-topology kernel | 变成 source-order/majority/recency 的 topology controls |
| mechanism identifiability | 变成 readout/native execution 分层与 secondary diagnostics |
| source-accessibility training objective | 暂不进入主文；只有在 rule-update failure 被稳定复现后才做 repair study |
| identification → commitment | 保留为 Experiment 1→2 的过程结构，但不再把 partner adaptation 当作主贡献 |

## 13. Claim ladder and failure paths

### Strong success

可以写：

> 在受控 synthetic typed-source environments 中，诊断性 demonstrations 不仅改变模型
> 对当前 value 的接受程度，还能诱导一个可迁移的 adjudication rule；当 source values 和
> latent world 固定、只交换 governing rule 时，模型的 native answer 在新内容和新编码
> 上随规则改变，并在一定范围内迁移到未见 conflict topology。

不能写：

- 模型拥有 intrinsic ToM；
- 模型内部存在可定位的 source-policy module；
- 模型获得了人类式语义理解；
- 该结果适用于所有 ICL 或所有 source conflicts；
- probe/readout 成功等于 native rule execution。

### Partial success

| 观察 | 论文降级为 |
|---|---|
| policy 可报告但 native answer 不变 | rule representation–commitment dissociation |
| native answer 只在 replay 有效 | same-item pattern following boundary |
| content 可迁移、encoding 不可迁移 | representation-dependent rule execution |
| rule-swap 只在自然 source names 有效 | semantic prior / source-cue dependence |
| 只有 context faithfulness 有效 | fact updating without adjudication updating |
| 所有 implicit conditions 失败 | strong negative result：demonstrations 不足以更新冲突裁决规则 |

每一种失败都改变 claim，不允许通过增加术语把结果重新包装成“模型已经学会规则”。

## 14. Frozen status

```text
exact-neighbor audit       = complete at metadata/abstract + selected full-text level
candidate gap              = partly holds, candidate only
mother object              = context-conditioned rule updating
primary phenomenon         = rule-change without fact-change
design                     = frozen
empirical evidence         = not_evaluated
LP4FM                      = Paper A motivation and design precedent
old source-policy proposal = superseded
old identification→commitment = retained as internal experiment sequence
oral status                = oral_shaped by design, not oral_ready by evidence
```

## 15. Immediate execution order

1. 冻结 policy family、source ontology、prompt templates、seed 和 scoring interface；
2. 先生成 Experiment 2 的 paired policy-swap pilot，不先跑大规模 benchmark；
3. 通过 validity screen 后生成 Experiment 1 的 information conditions；
4. 只在 native rule-swap effect 成立后生成 Experiment 3 的 topology holdout；
5. 最后做独立模型/checkpoint confirmation 和主图重绘；
6. 若 Experiment 2 不通过，立即停止“rule execution”叙事，转写为 fact-vs-rule
   updating boundary paper。
