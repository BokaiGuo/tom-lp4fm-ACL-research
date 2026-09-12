# 下一阶段研究方案更新：从 Identification 到 Commitment

英文对应稿：[Updated Next-Stage Research Program: Identification to Commitment](next_stage_relation_preservation_20260909.md)。

本项目目前的 canonical Best-Paper 方案见：[Conflict Identifies, Priors Decide](best_paper_relation_to_commitment_20260911_zh.md)。本文件保留项目级总图；canonical
论文把原“当冲突成为监督”的关系识别、ACG 的关系执行，以及 relation-to-commitment
边界合并为一条三实验论证，不再把它们拆成两个互相竞争的论文标题。

更新日期：2026-09-09

进一步的最近邻核对表明，宽泛 ACG 和单独的“关系—承诺断裂”都不是最终标题。当前
最强对象是：**demonstrations 何时识别 governing relation，以及已识别的 relation
能否在先验压力下控制最终答案和协调行动。** 详细协议见
[Conflict Identifies, Priors Decide](best_paper_relation_to_commitment_20260911_zh.md)。

本方案吸收并核对了五篇 ACL 2026 论文、`Shaping Schema via Language Representation`
以及 ICL、组合泛化、ToM 表征、记忆、激活干预和基准有效性方面的近邻工作。这里的
“核对”以 ACL Anthology、arXiv 和正式论文 PDF 为准，不以二手中文简介或论文奖项叙事
为证据。

## 0. 先给结论

这五篇论文不应该被拼成一个“记忆 + 注意力 + 因果 steering + ToM”的大而宽的故事。
它们真正提供的是**同一个研究张力在不同层级上的切片**：

```text
任务/语义关系是否被正确规定
-> 关系是否被表征
-> 关系是否在上下文和有限资源中被保留、访问
-> 冲突来源中谁获得答案权
-> 最终答案或第一次行动是否兑现该关系
```

下一阶段应把研究计划更新为三层：

1. **项目层母对象（更高层视角）**：关系从上下文到最终决策与行动的保持。它解释为什么
   “内部可读”“上下文适应”“行为变化”和“因果 steering”不能互相替代。
2. **canonical 下一篇论文**：用 `identification -> commitment -> functional adaptation`
   三个阶段检验 demonstrations 的信息量、先验竞争和行动后果；Conflict Is Supervision
   与 ACG 分别成为第一、第三实验的内部组件。
3. **条件性后续分支**：只有主行为规律成立后，才展开 checkpoint/training、resource/
   topology、causal mechanism 或外部语义域。

因此，ACG 不应被删除，也不应扩张成一个包揽五篇论文的总名词。它是 canonical 论文
中的执行与迁移组件；“关系保持”是研究计划级的解释框架，而不是现在就宣称已被验证
的统一神经机制。

当前状态：`argument_shaped`。新文献提高了研究对象的清晰度，但没有替代新的
cross-content、cross-encoding、unseen-topology 和独立语义有效性证据。

## 1. 这次更新改变了什么

### 1.1 从“schema 是否存在”改成“什么关系被带到答案中”

旧版 ACG 方案的核心问题是：demonstrations 是否诱导一个 source-authority schema。
这个问题仍然成立，但“schema”过于宽泛，容易与 schema retrieval、task vector、
语言表示设计和显式提示脚手架混为一谈。

更新后的问题是：

> 给定一个被明确定义的关系，模型能否把关系的身份和方向从上下文带到新的
> 内容、竞争来源和最终决策中？如果不能，断裂发生在任务规定、表征、访问、仲裁
> 还是执行？

这不是把研究降格为“寻找一个可证伪命题”。真正的贡献可以是**改变测量单位**：
不再把 accuracy、probe score、judge score 或 steering gain 当成同一个“理解能力”，
而是把它们放在关系传递链的不同位置。区分性实验只是为了让这些解释不被混写。

### 1.2 从单一 ToM 机制改成跨层级的功能关系

五篇新论文的对象并不相同：事件体义、注意力表达力、工作记忆编码、ToM steering
和隐性行为适应不能被说成同一个心理过程。可以统一的是它们都涉及：

```text
关系被给出或从上下文中诱导
-> 模型内部形成某种表示或痕迹
-> 关系在竞争、干扰或资源限制下被访问
-> 模型在一个具体输出时刻作出承诺
```

统一的是**功能问题**，不是实体机制。后续论文必须明确哪些结果是跨域功能类比，
哪些结果才是同一关系、同一表示或同一因果通路的证据。

### 1.3 把“基准有效性”提升为第一层实验对象

《The Imperfective Paradox Is Not Necessarily in Large Language Models: A Benchmark
Failure Before a Model Failure》指出，原未完成体基准存在 aspectual reduction、semantic
mis-specification、relation misassignment 和 lexical variation 等问题；在严格 NLI
标准下，原 Group A 中 76% 的实例没有明确排除 culmination，母语者标注还发现 Group A
和 Group C 存在实质解释不确定性。

这对本项目不是旁枝信息。LP4FM 自己已经有类似警告：自动 stimulus invariant 通过，
但 primary changed dimension 只有 `20/120` 一致、commitment relation 只有 `60/120`
一致。因此下一篇不能先把“语义机制失败”写死，再用更多模型补强；必须先证明关系
本身被清楚规定且跨表示保持。

## 2. 五篇论文的正式证据审计

### 2.1 来源与编号纠正

用户提供的 `https://www.alphaxiv.org/abs/2605.09271` 对应的是
`Shaping Schema via Language Representation as the Next Frontier for LLM Intelligence
Expanding`，不是未完成体论文。正式入口如下：

| 论文 | 正式入口 | 可以吸收的事实 | 不能直接声称的内容 |
|---|---|---|---|
| [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/) | [arXiv:2601.09373](https://arxiv.org/abs/2601.09373) | progressive/process 与 completion/result 的推理行为可以分离；提示可能把一种错误换成另一种错误 | 不能把 Teleological Bias 当成已经排除基准错误后的普遍语义缺陷 |
| [Characterizing the Expressivity of Local Attention in Transformers](https://aclanthology.org/2026.acl-long.1739/) | [arXiv:2605.00768](https://arxiv.org/abs/2605.00768) | 局部与全局注意力对应互补的形式计算能力；混合结构可以增加可表达片段 | 不能由形式语言定理直接推出 LLM 获得语义理解或 source arbitration |
| [Memory efficiency and resource-rational encoding in sentence processing](https://aclanthology.org/2026.acl-long.1550/) | ACL 正式 PDF | 编码精度约束可以改变上下文表征的压缩性和类别性，并改善部分人类阅读时数据拟合 | 不能把“更像人类”写成通用理解提升，也不能把噪声注入等同于人类记忆本身 |
| [CoSToM: Causal-oriented Steering for Intrinsic Theory-of-Mind Alignment in Large Language Models](https://aclanthology.org/2026.acl-long.421/) | ACL 正式页面 | causal tracing + activation steering 能在特定 ToM/对话任务中改变行为 | 不能把可 steering 写成已证明的 intrinsic ToM、同理心或跨域 typed control |
| [ImplicitMemBench: Measuring Unconscious Behavioral Adaptation in Large Language Models](https://aclanthology.org/2026.acl-long.1301/) | [arXiv:2604.08064](https://arxiv.org/abs/2604.08064) | 被动暴露、干扰后首次尝试和程序/启动/条件作用是可操作的行为协议 | 不能由首次行为适应直接证明人类意义上的无意识记忆或长期记忆系统 |

补充的关键近邻是 [The Imperfective Paradox Is Not Necessarily in Large Language
Models](https://arxiv.org/abs/2608.25005)。它不是要被当成最终裁判，而是提醒我们把
“任务语义有效性”和“模型语义能力”分开估计。

### 2.2 逐篇吸收与项目映射

| 新论文 | 它真正研究的层级 | 对 LP4FM/下一篇的直接启发 | 研究设计中的位置 |
|---|---|---|---|
| Imperfective Paradox | compositional event semantics 的中间关系与最终 NLI 决策 | 表征能区分 process/result，不代表最终判断使用了该区分；先做 predicate-level validity | 外部语义域 + validity-screened multi-step 对照 |
| Local Attention | 固定精度 Transformer 的形式表达力与局部/全局算子互补 | 架构决定哪些关系计算可表达，但不等于模型在任务中选择了正确关系 | 小型受控 substrate arm，不进入主语义结论 |
| Memory Efficiency | 资源约束如何改变编码精度与上下文表示 | retrieval/access 和 representation quality 是两个对象；干扰可以被设计成 relation-retention test | interference/resource arm |
| CoSToM | 通过 causal tracing 找位置，再用 activation steering 改变 ToM 行为 | response gain、probe reconstruction 和 typed controller transport 必须分开 | mechanism contrast / matched intervention baseline |
| ImplicitMemBench | 无显式回忆提示时，暴露是否改变首次行为 | 经验痕迹必须在干扰后测，且要把显式回忆、隐性适应和 judge 依赖拆开 | optional third domain / adaptation stress test |
| Shaping Schema | 语言表示设计影响知识激活和组织 | “schema”是宽泛解释，不是本项目的可识别贡献；要落到 source relation | related-work boundary，不作为主方法名 |

## 3. 项目级母对象：关系从上下文到决策的保持

### 3.1 工作定义

本文档暂时使用普通描述 **relation preservation from context to decision**，不把它
包装成已经被领域承认的新术语。它指：

> 在任务关系被清楚规定的条件下，模型是否在内容、表面表示、竞争来源、干扰和
> 输出格式改变后，仍然按照同一关系作出最终答案或第一次行动。

关系可以是不同类型，不能因此把它们当成同一个语义变量：

```text
event relation       = progressive activity entails occurrence, telic process does not necessarily entail result
source relation      = queried belief may override reality/other-agent under a specified topology
behavioral relation  = prior exposure to CS/US changes first response after interference
temporal relation    = a local or global dependency must be composed across positions
```

### 3.2 五个功能环节

```text
Specification  ->  Encoding  ->  Access/Retention  ->  Arbitration  ->  Commitment
       ^                ^               ^                  ^                 ^
   validity        representation   attention/memory    source priority   answer/action
```

- **Specification**：关系、标签和 gold relation 是否被完整、无歧义地规定。
- **Encoding**：内部表征是否区分关系相关的内容、角色、事件状态或规则。
- **Access/Retention**：关系在上下文位置、注意力拓扑、资源噪声和干扰中是否仍可取用。
- **Arbitration**：存在多个竞争来源时，模型把控制权分配给谁。
- **Commitment**：模型在最终 token、最终答案或第一次行动中实际兑现了什么。

`Adaptation` 是贯穿这条链的时间维度：关系痕迹是在一次上下文内形成，还是在
学习--干扰--测试后仍影响首次行为。

这里是功能分解，不是对 Transformer 内部模块的承诺。一个 hidden vector、attention
head 或 steering direction 不能自动对应上面某一环节。

### 3.3 一个可用但不承诺真实模块的形式化写法

令：

```text
w        = latent world / task instance
r        = intended typed relation
D        = demonstrations, description, or prior exposure
T        = relation/conflict topology
b        = attention or resource condition
k        = model checkpoint
g        = content, representation, or interference transformation
```

用功能记号表示：

```text
E_k(w, D; b)       = relation-bearing representation
A_k(E, T; b)       = accessible/retained relation information
H_k(A, D, T)       = source or relation arbitration policy
Y_k               = Dec_k(H_k, A, output_format)
```

关系保持不是要求每个内部量可直接观测，而是要求：

```text
R_k(w, D, T, b) = relation-consistent(Y_k)
```

在 `g(w,D,T)` 后，若 `r` 保持不变，`R_k` 应保持；若它改变，必须说明是关系改变、
表示变化、资源变化、竞争拓扑变化还是决策先验变化。

对 ACG，关系特化为：

```text
R(a, b | T) = 在冲突拓扑 T 下，source a 是否有资格压过 source b
```

ACG 的新意不在于给“schema”换名字，而在于要求 demonstrations 诱导的关系能离开
原始 item：

```text
content transfer       = 新事实、新实体、新 latent world
representation transfer = 新构式、新标签、新答案编码
topology composition    = 新的联合 source-conflict graph
```

### 3.4 术语地基账本

| 候选词 | 本项目想表达的操作意义 | 最近的已有术语 | 关系 | 决定 | 首次使用方式 |
|---|---|---|---|---|---|
| relation preservation | 变换后仍按同一 typed relation 作答/行动 | structural consistency, systematicity, relational generalization | neighboring | descriptive_plain_language | “we measure whether the relation is preserved from context to decision” |
| relation execution | 对已表示关系完成最终推理 | compositional reasoning, implicit-relation inference, task execution | neighboring | descriptive_plain_language | 只作为过程描述，不宣称新领域术语 |
| source authority | 冲突中哪个来源获得答案资格 | instruction hierarchy, source attribution, belief tracking | narrower | established_with_qualifier | “typed source authority, defined as ...” |
| schema | 组织和激活知识的宽泛框架 | schema learning, task representation, schema retrieval | broader | established_with_qualifier | 只作为解释假设，不作为独占 novelty |
| answer commitment | 最终答案/第一次行动中的行为承诺 | discourse/speaker commitment, final decision | neighboring | descriptive_plain_language | 首次定义为“the source or relation enacted by the final decision” |
| implicit memory | 认知科学中的非陈述性记忆构念 | procedural learning, priming, conditioning, implicit ICL | broader | established_with_qualifier | 写“behavioral adaptation after passive exposure”更稳妥 |
| causal steering | 激活干预改变输出 | activation steering, causal tracing, representation intervention | same for method | established_reuse | 只用于方法，不等于机制已识别 |
| attention expressivity | 注意力结构可表达的形式计算类 | formal-language expressivity, temporal logic | same for theory | established_reuse | 限定在形式模型，不迁移成语义证据 |

## 4. 推荐的论文分层，而不是一篇“大一统论文”

### 4.1 历史组件：识别与执行的素材层

旧工作标题：

**What Does Context Teach? Composing Source Relations Beyond Task Mapping**

中文：**上下文究竟教会了什么：超越任务映射的来源关系组合**。

这是旧版把 ACG 单独作为论文对象时的稳妥但较窄的方案。它现在保留为
canonical 论文的素材层和降级发表路径，不再与主线并列：

> demonstrations 是否诱导了一个可迁移、可组合的 typed source relation，而不是
> 仅仅提高相似样例检索、改变答案标签偏好或复制表面格式？

它的实验素材仍然有用，但不再是当前最高层标题。建议主域为：

1. **ToM source arbitration**：继承 LP4FM 的 latent world、queried belief、reality、
   other-agent 和 distractor；
2. **event-semantic relation**：使用未完成体问题作为第二个关系域，但只使用
   validity-screened、lexically matched、multi-step 的子集；
3. **隐性适应**先作为 stress test 或补充域，不在第一版主文中与前两域平分篇幅。

原因是：前两个域都能给出明确的 relation label 和 held-out composition；ImplicitMemBench
的 judge 依赖、心理学构念映射和任务异质性更适合做外部边界，而不是核心定义。

若只发表这一窄版本，主结论应是：

```text
ICL can induce a typed relation over competing sources,
and that relation can be re-instantiated on unseen content and topology
under a bounded set of representation changes.
```

它不应写成：

```text
LLMs acquire human-like schemas / intrinsic ToM / a universal reasoning circuit.
```

### 4.2 Canonical 下一篇论文：Identification to Commitment

外部意见吸收后，不再把它作为“论文 C”或等待跨域后才启动的远期项目。当前首选
标题是：

**Conflict Identifies, Priors Decide: Identification to Commitment in Language Models**

中文：**冲突识别关系，先验决定承诺：语言模型中的 Identification-to-Commitment**。

它把三个已有素材压缩成一个主文论证：

```text
Experiment 1  policy-separation determines teaching information
Experiment 2  relation identification and final commitment can dissociate
Experiment 3  partner-policy inference must change coordination action
```

论文的贡献不是把五篇论文并列引用，而是提出一个新的测量分层：

```text
benchmark validity != representation readability
representation readability != relation accessibility
relation accessibility != source arbitration
source arbitration != final commitment
```

这条线真正改变的是社区如何解释“模型理解了但答错”“steering 后变好了”“干扰后
仍然会做对”这些互相冲突的结果。它的主证据不再以 event domain 的跨域复现为必要
条件；更关键的是 policy-separation、prior-opposed readout/final 分离和 partner
policy swap。若功能性实验失败，就收紧为 relation/commitment boundary，不伪装成
社会适应理论。

### 4.3 机制和训练论文作为后续分支

```text
canonical paper: context -> policy identification -> commitment -> partner adaptation
earlier ACG paper: context -> typed source relation -> unseen composition
机制分支: resource/topology/activation 如何改变 relation retention 或 transport
训练分支: 能否选择性修复 wrong-source capture 而不抹平合法语言差异
```

这样既保留颠覆性上限，也不让一篇论文承担互不等价的心理学、形式语言和工程命题。

## 5. Canonical 论文的核心研究问题和预测（对应三项主实验）

### RQ1：示例诱导的是关系，还是标签/相似度？

对固定 latent world 和固定答案格式，改变 demonstrations 的：

- structural similarity；
- diversity；
- complexity；
- source-policy coverage；
- answer-label mapping 和 source-token 位置。

比较四种解释：

```text
label mapping       = 只学会答案 token 与标签的对应
surface retrieval   = 只偏好词面或结构相似的示例
content updating    = 更新当前事实，但不改变 source priority
relation induction  = 学到 source relation，并在新内容中执行
```

只有最后一种解释同时通过 cross-content 和 cross-encoding，才把结果写成 relation
induction。

### RQ2：关系是否脱离原 item 迁移？

把诱导关系 `Pi_A` 与新的内容 `w_B` factorial crossing：

```text
D_A -> induces Pi_A
w_B -> supplies new facts and entities
T_B -> supplies a new conflict topology
```

主要看新 item 是否使用 `w_B` 的事实，同时遵循 `Pi_A` 的 source priority。same-item
response reconstruction 只属于局部输出相似性，不计为 transfer。

### RQ3：primitive relation 能否组合到未见 topology？

demonstrations 只出现 primitive edges，例如：

```text
belief > reality
rule > report
```

测试中出现从未展示过的 binary、ternary 和 cross-family joint topology。比较：

- additive update；
- scalar saturation；
- weakest-link；
- topology-conditioned graph rewrite；
- context-dependent interaction；
- surface-only/no-schema baseline。

不提前把结果命名为 min、tropical 或 universal algebra。二元结果无法区分 weakest-link
和 average/smooth saturation，三元和 cross-family 设计才承担区分责任。

### RQ4：关系失败是 coverage、composition 还是 compatibility？

三类 supporting distribution 必须分开：

```text
coverage-deficient
  必要 primitive relation 没有出现在 demonstrations

composition-heldout
  所有 primitive 都出现，但联合 topology 从未出现

compatibility-shift
  数据量和 primitive coverage 匹配，但关系的组合方式改变
```

这直接吸收 Fu 与 Zheng 的 DCG 框架，但把抽象 composition rule 落到 source relation
上。否则任何失败都可能被错误地叫作“上下文不够”。

## 6. 实验总架构

下面的 Study 0–6 是支撑协议、边界实验和条件性后续的历史编号；canonical 主文只
保留第 12 节定义的三个主实验。保留这些编号是为了追踪既有设计和证据来源，不代表
它们是七项并列贡献。

### Study 0：关系规定与语义有效性

这是必做层，不是附录清洗。

#### ToM 侧

- 对每个 latent world 明确列出 content state、source ontology、conflict topology、
  intended relation 和候选答案；
- independent reviewer 或 human annotator 检查 role、object、reference、policy
  和 source identity 是否被单独改变；
- 保存 `invariant_factors` 与 `changed_relation`，禁止由生成器自证 semantic purity；
- 对 primary changed dimension 和 commitment relation 分别报告一致率。

#### event-semantic 侧

- 不按孤立动词直接赋予 telic/atelic，按完整 predicate 和上下文标注；
- 使用 lexically matched minimal pairs；
- 明确区分 occurrence、culmination、interruption 和 non-completion；
- 同时输出 intermediate aspectual classification、culmination judgment 和 final NLI；
- 采用 validity-screened subset，并把有争议样本保留为 sensitivity analysis。

#### 核心产物

```text
relation_spec.yaml
validity_annotation.csv
latent_world_manifest.jsonl
relation_ambiguity_report.md
```

没有这一层，后续“模型失败”只能叫 constructed-task behavior。

### Study 1：关系表征与关系执行的分离

对每个域收集三类结果：

1. **representation/readability**：held-out probe、intermediate judgment 或
   relation classification；
2. **access/retention**：位置、干扰、上下文长度和 resource condition 下的 relation
   availability；
3. **commitment/execution**：最终 NLI、typed source argmax 或首次行为。

主要显示不是一个总分，而是一个三段式矩阵：

| relation domain | specification validity | readable | preserved after transformation | final relation enacted |
|---|---:|---:|---:|---:|
| ToM source | independent estimate | probe/readout | cross-content/topology | source argmax |
| event semantics | predicate-level audit | intermediate judgment | lexical/context split | NLI label |
| passive adaptation | protocol validity | exposure trace if available | after interference | first attempt |

如果“readable 高、enacted 低”只在一个域出现，就只能写域内 dissociation；不能写统一
定律。

### Study 2：ICL source-relation induction

固定 query 内容，操纵 demonstrations；每个 support regime 至少有：

```text
no-context
label-shuffled
surface-similar / relation-wrong
relation-similar / surface-different
primitive-covered / compound-heldout
direct-compound upper bound
```

记录：

- intended-source capture；
- target-versus-competitor margin；
- authority entropy；
- off-policy leak；
- source identity under answer-label permutation；
- cross-content preservation。

采样单位是独立 latent world 或独立 predicate，不把同一 world 的多行当成独立证据。

对 Best Paper 候选的额外要求是：source-consensus、target-diagnostic 和
rival-policy-diagnostic 必须由同一 generator 生成，并满足 selection-blinded。不能把
一个孤立的 corrupted item 加到若干 consensus items 后就称为
inter-demonstration conflict；真正的冲突条件必须像 `D1+X1+C6` 一样，同时含有支持
目标 policy 和支持 rival policy 的 diagnostic items。

### Study 3：unseen composition 与 topology

至少包含：

- primitive-only support；
- binary held-out compound；
- ternary compound；
- cross-construction compound；
- source-token、label、order 和 position permutation；
- graph-satisfiable 与 graph-ambiguous 两类 topology。

先冻结 primitive estimates，再评估 compound。不能在看过 compound 后选择最合适的
operator，也不能把 post-hoc fitted curve 叫作 composition law。

建议主要 estimands：

```text
cross-content relation preservation
cross-encoding relation preservation
unseen-topology relation accuracy
primitive-to-compound degradation
coverage-adjusted composition gap
```

不把它们压成一个“general intelligence”总分。

### Study 4：干扰、注意力拓扑和资源限制

这个 Study 用来回答“关系为什么在访问阶段断裂”，不是用来证明语义机制。

#### 受控模型 substrate arm

- global-only、local-only、hybrid attention；
- 不同 local window；
- 固定深度与足够深度分开；
- 必须同时有形式语言 control task 和 source/event relation task；
- 报告形式表达力与 relation behavior，不能互换解释。

Local Attention 论文的结论是局部与全局互补、混合结构更丰富；它提供的是计算能力
边界，不是“局部注意力更懂语言”的证据。固定深度、位置编码和小规模自然语言实验
的限制必须写入 scope。

#### resource arm

- 按 Memory Efficiency 的思路操纵 value representation noise / encoding precision；
- 设置短、长、干扰强度和关系跨度；
- 比较内容事实保持与 relation identity 保持；
- 检查压缩表征是否优先保留 semantic gist，却丢失 source distinction 或细粒度
  commitment；
- 报告 retrieval 变化与 representation quality 变化的分离。

该臂的最有信息量结果不是“加噪后准确率下降”，而是：哪一类 relation 在同等内容
accuracy 下先失去最终决策权。

### Study 5：CoSToM 风格的因果桥接

只有 Study 1--3 找到稳定 relation-preservation gap 后才运行。

干预条件：

```text
aligned source/relation intervention
item-shuffled intervention
wrong-channel intervention
norm-matched random intervention
zero intervention
final-token-only intervention
non-final residual intervention
```

主要区分：

```text
same-item response reconstruction
source-specific controllability
cross-content policy transport
cross-topology relation transport
```

CoSToM 的 downstream judge gain 可以作为 baseline，但不能替代 deterministic source
identity 或 human-validated relation outcome。只有 aligned intervention 在新内容和新
topology 中仍选择 intended source，才有资格写 typed controller transport。

### Study 6：训练轨迹与选择性修复

这是高风险、高回报分支，不是论文 B 的必要起点。

#### 训练轨迹

- 同一 Base 的 continued pretraining、SFT、preference/alignment 中间 checkpoint；
- 冻结 relation manifest、primitive estimates、scorer 和 analysis code；
- 跟踪 default source prior、ICL plasticity、cross-content transfer、composition gap、
  item-level geometry；
- Base/Instruct/Qwen3 终点只能做 anchor，不能单独支持 instruction-tuning causality。

#### controlled tuning

- 只改变 reportative/factive、source attribution、voice/event-state 等训练暴露；
- 保持 primitive accuracy、calibration、token frequency 和 logit scale 匹配；
- 测量是否选择性改变 compound relation，而非把所有输出推向一个 source；
- 同时测试合法语言敏感性和 wrong-source suppression。

若能在同一训练轨迹中观察到 relation composition change point，并由 controlled exposure
选择性诱导或消除它，才可把旧 Branch 1 晋级为训练因果论文。

## 7. 五个旧分支的更新定位

| 旧分支 | 更新后的职责 | 必须放弃的过强说法 |
|---|---|---|
| Training-induced composition phase transition | 解释 checkpoint 如何改变 default relation prior、plasticity 和 composition | Base/Instruct 差异自动等于 instruction-tuning causality |
| Behavioral algebra of source accessibility | 在 held-out topology 上比较 relation composition class | 二元 subadditivity 已经等于 min/tropical law |
| Mechanistic identifiability | 规定 specification/readability/access/control/transport 的层级边界 | probe 或 same-item patch 就是 controller |
| Conflict-topology kernel | 测试 source-conflict graph 是否跨表面和域预测行为方向 | 单域 pooled effect 就是 universal neural kernel |
| Source-accessibility training objective | 选择性修复 wrong-source capture，同时保留合法语言差异 | accuracy/consistency 提升就是机制修复 |
| 合并分支 | 形成 `context -> relation -> composition -> checkpoint` 主链 | 把训练、组合和 steering 写成一个不可区分的大机制 |

## 8. 五篇新论文在旧分支中的位置

```text
Imperfective Paradox  -> Branch 3 的 representation/execution boundary + 外部语义域
Local Attention       -> Branch 4 的 topology/substrate condition，不是 semantic proof
Memory Efficiency     -> Branch 1/4 的 resource-conditioned retention hypothesis
CoSToM                -> Branch 3/5 的 intervention comparison，暴露 transport gap
ImplicitMemBench      -> Branch 1 的 exposure/adaptation axis + first-attempt protocol
```

最重要的升级是：旧分支不再围绕“某个机制名称”组织，而围绕关系在链条中的哪一环
断裂组织。

## 9. 与 Related Work 的真正区别

### 9.1 ICL、schema 和 task representation

最近邻包括：

- [How Do In-Context Examples Affect Compositional Generalization?](https://aclanthology.org/2023.acl-long.618/)：研究示例的结构相似性、多样性、复杂度与 coverage；
- [What Do Language Models Learn in Context? The Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/)：讨论任务结构的组合解释；
- [Schema-learning and rebinding as mechanisms of in-context learning](https://arxiv.org/abs/2307.01201)：提出 template/schema、context retrieval 和 token rebinding 的机制假设；
- [Schema for In-Context Learning](https://arxiv.org/abs/2510.13905)：显式抽取和激活较高层 schema；
- [Learning Task Representations from In-Context Learning](https://arxiv.org/abs/2502.05390)：从 attention heads 提取 task vector；
- [Breaking through the learning plateaus of in-context learning](https://proceedings.mlr.press/v235/fu24h.html)：区分 weights component 与 context component。

这些工作回答“示例如何帮助任务学习/识别”。canonical 论文必须继续追问：示例究竟指定了
哪一个 typed relation、该 relation 是否在 source competition 和 unseen topology 中保留，
以及它是否真的改变 partner 的协调行动。task vector、schema retrieval 或 representation
similarity 本身不是 source-authority evidence。

### 9.2 组合泛化与关系推理

最近邻包括 [A General Theory for Compositional Generalization](https://arxiv.org/abs/2405.11743)、
[On the Statistical Mechanisms of Distributional Compositional Generalization](https://proceedings.mlr.press/v267/fu25j.html)、
[Understanding and Patching Compositional Reasoning in LLMs](https://aclanthology.org/2024.findings-acl.576/)、
[Large Language and Reasoning Models are Shallow Disjunctive Reasoners](https://aclanthology.org/2025.acl-long.433/)
和 [GroundCocoa](https://aclanthology.org/2025.naacl-long.420/)。

它们分别研究 generative effect、coverage/compatibility、隐式推理结果和多路径关系
组合。论文 B 的差异必须是 composition unit：不是一般任务函数或路径答案，而是
在 conflict topology 中有方向的 typed source relation。

### 9.3 语义有效性与事件推理

Imperfective 论文及其复核工作共同说明：中间语义标签、NLI relation 和最终标签偏好
必须分开。论文 B 不会把一个有争议的 event benchmark 当作“LLM 深层语义失败”的
直接证据，而是把它作为一个能检验 relation specification、intermediate reasoning
和 final commitment 分离的外部域。

### 9.4 ToM、belief representation 与 source routing

现有工作已经展示：belief 可被 probe，lookback 或 shared router 可能绑定 mental
spaces，ToM benchmark 也容易被 shortcut 污染。LP4FM 的贡献和下一篇的差异是：

```text
已有工作：belief 是否存在、如何表示、某个 intervention 是否改变输出
本项目：在冲突拓扑中哪个 source 获得答案权，以及该关系能否由上下文归纳并迁移
```

这也解释为什么 Stage-57 的 same-item response reconstruction 不能被改写为 typed
controller transport。

### 9.5 记忆、隐性适应与上下文利用

`Lost in the Middle`、context utilization、long-context memory benchmarks 和
ImplicitMemBench 研究位置、存储、检索或首次行为。论文 B 只在实际操纵干扰和首次
决策时引用这些工作；否则不把一般 context length degradation 当成 source relation
failure。最关键的区别是：

```text
explicit recall = 被明确要求回忆
implicit adaptation = 暴露后无需提醒仍改变行为
relation preservation = 改变后仍保留关系身份和方向
```

三者不是同一个指标。

### 9.6 架构表达力、资源编码与 steering

Local Attention 提供形式表达力边界，Memory Efficiency 提供编码质量/资源分配视角，
CoSToM、ReFT 和 activation steering 提供干预工具。它们分别位于 substrate、encoding
和 intervention 层，不能被合并成“发现了一个 semantic circuit”。本项目的主结果仍
应是 relation-level transfer；机制工具只承担解释或边界任务。

## 10. Canonical 论文的主图和主表

### Figure 1：同一关系的五环节

```text
valid relation specification
-> readable representation
-> retained relation under interference
-> source/semantic arbitration
-> final answer or first action
```

图中以 ToM source arbitration 为主实例；event semantics 只作为可选 validity-screened
补充。不要把 local attention、memory noise 和 CoSToM 画成已被证实的共同神经通路。

### Figure 2：Information, not example count

- 横轴：`C1/C2/C4/C8/C16`、matched diagnostic 和 policy-corrupted conditions；
- 纵轴：held-out policy-consistent decision；
- 旁侧显示 `C_sep(D)` 或 `VR(D)`，并比较 count、similarity、diversity 的预测力；
- 所有 content、answer encoding 和 source token 均做独立置换。

### Figure 3：Identification × Prior Pressure 与 partner adaptation

上半部分显示 2×2 的 independent relation readability、native final commitment 和
prior override；下半部分显示 partner-policy swap 前后的协调行动：

```text
induced relation x new content x new representation x partner policy
```

不把 same-item reconstruction 与 cross-content transfer 放在同一柱状图里混合。

### Small consequence：policy-separation selector

- 固定 token budget 下比较 greedy `C_sep/VR`、random、similarity、diversity；
- 若 selector 不具备前瞻性，留在 supplement，不影响主命题。

主图只放预注册 operator family 和 coverage-adjusted comparison，详细 item geometry
放 supplement。

### Table 1：关系层级矩阵

每一行是一个 domain × transformation，列是：

```text
validity -> readability -> retention -> arbitration -> commitment -> transport
```

这张表的用途是显示不同论文/实验测量的是不同环节，而不是制造一个总排名。

## 11. 证据和统计协议

### 11.1 三个数据池

```text
discovery      = 发现 candidate relation pattern
holdout        = 冻结 primitive/operator 后评估未见组合
confirmation   = 新 latent worlds、new predicates 或 independent annotation pool
```

同一池中发现和选择 operator 的结果只能标为 `pilot_only` 或 `exploratory`。

### 11.2 主要对照

- semantic-validity / ambiguity audit；
- lexical and structural matching；
- label and answer-order permutation；
- source-token and position permutation；
- shuffled demonstrations；
- wrong-channel、item-shuffled、norm-matched random、zero intervention；
- direct-compound upper bound；
- independent judge 或 deterministic validator；
- item-clustered bootstrap 或 mixed-effects model，独立 latent world 为 sampling unit。

### 11.3 证据层级

| claim | 最低证据 | 允许写法 | 禁止写法 |
|---|---|---|---|
| benchmark relation valid | independent annotation + ambiguity report | controlled relation stimulus | human cognition benchmark |
| relation readable | held-out probe/intermediate judgment | relation is decodable | model uses relation |
| relation behaviorally preserved | cross-content/encoding/topology | relation-level transfer observed | universal schema |
| source-specific control | aligned vs wrong-channel + specificity + transport | bounded typed control | intrinsic semantic circuit |
| training causality | matched trajectory + controlled exposure | training changes relation behavior under scope | instruction tuning causes the mechanism |
| implicit adaptation | passive exposure + interference + first-attempt + deterministic/independent scoring | post-exposure behavioral adaptation | unconscious memory proven |

### 11.4 不把“可证伪”当成唯一价值

这项研究的高价值部分包括：

1. 重新定义评估单位，从 task accuracy 转为 relation identity and direction；
2. 建立 specification、representation、access、arbitration、commitment 的可分层
   语言；
3. 把负结果变成边界：表示存在但不被执行、输出改变但关系不迁移、记忆存储不等于
   首次行为适应；
4. 由此改变后续 benchmark、steering 和 training objective 应该报告什么。

实验中的区分性预测是保证这些贡献不退化为漂亮叙事的工具，而不是研究价值的全部。

## 12. 最小高信息量 pilot

### 必做版本：canonical 论文 B（已覆盖旧版 ACG 方案）

```text
main experiments = information value + identification/prior pressure + partner adaptation
domains          = ToM source arbitration; event semantics is optional supplement
checkpoints      = Qwen2.5-7B-Instruct + matched Qwen2.5-7B Base
worlds           = 96 independent ToM worlds plus a small partner panel
support regimes  = C1/C2/C4/C8/C16, matched diagnostic, rival-policy diagnostic, oracle
topologies       = binary and ternary held-out; policy swap in partner game
representations  = natural/controlled language + answer/source-label permutation
outputs          = native answer, independent relation readout, action, coordination outcome
```

先跑 validity audit、Experiment 1 的 dose-response 和 Experiment 2 的 2×2 小面板，再
决定是否扩展 partner game 与 topology matrix。不要先扩大模型数、模板数或机制实验。

Experiment 1 的具体对照见 [Conflict Is Supervision](best_paper_conflict_as_supervision_20260909_zh.md)：
`C1/C2/C4/C8/C16`、`D1`、`D1+C7` 和真正的 rival-policy diagnostic pair。不得把
corrupted item 单独加到 consensus items 上再称为 inter-demonstration policy conflict。

### 不再单独使用的旧版“论文 C”条件

旧版曾把跨域 representation-to-commitment framework 写成等待触发的 Paper C。当前
canonical 方案已将其中可验证的 commitment 部分前移到主实验 2，并把功能性部分前移
到 partner adaptation；剩余的跨域统一框架仍然只是条件性后续分支。它不能作为当前
论文的第二标题。

只有同时观察到以下条件，才可在后续论文中讨论更广的跨域框架：

1. canonical 主实验在至少两个模型族或 checkpoint 上复现；
2. partner-policy swap 在新内容上改变 native action 并改善协调；
3. 至少一个独立 relation domain 通过 validity-screened controls；
4. 主要分离不由 label、surface、judge 或 benchmark artifact 解释。

否则保持 canonical 论文的窄行为边界，不把跨域相似性写成统一规律。

### 分支决策

```text
只有 same-item inversion
  -> bounded context effect；不称 ACG transfer

有 cross-content + cross-encoding transfer
  -> ACG relation induction 进入完整实验

primitive 全见但 unseen topology 失败
  -> composition deficit / compatibility boundary；保留为结果

event validity 大量不稳定
  -> 该域转为 benchmark-validity case study，不当作模型失败证据

steering 只重建同一 item
  -> response reconstruction boundary，不写 typed controller transport

被动暴露只改变 style，不改变 relation identity
  -> implicit adaptation 与 semantic relation preservation 分离

同一训练轨迹出现可选择性改变的 composition change point
  -> 启动 training-induced phase-transition branch
```

## 13. 与当前 LP4FM 稿件的定位

当前已投稿 LP4FM 稿件是 **Paper A：静态语言控制界面的发现与边界**：

```text
same latent world
-> linguistic form changes typed source arbitration
-> compound construction effect is checkpoint-sensitive
-> same-item response reconstruction is not typed-controller transport
```

它提供给下一阶段的不是“ACG 已经成立”，而是：

- typed source ontology；
- conflict-topology grammar；
- source-indexed answer interface；
- frozen primitive/compound protocol；
- affine-gain null 和 checkpoint boundary；
- response reconstruction 与 controller transport 的分离；
- 对 benchmark validity 和 construction purity 的现实警告。

下一篇 canonical Paper B 是：

```text
demonstrations
-> policy identification
-> final commitment under prior pressure
-> partner adaptation and unseen topology execution
```

条件性后续才是：

```text
valid specification
-> relation representation
-> relation retention
-> source arbitration
-> final commitment / first action
```

因此 Paper A 与 Paper B 的主结果不能重复：Paper A 的 Stage-53--57 只能作为 motivation、
measurement provenance 和 negative boundary；Paper B 必须使用新的 demonstrations、
cross-content factorial、answer permutation、unseen topology、independent relation
readout 和 partner-policy swap。event-semantic 外部域不再是主文必需条件。

## 14. 五个新论文对 LP4FM 的具体增量

### Imperfective Paradox

它把 LP4FM 的“表征可读但推理被先验改写”推广到另一种关系：模型可以区分过程和结果，
但最终 NLI 决策仍可能接受不充分的 entailment。复核论文则迫使 LP4FM 把 relation validity
和 model failure 分开。吸收方式是 multi-step intermediate output，不是复制
Teleological Bias 这个标题。

### Local Attention

它提供一个严谨的 substrate 视角：不同 attention topology 不是纯粹的效率旋钮，而可能
改变可表达的 temporal operator class。对 LP4FM 的启发是把 conflict topology 和 attention
topology 区分：前者是任务关系，后者是模型计算约束。两者相交时才构成可研究的机制问题。

### Memory Efficiency

它把“记住了什么”拆为 retrieval mechanism 与 encoded representation quality。对 LP4FM
最有价值的实验不是通用 memory score，而是比较 source identity、content fact 和
commitment relation 在同一 resource budget 下的退化次序。

### CoSToM

它提供了从 causal tracing 到 activation steering 的可复用程序，也反向提醒我们：
保持更高的 probe/对话 judge 分数不等于跨内容的 typed source control。LP4FM 的
wrong-channel、item-shuffled、final-token-only 和 transport controls 应成为必要对照。

### ImplicitMemBench

它把“上下文影响行为”放到干扰之后的首次尝试，补上了普通 ICL accuracy 看不到的时间
维度。对下一阶段的启发是测试 relation 是否变成自动化行为；但必须把 style priming、
explicit recall、procedural rule execution 和 source relation preservation 分开。

## 15. 相关工作定位总表

| 文献簇 | 已经回答什么 | 下一篇必须新增什么 |
|---|---|---|
| ICL example selection / CoFe | 哪些示例结构有利于组合泛化 | 示例诱导的具体 relation identity 与 authority direction |
| structured task / schema / task vector | ICL 可能形成 task-level representation | relation 是否跨 content、encoding 和 topology 被执行 |
| Fu--Zheng DCG | coverage、composition 和 algorithm-rule compatibility 如何分开 | 将抽象 rule 实例化为 typed source relation |
| event semantics / NLI | 体义和 entailment 存在细粒度组合边界 | validity-screened relation specification + intermediate/final split |
| ToM/belief probing | belief 可读、可绑定或可被局部干预 | source competition 中的 authority transfer 与 transport |
| long-context/memory | 位置、存储、检索和干扰影响上下文使用 | relation identity 在干扰后是否保留 |
| implicit learning/memory | 暴露可改变首次行为或语言模式 | 行为适应是否保留 typed relation，而非只改变风格 |
| local/global attention theory | 架构拓扑改变形式表达力 | 将 substrate condition 与 relation behavior 分开测量 |
| activation steering/CoSToM | 激活干预可以改变局部行为 | output gain 与 relation-level causal transport 的区分 |
| language representation/schema | 表示设计会改变知识激活和组织 | 一个窄、可计算、可跨内容测试的 source relation |

## 16. 最可能的审稿质疑与回答

### “这只是 prompt sensitivity”

回答不能是“我们把它叫 schema”。必须显示 source/relation identity 在新内容、新标签、
新表面形式和新 topology 中仍保存，并排除位置、词面和答案 token。

### “这只是普通 compositional generalization”

承认其上位关系。差异在于 composition unit 是 conflict-conditioned typed relation，
而不是普通 input-output primitive；如果实验无法显示这个 unit 的跨域预测优势，就
不要声称超越普通 CG。

### “这是 belief probing 重复”

probe 只是 readability。必须报告 source-specific use、wrong-channel specificity 和
cross-content transport；Stage-57 的 same-item reconstruction 不能充当后者。

### “这是 instruction hierarchy”

instruction hierarchy 预设显式 privileged channels。本文研究的是没有固定 privileged
channel 时，demonstrations 如何诱导关系图；如果实现中给了显式优先级，就应改称
hierarchy-following baseline。

### “这是 schema shaping 的应用”

承认 Shaping Schema 已经提出宽泛的 representation/schema 观点。本文只有在定义
`R(a,b|T)`、做 validity control、并在 unseen topology 上预测时，才有窄化的实证分离。

### “这是 steering 方法”

把 steering 放在 mechanism arm。主贡献必须是关系保持和迁移，不是更高的局部 accuracy
或 judge score。

## 17. 预期结果与论文选择

| 观察 | 最合理的论文选择 | 不应做的解释 |
|---|---|---|
| ToM 的识别、承诺分离和 partner swap 均通过 | canonical 论文具备 oral-shaped 证据链 | intrinsic unified neural mechanism |
| 只有 ToM 有 transfer，event 域无 transfer | canonical 论文保留 ToM-specific 边界；event 转补充材料 | 跨语义域普遍 relation law |
| 两域都有 transfer，且 topology 可预测 | 作为后续跨域扩展的动机，不自动新增 Paper C | intrinsic unified neural mechanism |
| 两域都有 readability gap，但 validity/control 不稳 | benchmark/measurement paper | LLM 完全不理解 |
| 只有 passive exposure 改变 style | Implicit adaptation boundary | human-like unconscious memory |
| resource/topology 系统性改变 retention | 独立 substrate/resource paper | attention topology 直接证明语义 |
| controlled SFT 选择性改变 compound | training phase-transition paper | Base/Instruct observational difference 已是因果 |
| steering 改变局部输出但不迁移 | mechanism non-identifiability/boundary result | intrinsic controller discovered |

## 18. 实施顺序与产物

### Phase 1：文献和定义冻结

```text
docs/research/next_stage_relation_preservation_20260909_zh.md
docs/research/related_work_positioning_20260826_zh.md
relation_spec.yaml
validity_annotation.csv
```

### Phase 2：最小 benchmark 与 scorer

```text
relation_manifest.jsonl
relation_execution_runner.py
relation_intermediate_scorer.py
answer_label_permutation_report.md
```

### Phase 3：canonical policy-identification/commitment pilot

```text
policy_identification_results.csv
identification_commitment_2x2.csv
partner_adaptation_results.csv
canonical_claim_matrix.md
```

### Phase 4：可选 substrate/mechanism

```text
resource_retention_audit.md
attention_topology_relation_audit.md
typed_transport_audit.md
```

### Phase 5：可选训练轨迹

```text
checkpoint_relation_trajectory.csv
controlled_exposure_manifest.jsonl
composition_change_point_report.md
```

每个产物都要标记 `implemented`、`validated`、`partial`、`negative`、`proxy` 或
`future-plan`；文件数量不能替代 evidence bucket。

## 19. 最终定位

```text
program-level object
  relation preservation from context to decision

immediate paper-level object
  policy identification -> final commitment -> partner adaptation

LP4FM contribution
  static typed source arbitration and checkpoint-sensitive composition boundary

supporting branches' role
  ACG execution, validity, resource, topology, intervention, and passive-adaptation axes

current status
  argument_shaped; no unified mechanism claim
```

最值得推进的一句话不是“LLM 有没有真正理解”，而是：

> **当一个关系已经被表征或被上下文诱导时，什么决定它能否保留到最终答案；这个
> 保持过程是否受 source topology、资源约束、训练阶段和干扰方式系统性地重写？**

这句话同时保留了颠覆性问题、LP4FM 的独特资产和可执行的研究边界。

## 20. 参考入口

### 新增五篇论文与关键复核

- [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/)
- [The Imperfective Paradox Is Not Necessarily in Large Language Models](https://arxiv.org/abs/2608.25005)
- [Characterizing the Expressivity of Local Attention in Transformers](https://aclanthology.org/2026.acl-long.1739/)
- [Memory efficiency and resource-rational encoding in sentence processing](https://aclanthology.org/2026.acl-long.1550/)
- [CoSToM](https://aclanthology.org/2026.acl-long.421/)
- [ImplicitMemBench](https://aclanthology.org/2026.acl-long.1301/)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)

### ICL、组合泛化与关系执行

- [How Do In-Context Examples Affect Compositional Generalization?](https://aclanthology.org/2023.acl-long.618/)
- [What Do Language Models Learn in Context? The Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/)
- [Schema-learning and rebinding as mechanisms of in-context learning](https://arxiv.org/abs/2307.01201)
- [Schema for In-Context Learning](https://arxiv.org/abs/2510.13905)
- [Learning Task Representations from In-Context Learning](https://arxiv.org/abs/2502.05390)
- [Implicit In-Context Learning: Evidence from Artificial Language Experiments](https://arxiv.org/abs/2503.24190)
- [LLMs can implicitly learn from mistakes in-context](https://arxiv.org/abs/2502.08550)
- [In-Context Learning with Representations](https://arxiv.org/abs/2408.10147)
- [Breaking through the learning plateaus of in-context learning](https://proceedings.mlr.press/v235/fu24h.html)
- [A General Theory for Compositional Generalization](https://arxiv.org/abs/2405.11743)
- [On the Statistical Mechanisms of Distributional Compositional Generalization](https://proceedings.mlr.press/v267/fu25j.html)
- [Understanding and Patching Compositional Reasoning in LLMs](https://aclanthology.org/2024.findings-acl.576/)
- [Large Language and Reasoning Models are Shallow Disjunctive Reasoners](https://aclanthology.org/2025.acl-long.433/)
- [GroundCocoa](https://aclanthology.org/2025.naacl-long.420/)

### ToM、表征与干预

- [Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html)
- [Clever Hans or Neural Theory of Mind?](https://aclanthology.org/2024.eacl-long.138/)
- [Standards for Belief Representations in LLMs](https://arxiv.org/abs/2405.21030)
- [Language Models Use Lookbacks to Track Beliefs](https://arxiv.org/abs/2505.14685)
- [ReFT](https://arxiv.org/abs/2404.03592)
- [Causal Abstraction](https://arxiv.org/abs/2301.04709)
- [Dissociating language and thought in large language models](https://pmc.ncbi.nlm.nih.gov/articles/PMC11416727/)

### 上下文、记忆和架构

- [Lost in the Middle](https://cs.stanford.edu/~nfliu/papers/lost-in-the-middle.arxiv2023.pdf)
- [Make Your LLM Fully Utilize the Context](https://arxiv.org/abs/2404.16811)
- [Disentangling Memory and Reasoning Ability in Large Language Models](https://aclanthology.org/2025.acl-long.84/)
- [Inferring Implicit Relations in Complex Questions with Language Models](https://arxiv.org/abs/2204.13778)

## 当前状态

```text
mother object: relation preservation from context to decision
canonical paper B: policy identification -> commitment -> partner adaptation
supporting components: Conflict Is Supervision + ACG execution/topology audit
novelty: conceptually sharpened; empirical law not validated
main risk: collapse into prompt sensitivity, benchmark ambiguity, or passive QA
next gate: validity audit + ToM dose-response + identification/prior-pressure 2x2
status: argument_shaped
```
