# 下一阶段研究的 Related Work 定位地图

最新的总方案入口是：[从关系到答案承诺：先验改写](best_paper_relation_to_commitment_20260911_zh.md)。本定位地图中的 ICL、schema、ToM 表征、记忆和 steering
近邻，现统一服务于“关系从上下文到最终答案是否保持”这一更窄的比较问题；并不把
这些工作拼成一个共同的内部机制。

## 文档目的

这份文档不是文献清单，而是下一阶段研究的**定位论证**：每一类最近邻工作研究了
什么、与本项目共享什么假设、还没有识别什么，以及本项目必须用什么新对象和证据
拉开距离。

当前项目不应声称“此前没有人研究过上下文学习、组合泛化、belief representation
或 schema”。更准确的说法是：已有工作分别研究了这些对象，但尚未把它们统一为
**从 demonstrations 归纳 typed source-relation program，并在未见冲突拓扑中进行
答案权威组合泛化**这一对象。

## 0. 2026-09-09：最近邻检索后的必要收紧

新检索关闭了三个原本可能被误写成 novelty 的空间：

- [AmbiBench](https://arxiv.org/abs/2212.10711) 已研究 demonstrations 不能唯一识别
  intended task；
- [Ye et al. (2025)](https://aclanthology.org/2025.findings-acl.1092/) 已构造一个
  critical disambiguating example，并用它审计 ICL attribution；
- [Jiao et al. (2026)](https://arxiv.org/abs/2603.04464) 已研究一个 corrupted rule
  demonstration 如何造成 inter-example conflict。

因此，本项目不能以“任务会歧义”“一个 aha example 很重要”或“conflicting demos 会
伤害模型”为中心贡献。新的严格分离点是：

```text
within-example source conflict
  candidate source policies predict different answers
  + target-consistent gold label
  = diagnostic relational supervision

across-example policy conflict
  demonstrations support incompatible governing rules
  = corruption, noise, or unresolved normative ambiguity
```

在这一定位下，ACG 是后半段：先证明 demonstrations 已唯一识别 source policy，再测
它能否跨内容、编码和未见 topology 执行。完整论证见
[Best Paper 级候选方案](best_paper_conflict_as_supervision_20260909_zh.md)。

## 1. 最近邻总表

| 文献簇 | 代表工作 | 已有工作的对象 | 本项目的真正分离点 |
|---|---|---|---|
| ICL 示例选择 | [CoFe](https://arxiv.org/abs/2305.04835) | 哪些 similarity、diversity、complexity 和结构覆盖使未见组合更容易 | 不只测 accuracy；判断示例是否诱导了 source-authority relation，而非只改善检索 |
| ICL 内部组成 | [Learning plateaus](https://proceedings.mlr.press/v235/fu24h.html)、[task representations](https://aclanthology.org/2025.findings-acl.345/) | 参数成分、上下文成分或 task vector 如何共同产生 ICL | 将 context component 具体化为对“谁有资格写答案”的 authority update，并测试是否跨内容迁移 |
| 分布式组合泛化 | [Fu–Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html)、[general CG theory](https://arxiv.org/abs/2405.11743) | supporting distribution、数据覆盖、算法与组合规则的兼容性 | 把 composition rule 定义为 typed relation `R(a,b\|T)`，区分 coverage failure、rule-composition failure 和 algorithm incompatibility |
| 长上下文与信息利用 | [Lost in the Middle](https://cs.stanford.edu/~nfliu/papers/lost-in-the-middle.arxiv2023.pdf)、[IN2](https://arxiv.org/abs/2404.16811)、[NoLiMa](https://arxiv.org/abs/2502.05167) | 位置、长度、词面重叠和监督如何影响上下文利用 | 研究上下文改变的是 source authority policy，而不是一般的信息召回率 |
| ToM benchmark 与 shortcut | [Clever Hans](https://aclanthology.org/2024.eacl-long.138/)、[FANToM](https://arxiv.org/abs/2310.15421)、[DynToM](https://aclanthology.org/2025.acl-long.1171/) | ToM 是否稳健、动态、交互化，及 benchmark 是否被浅层线索污染 | 继承 counterfactual/shortcut controls，但母对象不是更大的 ToM benchmark，而是 typed arbitration |
| belief 表征与机制 | [Language Models Represent Beliefs](https://proceedings.mlr.press/v235/zhu24o.html)、[Brittle Minds](https://arxiv.org/abs/2406.17513)、[Lookbacks](https://arxiv.org/abs/2505.14685) | belief 是否可读、如何绑定 agent-object-state、干预能否改善行为 | 把 readability、controllability、specificity、transportability 分开；不把 probe 或同 item patch 当成 authority transfer |
| belief 表征的标准 | [Standards for Belief Representations](https://arxiv.org/abs/2405.21030) | accuracy、coherence、uniformity、use 四项 adequacy 条件 | 将这些条件转成 source-level 识别协议，尤其加入 cross-content relation preservation |
| 语言表示与 schema | [Shaping Schema](https://arxiv.org/abs/2605.09271) | 表示方式如何改变知识激活和组织 | 不使用宽泛的 schema 作为贡献；只研究可操作的 source qualification / authority relation，并要求跨拓扑组合 |
| 指令、数据与知识冲突 | [Instruction Hierarchy](https://arxiv.org/abs/2404.13208)、[Context-faithful Prompting](https://arxiv.org/abs/2303.11315)、[Instruction–Data Separation](https://arxiv.org/abs/2403.06833) | 明示优先级、上下文忠实性、指令与数据是否分离 | 研究没有显式 system hierarchy 时，demonstrations 如何隐式生成 authority graph；不是安全对齐 benchmark |
| activation steering 与因果解释 | [Activation Steering](https://arxiv.org/abs/2410.12877)、[ReFT](https://arxiv.org/abs/2404.03592)、[Causal Abstraction](https://arxiv.org/abs/2301.04709) | 向量是否能改变行为、跨任务迁移、以及如何形式化干预 | 只把 steering 当作诊断工具；主要结果必须证明 relation-level transport，而非 steering gain |
| shared router / mental spaces | [Belief–reality routing](https://arxiv.org/abs/2607.11945)、[One mechanism for many mental spaces](https://arxiv.org/abs/2607.10248) | 一个共享 value slot 和 router 是否支持多种 mental spaces | 这些工作研究模型内部已有的路由机制；本项目研究 ICL 如何从示例中学习并组合“谁应获授权”的关系规则，属于功能归纳层而非同一机制复现 |

## 2. 五个分支分别如何与 Related Work 拉开距离

### Branch 1：Training-Induced Composition Phase Transition

最近邻是 ICL training dynamics、learning trajectory 和 compositional generalization。
[Learning Trajectories are Generalization Indicators](https://arxiv.org/abs/2304.12579)
把训练过程作为 generalization 的分析对象；[Learning plateaus](https://proceedings.mlr.press/v235/fu24h.html)
把 weights component 与 context component 区分开；[Fu–Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html)
进一步指出，组合能力不能与 supporting-data coverage 混为一谈。

本分支不应写成“我们也研究训练动态”或“instruction tuning 让模型变好了”。真正
的区别是：把 `checkpoint` 作为一个**组合规则的索引**，问训练是否改变了 typed
source-access 的 operator class，而不只是改变准确率、logit scale 或 primitive
margin。最近邻工作提供训练动态的分析语言，但没有把变化对象定义为
`source-access composition law`。

因此，必要的对照是同一训练轨迹、冻结 primitive cost、held-out compound、affine
gain 和 controlled exposure；不同模型终点只能是 anchor，不能作为 instruction-tuning
因果证据。

### Branch 2：Behavioral Algebra of Source Accessibility

最近邻是 CoFe、COGS 以及一般 compositional generalization。它们关注已见 primitive
如何组成未见任务或表达；CoFe 特别强调示例选择和结构覆盖。与之相比，本分支的
composition 单位不是词法/任务函数，而是**typed source relation 在 conflict topology
下的行为代价或 margin**。

本分支不能把 `subadditivity` 本身包装成新理论，也不能预先宣布 weakest-link、min
或 tropical law。真正的分离点是：primitive measurements 在 compound evaluation 前
冻结，并在 construction-split、topology-split 的未见组合上比较最小 operator class。
[Fu–Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html) 提供“数据覆盖 vs 组合能力”
的上位诊断框架；本分支要把它具体化为 source topology-conditioned composition。

### Combined Branch：Learning the Algebra of Belief-Source Access

合并分支不是简单把 Branch 1 和 2 拼接。它的最近邻分别是“组合泛化的统计机制”和
“训练轨迹中的泛化变化”，但本分支的母对象是二者交叉处：

```text
训练轨迹 k -> checkpoint-indexed source-relation operator C_k
```

已有工作可以说明组合泛化受数据和算法影响，却没有回答：同一 model trajectory
何时改变的是 source-access composition class。因而主贡献必须是纵向 operator map +
held-out compound prediction + controlled transition，而不是更多 checkpoint 的
横向表格。

### Branch 3：Mechanistic Identifiability of Typed Controllers

最近邻是 probing limitations、amnesic probing、belief representation standards、
activation patching 和 causal abstraction。[Amnesic Probing](https://arxiv.org/abs/2006.00995)
已经明确指出“可读”不等于“对行为有用”；[Standards](https://arxiv.org/abs/2405.21030)
要求 belief-like representation 同时满足 accuracy、coherence、uniformity、use。

本分支的区别不是再找一个更好的 probe 或声称发现 neural circuit，而是把 mechanism
claim 拆成四个可区分对象：readability、controllability、specificity、transportability，
并给出 full-simplex vertex non-identifiability、geometry–causality dissociation 等
一般性反例。当前 Stage-57 的 same-item reconstruction 只能作为边界证据，不能当作
transportability 结果。

### Branch 4：Conflict-Topology Kernel

最近邻包括 contrast sets、behavioral testing、ToM shortcut stress tests，以及近期
shared-router / mental-space 工作。它们分别改变表面形式、测试分布或寻找内部路由；
本分支提出的对象是更窄的**typed conflict topology**：哪个 source 必须击败哪些
competitors，是否存在跨 prompt family 的方向稳定 kernel。

因此，不能声称发现了一个普遍的 linguistic kernel，除非 kernel 能在新 domain 和新
prompt family 上预测 source-routing change，并且优于 surface similarity、位置、
词面 overlap 和 raw accuracy。shared-router 论文是重要的近邻与 novelty risk，但
它们的内部机制结论不等于行为层的 topology kernel。

### Branch 5：Source-Accessibility Training Objective

最近邻是 instruction hierarchy、context faithfulness、paraphrase consistency、
activation steering 和 representation finetuning。它们大多优化“遵循指令”“忠于
上下文”或“保持输出一致”；这些目标可能把合法的 linguistic sensitivity 一并抹平。

本分支的差异是把训练目标写成**calibrated source routing**：在合法 source change
时保持敏感，在 distractor/wrong-channel 下拒绝错误来源，并在 unseen construction
上保持 typed control。训练收益不能只看 accuracy 或 consistency，必须同时报告
source-specific margin、off-policy leak、topology sensitivity 和 cross-content
transfer。它是一个 intervention branch，不是对已有 instruction tuning 的重新命名。

## 3. 当前 LP4FM 稿件与下一阶段的关系

当前 LP4FM 稿件是 Paper A：

```text
language form -> typed source arbitration -> checkpoint-sensitive composition boundary
```

它与 ToM benchmark、belief probing 和 behavioral testing 相邻，但贡献集中在把 answer
correctness 改写为 typed source arbitration，并明确 compound transport 和机制识别
边界。

下一阶段 ACG / Paper B 则是：

```text
demonstrations -> source-relation program -> unseen-content/topology composition
```

Paper B 不应把 Paper A 的 Stage-53–57 重新命名为 ICL 证据。Paper A 的结果只提供
source ontology、topology grammar、measurement substrate 和负边界；Paper B 的主结果
必须来自新 demonstrations、cross-content factorial、answer-encoding permutation、
unseen topology，以及至少一个新 typed domain。

## 4. 审稿人最可能提出的“这不新”与回答方式

| 可能质疑 | 不能使用的回答 | 必须提供的真正分离 |
|---|---|---|
| “这只是 prompt sensitivity” | 换一个 schema 名称 | source relation 跨 content/encoding/topology 保存，并排除 label、position、surface retrieval |
| “这只是普通 compositional generalization” | 强调用了 ToM | composition unit 是 typed authority relation，不是普通 task/output primitive |
| “这是 belief probing 的重复” | 展示 probe accuracy | 证明 relation-level use、specificity 和 transportability，且区分 same-item reconstruction |
| “这是 instruction hierarchy” | 说 source 也有优先级 | 本文没有预设显式 privileged channel，而是研究 demonstrations 如何诱导隐式 relation graph |
| “这是 schema paper 的应用” | 说本项目更具体 | 给出可计算的 `R(a,b\|T)`、supporting-distribution split 和 unseen joint-topology prediction |
| “这是 steering 方法” | 报告更高 accuracy | 把 steering 作为诊断/干预工具，主结果是关系程序是否可迁移和可组合 |

## 5. 引用优先级

### 必须进入主文 Related Work

1. CoFe：ICL 示例结构与 compositional generalization 的直接先例。
2. Fu–Zheng 2025：数据覆盖、组合能力和 algorithm–rule compatibility 的直接理论近邻。
3. Learning plateaus：weights/context 分工的直接先例。
4. 一般 compositional generalization theory：说明不能预设 primitive 独立组合。
5. ToM shortcut / belief representation 工作：说明 typed source 与 mechanism boundary 的问题来源。
6. Shaping Schema：说明宽泛 schema 叙事已有先例，迫使本文收紧对象。

### 按分支进入主文或补充材料

- Branch 3：Amnesic Probing、Standards、Causal Abstraction 进入主文；完整方法谱系放补充。
- Branch 4：contrast sets、FANToM/DynToM 和 shared-router 工作按实验 domain 选择。
- Branch 5：Instruction Hierarchy、Context-faithful Prompting、Activation Steering 进入主文。
- 长上下文工作只有在实际操纵 context length/position 时才应进入主文，不为增加引用而加入。

## 6. 证据与定位边界

```text
已有文献关系 = 已核实的背景、先例、最近邻和对照
当前 LP4FM 证据 = static typed arbitration 与 composition boundary
下一阶段假设 = ACG / relation-program induction
下一阶段主结果 = 尚未验证，状态为 argument_shaped
```

因此，本文档中的“新”“更强”“oral 潜力”都是研究设计层判断，不是已完成的经验
结论。只有当 cross-content、cross-encoding 和 unseen-topology 结果成立后，才能把
ACG 从候选母对象晋级为论文贡献。

## 参考入口

- [How Do In-Context Examples Affect Compositional Generalization?](https://arxiv.org/abs/2305.04835)
- [Breaking through the learning plateaus of in-context learning in Transformer](https://proceedings.mlr.press/v235/fu24h.html)
- [Learning Trajectories are Generalization Indicators](https://arxiv.org/abs/2304.12579)
- [A General Theory for Compositional Generalization](https://arxiv.org/abs/2405.11743)
- [On the Statistical Mechanisms of Distributional Compositional Generalization](https://proceedings.mlr.press/v267/fu25j.html)
- [Make Your LLM Fully Utilize the Context](https://arxiv.org/abs/2404.16811)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
- [Clever Hans or Neural Theory of Mind?](https://aclanthology.org/2024.eacl-long.138/)
- [Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html)
- [Standards for Belief Representations in LLMs](https://arxiv.org/abs/2405.21030)
- [The Instruction Hierarchy](https://arxiv.org/abs/2404.13208)
- [Context-faithful Prompting for Large Language Models](https://arxiv.org/abs/2303.11315)
- [Belief–reality separation lives in routing over a shared value slot](https://arxiv.org/abs/2607.11945)
- [One mechanism for many mental spaces](https://arxiv.org/abs/2607.10248)

## 当前状态

```text
purpose: related-work positioning for five branches and ACG
novelty: relation-level object is conceptually differentiated; empirical novelty not validated
main risk: ACG may collapse to prompt sensitivity or ordinary compositional generalization
required evidence: cross-content + cross-encoding + unseen-topology transfer
status: argument_shaped
```
