# Best Paper 级下一阶段方案：从关系到答案承诺

英文对应稿：[From Relation to Commitment: Prior Rewriting in Language Models](best_paper_relation_to_commitment_20260911.md)。

更新日期：2026-09-11

当前状态：`not_evaluated`。这是一份基于现有 LP4FM 证据、既有五个分支和
《The Imperfective Paradox in Large Language Models》的研究推演方案；它不是新规律
已经成立的经验结论。

## 0. 直接判断

这张截图真正值得借鉴的地方，不是再做一个“语言学现象 benchmark”，而是它把一个
非常细的关系拆成了两个不同问题：

```text
模型是否能区分关系
-> 模型是否在最终判断中使用了这个关系
```

这会把我们当前的下一阶段从“demonstrations 能否诱导 source policy”再推进一步：

```text
Conflict Is Supervision       = 关系在上下文中是否被识别
ACG                            = 被识别的关系能否跨内容、编码和拓扑执行
Relation-to-Commitment         = 被识别的关系能否 survive 到最终答案
```

因此，原来的“当冲突成为监督”不应被删除，但不宜继续作为最高层标题。更强的论文
主线应当是：

> **语言模型可能已经读出了一个关系，却在最终承诺时服从另一个默认先验。上下文
> 中的诊断性冲突能够识别关系，但并不自动保证关系会被兑现。**

暂定的读者可见标题是：

> **From Relation to Commitment: When Language Models Represent a Relation but Follow a Prior**

“Commitment Gap”可以作为本文内部的操作性简称，但目前不能写成领域已经公认的
理论术语。第一处正式定义应使用普通描述：**relation–commitment dissociation**，并
明确说明本文把它操作化为一个测量量。

这条线比单独扩张 ACG 更接近 Best Paper，原因是它试图改变基本测量单位：不再把
准确率、probe 分数、steering gain 和上下文适应都叫作“理解”，而是追踪同一个关系
从规定、表征、访问、来源仲裁到最终答案的完整传递。

## 1. 截图论文真正提供了什么

### 1.1 可以直接吸收的研究结构

《The Imperfective Paradox in Large Language Models》以事件体义为对象，区分：

- 对无界活动（例如 running），过去进行时描述通常蕴含事件发生；
- 对有界成就/完成事件（例如 building a gazebo），进行中的过程不蕴含终点已经
  实现；
- 模型内部表示可能区分 progressive 和 simple past，但最终 NLI 判断仍被“目标会
  完成”的默认叙事先验牵引；
- 直接提示可以减少一种错误，却可能增加另一种错误，形成决策层面的 trade-off。

对我们的直接启发有四点：

1. **选择一个关系，而不是一个大能力名词。** 研究对象应是 source relation、
   process/result relation 或 context/default relation，而不是“语义理解”。
2. **用最小对比制造关系分叉。** 只改变候选来源、关系方向或终点条件，尽量固定
   词汇、长度、格式和世界状态。
3. **把中间关系判断与最终答案分开。** “模型说得出关系”不能替代“模型按关系作答”。
4. **把修复副作用作为结果。** 一个提示让模型少犯错误，不等于它获得了关系规则；
   如果它同时破坏邻近的合法判断，说明的是决策重新分配，而不是理解被修复。

### 1.2 不能直接照搬的部分

| 方面 | 事件语义论文研究什么 | 本项目不能直接照搬的原因 |
|---|---|---|
| 关系 | progressive 与 culmination 的蕴含 | 我们的核心关系是 typed source arbitration，不是 telicity 本身 |
| 输入单位 | premise–hypothesis NLI 对 | 我们必须研究 demonstrations、target 和跨内容迁移 |
| 先验 | goal attainment / teleological prior | 我们需要比较 reality-copying、parametric-default、surface-form 等不同先验 |
| 干预 | strict definition、counterfactual 等 prompt | 我们还要区分 relation identification、retention 和 final commitment |
| 泛化 | 词汇/语义类别内的外推 | 我们要求 cross-content、cross-encoding 和 unseen conflict topology |
| 机制 | 表征相似度与最终判断的分离 | 我们不能把 probe 或最后 token 的变化直接命名为 source circuit |

### 1.3 必须吸收的反驳

后续批评稿《The Imperfective Paradox Is Not Necessarily in Large Language Models》
把一个重要方法论边界说得很清楚：如果 premise 没有明确排除终点，模型的答案可能
反映的是自然语言的多重解释，而不是模型的语义失败。它还区分了：

- aspectual 或关系分类错误；
- intermediate semantic judgment 与最终 label 的映射错误；
- 对表面形式或常见答案的吸引；
- prompt 改变 label 分布但没有改善底层推理。

这不是本项目的旁枝。LP4FM 已经出现了同型风险：自动 stimulus invariant 可以通过，
但 changed dimension 和 commitment relation 的一致性并不自动成立。因此，我们要
把“关系有效性”放在所有模型结论之前，并把不确定语义样本从主分析中隔离出去。

## 2. 新的科学对象

### 2.1 一句话定义

本文研究的是：

> 在一个关系被语义上充分规定、且上下文包含竞争来源的条件下，模型是否把该关系
> 从上下文带到最终答案；若关系在中间状态可读而最终答案服从默认先验，则出现
> relation–commitment dissociation。

这里的“承诺”是功能描述，指最终 label、答案 token、排序决定或第一次行动；它不
   预设模型内部存在一个名为 commitment 的独立模块。

### 2.2 形式对象

对一个实例定义：

```text
w        latent world or task instance
R        intended typed relation
S        candidate information sources
z_s      answer implied by source s
T        source/conflict topology
D        demonstrations, description, or contextual evidence
P        default prior or surface-favored answer policy
M        intermediate relation state or relation readout
Y        final answer/label/first action
k        model checkpoint
g        content, encoding, or interference transformation
```

候选 source policy 为：

```text
pi : (x, T) -> S
h_pi(x, T) = z_{pi(x,T)}(x)
```

`R` 规定在当前任务中哪个关系或来源应控制答案；`P` 表示在没有足够诊断证据、或
关系与表面叙事冲突时，模型倾向采用的默认答案。`M` 是可通过独立 probe、受限
中间输出或 oracle 对照观察到的关系状态；它不是“真实 hidden state”的同义词。

### 2.3 四种断裂必须分开

```text
Validity gap       = 关系或 gold label 本身没有被充分规定
Identification gap = demonstrations 没有区分候选关系/来源策略
Retention gap      = 关系被规定或识别了，但在位置、干扰或资源限制下不可访问
Commitment gap     = 关系仍可读，却没有进入最终答案
Execution gap      = 关系已显式给出，但在新内容/编码/拓扑上执行失败
```

它们对应不同的实验和不同的解释：

| 观测 | 允许的最窄解释 | 不允许直接推出 |
|---|---|---|
| 最终答案错误 | 行为层 relation failure | hidden-state mechanism failure |
| probe 能读出关系 | 某种可读表征或任务相关信号 | 模型使用了该信号作答 |
| prompt 改变答案 | decision shift | intrinsic understanding improved |
| steering 改变答案 | 在该设置下行为可被干预 | typed source controller 已被定位 |
| demonstrations 改变答案 | context responsiveness | relation 已跨内容迁移 |

### 2.4 操作性指标

主要分析不依赖单一 probe。对同一个经语义审核的 item，定义：

```text
Relation readability:
    RR = Pr(M = R | valid item, held-out readout protocol)

Final relation commitment:
    FC = Pr(Y = R | valid item, prior-conflict condition)

Prior override:
    PO = Pr(M = R, Y = P, P != R | valid item, prior-conflict condition)

Diagnostic gain:
    DG = FC(diagnostic demonstrations) - FC(consensus demonstrations)

Commitment dissociation rate:
    CD = Pr(M = R, Y != R | valid item, with independent readout agreement)
```

`RR - FC` 可以作为摘要性描述，但不是跨不同输出空间的天然可比量。主结论应优先
报告 `PO` 和 `CD`，并在同一模型、同一 item、同一答案编码和同一先验冲突条件下估计。

如果使用语言模型生成中间判断，必须把它标为 proxy，并至少加入：

- 不要求中间解释的 native final-answer arm；
- 人工或规则可计算的 relation oracle；
- 不同 readout wording 的重复；
- shuffled/wrong-channel readout control；
- 对 intermediate label 与 final label 的一致性单独报告。

## 3. 论文的中心规律候选

### 3.1 暂定规律

在关系有效性、选择分布和答案编码都被控制后：

> **最终答案对上下文关系的响应取决于两个正交条件：demonstrations 是否区分候选
> 关系，以及该关系是否能抵抗默认先验在最终承诺处的竞争。只有诊断性证据和关系
> 保持同时成立时，关系才会稳定进入答案。**

这不是宣称所有领域共享同一个神经机制，而是一个可以跨域比较的功能规律。

### 3.2 决定性 2×2

第一张主图不应是模型总准确率，而应是下面的交互：

| | 默认先验与目标关系一致 | 默认先验与目标关系冲突 |
|---|---:|---:|
| **来源一致 / 非诊断 demonstrations** | 高 final accuracy，但关系未被识别 | 先验主导，低 relation commitment |
| **来源冲突 / 诊断 demonstrations** | 高 final accuracy，可观察 diagnostic gain | **关键单元：关系是否被读出并在最终答案中兑现** |

四个格子必须使用 matched worlds、matched surface distribution 和 matched answer
frequency。否则“先验冲突”可能只是词汇或选择偏差。

预期的 Best Paper 级签名不是“最后一格一定低”，而是下面三种量同时出现：

1. 诊断 examples 能显著提高 source-policy identification；
2. 在 prior-conflict target 上，中间关系 readout 已经正确；
3. 最终答案仍系统性跟随默认先验，且这种偏移不由 primitive accuracy、长度、
   tokenization 或单一 affine gain 解释。

如果第 2 条不成立，结果是 representation/access failure；如果第 2 条成立但第 3
条不成立，则支持 relation–commitment dissociation；如果两者都不成立，不能写成
“关系被先验改写”。

### 3.3 为什么这比单纯的“冲突是监督”更强

单独的 conflict-as-supervision 只回答：

```text
模型有没有从 demonstrations 识别应跟随哪一个来源？
```

新主线还回答：

```text
即使模型已经识别了来源关系，为什么它仍可能在最终答案处回到 reality、surface
form、goal completion 或 parametric default？
```

于是可以把现有矛盾放进一个链条：

```text
多数正确但 source-consensus 的 examples
    -> identification information 很少
一个 target-consistent source conflict
    -> candidate policies 被分开
诊断关系已被读出
    -> 仍可能在 final commitment 被 default prior 覆盖
explicit policy / oracle
    -> 测量剩余的 execution gap
```

## 4. 与最近邻研究的精确区别

### 4.1 与 Imperfective Paradox 的区别

| 维度 | Imperfective Paradox | 本方案 |
|---|---|---|
| 核心对象 | 事件体义蕴含和完成先验 | 上下文诱导的 typed relation 到最终答案的保持 |
| 诊断单位 | progressive–past premise/hypothesis | demonstration set–target pair |
| 关键冲突 | process vs. culmination | queried belief vs. reality、context vs. default、process vs. result |
| 主要新变量 | telicity / verb class | diagnosticity、source topology、prior pressure、cross-content transfer |
| 中间层 | aspectual classification / event occurrence | source-policy identity / relation readout |
| 最强增量 | 发现语义判断与表征的分离 | 把分离放入 ICL 的关系识别、仲裁和最终承诺链条 |
| 语义控制 | 事件语义和 lexical classes | relation audit、selection blinding、同世界反事实和答案频率匹配 |

我们可以使用 event domain 作为外部复现，但不能把“teleological bias”换成一个新
名字再声称新颖。我们的区别必须来自 demonstration-level relation induction、
typed source competition 和 cross-domain transfer。

### 4.2 与 ICL 任务识别和示例选择的区别

AmbiBench 已经说明 demonstrations 可能不能唯一确定任务；critical-example 工作
已经研究一个关键示例如何消除规则歧义；corrupted-demonstration 工作已经研究示例
之间的规则冲突；context-faithful prompting 已经使用反事实上下文对抗参数先验。

本方案的差异不是再次发现“一个示例更重要”，而是定义：

```text
示例的价值 = 它把多少候选关系/来源策略的答案分开
而不是示例数量、表面相似度或 label correctness 本身
```

并进一步测量：关系被分开之后，最终答案是否仍被 prior override。

### 4.3 与 schema、task representation 和 Shaping Schema 的区别

schema 或 task representation 工作研究任务结构、潜在任务变量或语言表示如何帮助
组织知识；`Shaping Schema via Language Representation...` 提供了语言表示影响
schema 激活的研究视角。它们不能直接替代本方案，因为本方案要求一个被明确规定的
候选关系、互相竞争的 source policy、matched target 和最终答案承诺。

“schema”可以作为背景词，但不应作为我们的核心科学对象。核心对象是可枚举、可
分离、可在新内容上检验的 relation policy。

### 4.4 与 CoSToM、activation steering 和机制解释的区别

CoSToM 和类似工作证明，在特定任务和表示下，因果 tracing 或 steering 可以改变
行为。这为本方案提供 intervention baseline，但不能自动证明：

- 模型有一个 intrinsic ToM；
- 被改变的是 typed source policy，而不是答案接口或表面方向；
- 关系在跨内容和跨拓扑上被传输了。

本方案把 steering 放在后续机制层，主论文首先依靠 matched behavioral dissociation
和跨域规律成立。

### 4.5 与 Memory Efficiency、ImplicitMemBench 和 Local Attention 的区别

这三类工作分别提供 resource/retention、被动适应和架构表达力的对照视角。它们可
帮助解释 relation 是否被保留、是否受干扰、哪些结构可表达，但不能被拼成一个统一
的“模型意识/记忆/语义机制”。在本方案中，它们最多承担：

```text
retention boundary -> commitment boundary 的解释性对照
```

而不是主标题或主结论。

## 5. 实验总架构

### Study 0：关系有效性、选择盲化和答案编码审计

这是所有主结果的前置条件，不是附录清单。

#### 5.1 三个关系域

**Domain A：ToM source arbitration**

```text
S = {queried belief, reality, other-agent report, distractor}
R = query type selects the typed source
```

使用同一 latent world 构造 belief/reality 分离，再改变 query wording、角色顺序和
表面表达。必须确保 query 真正询问哪个 source，而不是靠名字位置或最后出现实体
猜答案。

**Domain B：event process/result**

沿用事件语义的理论关系，但只使用 lexically matched minimal pairs，并明确写出：

- process occurred；
- culmination occurred；
- culmination is not established；
- relation is genuinely uncertain。

不能把“没有被证明完成”机械地转成“完成为假”。对有争议的实例保留 `unknown`
或从 primary analysis 排除。

**Domain C：context versus parametric default**

构造一个局部上下文与模型常识/参数默认答案相冲突的 target。context policy 和
default policy 在 consensus items 上给相同答案，在 diagnostic items 上给不同答案。

#### 5.2 必做控制

- native-speaker 或领域专家对关键关系的盲标注；
- lexical matched minimal pairs；
- relation-preserving paraphrase；
- answer-token frequency matching；
- target selection distribution 对 policy 隐藏；
- relation-swapped、source-swapped 和 label-shuffled controls；
- explicit uncertainty/unknown label；
- 不使用 LLM judge 作为 primary gold；
- 训练、发现、holdout 和独立确认使用不重叠 world/predicate/template。

#### 5.3 selection-blinding 条件

source-consensus examples “零关系信息”只在以下条件下成立：

```text
q(x, T, surface | pi_a) = q(x, T, surface | pi_b)
```

至少在可观察的 lexical、length、position、answer-frequency 和 target-selection 统计
上近似成立。否则模型可能从例子的分布而非 label 学到 policy。

### Study 1：一张 2×2 主图决定主命题

第一阶段只在 ToM 上做窄而干净的实验，避免一开始横跨三域导致每个域都不深。

#### 5.4 因子

```text
Factor A: demonstration diagnosticity
    consensus: competing source policies agree on all demonstrations
    diagnostic: at least one matched demonstration separates them

Factor B: target prior pressure
    aligned: default prior predicts the intended relation answer
    opposed: default prior predicts the rival-source answer
```

每个 latent world 生成四个 matched condition。source states、query semantics、答案
空间和输出格式保持不变，只改变 demonstration diagnosticity 与 prior pressure。

#### 5.5 ToM 例子结构

```text
World: Alice places a marble in box A; Bob sees it moved to box B.
Query: Where does Alice believe the marble is?
Candidate policy 1: queried belief
Candidate policy 2: reality
```

consensus demonstrations 让 Alice belief 与 reality 一致；diagnostic demonstration
让二者分离，并用 target-consistent label 指示应跟随 belief。prior pressure 通过
匹配的叙事显著性、query order、现实答案的表面可及性和答案频率构造，但不能改变
gold relation。每一个 pressure manipulation 都要有 neutral matched control。

#### 5.6 三种输出臂

1. **Native final arm**：只要求最终答案，测量真实的 final commitment。
2. **Independent relation-readout arm**：在另一个复制集上询问 source policy 或
   relation，不把中间输出插入 native answer 的上下文。
3. **Oracle-guided arm**：提供 gold relation，再测量剩余 execution/commitment gap。

不能用同一次 chain-of-thought 同时证明“模型读出了关系”和“模型使用了关系”。

#### 5.7 关键结果判别

| 结果 | 最窄结论 |
|---|---|
| diagnosticity 无效，readout 也错 | relation identification/access failure |
| readout 对，native final 错且跟随 prior | relation–commitment dissociation 候选 |
| oracle 后仍错 | execution/answer-interface failure |
| prompt 后 target 变好但邻近合法项变差 | decision shift / calibration trade-off |
| 只在一个表面模板成立 | surface-form attraction 或模板特异性 |

### Study 2：Conflict Is Supervision 的严格识别实验

这是新主线的上游部分，沿用既有方案但把它放进 2×2 框架。

#### 5.8 demonstrations 条件

```text
C1/C4/C8/C16       只有 source-consensus 的正确示例
D1 + C7            一个 target-consistent diagnostic example 加七个 consensus
D1 + X1 + C6       一个 target-consistent 与一个 rival-policy diagnostic pair 加六个 consensus
Explicit policy    直接给出 source rule 的上界条件
```

`D1 + X1 + C6` 必须明确区分：两个 diagnostic examples 支持不同 policy 时，测量的是
inter-demonstration policy conflict；它不能再被误称为“一个冲突示例提供更多信息”。

#### 5.9 版本集合

令：

```text
V(D) = {pi : h_pi(d) = y_d for every demonstration d}
```

若 `V(D)` 仍包含多个 policy，则标签正确不等于任务已识别。主要 estimand 是：

```text
policy identification = Pr(V(D) = {pi*})
diagnostic gain        = target performance(D1) - target performance(Cn)
policy-conflict cost   = performance(D1 + X1 + C6) - performance(D1 + C7)
```

这些量在同一候选 policy class、同一 item-selection 分布和同一 target split 上计算。

### Study 3：关系到最终承诺的 prior boundary

在 Study 1 的 ToM 结果稳定后，把 prior pressure 从一个 ToM 变量扩展为一组明确的
默认策略：

| 域 | intended relation | rival/default prior |
|---|---|---|
| ToM | queried belief controls answer | reality-copying / narrator salience |
| context | local context controls answer | parametric/default knowledge |
| event | process does not imply culmination | goal-completion / sufficiency prior |
| reportative | reported commitment controls answer | narrator/world fact |

每个域都必须有：

- consensus baseline；
- diagnostic relation reversal；
- prior-aligned target；
- prior-opposed target；
- explicit relation upper bound；
- relation-preserving paraphrase；
- independent holdout predicates/worlds。

主问题不是“三个域的准确率谁更高”，而是四个条件的交互是否有相同的功能形状。
即使具体 prior 不相同，也可以得到一个更保守但仍有价值的结论：**不同领域存在
不同的 default policy，但它们在 relation-to-commitment 断裂处具有可比较的结构。**

### Study 4：Prompt repair 是否真的修复关系

复制事件语义论文最有价值的设计，但把它转译到 LP4FM：

```text
strict-definition prompt
counterfactual prompt
explicit-source-rule prompt
diagnostic-demonstration prompt
representation/activation intervention（可选）
```

每个干预必须同时报告：

```text
target relation gain
neighboring valid relation loss
prior override reduction
calibration / abstention change
cross-content transfer
```

“目标错误减少”只有在邻近合法关系没有被一起压平时，才可称为 relation improvement。
如果 prompt 让模型从“总跟现实”变成“总跟上下文”，那是 prior replacement 或
decision shift，不是 source arbitration 已被正确学习。

### Study 5：cross-content、cross-encoding 和 unseen topology

这是 ACG 的执行半部，也是区别于事件语义论文的关键。

训练/示例阶段只覆盖：

- 一部分人物、物体和关系词；
- 一部分 answer encoding；
- 一部分二元 source topology。

测试阶段加入：

- 新人物、新物体、新句法表达；
- yes/no、choice、short span 等不同 answer encoding；
- 三元 source topology；
- 未见 source conflict graph；
- relation reversal 与 paired counterfactual。

如果 relation identification 很好但 unseen topology 失败，应写成 execution gap；如果
cross-content transfer 失败，不能把同 item reconstruction 叫作 relation generalization。

### Study 6：checkpoint、资源和机制桥

机制实验只能在主行为规律成立后开始。

**Checkpoint 轨迹**：比较 Base → SFT → alignment 或可获得的中间 checkpoint，测量：

- default prior 的方向和强度；
- diagnostic responsiveness；
- relation readability；
- prior override；
- cross-content execution。

这继承 LP4FM 的 checkpoint-sensitive boundary，但不把 checkpoint 差异直接解释成
训练因果。只有 controlled SFT 操纵 reportative、factive、voice、source-attribution
暴露，并排除 primitive accuracy、calibration、logit scale 和 affine gain 后，才可
讨论 training-induced change。

**资源/注意力 arm**：用 Local Attention 和 Memory Efficiency 的思想测试 relation
retention，但只回答“关系是否可访问”，不回答“模型为何选择某来源”。

**causal arm**：用 CoSToM 风格 tracing/steering 作为候选干预，必须加入 final-token
only、wrong-channel、residualized multi-site、transportability 和 specificity
control。成功改变答案仍不自动等于发现了 typed controller。

## 6. 预注册预测与结果分支

### P1：正确示例数量不等于关系监督量

在 source-consensus 条件下，增加 C1 → C4 → C8 → C16 的正确示例数量，若 selection
blinding 成立，relation identification 和 prior-opposed target 的改善应显著弱于
加入一个 matched diagnostic example。

若 C16 显著改善，可能说明表面分布、覆盖或模型使用了不同于候选 policy class 的
规则；这不是直接否定，而是要求重查 information audit。

### P2：一个诊断示例产生非线性跃迁

D1 相对 Cn 应在 target-consistent source policy 上产生显著 gain，并迁移到未见内容。
只在同 item 或同词汇上变化则属于 memorization/format effect。

### P3：识别后仍存在 prior override

在 diagnostic + prior-opposed 条件下，独立 relation readout 正确，但 native final
answer 仍跟随 P；这才是 relation–commitment dissociation 的关键证据。

### P4：两个冲突方向相反

within-example source conflict 若 label 指示目标 policy，可以是 supervision；
across-example policy conflict 若支持相反规则，应产生 conflict cost。这一区分应在
同一 source graph 和同一答案空间内成立。

### P5：prompt 修复形成邻近关系的 trade-off

如果 generic strict prompt 降低 prior override，却同时增加另一类合法关系的 false
rejection，说明修复改变了决策阈值或 policy prior，而非完成结构性关系学习。

### P6：跨域共享功能形状，而非共享具体偏差

ToM、context 和 event 可以有不同的 default prior；Best Paper 级结果要求它们都能
在同一 2×2 逻辑中被比较，而不要求它们有相同的内部向量、attention head 或错误
方向。

### P7：oracle relation 只消除部分错误

如果显式提供 relation 后仍有 prior-opposed failure，说明关系执行/答案接口仍是
独立瓶颈；这使论文不止停留在“模型没看懂 instruction”。

## 7. 统计和证据协议

### 7.1 独立单位

统计独立单位优先是 latent world、predicate、source graph 或模板簇，而不是同一世界
下的每个 prompt row。每个 world 的 paired conditions 必须用 cluster bootstrap 或
混合效应模型处理。

### 7.2 主要模型

```text
answer ~ diagnosticity * prior_pressure * domain
       + encoding + length + lexical_controls
       + (1 | world) + (1 | template) + (1 | model/checkpoint)
```

`RR`、`FC`、`PO` 和 `CD` 分别建模，不把 probe score 与 final accuracy 合并为一个
总分。报告 effect size、置信区间、item-level paired difference 和多重比较策略。

### 7.3 必须保留的负结果

- semantic audit 不通过的关系域；
- diagnostic example 不产生 transfer 的域；
- relation readout 与 final answer 同时失败的条件；
- prompt 只产生 label shift 的条件；
- final-token-only 足以复现 steering 的条件；
- checkpoint 之间 law direction 改变的条件。

这些结果不是需要隐藏的“失败”，而是决定论文到底写 relation induction、commitment
boundary 还是 domain-specific effect。

## 8. Best Paper 级证据门槛

### Gate A：关系真的被规定了

至少一个外部语义域和 ToM 域通过 human/linguistic validity audit；主结果不依赖有
实质多重解释的样本。

### Gate B：不是普通 ambiguity 或 aha-example 重做

证明 source-consensus 与 source-diagnostic 的 label information 在 matched selection
下不同，并测量 `V(D)` 而不是只看 target accuracy。

### Gate C：关系识别与最终承诺分离

在 prior-opposed 条件中，同时观察到独立 relation readout 与 native final answer 的
系统性分离；至少两个 readout protocol 和一个 oracle/control 支持该分离。

### Gate D：关系转移超出原 item

diagnostic gain 迁移到新 world、新 lexical realization、新 answer encoding 或 unseen
topology；否则只能写成局部 source arbitration。

### Gate E：修复不是简单 label replacement

干预降低目标 prior override，同时保留邻近合法关系，或明确展示不可避免的 calibration
frontier。只报告一个准确率提升不够。

### Gate F：跨域结果改变领域直觉

至少两个语义域呈现同一关系—承诺交互结构，且每个域的 validity、source policy 和
default prior 都有独立定义。跨域相似不能由同一模板或同一 judge 产生。

### Gate G：机制只在被许可时上升

若 causal/steering 不能通过 specificity、wrong-channel、transportability 和
final-token-only controls，主论文仍可成立，但只能保留行为层 commitment boundary，
不能写成内部 typed controller。

## 9. 与 LP4FM 和五个分支的重新分工

| 研究资产 | 在新方案中的位置 | 不应承担的结论 |
|---|---|---|
| 已投稿 LP4FM | Paper A：描述如何言语形式改变 typed source arbitration，并建立 checkpoint-sensitive boundary | 不证明 ICL relation induction 或跨域 commitment law |
| Conflict Is Supervision | identification half：诊断性冲突如何区分 source policy | 不单独解释识别后为何仍被 prior 覆盖 |
| ACG | execution half：新内容、编码和拓扑上的 relation transfer | 不把同 item reconstruction 当作泛化 |
| Schema rewriting | 解释语言表示如何改变可访问任务结构的背景路线 | 不把 schema 作为不可识别的大一统机制 |
| Mechanism identifiability | probe、control、transport 和 causal claim 的边界 | 不从可读性直接推出可控性 |
| Conflict-topology kernel | source graph 的结构化泛化 | 需先证明 topology 不是模板/长度代理 |
| Source-accessibility training objective | 后续 intervention/training paper | 目前不是主论文贡献 |

最终的项目层结构应是：

```text
LP4FM: language can alter which source controls an answer
Paper B: diagnostic conflict identifies the governing relation
Best-Paper upgrade: identifying a relation is not the same as committing to it
```

## 10. 论文主图和主表

### Figure 1：同一关系的四个功能状态

横轴为 demonstration diagnosticity，纵轴为 prior pressure；每个 cell 同时显示：

- native final commitment；
- independent relation readability；
- prior override rate。

图标题应直接问：**When does a relation survive to the final answer?** 不使用内部
“claim/gate/status”字段。

### Figure 2：正确示例数量与诊断覆盖

展示 C1/C4/C8/C16 与 D1 的 matched comparison，以及 `V(D)` 的候选 policy 数量。
核心视觉不是“更多 examples 更好”，而是“更多非诊断 examples 可能不减少 ambiguity”。

### Figure 3：跨域 relation–commitment profile

ToM、context 和 event 各自显示 consensus、diagnostic、prior-aligned、prior-opposed
四个条件；用 domain-specific semantic labels，不把不同关系压成一个“reasoning score”。

### Figure 4：prompt repair 的 trade-off frontier

横轴为目标 prior override reduction，纵轴为邻近合法关系的 false rejection；显示
strict definition、counterfactual、explicit rule 和 diagnostic demonstration。

### Table 1：候选解释比较

| 解释 | 能解释 diagnostic gain | 能解释 relation readout 正确但 final 错 | 能解释 cross-content 失败 | 能解释 prompt trade-off |
|---|---:|---:|---:|---:|
| task ambiguity | 是 | 否 | 部分 | 否 |
| surface-form attraction | 否/部分 | 部分 | 否 | 是 |
| retention/access failure | 部分 | 否 | 是 | 部分 |
| source-policy identification failure | 是 | 否 | 是 | 部分 |
| relation–commitment dissociation | 是 | 是 | 是/边界化 | 是 |
| hidden causal controller failure | 不能仅凭行为确定 | 不能仅凭 probe 确定 | 部分 | 部分 |

## 11. 最小高信息量 pilot

在投入跨模型和机制资源前，只做一个能决定主线的窄 pilot：

### 11.1 数据

- 96 个独立 ToM latent worlds；
- 每个 world 四个 2×2 条件；
- 每个条件至少两个 lexical realization；
- 训练/发现、holdout 和 confirmation 使用不重叠人物、物体和模板；
- 另建小规模 event/context validity-screened replication，不与 ToM 共享模板。

### 11.2 输出

- native final answer；
- 独立 relation readout；
- explicit-rule oracle；
- answer-token log probability 或等价连续分数；
- ambiguity/validity status；
- 每个条件的 prior-aligned 与 prior-opposed paired difference。

### 11.3 Go / no-go

进入完整论文的最低条件：

1. ToM diagnosticity 在 holdout target 上产生稳定 policy gain；
2. prior-opposed 条件存在 relation readout 与 final answer 的可重复分离；
3. 分离不由 lexical、position、answer frequency 或同 item leakage 解释；
4. 至少一个 prompt repair 显示“目标改善—邻近损失”的可测量 trade-off，或显示
   一个真正选择性的修复；
5. event 或 context 至少有一个独立域复现 2×2 功能形状。

如果第 2 条失败，回到窄 ACG/Conflict Is Supervision；如果第 1 条失败，不应再把
论文包装成 relation induction；如果第 5 条失败，保留为 ToM-specific commitment
boundary，不声称跨域规律。

## 12. 实施顺序与产物

### Phase 1：冻结关系和语义审计

产物：

```text
relation_commitment_manifest.jsonl
relation_validity_annotation.csv
policy_class_spec.yaml
selection_blinding_report.md
answer_encoding_audit.md
```

### Phase 2：ToM 2×2 runner

每行至少记录：

```text
domain, world_id, relation_id, source_states, candidate_policies,
demonstration_condition, diagnosticity, prior_pressure, lexical_template,
answer_encoding, model, checkpoint, seed, split, gold_relation, gold_answer,
validity_status, native_answer, relation_readout, oracle_condition
```

### Phase 3：关系识别与最终承诺报告

产物：

```text
relation_readability_report.json
final_commitment_report.json
prior_override_report.json
diagnostic_gain_report.json
paired_2x2_main_figure.pdf
```

### Phase 4：独立域和跨结构泛化

只在 Study 1 的主命题成立后增加 event/context、new encoding 和 unseen topology；不
以扩大模型数量替代独立语义域。

### Phase 5：机制和训练

只有当行为层 `CD` 和 `PO` 在独立确认集稳定后，才启动 tracing、steering、checkpoint
trajectory 或 controlled SFT。所有机制结果单独标记为 proxy、causal candidate 或
negative boundary。

## 13. 论文叙事骨架

### 开场反例

给出同一个 latent world：

```text
模型能正确回答“关系是什么”，
但在最终答案中仍选择关系所反对的默认来源。
```

再给出一组 source-consensus demonstrations：它们全部正确，却没有告诉模型该跟随
belief 还是 reality。

### 主论证

1. 语言关系必须先被语义充分规定；
2. 正确 demonstrations 只有在区分候选 policy 时才提供 relation supervision；
3. diagnostic evidence 能识别关系，但 relation readability 不保证 final commitment；
4. prior pressure 在 commitment boundary 处重写部分关系；
5. prompt repair 可能重新分配错误，而不是恢复一个统一的“理解能力”；
6. 该结构在至少一个外部关系域中复现，或明确收紧为 ToM-specific boundary。

### 不采用的叙事

- “LLM 根本不理解语义”；
- “一个 probe 找到了 belief circuit”；
- “所有错误都来自 teleological bias”；
- “更多 examples 一定没有用”；
- “跨域相似就证明统一神经机制”；
- “prompt 提升准确率就证明结构性修复”。

## 14. 摘要骨架

> Language models often succeed on relational tasks when the default answer agrees with
> the relation required by the context. This makes it difficult to determine whether a
> model has identified the relation or merely followed a familiar prior. We introduce a
> matched diagnostic protocol that separates source-consensus demonstrations from
> demonstrations that distinguish competing source policies, and separates independent
> relation readout from the native final answer. Across typed belief-source, contextual-
> versus-parametric, and event process-result settings, we test when a relation survives
> from context to final commitment. The central prediction is that diagnostic evidence
> improves policy identification, but relation readability and final commitment can
> dissociate under prior-conflicting targets. Prompt interventions may reduce one class of
> prior-driven errors while increasing rejection of neighboring valid relations. We use
> semantic validity audits, selection-blinded matched pairs, unseen-content transfer, and
> oracle controls to distinguish task ambiguity, surface attraction, retention failure,
> policy identification, and final commitment failure. The resulting framework treats
> demonstrations as relational supervision and final answers as a separate commitment
> boundary, without claiming a single hidden mechanism across domains.

## 15. 最终定位

当前最值得推进的不是“再加一个 benchmark”，而是把 LP4FM 已经发现的 source
arbitration 与新的 ICL 识别问题连接起来：

> **上下文并不只是告诉模型答案；它首先决定哪一种关系有资格控制答案。即使关系
> 被识别，最终答案仍可能被默认先验改写。**

如果 2×2、独立 readout、oracle、跨内容和独立语义域都成立，这会形成一个真正有
期望改变作用的研究更新：**模型的“会不会推理”不是一个标量，而是关系从上下文
到最终承诺是否保持的问题。**

如果这些证据不成立，方案也提供了清晰的收缩路径：保留有效的 diagnostic
demonstration/ACG 结果，删除跨域或内部机制叙事，不用漂亮的新术语掩盖证据边界。

## 16. 参考入口

### 直接参考

- [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/)
- [The Imperfective Paradox Is Not Necessarily in Large Language Models](https://arxiv.org/abs/2608.25005)

### ICL 与关系识别

- [AmbiBench](https://arxiv.org/abs/2212.10711)
- [Critical Example Attribution](https://aclanthology.org/2025.findings-acl.1092/)
- [Demonstration Conflict](https://arxiv.org/abs/2603.04464)
- [Context-Faithful Prompting](https://aclanthology.org/2023.findings-emnlp.968/)
- [Active Example Selection](https://aclanthology.org/2022.emnlp-main.622/)
- [Teaching Dimension](https://proceedings.mlr.press/v48/liua16.html)

### ToM、schema 与机制

- [CoSToM](https://aclanthology.org/2026.acl-long.421/)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
- [Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/)
- [Schema Learning and Rebinding](https://arxiv.org/abs/2307.01201)
- [Disentangling Memory and Reasoning](https://aclanthology.org/2025.acl-long.84/)

### 资源与架构边界

- [Characterizing the Expressivity of Local Attention](https://aclanthology.org/2026.acl-long.1739/)
- [Memory Efficiency and Resource-Rational Encoding](https://aclanthology.org/2026.acl-long.1550/)
- [ImplicitMemBench](https://aclanthology.org/2026.acl-long.1301/)
