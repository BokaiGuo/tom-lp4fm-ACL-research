可以。我会把现在这条线正式看成一个 **research program**，而不是继续往 Paper B 里叠实验。重新对照你当前仓库和最新文献后，我认为最合理的结构是：

[
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
]

你当前仓库已经把 Paper A 定义为 static source-arbitration foundation，把下一阶段母对象冻结为 `context-conditioned rule updating`，而且明确区分 `rule-change without fact-change`；这个基础非常适合向下面几篇自然递进。([GitHub][1])

我建议规划成 **A 已有 + B/C/D 三篇核心 + E/F 两篇条件性论文**。其中 B、C、D 每一篇都应该有完全不同的 causal question，绝对不要混在一起。

---

## 整个研究计划怎么切

| 阶段          | 核心问题                                                   | 主要干预                                                | 核心 estimand                                              | 应该成为独立论文吗      |
| ----------- | ------------------------------------------------------ | --------------------------------------------------- | -------------------------------------------------------- | -------------- |
| Paper A     | 当前到底哪个 source 在控制答案？                                   | source conflict                                     | source routing / arbitration                             | 已经是            |
| **Paper B** | 什么 evidence 能让模型识别规则，并让规则控制行为？                         | diagnostic evidence × rule swap × prior pressure    | Relation Information + Relation Control                  | **是，当前旗舰**     |
| **Paper C** | 改变语言时，变化发生在 rule identification 还是 behavioral control？ | language-of-evidence × language-of-query            | Language Identification Effect + Language Control Effect | **是**          |
| **Paper D** | 语言结构本身能否因果性地产生这种 control bias？                         | controlled linguistic exposure / artificial grammar | training-treatment causal effect                         | **是，而且理论价值很高** |
| **Paper E** | language/rule/control 在网络内部如何组合？                       | activation / representation interventions           | causal mediation / transport                             | 条件性            |
| **Paper F** | 人类是否出现相同的 information→control 结构？                      | bilingual / artificial-language human study         | human–model structural correspondence                    | 条件性            |

我尤其建议：**Paper B 不要为了多发一篇而拆成两个小 paper。**

你现在仓库里的三个主实验——policy-separation information、identification × prior pressure、functional partner adaptation——放在一起才有完整的：

[
Evidence\rightarrow Identification\rightarrow Control\rightarrow Action
]

这恰恰是最有 Best Paper 形状的版本。仓库当前也是这么冻结的。([GitHub][1])

---

# Paper B：先把“Evidence → Control”做成母定律

这一篇仍然应该是当前最高优先级。

我会把它的科学命题冻结成：

> **Demonstrations teach a governing relation only insofar as they distinguish competing policies; identifying that relation is necessary but not sufficient for the relation to control behavior.**

这里其实有两个量。

第一个是 **Relation Information**。

如果候选规则集合为 (\Pi)，定义：

[
I_R(D)=H(\Pi)-H(\Pi\mid D)
]

deterministic 条件下也可以直接使用：

[
I_R(D)=\log |\Pi|-\log |V(D)|
]

其中 (V(D)) 是 demonstrations 后仍然没有被排除的 candidate policies。

于是：

consensus examples 即使全部正确，也可能有

[
I_R\approx0
]

而一个 source-conflicting example 可以有很高的 (I_R)。

这一点与你现在仓库里 `within-example source conflict` 和 `across-example policy conflict` 的严格区分完全一致。([GitHub][2])

第二个量是 **Relation Control Effect**。

固定同一个 world：

[
belief=A,\quad reality=B
]

只把 governing rule 从：

[
R=belief
]

换成：

[
R=reality
]

模型行为应该从 A 翻到 B。

因此可以把：

[
RCE=
P(\text{appropriate behavioral flip}\mid rule\ swap)
----------------------------------------------------

P(\text{flip}\mid sham\ swap)
]

作为全项目长期复用的核心 estimand。

这会比单纯 accuracy 强很多，因为它问的是：

> **行为究竟有没有因 governing relation 的改变而改变。**

### Paper B 最漂亮的三个结果

第一张 hero figure 应该是：

[
1\ diagnostic\ demo > 16\ consensus\ demos
]

但 headline 不要停在 1 > 16，因为 critical-example literature 已经很接近。

真正应该证明：

[
I_R(D)
]

比 example count、semantic similarity、diversity、length、position 更好地预测 held-out rule learning。

第二张 hero figure 是：

[
I_R\times PriorPressure
]

的二维 control surface。

你会得到两个 surface：

[
P(\hat R=R)
]

以及

[
P(Y=Y_R\mid \hat R=R)
]

如果第一个已经饱和，而第二个仍然随着 competing prior 崩塌，就有真正的 identification/control separation。

第三个实验就是你现有 partner adaptation，但定位要改成：

> **functional assay of relation control**

而不是“另一个 ToM benchmark”。

---

## Paper B 有一个现在必须新增的强 negative control

2026 年最新工作已经把“内部信号→行为”的门槛抬高了。

Nature Machine Intelligence 刚刚用 confidence 做出了完整的 representation → threshold policy → behavior 因果链。([Nature][3])

因此你必须主动测试：

[
\text{commitment gap}
\overset{?}{=}
\text{simple output calibration problem}
]

至少要拟合一个 held-out global：

[
z'=az+b
]

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

因为 XToM 已经直接证明 LLM 的 ToM 表现在五种语言间有明显差异。([ACL Anthology][4])

BICAUSE 也已经证明语言条件会对应不同的 causal reasoning preference，并观察到跨语言的表示差异和收敛。([ACL Anthology][5])

NAACL 2025 更发现同一个 equivalence/inheritance reasoning question 跨语言可以出现 17.3%–57.5% 的冲突答案。([ACL Anthology][6])

所以 Paper C 必须问一个别人还没有精准回答的问题：

> **When language changes reasoning, does it change what relation the model infers, or how much control an already identified relation obtains over behavior?**

这就把语言相对论变成一个 computation-stage question。

---

## Paper C 最关键的设计，不是 Language × Accuracy

而是：

[
\boxed{
Language_{demo}
\times
Language_{query}
\times
Rule
\times
Diagnosticity
}
]

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

[
P(\hat R=R)
]

说明语言影响 **relation induction**。

如果 relation readout 不变，但改变 query language 就改变：

[
P(Y=Y_R\mid\hat R=R)
]

说明语言影响 **relation control**。

这比单纯 bilingual accuracy 差异高级很多。

---

# Paper C 可以产生三个理论结果

这是我很喜欢它的地方，因为三个结果都能明确解释。

如果 diagnostic evidence 足够多之后语言差异消失：

[
Language\rightarrow relational\ prior
]

也就是说，语言主要改变 default prior，强证据可以覆盖它。

这其实是一个非常现代的弱语言相对论：

> language biases which relation is initially accessible, but does not constrain eventual reasoning.

如果 identification 在语言间相同，但 final control 仍然不同：

[
Language\rightarrow control\ policy
]

这会是更令人惊讶的发现：

> 语言没有改变模型认为规则是什么，却改变规则能否赢得行为控制权。

如果强 diagnostic evidence 下 identification 仍然系统性跨语言不同：

[
Language\rightarrow rule\ induction
]

这才是一个更深的 reasoning relativity effect。

---

# Paper C 里 culture 应该是 control，不要抢 headline

这一点要切得非常狠。

ACL 2025 已经专门把 language medium 和 cultural context 正交化，并报告 Cultural-Linguistic Synergy。([ACL Anthology][7])

Nature Human Behaviour 2025 也已经报告中文/英文条件会改变 GPT 和 ERNIE 的 social orientation 与 cognitive style。([Nature][8])

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

[
L_1\neq L_2
\Rightarrow
behavior\ differs
]

但不能真正证明：

> 是语言结构本身造成了这种差异。

因为：

[
Language
+
Culture
+
Corpus
+
Tokenizer
+
TrainingFrequency
]

始终共变。

所以第三篇必须切到：

# **Controlled Linguistic Experience**

这里不要再用 natural languages。

用 **人工语言 / counterfactual grammar / controlled continued training**。

这条方法已有很好的基础：White & Cotterell 早在 ACL 2021 就提出用只在某个 typological feature 上不同的人工语言训练模型，从而建立 fully controlled causal framework；2025–2026 又有工作继续用 synthetic/counterfactual languages 研究 linguistic priors 和 typological learnability。([ACL Anthology][9])

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

[
Corpus_{treatment}
]

和

[
Corpus_{control}
]

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

他们在 52 个模型上发现 instruction-tuned models 常有强 user bias，并通过 controlled fine-tuning 发现 human-preference alignment 增强 user bias、reasoning fine-tuning 减弱它，还能用 DPO 双向改变这种 bias。([ACL Anthology][10])

所以 Paper D 的 novelty 必须明确比它多一步：

不是：

[
training\rightarrow source\ preference
]

而是：

[
\boxed{
obligatory\ linguistic\ distinction
\rightarrow
abstract\ relational\ learning/control
\rightarrow
cross-task\ transfer
}
]

也就是说，**训练时只是语言习惯，测试时是新的 reasoning task。**

这会非常关键。

---

# Paper D 最漂亮的因果 estimand

可以用 difference-in-differences：

[
\Delta_{\text{ling}}
====================

## (RCE_{\text{post}}-RCE_{\text{pre}})_{\text{source-marking}}

(RCE_{\text{post}}-RCE_{\text{pre}})_{\text{matched-control}}
]

再做 exposure dose：

[
0,\quad low,\quad medium,\quad high
]

如果出现稳定剂量效应：

[
linguistic\ exposure
\uparrow
\Rightarrow
source\ sensitivity
\uparrow
]

而且迁移到未见格式，那是一张非常强的 causal figure。

---

# Paper E：机制论文，不要现在就承诺

如果 B、C、D 中至少两个得到稳定结果，再做这一篇。

核心问题是：

> **Language-conditioned prior、identified rule、behavioral control 在内部到底是共享表示还是独立变量？**

可以形成三个 probe：

[
z_L=\text{language state}
]

[
z_R=\text{relation state}
]

[
z_C=\text{control state}
]

但 probe 不应该是 headline。

真正应该做的是 intervention。

例如：

保持：

[
R,W,D
]

不变，

把 language-conditioned activation 从 (L_1) condition transplant 到 (L_2)，问：

[
RCE
]

是否随之移动。

或者针对已经识别出来的 source relation 做 targeted steering，看：

是否只改变对应 conflict items；

是否跨 content transfer；

是否不改变 unrelated relations；

是否不只是改变 output language。

你现在 Paper A 的一个重要教训就是：mechanism 可能 checkpoint-sensitive，所以不要过早把“某个 direction”写成 universal controller。你自己的仓库已经非常清楚地把 readability、control、specificity、transport 分开了。([GitHub][11])

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

[
human:\quad
LinguisticExposure\rightarrow RelationalControl
]

与

[
LLM:\quad
LinguisticExposure\rightarrow RelationalControl
]

注意这里不要声称：

> LLM models human cognition.

而是问：

> **相同的 controlled linguistic experience 是否在两种学习系统中产生同方向的 relational bias？**

如果答案 yes，这就真正从 NLP 开始触及 cognitive science。

已有 2026 年土耳其 evidentiality 工作已经直接比较 humans 和 10 个 LLM，发现人类 source-trust effect 稳定，而模型高度 prompt/model-dependent。([arXiv][12])

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

[
G(W,R,D,L,C,A,E)
]

其中：

(W) = latent world；

(R) = governing relation；

(D) = diagnostic evidence；

(L) = linguistic medium；

(C) = cultural context；

(A) = answer/action interface；

(E) = linguistic training experience。

Paper B 只动：

[
R,D,Prior
]

Paper C 只额外动：

[
L
]

Paper D 只额外动：

[
E
]

Paper E 不动 stimulus，直接动：

[
internal\ state
]

Paper F 把：

[
Population={LLM,Human}
]

加进来。

这样每篇论文都在回答同一个 program 的不同 causal edge。

---

# 我建议现在就画成这棵决策树

当前先跑 Paper B。

如果：

[
I_R\rightarrow Identification
]

很强，但 identification→control gap 不稳定，

那么直接收缩为：

**Conflict Is Supervision / Relation Information in ICL**

这是完整 paper，不要硬造 commitment story。

如果：

[
I_R\rightarrow Identification
]

以及

[
Identification\not\Rightarrow Control
]

都强，

就做完整旗舰 Paper B。

然后才启动 Paper C。

如果 Paper C 发现没有稳定 language effect：

**停止 linguistic-relativity branch。**

这不是失败；说明 relation-control 可能比普通 multilingual reasoning 更 language-invariant。

如果 Paper C 有强 effect，但全部被 tokenization / model competence / global calibration 解释：

也不要启动 Paper D。

只有当发现：

[
Language
\rightarrow
stage\text{-}specific\ relational\ effect
]

而不是 generic accuracy drop，

才值得进入 causal linguistic exposure。

如果 Paper D 再证明：

[
controlled\ linguistic\ experience
\rightarrow
out\text{-}of\text{-}distribution\ relational\ control
]

那时候整个 research program 的理论地位就完全不同了。

---

# 哪一篇最有 Best Paper 潜力？

目前我的排序会是：

**Paper B：最高的近期成功概率。**
因为任务、ontology、验证工具几乎都已经在你的仓库里，且与你 Paper A 连续。

**Paper D：最高的长期理论上限。**
因为它有机会真正回答“语言经验是否因果改变 reasoning policy”，而不是只做 observational multilingual comparison。人工语言方法本身已有严格 causal precedent，但目前的主流工作主要研究 grammar learnability，而不是 grammar experience 如何改变后续 relation control。([ACL Anthology][9])

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

[
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
]

Paper A 已经从中间的 **source arbitration** 开了口子。

Paper B 建立 `Evidence → Identification → Control`。

Paper C 把 `Language` 加进来。

Paper D 真正操纵 `Linguistic Experience`。

Paper E 打开内部机制。

Paper F 才有资格重新回到 Boroditsky / Sapir–Whorf 的人类认知问题。

**如果按这个切法推进，你就不需要每次发现一篇新文献便重写整个项目。新文献只会告诉我们它占掉了链条中的哪一条边，而我们继续沿尚未被因果识别的边往前推。**

这会比“不断找一个新的 Best Paper idea”稳定得多。

[1]: https://github.com/BokaiGuo/tom-lp4fm-research "GitHub - BokaiGuo/tom-lp4fm-research: Private LP4FM manuscript, evidence notes, and oral research proposals · GitHub"
[2]: https://github.com/BokaiGuo/tom-lp4fm-research/blob/main/research/related_work_positioning_20260826.md "tom-lp4fm-research/research/related_work_positioning_20260826.md at main · BokaiGuo/tom-lp4fm-research · GitHub"
[3]: https://www.nature.com/articles/s42256-026-01293-x?utm_source=chatgpt.com "Causal evidence that language models use confidence to drive behaviour | Nature Machine Intelligence"
[4]: https://aclanthology.org/2026.acl-long.805/?utm_source=chatgpt.com "XToM: Exploring the Multilingual Theory of Mind for Large Language Models - ACL Anthology"
[5]: https://aclanthology.org/2025.findings-emnlp.1321/?utm_source=chatgpt.com "Under the Shadow of Babel: How Language Shapes Reasoning in LLMs - ACL Anthology"
[6]: https://aclanthology.org/2025.naacl-long.394/?utm_source=chatgpt.com "Towards Robust Knowledge Representations in Multilingual LLMs for Equivalence and Inheritance based Consistent Reasoning - ACL Anthology"
[7]: https://aclanthology.org/2025.acl-long.1082/?utm_source=chatgpt.com "Disentangling Language and Culture for Evaluating Multilingual Large Language Models - ACL Anthology"
[8]: https://www.nature.com/articles/s41562-025-02242-1?utm_source=chatgpt.com "Cultural tendencies in generative AI | Nature Human Behaviour"
[9]: https://aclanthology.org/2021.acl-long.38/?utm_source=chatgpt.com "Examining the Inductive Bias of Neural Language Models with Artificial Languages - ACL Anthology"
[10]: https://aclanthology.org/2026.findings-acl.449/?utm_source=chatgpt.com "User-Assistant Bias in LLMs - ACL Anthology"
[11]: https://github.com/BokaiGuo/tom-lp4fm-research/blob/main/research/next_stage_relation_preservation_20260909.md "tom-lp4fm-research/research/next_stage_relation_preservation_20260909.md at main · BokaiGuo/tom-lp4fm-research · GitHub"
[12]: https://arxiv.org/abs/2604.24665?utm_source=chatgpt.com "Benchmarking Source-Sensitive Reasoning in Turkish: Humans and LLMs under Evidential Trust Manipulation"
