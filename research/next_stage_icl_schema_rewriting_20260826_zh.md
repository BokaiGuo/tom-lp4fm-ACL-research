# 下一阶段论文研究方案：上下文学习中的答案权威组合泛化

英文对应稿：[Next-Stage Paper Plan: Authority Compositional Generalization](next_stage_authority_compositional_generalization_20260826.md)。

## 暂定标题

**What Does In-Context Learning Learn? Authority Compositional Generalization in Language Models**

中文标题：**上下文学习究竟学到了什么：语言模型中的答案权威组合泛化**。

备选标题：

- **Who Gets to Write the Answer? In-Context Schema Selection in Language Models**
- **Learning Who May Override Whom: Authority Composition in Context**
- **Beyond Task Mapping: Context-Induced Authority Schemas in Language Models**
- **从信息利用到答案授权：上下文学习的图式选择机制**

## 1. 先给结论

下一篇论文不应继续把五个分支并列成五组实验，也不应再把核心问题写成“哪个
composition operator 最好”。更有潜力的统一对象是：

> **上下文学习不是单纯地从示例中拟合输入—输出映射，而是在选择或重写一个
> 决定“哪类信息有资格支配答案”的任务图式。**

本文把由此产生的新研究对象称为 **Authority Compositional Generalization
(ACG，答案权威组合泛化)**：模型能否从 demonstrations 中学习“哪类来源在什么
冲突拓扑下可以压过哪类来源”的关系规则，并把该规则重新实例化到未见内容分布、
答案编码和联合冲突拓扑上。

这里的“权威”不是哲学隐喻，而是一个可操作的 source-control object：在相同的
latent world、相同的候选答案和相同的答案编码下，模型被不同上下文诱导去跟随
queried agent、reality、reporter、rule、other agent 或 distractor 中的哪一个来源。

当前 LP4FM 只完成了这个对象的**静态发现层**：语言构式可以改变 typed source
arbitration，compound effect 具有 checkpoint-specific 的非加性和 transport 边界。
下一篇论文要推进到**动态上下文层**：示例和语言表示是否能重写模型的 source
authority schema，以及这种 schema 能否脱离原始内容进行关系级组合泛化。

该方向目前是 `argument_shaped` 的研究候选，不是已经验证的颠覆性结论。真正的
突破点不是“提出 schema 这个词”，而是证明模型学到的是可迁移、可组合的 source
relation，而不是复现答案标签、词面模板或单个 item 的响应。ACG 是论文母对象；
schema rewriting 是对其内部实现方式的解释假设。

## 2. 为什么以 ICL 为中心重组五个分支

付靖文与郑南宁相关的上下文学习和组合泛化研究提供了五个重要启发：

1. **上下文不只是更多信息。** `How Do In-Context Examples Affect Compositional Generalization?`
   发现示例的 structural similarity、diversity、complexity 以及对所需结构的覆盖，
   会决定 unseen composition 的表现。下一篇论文要问：这些因素是否实际上在选择
   一个 latent task schema，而不只是改变相似度匹配？
2. **模型内存在 weights/context 的功能分工。** `Breaking through the Learning
   Plateaus of In-context Learning in Transformer` 将 ICL 行为区分为受参数影响的
   weights component 和由当前上下文诱导的 context component。我们的对象可以把
   两者具体化为：训练形成的默认 authority prior，以及当前 demonstrations 对该
   prior 的 schema update。
3. **组合不是必然独立。** `A General Theory for Compositional Generalization`
   引入 generative effects，提醒我们不能默认 primitive 之间独立组合。当前项目的
   Stage-53–55 正好显示 pooled composition effect 与 item-level alignment 可以分离，
   因而下一步应研究 context/schema 的联合生成，而非继续比较几个标量函数。
4. **数据不足与不会组合是两种失败。** `On the Statistical Mechanisms of
   Distributional Compositional Generalization` 将 supporting distributions 的覆盖
   与算法的 knowledge-composition capacity 分开，并强调学习算法与 composition
   rule 的 compatibility/sensitivity。对本项目而言，这意味着未见 compound 失败时，
   必须区分 demonstration coverage deficit、authority-rule composition deficit 和
   algorithm–rule incompatibility，不能都归结为“上下文不够”。
5. **要看学习轨迹，不只看最终模型。** `Learning Trajectories are Generalization
   Indicators` 以及相关训练动态工作启发我们跟踪 authority schema 的形成、可塑性和
   change point，而不是只比较 Base 与 Instruct 两个终点。

郑南宁相关的 `Make Your LLM Fully Utilize the Context` 和关于 language
representation shaping schema 的工作进一步提示：提升 context utilization 的关键
不一定是增加上下文长度，而可能是改变上下文如何组织和激活内部 schema。我们的
贡献是把这个一般性命题落实为一个更窄、更可识别的 schema 维度：**答案来源的
资格分配**。

### 2.1 逐篇吸收后的研究设计映射

| 工作 | 原论文的核心对象 | 本文吸收的设计 | 本文新增的对象 |
|---|---|---|---|
| An et al., CoFe (ACL 2023) | 示例 similarity、diversity、complexity 与 linguistic-structure coverage 如何影响 ICL composition | 把四个因素做成 demonstration factorial，不只做随机 few-shot | 检查这些因素诱导的是 source relation，还是只提高结构检索 |
| Fu et al., learning plateaus (ICML 2024) | weights component 与 context component 的功能分工 | 分开 default authority prior 与 context-induced update | 研究二者如何共同决定 typed source authority，而非一般 ICL accuracy |
| Fu & Zheng, DCG mechanisms (ICML 2025) | supporting distributions、数据作用、algorithm–rule compatibility/sensitivity | 设置 coverage-deficient、composition-heldout 与 compatibility-shift 三类条件 | 将 composition rule 具体化为 `R(a,b|T)`，寻找 authority-specific predictor |
| Fu et al., learning trajectories (NeurIPS 2023) | 每次参数更新对 generalization 的贡献及 trajectory complexity | 沿同一训练路径测 default policy、plasticity 与 ACG change point | 判断训练改变的是 source ontology、authority prior，还是 relation-composition capacity |
| Fu et al., general CG theory (2024) | task-agnostic generalization 与非独立 generative effect | 不预设 primitive authority constraints 独立相加 | 用 joint source topology 定位 relation 何时生成新 policy、何时只发生 itemwise rewiring |

这张映射表也界定了不能声称的 novelty：本文没有首先发现 ICL 依赖示例结构、
weights/context 分工或 composition 的 data dependence；新的贡献必须是把它们统一到
一个此前未被明确提出的功能对象——**typed source-relation program 的上下文归纳与
组合泛化**。

## 3. 与现有工作真正的区别

本节给出 paper-level 的直接比较；五个旧分支、ACG 与完整文献簇之间的统一关系见
[Related Work 定位地图](related_work_positioning_20260826_zh.md)，英文版见
[Related-Work Positioning](related_work_positioning_20260826.md)。

| 研究方向 | 已经回答的问题 | 本文必须回答的不同问题 |
|---|---|---|
| ICL 示例选择 | 什么样的示例有利于结构泛化 | 示例究竟诱导了哪一种 source-authority schema |
| weights/context 分解 | 参数知识与上下文信息如何共同产生 ICL | 哪一部分决定“谁能写答案”，以及 context update 是否可迁移 |
| distributional compositional generalization | 如何区分数据覆盖与算法的知识组合能力 | source-relation rule 能否在未见内容分布和冲突拓扑中重新实例化 |
| language representation design | 不同表示会激活不同 schema | 哪个具体 schema 维度被改变，能否做 source-specific causal transfer |
| shared router/value slot | 多个 mental spaces 是否共享路由和 value slot | 多个相互冲突的 typed sources 如何竞争回答资格，是否存在 authority rewiring |
| imperfective/representation–reasoning 工作 | 语义区分可能被 prior 压过 | 为什么某个表示获得决策权，如何让正确 source 重新取得控制权 |
| instruction hierarchy / knowledge conflict | 明示指令或知识来源冲突如何处理 | 语言构式和 demonstrations 如何在没有显式 system policy 时生成隐含权威图式 |
| 当前 LP4FM | 语言形式改变 typed arbitration，组合存在边界 | ICL 是否能主动选择、迁移和训练出一个 source-control schema |

因此，本文不能声称“发现了一个更好的 router”，也不能把普通 prompt
robustness 改名为 epistemic schema。核心差异必须落在 source relation 的**跨内容、
跨表示、跨未见拓扑重新实例化**上。

## 4. 母对象：Authority Compositional Generalization

### 4.1 ACG 的关系对象

定义一个 typed source relation：

```text
R(a, b | T) = 在冲突拓扑 T 下，source a 是否有资格压过 source b
```

demonstrations 表面上提供若干 `(x, y)`，但真正需要抽取的是隐含的 `R`。ACG 不考查
模型能否复制 `y`，而考查同一个关系规则能否经历三类重新实例化：

```text
content generalization        = 固定 R，替换事实、实体和 latent world
representation generalization = 固定 R，替换语言构式、标签和答案编码
authority compositionality    = 组合已见 primitive R，求解未见 joint R / topology
```

因此，传统 ICL accuracy 只观察结果；ACG 观察 demonstrations 是否改变了模型处理
source relation 的方式，以及这种关系能力能否脱离 supporting distribution。

### 4.2 四层 functional schema

对一个测试问题，区分四个对象：

```text
content state       = 世界中发生了什么、每个 agent 知道什么
source ontology     = 哪些 typed sources 可以提供答案
conflict topology   = 哪个 source 必须击败哪些 competitors
authority policy    = 当前上下文授予哪个 source 支配答案的资格
```

定义上下文诱导的 authority schema：

```text
S_k(D, T) = (O, T, Pi_k(D, T), C_k(D, T))
```

其中：

- `k` 是模型 checkpoint；
- `D` 是 demonstrations、任务描述或语言表示；
- `O` 是 typed source ontology；
- `T` 是 conflict topology；
- `Pi_k(D, T)` 是 source authority policy；
- `C_k(D, T)` 是多个构式或示例共同作用时的 schema composition rule。

模型输出不再只表示为 `f_k(x, D)`，而表示为：

```text
z(x) = content representation of the current item
y     = Readout_k(z(x), Pi_k(D, T(x)))
```

关键分离是：`z(x)` 可以已经包含正确的 belief、reality 和 report 信息，
但 `Pi` 仍然可能把回答权授予错误来源。

### 4.3 weights/context 分解

把训练和 ICL 连接起来：

```text
Pi_k(D, T) = Pi_weight,k(T) + Update_context,k(D, T)
             + Interaction_k(D, T)
```

这不是声称真实神经网络必然存在三个可分离模块，而是一个需要通过 factorial
和 causal transfer 检验的 functional decomposition：

- `Pi_weight`：训练形成的默认 source priority；
- `Update_context`：当前示例临时诱导的 authority change；
- `Interaction`：模型先验与上下文结构共同决定的非独立部分。

这使付靖文的 weights/context distinction、训练轨迹研究和当前 Base/Instruct
边界能够进入同一个研究对象，而不是各自成为一个分支。

### 4.4 理论中心不是一组实验 gate

ACG 的理论主张是改变 ICL 的基本分析单位：

```text
传统单位 = 从 demonstrations 归纳 input-output mapping
本文单位 = 从 demonstrations 归纳 typed source-relation program
```

这个新单位同时解释为什么相同内容会产生不同答案、为什么 primitive accuracy 高却
不能保证 compound 成功、以及为什么 Base/Instruct 可能保留 source ontology 却改变
组合轨迹。实验的作用是确定这个理论对象能解释到哪里，而不是把论文缩减成一串
“通过/未通过”的检验。

## 5. 核心论文命题

### 总命题

> **In-context learning can induce a compositional policy over source authority:
> demonstrations specify which represented source may override which competitor,
> and the learned relation can be re-instantiated on unseen content,
> representations, and conflict topologies.**

### 四个必须展示的现象

#### A. Authority inversion

同一 latent world、同一问题、同一候选答案，只改变 demonstrations 或 language
representation，模型从跟随 reality 切换为跟随 queried agent，或从 report 切换为
rule。该切换不能由 answer-token frequency、位置、长度或标签映射解释。

#### B. Content–authority dissociation

在 authority schema 改变时，当前 item 的 proposition/content readout 基本保持；
但 source-specific answer margin 和 argmax 改变。反过来，替换 content 而保留
诱导 schema 时，模型仍按新的 content 回答，却遵循原 schema 的 source priority。

#### C. Schema transfer

在一组 latent worlds 或一个 construction family 中诱导的 authority policy，
迁移到新的 world、答案编码、构式或 typed domain。只有同 item 的 response
reconstruction，不算 schema transfer。

#### D. Authority compositional generalization

模型只在 demonstrations 中见过 primitive source relations，例如 `belief > reality`
与 `rule > report`，却能在新的实体、表述和 joint topology 中求解二者共同约束的
source policy。该结果必须排除“记住某个 source token 总是优先”和“训练集中已经
出现同构 compound”两种解释。

## 6. 五个原有分支在新论文中的位置

| 原分支 | 在新论文中的角色 | 不再作为独立主张的内容 |
|---|---|---|
| 训练诱导 composition phase transition | 追踪 `Pi_weight,k`、context plasticity 和 schema change point | 不再把 Base/Instruct 差异直接叫作 instruction-tuning causality |
| behavioral source-access algebra | 研究 authority schema 的 composition 和 generative interaction | 不再把 weakest-link/min 当作预设母对象 |
| mechanistic identifiability | 建立 content、authority、control、transport 四层识别协议 | 不再把 response reconstruction 当成 controller identification |
| conflict-topology kernel | 把 source competition 表示为可迁移的 graph/topology object | 不再假设一个 universal kernel 已经存在 |
| source-accessibility training objective | 用 schema-aware objective 选择性改变 authority policy | 不再把训练收益等同于机制修复 |
| 前两条合并分支 | 形成 `context-induced schema -> authority composition -> training trajectory` 主链 | 不再以“寻找一个唯一 composition operator”为论文中心 |

## 7. 实验总架构

### Study 0：构造 Authority-Schema Benchmark

建立一个比现有 false-belief matrix 更抽象、但仍由 latent world 生成的 benchmark。

每个 item 同时包含：

- `content factors`：事件、对象、状态和各 agent 的知识；
- `source factors`：belief、reality、report、rule、other-agent、distractor；
- `topology factors`：single conflict、pair conflict、joint conflict、chain conflict；
- `representation factors`：自然语言、结构化 slot、逻辑/符号表示和 controlled rewrite；
- `answer factors`：候选顺序、answer token、标签和 source identity 的独立置换。

ICL demonstrations 设计为四类：

1. 只覆盖 primitive source distinction；
2. 结构与测试 item 相似但 source policy 不同；
3. source policy 相同但表面形式不同；
4. 多个 examples 共同诱导一个 unseen compound authority schema。

每个 target compound 还要配三种 supporting-distribution 条件：

```text
coverage-deficient = 至少一个必要 primitive relation 从未出现
composition-heldout = 所有 primitive relation 都出现，但 joint rule 从未出现
compatibility-shift = 数据量和 primitive coverage 匹配，但 relation 的组合方式改变
```

这样才能把“没给足数据”“给了 primitive 但不会组合”和“模型偏好的算法与目标规则
不兼容”分开，而不是用一个 aggregate accuracy 解释全部失败。

主实验至少使用 ToM、temporal/imperfective 和 deontic 或 multi-document authority
中的两个 domain。ToM 是继承现有资产的主域，其他 domain 用来检验 schema 是否超出
false-belief wording。

自然语言 arms 在进入主实验前必须完成独立 linguistic-validity annotation：至少检查
grammaticality、事件语义等价、source attribution 是否清楚，以及改写是否意外泄露
目标 authority relation。当前 LP4FM 的人工 annotation 尚未完成，因此生成器定义的
对照只能作为 stimulus construction，不能自动继承为 human-validated manipulation。

### Study 1：ICL 是否诱导 source authority

固定测试 item，改变 demonstrations 的 structural similarity、diversity、complexity
和 source-policy coverage，测量：

- intended-source capture；
- target-versus-strongest-competitor margin；
- source-switch curve；
- authority entropy；
- off-policy leak；
- cross-encoding source-identity accuracy。

核心对照不是更多模型，而是四个解释：

```text
label mapping      -> 只学会答案 token 对应
surface retrieval  -> 只依赖词面相似度
content updating   -> 只改变事实/命题表示
schema induction   -> 改变 source priority，并能迁移到新 content
```

只有第四种解释通过 cross-content 和 cross-encoding transfer，才能进入主结论。

### Study 2：Authority schema 的跨内容因果迁移

这是全篇最关键的实验，不应被同 item response reconstruction 替代。

对 source 与 content 做 factorial crossing：

```text
context A induces authority policy Pi_A
content X supplies the current world facts
content Y supplies a new world with new labels
```

检验把 `Pi_A` 迁移到 `Y` 后，模型是否仍依据 `Y` 的事实作答，但遵循 `Pi_A`
的 source priority。

必须包含：

- content-only intervention；
- source-authority delta intervention；
- answer-label permutation；
- final-token-only；
- wrong-channel；
- item-shuffled；
- norm-matched random；
- zero intervention；
- same-item reconstruction 对照。

主要 estimand 是 `cross-content source-policy preservation`，而不是 response
reconstruction error。若只复制原 item 的输出分布，不算因果 authority control。

### Study 3：Authority compositional generalization

将 reportative/factive、voice/event-state、order/locality、coreference 等因素
重新定义为对 authority graph 的 schema transformations，而不是独立 scalar costs。

从 primitive demonstrations 学习：

```text
source ontology + topology + primitive transformation
```

然后预测完全未见的 binary、ternary 和 cross-family compounds。主要比较不只是
哪一种 operator 拟合得更好，而是哪一种解释能跨 supporting distribution 预测：

- independent additive update；
- scalar saturation；
- topology-conditioned graph rewrite；
- context-dependent generative interaction；
- no-schema / surface-only baseline。

对每个失败都进行三分诊断：

```text
data coverage deficit                  = 必要 primitive evidence 不足
authority-rule composition deficit     = primitives 齐全但 unseen joint relation 失败
algorithm–authority-rule incompatibility = 改变表示或训练方式后，同一规则显著改变可学性
```

最有理论价值的结果不是“compound 比 primitive 难”，而是找到一个 relation-level
compatibility criterion：它在给定 demonstrations 之前或只用 primitive behavior，就能
预测哪些 authority composition 会成功、哪些会发生 checkpoint-specific rewiring。

首选候选判据由两部分构成：

```text
graph compatibility = primitive authority edges 合并后是否一致、可满足、且能确定目标 source
model sensitivity    = 每条 primitive edge 引起的 source-margin change 是否跨 content 稳定
```

若关系图本身冲突或欠定，失败属于 task/data definition；若图可满足但 primitive
sensitivity 不稳定，失败属于 representation/learning algorithm；若二者都通过而 joint
topology 仍失败，才是严格意义上的 authority-rule composition deficit。论文不必预设
二者是简单乘法，但必须检验它们能否比 example count、surface similarity 和 primitive
accuracy 更早、更好地预测 ACG。

当前 Stage-53 的 typed-topology-localized third-order nonadditivity 是动机和 discovery
证据，不是新论文的最终证明。Stage-54/55 的 pooled-versus-item rewiring 则应被
用来预测：schema 的总体方向可能稳定，但具体实例化不能自动 transport。

### Study 4：训练轨迹中的 default schema 与可塑性

优先使用同一模型的：

```text
Base -> continued pretraining -> early SFT -> late SFT
     -> early preference/alignment -> final checkpoint
```

每个 checkpoint 用完全相同的 benchmark 测量：

- default authority policy；
- context-induced policy change；
- schema plasticity；
- cross-content transfer；
- topology sensitivity；
- composition residual；
- item-level schema geometry。

研究重点不再只是“residual 正负是否变化”，而是：训练是否改变模型接受上下文
重写 authority policy 的能力，以及改变的是默认 schema、可塑性还是二者的耦合。

如果拿不到公开中间 checkpoint，则训练一个小规模、协议完全受控的 trajectory；
外部模型只能作为 anchor，不能替代 within-trajectory evidence。

### Study 5：Schema-aware training objective

把第五分支降为新论文的因果验证层。训练目标可写为：

```text
L = L_answer
  + lambda_source * L_typed_authority
  + lambda_topology * L_conflict_graph
  + lambda_comp * L_schema_composition
  + lambda_leak * L_off_policy
```

比较：

1. standard SFT；
2. paraphrase consistency；
3. primitive typed-routing objective；
4. topology-balanced schema objective；
5. full schema-aware objective。

训练成功的标准不是所有 prompt 都更稳定，而是：

- 合法 source change 时保持敏感；
- 非法 distractor 或 wrong source 不取得回答权；
- unseen construction 和 latent world 上保持 source-specific control；
- 不通过抹平语言差异或提高所有 answer margin 获胜。

若 Study 5 预算不足，它可以成为后续论文；不能让它阻塞 Study 1–3 的核心发现。

## 8. 机制识别协议

本文必须把四种容易混淆的对象严格分开：

| 层级 | 问题 | 充分证据 |
|---|---|---|
| Content readability | 模型是否表示了各 proposition/source 内容 | held-out typed probe 与 content swap |
| Authority accessibility | 上下文是否改变了 source margin | topology-crossed behavioral matrix |
| Authority controllability | 干预是否让 intended source 取得 argmax | source-specific intervention 与 wrong-channel controls |
| Schema transportability | policy 是否脱离原 item/label/content 迁移 | cross-content、cross-encoding、cross-domain transfer |

Stage-57 已明确：construction-specific activation state 可以重建 paired response，
但不能因此声称 typed controller transport。新论文的机制主结果必须是第四层，
否则“schema rewriting”仍只是行为解释。

## 9. 主图与主表

### Figure 1：从 data plane 到 authority control plane

展示同一 world content 经过不同 demonstrations 后，source-authority graph 如何
改变；右侧显示相同 proposition representation 下的不同答案控制来源。

### Figure 2：Authority inversion

同一测试 item 在四类 context schema 下的 typed source margin、argmax 和 answer
encoding permutation。主问题是“谁获得答案写入权”。

### Figure 3：Cross-content schema transfer

训练/诱导 context 与 held-out content 的矩阵；只有 source-policy preservation
而非同 item reconstruction 的结果进入主图。

### Figure 4：Authority compositional generalization map

横轴区分 supporting-distribution coverage，纵轴表示 graph compatibility 与 model
sensitivity；每个格子展示 unseen authority topology 的成功率。该图直接区分“数据
不足”“关系不可满足”“模型对 primitive edge 不敏感”和“真正的组合失败”。

### Figure 5：Training trajectory and repair

展示 default schema、context plasticity、transportability 和 schema-aware objective
的 Pareto trade-off。

## 10. 论文贡献应该怎样写

### 最低可发表版本

如果只有 Study 1 通过：

> Controlled demonstrations can change typed source arbitration under fixed latent
> semantics, and the effect depends on structural coverage rather than answer labels.

这是强的 ICL/source-arbitration 论文，但还不能使用“schema rewriting”作为强机制名。

### Oral-shaped 版本

如果 Study 1–3 通过：

> In-context learning performs authority compositional generalization: it infers
> typed source relations from demonstrations and re-instantiates them on unseen
> content, representations, and conflict topologies. Graph compatibility and model
> sensitivity predict this ability beyond demonstration coverage and primitive accuracy.

这是下一阶段最值得追求的主会版本。

### 真正具有颠覆潜力的版本

如果 Study 1–4，且至少一个独立 domain 通过：

> The functional unit learned in context is not merely a task mapping but a
> relational authority program over information sources. Training determines which
> relation programs are compatible with the model and how plastically context can
> instantiate them; language representation shapes that compatibility.

这会把 ICL、语言表示、ToM、compositional generalization 和 mechanism
identifiability 放进同一个解释框架。

### 不能提前写的内容

- 不能写成所有 LLM 都有统一的 epistemic institution；
- 不能把 behavior schema 直接等同于单一 neural circuit；
- 不能把 same-item reconstruction 写成 cross-content causal transfer；
- 不能把 Base/Instruct 差异直接归因于 instruction tuning，除非有受控训练轨迹；
- 不能把 `schema` 当作新名词替代已有 prompt effect；必须展示迁移和干预；
- 不能把当前 LP4FM 的 Stage-53–57 结果重复包装成新论文的最终证据。

## 11. 最小高信息量 pilot

不先做大模型 sweep，也不先增加第三、第四个 linguistic factor。先做一个信息量
最高、能区分 task mapping 与 relation program 的 pilot：

```text
2 domains: ToM + temporal/imperfective or deontic
3 induced policies: queried-source, reality/source-report, distractor-resistant
5 support regimes: none, coverage-deficient, composition-heldout,
                   compatibility-shift, direct-compound upper bound
20 independent latent worlds per regime
2 checkpoints: Qwen2.5-7B-Instruct + matched Base
```

pilot 必须一次性输出：

1. same-item authority inversion；
2. cross-content source-policy transfer；
3. cross-encoding relation preservation；
4. primitive-to-unseen-topology composition；
5. graph-compatibility/model-sensitivity predictor；
6. answer-label、position 和 source-token permutation controls。

行为层 ACG 通过后，再进入 content-only versus authority-only intervention、
source-specific argmax 和 wrong-channel controls；不能用尚未成熟的神经干预阻塞对
新功能单元的第一轮识别。

### 决策规则

- 若只有 same-item inversion：只能解释为 source-arbitration/context effect；
- 若 relation 跨 content 和 encoding 保存：ACG 进入主论文开发；
- 若 primitive relations 能组合到 unseen topology，且判据优于 coverage/accuracy baseline：
  形成 oral-shaped 核心；
- 若再加 cross-domain transfer：升级为一般 ICL 功能单元主张；
- 若 training objective 能选择性改变 policy 且保留合法语义敏感性：形成完整闭环；
- 若 cross-content transfer 失败：结论回到 LP4FM 的 construction-specific boundary，
  不用增加更多 construction 或模型来掩盖。

## 12. 与当前 LP4FM 的关系

当前稿件是 **Paper A：静态语言控制界面的发现与边界**。它的价值不是被下一篇
取代，而是提供：

- typed source ontology；
- conflict-topology stimulus grammar；
- source-indexed answer interface；
- composition residual 与 affine-gain audit；
- response reconstruction 与 typed-controller transport 的区分；
- Base/Instruct/Qwen3 的 checkpoint boundary。

下一篇是 **Paper B：答案权威组合泛化**。两篇的关系应是：

```text
Paper A: language can alter which source controls the answer
Paper B: context can infer source-relation rules and compose them on new distributions
```

Paper B 的主实验必须使用新设计的 demonstrations、cross-content factorial 和至少
一个新的 typed domain；不能只把现有 Stage-53/54 的 compound rows 重新命名。

为避免与已投稿 Paper A 形成实质重叠，Paper B 的 primary endpoints 固定为：

```text
cross-content relation preservation
cross-encoding relation preservation
primitive-to-unseen-topology authority composition
coverage-versus-compatibility decomposition
```

Paper A 的 static arbitration、composition residual 和 Stage-53–57 只承担 motivation、
benchmark provenance 与 boundary evidence；不计入 Paper B 的主结果，也不等待 Paper A
审稿结果才启动。

## 13. 一句话研究路线

```text
information sources are represented
-> demonstrations specify primitive authority relations
-> context induces a source-relation program
-> the program assigns answer authority
-> relations compose or rewire on unseen topology
-> training changes default policy/plasticity
-> explicit schema supervision repairs selective authority failures
```

这条路线比“继续寻找一个更准确的组合算子”更有可能产生真正的理论跃迁，同时仍
牢牢建立在当前项目已经观察到的 typed arbitration、composition rewiring 和
mechanism non-identifiability 之上。

## 14. 参考文献入口

- [How Do In-Context Examples Affect Compositional Generalization?](https://arxiv.org/abs/2305.04835)
- [Breaking through the learning plateaus of in-context learning in Transformer](https://arxiv.org/abs/2309.06054)
- [Learning Trajectories are Generalization Indicators](https://arxiv.org/abs/2304.12579)
- [A General Theory for Compositional Generalization](https://arxiv.org/abs/2405.11743)
- [On the Statistical Mechanisms of Distributional Compositional Generalization](https://proceedings.mlr.press/v267/fu25j.html)
- [Make Your LLM Fully Utilize the Context](https://arxiv.org/abs/2404.16811)
- [Shaping Schema via Language Representation as the Next Frontier for LLM Intelligence Expanding](https://arxiv.org/abs/2605.09271)
- [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/)
- [Belief–reality separation lives in routing over a shared value slot in language models](https://arxiv.org/abs/2607.11945)
- [One mechanism for many mental spaces: a shared router over a value slot in language models](https://arxiv.org/abs/2607.10248)

## 15. 当前状态

```text
mother object: Authority Compositional Generalization (research candidate)
novelty: conceptually differentiated, empirical novelty not yet validated
existing evidence: supports static motivation and boundary, not ACG
next artifact: cross-content/cross-encoding/unseen-topology ACG pilot
status: argument_shaped
```
