可以。把这五份材料和我们前面已经形成的 **LP4FM → Relation Information → Relation Control** 主线全部放在一起以后，我认为现在已经不是“想几篇论文”的阶段了，而是可以正式建立一个 **4–5 年、约 8–10 篇核心论文的研究纲领**。

最重要的变化是：以后不要再按“ToM、形式语言、多语言、表示几何”来划项目。它们真正研究的是同一条计算链上的不同环节。

# From Representation to Control

## A Long-Term Research Program on Language, Structure, Reasoning, and Behavioral Control

## 0. 总纲：未来几年只研究一个母问题

整个研究计划可以统一为：

> **任务相关的结构如何被语言与表示构造出来，被模型从表面形式中恢复出来，被上下文证据识别出来，并最终取得对行为的控制权？长期语言经验又如何改变这一整条过程？**

形式上：

[
\boxed{
Experience
\rightarrow
Representation
\rightarrow
Construction
\rightarrow
Recovery
\rightarrow
Identification
\rightarrow
Control
\rightarrow
Action
}
]

其中：

[
E=\text{linguistic experience}
]

[
\rho=\text{representation}
]

[
Z=\text{task-relevant semantic/computational structure}
]

[
\hat Z=\text{internally recovered structure}
]

[
R=\text{governing relation/rule}
]

[
\hat R=\text{identified relation}
]

[
C_R=\text{control exerted by that relation}
]

[
Y=\text{answer/action}
]

这条链把目前所有材料真正统一起来了。

语言相对论材料本身已经从传统的“语言结构→认知差异”推进到“语言经验→表征几何→检索/注意→推理→行为”的机制化问题。 最后真正值得追的问题也已经不是简单比较中英文输出，而是控制语义和文化之后进行 representation intervention，观察 reasoning 是否因果变化。

---

# 1. 一个非常重要的新理论：把 reasoning failure 拆成五类

未来所有论文最好共享下面这套 taxonomy。

### I. Construction Failure

模型需要的东西是否真的被表示结构**构造出来**？

例如：

约束是 type-level invariant，还是一句需要模型记住并调用的命题？

一个实体是否作为 witness 存在？

概念的 introduction / elimination rules 是否足够确定它的使用？

这正是现有《Constructed, Not Entailed》方案的统一核心：约束、见证和规则都存在“被构造”与“仅仅被蕴涵”的区别。

### II. Recovery Failure

即使正确结构已经存在，模型能否从 token serialization 中把它重新恢复出来？

[
G
\xrightarrow{\rho}
S
\xrightarrow{\text{LLM}}
\hat G
]

如果：

[
\hat G\not\approx G
]

那么问题首先不是 reasoning，而是 **structure reconstruction failure**。

ReGeo 材料已经把这一点收紧得很好：好的 representation 不一定增加知识，而可能只是减少模型从 surface serialization 重建 task topology 所需的内部计算。

### III. Identification Failure

结构恢复以后，模型是否知道：

> 当前究竟应该遵循哪个 relation / source / rule？

这是：

LP4FM
→ Conflict Is Supervision
→ Relation Information

所解决的问题。

核心变量：

[
I_R(D)
======

H(R)-H(R|D)
]

即 demonstrations 到底提供了多少用于区分 competing relations 的信息。

### IV. Control Failure

即使：

[
\hat R=R
]

也不代表：

[
Y=Y_R
]

也就是说：

> “知道应该遵循什么”与“真正让它控制行为”仍可能是两个计算阶段。

核心量：

[
RCE=
P(\text{appropriate flip}|rule\ swap)
-------------------------------------

P(\text{flip}|sham)
]

这是 Relation Control。

### V. Execution Failure

只有前四层都正确以后，剩下的才是传统意义上的：

搜索失败；

算术失败；

planning failure；

长链推导失败。

这套五层 decomposition 有机会成为整个研究纲领最重要的方法论贡献之一：

[
\boxed{
Reasoning\ failure
\neq
one\ thing
}
]

---

# 2. 整个计划切成四个核心 Program

| Program                           | 研究对象                                                 | 核心问题                                 | 时间尺度  |
| --------------------------------- | ---------------------------------------------------- | ------------------------------------ | ----- |
| **A. Relational Control**         | evidence / relation / prior / action                 | 什么证据确定规则？规则何时控制行为？                   | 现在立即做 |
| **B. Constructed Representation** | types / witnesses / rules / proofs                   | 哪些东西必须被结构性构造，而不能只是被蕴涵？               | 1–3 年 |
| **C. Structure Recovery**         | latent structure / serialization / internal geometry | 同样的意义为什么产生不同 computation？            | 1–2 年 |
| **D. Linguistic Experience**      | language / culture / training history                | 长期语言经验如何改变 identification 与 control？ | 2–5 年 |

Mechanistic interpretability 和 human experiments 不再作为并列 Program。

它们属于**验证层**：

> 行为规律先成立 → 才升级到机制；
>
> 模型因果结果先稳定 → 才值得做人类实验。

---

# 3. Program A：Relational Control

## 当前最高优先级，也是近期最值得冲 Best Paper 的主线

### Paper A0 — Source Arbitration

这是已有 LP4FM foundation。

核心问题：

> belief、reality、other-agent、report、distractor 发生冲突时，最终是谁控制答案？

这一篇的历史作用已经非常明确：

它把：

[
accuracy
]

升级成：

[
source\text{-}indexed\ answer\ control
]

不要继续无限扩这一篇。

---

## Paper A1 — From Evidence to Control

这是接下来真正应该全力完成的 flagship。

核心链：

[
\boxed{
Relation\ Information
\rightarrow
Identification
\rightarrow
Relational\ Control
\rightarrow
Action
}
]

### 主实验 1：Information, not example count

目标不是简单证明：

[
1\ diagnostic >16\ consensus
]

因为 critical-example literature 已经很接近。

真正要证明：

[
I_R(D)
]

比：

example count；

semantic similarity；

length；

position；

diversity

更稳定预测 held-out rule learning。

如果成立：

> demonstration value 不由“正确”和“数量”决定，而由它排除多少 competing relations 决定。

---

### 主实验 2：Identification × Prior Pressure

建立两个 surface：

[
P(\hat R=R)
]

以及：

[
P(Y=Y_R|\hat R=R)
]

真正 killer 区域是：

[
P(\hat R=R)\approx1
]

同时：

[
P(Y=Y_R|\hat R=R)\ll1
]

这时候才能证明第二阶段。

而且正文必须主动测试：

global scalar bias；

temperature；

confidence；

response interface；

threshold calibration。

如果简单 scalar correction 就恢复行为：

不要叫 relation-specific control failure。

---

### 主实验 3：Functional Relation Control

partner adaptation 不要包装成“新的 ToM benchmark”。

定位成：

> **functional assay of relational control**

当 partner policy：

[
belief\rightarrow reality
]

模型自己的最优 action 是否相应翻转？

这样三个实验共享一个 estimand：

[
RCE
]

这会让整篇论文非常完整。

---

### Paper A1 Kill Gate

如果：

[
I_R\rightarrow Identification
]

很稳定，

但是：

[
Identification\not\Rightarrow Control
]

不稳定，

就收缩成：

**Conflict Is Supervision / Relation Information in ICL**

不要硬造 commitment theory。

如果两者都成立，再做完整 flagship。

---

# 4. Program B：Constructed Representation

## 类型论、动态语义、证明论应该形成一个紧密集群

现有 integrated plan 已经有非常好的内部依赖设计：

先验证 (\kappa)；

同时做 accessibility；

再做 constraint migration。

这是正确顺序。现有方案已经明确指出，L1 对 (\kappa) 的使用需要 L3 先验证，而 L2 又迫使等价性从经典等价升级为 proof-relevant equivalence。

---

## Paper B1 — Where Chains of Thought Choose

对应现在的 L3。

科学问题：

> LLM reasoning 的真正困难，是 proof 长，还是必须作出的非确定性选择多？

核心量：

[
\kappa
]

不要一开始就假定哪个定义最好。

比较：

[
\kappa_0
]

focusing phase count；

[
\kappa_1
]

effective choice count；

[
\kappa_2
]

search-tree complexity。

与：

[
n_{\text{step}}
]

竞争。

### Killer condition

proof steps；

rule count；

token length

全部匹配，只改变：

[
\kappa
]

如果：

[
Err\uparrow\quad as\quad\kappa\uparrow
]

并且 focusing scaffold 在高 (\kappa) 上选择性改善性能，

那么得到的不只是 CoT technique。

而是一条：

> **choice complexity law**

---

## Paper B2 — Rules or Recall?

和 B1 可以根据结果选择合并或拆开。

这里研究：

Harmony；

Inversion；

Conservativity；

Tonk；

prior intrusion。

不要再问：

> LLM 是 proof-theoretic 还是 model-theoretic？

现在的方案已经正确把它操作化成三种可观测模式：

规则推理；

心智模型；

先验检索。

证明论真正贡献的是一组**可先验计算的结构变量**。

和谐性、反演、规范化和 focusing 都可以在不运行模型之前生成预测。

这才是 novelty。

---

## Paper B3 — Classically Equivalent, Dynamically Distinct

对应 L2。

这可能是整个计划里语言学辨识度最高的一篇。

核心问题：

> LLM 使用一个实体时，依赖的是“世界中可推出它存在”，还是“语篇中真的构造出了可访问的 witness”？

核心变量：

[
Acc(r,c)
]

与：

[
\lambda(r,c)
]

最漂亮的 contrast：

[
\exists x.P(x)
]

vs.

[
\neg\neg\exists x.P(x)
]

经典逻辑上等价。

但在构造类型论下，后者不产生具体 witness。

这使得“经典等价”和“证明相关等价”成为实验变量，而不是哲学争论。

现有 proposal 还非常正确地将 outcome 扩展成双向：

under-use；

leakage；

global accommodation。

同时把“上下文窗口就是 DRS”改成可证伪 mechanism hypothesis，而不是前提。

---

## Paper B4 — Schemas Are Types

只有 B1/B3 的工具和度量比较稳定以后，再全面启动。

核心 thesis：

> **表示的优势不是因为“更形式化”，而因为 task invariants 被从动态推理搬迁到了静态 construction。**

核心变量：

[
\sigma
]

以及：

[
\kappa^*
]

最重要设计仍然是：

[
I\rightarrow P\rightarrow T
]

Implicit；

Propositional；

Type-level。

还要保留最锋利的 placebo：

**S2：表面像 Lean/Haskell，但 (\sigma,\kappa) 不改变。**

如果：

[
S2\approx S1
]

同时真正 type migration：

[
S3,S4>S1
]

那么可以直接否证：

> symbolic-looking syntax itself helps reasoning.

原方案的核心非常清楚：表示收益不是信息增益，而是把推理负担搬迁到 well-formedness / type checking。

---

## Paper B5 — Constructed, Not Entailed

这篇不要现在写。

它应该成为 B1–B4 得到稳定结果以后的综合理论论文。

最终主张：

> **LLMs use information more reliably when the relevant structure is constructed by the representation than when the same consequence is merely entailed by it.**

统一：

constraints；

witnesses；

rules；

proof search。

共享：

[
A^+/A/B/C
]

equivalence hierarchy，

以及：

[
\sigma,\kappa,Acc,\lambda,h,Cons
]

等可预先计算的量。

这篇更适合作为 TACL / JMLR / journal-length work，而不是为了赶会议强行整合。

---

# 5. Program C：Structure Recovery

## 这一条和类型论主线必须区分开

这里有一个特别重要的理论切割。

Constructed Representation 问：

> **需要的东西是否真的存在于 representation 的结构中？**

Structure Recovery 问：

> **当结构已经存在时，surface serialization 让它有多难恢复？**

两者分别是：

[
Construction\ Gap
]

和：

[
Recovery\ Gap
]

不能混成同一个 paper。

---

## Paper C1 — Same Semantics, Different Computations

核心 thesis 应冻结为：

[
\boxed{
Representation\ Distortion
\rightarrow
Structure\ Recovery
\rightarrow
Reasoning
}
]

而不是：

[
representation\rightarrow geometry
]

现有方案已经完成这一升级。

### Primary intervention

不是：

NL vs JSON vs DSL。

而是完全 token-matched：

aligned；

random；

anti-aligned。

保持：

token multiset 相同；

长度相同；

semantics 相同；

lexicon 相同；

唯一变化：

[
\pi_r
]

即 computational dependencies 在线性序列中的排列。

### 三个主任务

Program-DAG；

Proof-DAG；

Constraint-Graph。

它们分别代表：

execution structure；

deductive structure；

global constraint structure。

### Mechanism

Geometry Recovery Curve；

Structure Decodability；

Format Decodability；

cross-format probe；

structure-specific subspace intervention。

证据必须达到四层：

[
Behavioral
]

[
Mechanistic
]

[
Predictive
]

[
Causal
]

现有材料也明确要求四层同时成立才有资格声称 representation → recovery → reasoning。

其中：

t-SNE；

UMAP；

单一 linear probe；

attention entropy

全部只能做 supporting analysis。真正核心是 controlled manipulation + cross-format recovery + out-of-sample causal intervention。

---

# 6. Program D：Computational Linguistic Relativity

## 直到前面的计算理论建立以后，再真正回到 Boroditsky

这部分不要变成“multilingual benchmark factory”。

---

## Paper D1 — Linguistic Relativity of Relational Control

最重要的 factorial design：

[
Language_{evidence}
\times
Language_{query}
\times
Diagnosticity
\times
Rule
]

问：

### Language 是否改变 identification？

[
P(\hat R=R|L)
]

### Language 是否改变 control？

[
P(Y=Y_R|\hat R=R,L)
]

这会区分三个理论。

**Theory 1：Language alters priors**

强 diagnostic evidence 后差异消失。

**Theory 2：Language alters control**

relation identification 相同，但最终行为不同。

**Theory 3：Language alters induction**

即使强 evidence 下，rule identification 本身仍不同。

这比：

> English ToM = 73%，Chinese = 68%

强很多。

---

## Paper D2 — Controlled Linguistic Experience

D1 仍然无法解决自然语言的大混杂：

Language；

Culture；

Corpus；

Tokenizer；

Training distribution。

因此下一篇必须真正：

[
do(LinguisticExperience)
]

最自然的 feature 是：

# Obligatory Evidentiality / Source Marking

Treatment corpus：

必须编码 direct perception / report / inference / belief source。

Control corpus：

完全相同事实、长度、frequency、token budget；

marker 改为与 source 无关的任意语法类别。

同一 base model：

[
M_0
]

复制：

[
M_T,\ M_C
]

matched continued training。

然后**绝对不要只测试训练格式**。

测试：

ToM conflict；

report/reality arbitration；

new symbols；

JSON interface；

partner adaptation；

rule swap。

如果 treatment 导致：

[
sample\ complexity\downarrow
]

[
RCE\uparrow
]

并且 cross-interface transfer，

才有资格声称：

> controlled linguistic experience causally reshapes relational reasoning.

这会真正把计算工作接回现代语言相对论。

---

# 7. Mechanistic Paper：条件性启动

未来不要看到漂亮 probe 就另开一篇。

只有至少两个 behavioral law 跨 family 稳定以后，才启动：

## Paper M — Mechanisms of Constructed Control

内部状态至少区分：

[
z_{\text{structure}}
]

[
z_{\text{relation}}
]

[
z_{\text{control}}
]

[
z_{\text{language prior}}
]

真正的要求是：

**causal specificity**

而不是 decodability。

必须至少争取：

bidirectional intervention；

content transfer；

relation specificity；

cross-language transfer；

最好再有一定 checkpoint/family transport。

否则机制结果作为对应 behavior paper 的 section/appendix，而不是独立论文。

---

# 8. Human Paper：最后才做

真正回到 Sapir–Whorf / Boroditsky，最干净的不是：

Chinese native speakers vs English native speakers。

那样又掉回 culture confound。

应该做：

## Paper H — Artificial Linguistic Experience in Humans and LLMs

随机分配参与者：

source-obligatory miniature language；

matched-control miniature language。

训练后不测语言记忆，而测试：

source conflict；

memory；

relational identification；

partner coordination。

然后比较：

[
Human:
Experience\rightarrow Control
]

与：

[
LLM:
Experience\rightarrow Control
]

问题是：

> 相同的 linguistic-experience intervention 是否在两个学习系统中产生结构同型的 relational effect？

不是：

> LLM 是否模拟人脑。

---

# 9. 最终 Capstone：Machine-Designed Reasoning Languages

这是整个计划最后真正闭环的一步。

前面最终会得到一组可计算量：

[
D_{\text{rep}}
]

representation distortion；

[
\sigma
]

constraint staticization；

[
\kappa
]

proof-search choice complexity；

[
Acc,\lambda
]

witness accessibility；

[
h,Cons
]

harmony / conservativity；

[
I_R
]

relation information；

[
RCE
]

relational control。

于是终于能回答 *Shaping Schema via Language Representation* 最有野心、但当前证据还没真正完成的问题：

> **能否自动设计适合模型思考的语言？**

形式上：

[
\rho^*
======

\arg\min_{\rho\in Eq^+}
\Big[
\alpha D_{\text{rep}}
+
\beta(1-\sigma)
+
\gamma\kappa
+
\delta\lambda
+\dots
\Big]
]

subject to：

semantic equivalence；

harmony；

conservativity；

answer non-leakage。

这就是：

# Representation Synthesis

不是 prompt optimization。

不是“搜索一句最好 prompt”。

而是：

> **自动设计任务语言。**

最终测试必须要求：

train tasks 上发现表示；

unseen instances 上有效；

最好 adjacent task family 上有效；

最好 transfer 到 unseen model。

如果能做到这一点，整个研究计划就完成：

[
\boxed{
Observe
\rightarrow
Formalize
\rightarrow
Predict
\rightarrow
Intervene
\rightarrow
Design
}
]

---

# 10. 推荐论文序列

| 顺序 | 暂定论文                                             | 主问题                                                | 优先级        |
| -- | ------------------------------------------------ | -------------------------------------------------- | ---------- |
| 0  | LP4FM / Source Arbitration                       | 谁控制答案？                                             | foundation |
| 1  | **From Evidence to Control**                     | evidence 怎样产生 relation control？                    | **当前旗舰**   |
| 2  | **Where Chains of Thought Choose**               | choice complexity 是否优于 chain length？               | 高          |
| 3  | **Same Semantics, Different Computations**       | representation distortion 如何影响 structure recovery？ | **高**      |
| 4  | **Classically Equivalent, Dynamically Distinct** | witness accessibility 决定了什么？                       | 高          |
| 5  | **Schemas Are Types**                            | constraint migration 是否降低 reasoning burden？        | 高          |
| 6  | **Constructed, Not Entailed**                    | 前三条能否形成统一表示理论？                                     | 条件性旗舰      |
| 7  | **Linguistic Relativity of Relational Control**  | language effect 落在哪个计算阶段？                          | 中期         |
| 8  | **Controlled Linguistic Experience**             | linguistic experience 是否因果改变 reasoning？            | **长期高上限**  |
| 9  | Mechanisms of Constructed Control                | 内部 causal mediator 是什么？                            | 条件性        |
| 10 | Humans × Artificial Language                     | 人类是否有结构同型效应？                                       | 条件性        |
| 11 | Machine-Designed Reasoning Languages             | 理论能否反过来设计表示？                                       | Capstone   |

---

# 11. 五年时间轴

### Year 1：建立 behavioral laws

重点：

Paper 1 From Evidence to Control；

(\kappa) pilot；

ReGeo Program-DAG pilot；

共享工具链。

目标：

至少拿下一个非常稳定的 empirical law。

---

### Year 2：建立 representation theory

完成：

Where Chains of Thought Choose；

Same Semantics, Different Computations；

Accessibility 主实验。

开始形成：

[
Construction
\neq
Recovery
\neq
Identification
]

的理论分解。

---

### Year 3：形成统一结构理论

完成：

Schemas Are Types；

Constructed, Not Entailed；

language-conditioned relational control。

到这里博士论文/研究 agenda 应该已经非常清楚。

---

### Year 4：进入真正因果性的 linguistic relativity

做：

controlled linguistic experience；

training dose；

transfer；

mechanistic intervention。

如果这里结果强，整个项目会从 NLP reasoning 进入 cognitive science / language learning 的理论空间。

---

### Year 5：回到人类 + 自动语言设计

根据结果选择：

human artificial-language experiment；

automatic representation synthesis；

machine-designed DSL；

cross-model transfer。

最终回答的不再是：

> 什么 prompt 好？

而是：

> **什么样的语言结构能系统性地产生更可靠的推理？**

---

# 12. 所有论文共用一套基础设施

不要以后每篇重新生成 benchmark。

建立长期 semantic source：

[
\mathcal G(W,G,R,D,L,E,A)
]

其中：

(W)：world；

(G)：latent computational structure；

(R)：governing relation；

(D)：diagnostic evidence；

(L)：language；

(E)：linguistic experience；

(A)：answer/action interface。

形式研究已经确立一个非常好的方法：先从统一 semantic source 生成，再渲染为不同自然语言、伪代码和形式表示；Lean 检查“是否相同”，自写 prover 计算“有多难”。

把现有 `ctorkit` 最终扩展成：

`core`

`equiv`

`focus`

`harmony`

`dts`

`sigma`

`regeo`

`routing`

`multilingual`

`intervention`

这样数据集、formal checker、model panel、evaluation、statistics 都能复用。

---

# 13. 每篇论文只允许回答一个 because

这是以后最重要的 scope discipline。

**Relation Control**

Behavior changes **because evidence distinguishes the governing relation**.

**ReGeo**

Representation matters **because it changes the difficulty of recovering latent structure**.

**Focusing**

Reasoning fails **because nondeterministic choices, not merely chain length, dominate proof search**.

**Accessibility**

Referents differ **because witness accessibility differs despite classical entailment**.

**Schemas Are Types**

Typed representations help **because constraints move from inference into construction**.

**Linguistic Relativity**

Cross-language differences arise **because language acts on a particular computation stage**.

**Controlled Experience**

The effect exists **because linguistic experience itself was experimentally manipulated**.

**Mechanism**

Behavior changes **because a specific internal state causally mediates the effect**.

**Representation Synthesis**

A representation wins **because theory predicts its structural properties before model evaluation**.

如果一篇 paper 同时出现三个 because，就应该切。

---

# 14. 全项目 Kill Criteria

这个长期计划最重要的是能够“死”。

**Evidence → Control**
没有 stable control gap → 不讲 two-stage control。

**ReGeo**
token-matched permutation 没有效应 → 不讲 structure distortion law。

**Focusing**
(\kappa) 不优于 step count → 放弃 universal focusing bottleneck。

**Accessibility**
truth-conditional model 胜 → 就报告模型更像 truth-conditional tracker。

**Schemas Are Types**
(P\to T) 无增益 → 类型化没有额外 cognitive benefit。

**Natural-language relativity**
效应被 tokenization/competence/calibration 吃掉 → 不启动 causal relativity。

**Controlled experience**
只在训练格式内有效 → 不叫 cognitive transfer。

**Mechanism**
没有 causal specificity → 不单独发机制论文。

**Human study**
模型 treatment effect 都没稳定 → 暂停 human branch。

这个思想和当前 integrated plan 的门控结构完全一致：门失败不是整个研究失败，而是强制缩小 claim 或切换叙事。

---

# 15. Best-Paper 潜力排序

### 近期最高概率

**From Evidence to Control**

因为现有 LP4FM 已经给它提供了 foundation，而且 relation information + relation control 是一个可能被社区长期复用的 decomposition。

### 中期最高辨识度

**Same Semantics, Different Computations**

尤其如果：

token-matched distortion；

cross-format structure recovery；

causal structure intervention

全部成功。

### 中期理论最深

**Constructed, Not Entailed**

前提是 B1–B4 真正形成共同 empirical pattern，而不是强行统一。

### 长期理论上限最高

**Controlled Linguistic Experience**

因为它第一次可能真正建立：

[
do(LinguisticExperience)
\rightarrow
ReasoningPolicy
]

而不仅仅是跨语言相关性。

### 最终最有“研究纲领闭环”价值

**Machine-Designed Reasoning Languages**

因为它把解释科学转成设计科学。

---

# 16. 最终统一理论

我建议以后整个研究项目内部用一个总标签：

# Constructed Relational Control

其核心理论可以暂时写成：

> **Intelligent behavior depends not merely on what information is present, but on how task-relevant structures are constructed by representation, recovered from surface form, identified from evidence, and granted behavioral control. Linguistic experience shapes reasoning by repeatedly privileging particular distinctions, structures, and relations throughout this pipeline.**

中文：

> **智能行为并不只取决于信息“有没有”，而取决于任务结构是否被表示构造出来、能否被模型从表面形式恢复、是否被证据识别，以及最终是否取得行为控制权。长期语言经验则通过反复强化某些区分、关系和结构，塑造这条计算链。**

所以最终：

LP4FM 研究：

> **谁在控制？**

Relation Information 研究：

> **模型怎么知道谁应该控制？**

Relation Control 研究：

> **知道以后为什么不一定控制？**

ReGeo 研究：

> **任务结构有没有被恢复？**

类型论研究：

> **约束有没有被直接构造？**

动态语义学研究：

> **需要的 witness 是否真的可达？**

证明论研究：

> **规则是否完整地确定了意义，搜索在哪里发生选择？**

语言相对论研究：

> **长期语言经验为何让某些结构更容易被构造、恢复与控制？**

最终 representation synthesis 研究：

> **我们能否反过来设计一种语言，让这些过程更容易发生？**

整个计划因此不再是四五个互不相干的 NLP 方向，而是一条连续的科学路线：

[
\boxed{
Linguistic\ Experience
\rightarrow
Representation
\rightarrow
Construction
\rightarrow
Recovery
\rightarrow
Identification
\rightarrow
Control
\rightarrow
Action
}
]

这条链，建议未来几年都不要轻易改变。

新文献出现以后，不再问：

> “是不是又要换方向？”

而只问：

> **它占掉了这条链上的哪条 causal edge？我们还没有识别的是哪一条？**

这样整个研究计划才会持续积累，而不是不断重启。

这版里，我会把你现在已有的 **44 周形式研究计划保留下来，但降级为 Program B 的内部执行计划**，而不是让它等于整个博士/长期研究计划。它本身已经有很好的工具链、预注册和门控设计，也已经规划了独立论文与 `ctorkit / ACCESS / HARP / BIB` 等产出，这些都非常值得复用。 

现在最重要的执行决策仍然很简单：**不要同时启动十篇。当前主资源压 Paper A1（Evidence → Control）；并行只做两个低成本生死 pilot——(\kappa) pilot 和 ReGeo token-matched Program-DAG pilot。** 这三个结果会决定未来 12–18 个月到底哪两条分支最值得重仓。
