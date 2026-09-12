# 从 Typed Belief Binding 到 Identification → Commitment

## 1. 总判断

这个项目不是沿着一个预先确定的机制假设不断补实验，而是经历了五次研究对象升级：

```text
false-belief accuracy / description sufficiency
-> typed belief-source arbitration
-> readable state versus causal answer interface
-> linguistic source accessibility and saturating composition
-> checkpoint-sensitive composition-shape rewiring
-> policy identification -> final commitment -> functional partner adaptation
```

每次升级都来自前一个对象没有通过更严格的 identification gate。项目最有价值的
积累不是某一个连续成立的 hidden-state 机制，而是已经建立了一套逐步排除错误对象、
重新定义可识别对象的研究程序。

当前 LP4FM 主线已经可以形成一篇 claim-safe 论文。若目标是更强的主会 oral，当前
canonical 分叉不是继续扩大模板或模型数量，而是研究：

> 为什么相同架构、不同 checkpoint 会保留控制相对形状，却改变语言构式组合的方向和幅度？

这比“某个模型存在 subadditive saturation”更深，因为它把 demonstrations 的信息量、
prior pressure、functional adaptation 和 behavioral composition 组织到一个可测量的
`identification -> commitment` 链条中；完整方案见 [canonical 研究方案](best_paper_relation_to_commitment_20260911_zh.md)。

## 2. 研究变迁

### 时期 A：对象定义与行为发现（Stage-0–2）

最初的问题接近普通 ToM robustness：描述是否足够、上下文是否过长、模型是否答对。
项目的重要升级是把二元 accuracy 拆成 `queried_belief`、`reality`、`other_agent`、
`object_distractor` 和 `policy_target`。于是研究对象从“是否正确”变成“答案由哪个 typed
source 控制”。Stage-2 的 description-sufficiency 结果支持有限的行为恢复，但不支持
内部 belief state 被恢复。

### 时期 B：机制候选探索与因果晋级失败（Stage-3–6）

Stage-3/4 发现 query-local belief information 可读，但 story/query patching 基本不能
rescue，final-token transfer 却很强。Stage-5 的 typed steering 没有同时通过随机方向、
argmax、erasure、specificity 等 causal gates。Stage-6 扩展了 behavioral arbitration，
却没有修复内部机制缺口。

这一时期真正成立的 insight 是：

> behavior、probe readability 和 causal controllability 是三个不同的可识别层级。

这使早期“description restores an internal belief representation”假设被降级为
“typed behavioral effect plus a probe-positive/intervention-negative boundary”。

### 时期 C：从 hidden belief 转向 answer-interface boundary（Stage-13–25）

Stage-13 表明早期 steering failure 可以由 flip-budget 不足解释。Stage-14 的 targeted
multi-site intervention 看似强，但被 final-token-only 与 wrong-channel controls 吸收。
Stage-15 residualization 后非 final-site 信号消失，Stage-16 将结果明确为 final-readout
shortcut。Stage-17–20 没有定位到稳定 component writer、distributed writer、readout
layer path 或 readout subspace。Stage-23 的 held-out replication 和 Stage-25 的 model-family
functional replication也没有把该机制推广出去。

研究对象因此从“belief representation 在哪里”改成：

> 什么样的 answer-interface intervention 能改变输出，以及这种控制为何窄、脆弱且
> checkpoint-specific？

负结果不再只是失败清单，而形成了一个 identification boundary：可读的 belief signal
不自动构成一个可迁移、非 final-site、typed causal controller。

### 时期 D：answer simplex、checkpoint transport 与结构性不可识别（Stage-32–43）

Stage-32–35 尝试把窄的 Qwen2.5-7B-Instruct positive 解释为共享 answer-simplex geometry。
实验发现 Base/Instruct 的完整 answer-simplex 子空间几乎重合，但 transport 高度方向不对称。
进一步分析证明 full `K-1` simplex projector 对不同 target vertex 本质上相同，因此无法
识别 typed channel。这不是一次经验失败，而是线性对象的结构性 non-identifiability。

Stage-36 用 canonical one-vs-rest contrast 修复 typed identifiability，显示 checkpoint 差异
主要来自 activation trajectory source，而非 readout contrast source。Stage-37 的 stitching
只实现局部、方向和层位依赖的 causal rescue，排除“只是全局 coordinate gauge mismatch”。
Stage-39–42 一度得到 functional metric、typed response operator 和局部 isotropic gain
compression，但 Stage-43 外部 family 结果阻止其升级为可移植机制。

这一时期留下两个强理论资产：

1. competitor-complete full-simplex control 与 typed linear identifiability 不可兼得；
2. checkpoint 差异不能简单等同于 readout-space rotation，关键在 counterfactual trajectory。

### 时期 E：语言作为控制界面（Stage-44–49）

研究随后退出高风险的 hidden-state localization，转向更适合 LP4FM 的行为识别问题：在
latent world 与 answer candidates 不变时，语言形式是否改变 source selection。

Stage-44 运行 order、coreference、evidential wording 等语言压力；Stage-45 因 replay-equivalent
source independence 失败而被降级；Stage-46 用 item-disjoint source 修复独立性；Stage-47
检验语言压力与 typed topology 的 interaction；Stage-48 在 same-item crossed construction
中验证 conflict topology moderation；Stage-49 在四个 realization 中得到 direction-stable
topology kernel，但 gain 对构式敏感。

此时母对象从 prompt robustness 升级为：

> linguistic construction regulates access to a typed belief-bearing source, while conflict topology
> determines when that source must win.

### 时期 F：从构式效应到组合定律（Stage-50–52）

Stage-50 试图定位 reportative/factive evidential scope，局部机制没有验证。Stage-51 的
primitive-cost additive model 在 held-out compounds 上失败，但残差显示稳定的 subadditivity，
由此形成 saturation candidate。Stage-52 在 20 个全新 latent worlds、1,440 个 frozen rows
上验证 Qwen2.5-7B-Instruct 的正 saturation residual。weakest-link 与 average-saturation 都
优于 additive null，但两者不能相互区分，因此只能识别 saturating composition class，
不能声称 unique min/tropical operator。

这一步是项目首次从“观察一种效应”升级为“冻结 primitive 后预测 unseen compounds”。

### 时期 G：跨 checkpoint 的定律破裂（2026-08-05）

冻结迁移到 Qwen3-8B 与 matched Qwen2.5-7B Base 后，pooled control-relative saturation shape
部分保留，但正 directional residual 不再成立。primitive dynamic range 的收缩不足以解释
residual 的更剧烈坍缩，primitive-only affine transport 也被 matched、shuffle、zero-residual
和 wrong-predicate controls 拒绝。

因此当前最深的新对象不是“跨模型复现失败”，而是：

> checkpoint changes rewire the composition law at the answer interface, beyond scalar gain compression.

Instruct、Base 和 Qwen3 之间呈现的是 amplitude、direction 和 itemwise residual geometry 的
共同变化。现有证据支持 training/checkpoint association，不支持 instruction tuning causality。

## 3. 可分叉的 oral 研究

### Branch 1：Training-Induced Composition Phase Transition

**母问题**：instruction tuning 的什么阶段，使构式组合从无方向或负 residual 变成稳定正
subadditive saturation？

**核心 insight**：instruction tuning 不只是提高 accuracy 或扩大 logit margin，而可能改变
语言约束的组合代数。真正对象是 checkpoint trajectory 上的 composition phase transition。

**决定性设计**：

- 使用同一 Base 模型的多个 SFT/DPO/对齐中间 checkpoint，而不是无关模型横向堆叠；
- 全程冻结 Stage-52 manifest、primitive forms、scorer 和 gate；
- 测量 directional residual、control-relative gains、primitive scale 与 itemwise geometry；
- 将 transition 与训练数据中的 reportative/factive、voice、source-attribution coverage 对齐；
- 最强版本加入小规模 controlled SFT，随机化相关构式的训练频率，直接检验 composition law 是否被诱导。

**oral 潜力：最高。** 它把当前单 checkpoint law 和跨 checkpoint failure 合成一个更强机制问题，
也能从 association 推进到 training-time causality。

**晋级门槛**：至少一个训练轨迹中存在可重复的 law emergence/change point；controlled SFT 的
构式暴露能选择性改变 compound residual，且不能由 primitive margin 或 calibration 解释。

### Branch 2：Behavioral Algebra of Source Accessibility

**母问题**：语言约束的组合属于哪一种可泛化代数：additive、max/weakest-link、smooth saturation、
capacity sharing，还是 topology-dependent mixture？

**核心 insight**：prompt effect 不应逐轴汇报，而应视为可组合 intervention。不同构式是对同一
source-access route 的 operators，typed topology 决定 operator 的适用域。

**决定性设计**：

- 从二元 compounds 扩展到三元 compounds，产生不同模型真正分叉的预测；
- 预注册 weakest-link、smooth saturation、capacity-sharing 和 topology-mixture 的 held-out ordering；
- 用跨语言或结构差异更大的 construction family 验证，而不是继续当前四模板的近邻改写；
- 以最少 primitive observations 预测大量 unseen compounds，评价 sample efficiency 和 MDL。

**oral 潜力：高。** 若能识别一个跨 construction family、至少跨两个 checkpoint 成立的 operator
class，会比当前“subadditivity exists”更有理论力度。

**晋级门槛**：独立 family 上预注册模型排序明确胜出；不能只靠当前 discovery family post-hoc
拟合，也不能把所有平滑饱和模型统称为成功。

### Branch 3：Identifiability Limits of Mechanistic ToM Probing

**母问题**：在多候选 typed answer interface 中，哪些 representation/control objects 原理上不可
识别，哪些 canonical relaxation 才能支持 channel-specific causal claims？

**核心 insight**：项目早期的 probe-positive/intervention-negative、full-simplex vertex
non-identifiability、final-token shortcut 和 partial stitching rescue 可以统一为一篇关于机制解释
可识别性的论文，而不是 ToM case study。

**决定性设计**：

- 将 full-simplex non-identifiability 推广到一般 multiclass readout 与 intervention projector；
- 定义 probe readability、output controllability、source specificity、transportability 四层 claim ladder；
- 在多个任务上构造“probe 相同但 causal controller 不同”或“subspace 相同但 trajectory 不同”的反例；
- 比较 one-vs-rest、pairwise local contrast、learned nonlinear local probes 的充分与必要条件。

**oral 潜力：高，但更偏主会方法/解释性。** 优点是理论资产已经存在；风险是必须脱离单一 ToM
benchmark，并用至少两个外部任务证明普适性。

**晋级门槛**：有正式定理、可复现实证反例、以及一套改变社区实践的 evaluation protocol。

### Branch 4：Conflict-Topology Kernels as a General Diagnostic

**母问题**：语言模型的 prompt sensitivity 是否主要由表面 wording 决定，还是由“哪个来源必须击败
哪些 typed competitors”的冲突拓扑决定？

**核心 insight**：Stage-48/49 暗示方向稳定的是 topology kernel，构式主要调节 gain。可将其推广
为一种比 accuracy 更可迁移的 behavioral fingerprint。

**决定性设计**：

- 在 epistemic、temporal、deontic、spatial source-routing 任务中复用同一 typed topology；
- 测试同一模型的 topology kernel 是否跨 domain 保持，而 gain 随语言构式变化；
- 比较 Base、Instruct、reasoning、VL decoder 的 kernel rank、sign pattern 和 gain spectrum；
- 用 kernel 预测未知 prompt family 中的错误迁移，而不是只解释已见矩阵。

**oral 潜力：中高。** 若跨任务预测成立，它会成为 foundation model behavioral diagnostics；若只在
false-belief worlds 中成立，则容易退化为精细 benchmark paper。

**晋级门槛**：至少两个非 ToM domain 的独立预测；kernel 必须比 prompt embedding similarity、长度、
perplexity、原始 accuracy 等 baseline 更好。

### Branch 5：Source-Accessibility Training Objective

**母问题**：能否训练模型在语言扰动下保持正确 source routing，同时不把模型变成只会忽略构式差异？

**核心 insight**：现有工作不仅能诊断错误，还能提供 typed margin、topology interaction 和 compound
saturation residual 作为训练信号。目标不是普通 robustness，而是校准 source-specific control。

**决定性设计**：

- 构造 topology-balanced contrastive objective；
- 分别约束 primitive source margin、compound composition 和 off-policy leak；
- 比较 data augmentation、margin regularization、representation intervention 与 preference tuning；
- 检验修复是否跨未见 construction 和 latent worlds，同时保持语义敏感性。

**oral 潜力：中高，工程风险较大。** 成功会从诊断论文升级为训练原则；失败也可能揭示 robustness 与
linguistic sensitivity 的不可兼得边界。

**晋级门槛**：必须在未见构式上改善 typed routing，并证明不是通过扁平化所有语言效应或输出偏置实现。

## 4. 不建议作为独立 oral 分支的方向

1. **继续堆无关模型的 Stage-52 replication**：只能扩大 portability table，难以解释 law 为什么改变。
2. **继续重试 non-final patching、随机 steering 或全局 shared basis**：现有 final-readout controls 和
   non-identifiability 已经关闭这条低收益路径。
3. **仅增加 paraphrase/template 数量**：Stage-49 已支持有限 cross-realization kernel；更多近邻模板只增加
   coverage，不自动增加 insight。
4. **直接把 saturation 命名为 min/tropical law**：当前 weakest-link 与 average saturation 不可区分，
   强命名会超过证据。
5. **声称 instruction tuning 产生机制**：Base/Instruct matched comparison 仍是 observational association；
   没有训练轨迹或 controlled tuning 就不能写 causality。

## 5. 推荐组合

### 最推荐的主会分叉

将 Branch 1 与 Branch 2 合并：

> **Learning the Algebra of Belief-Source Access: When Instruction Tuning Changes Linguistic Composition Laws**

论文结构可以是：

```text
typed topology makes source control identifiable
-> frozen construction algebra defines additive and saturating hypotheses
-> training checkpoints reveal when the composition class changes
-> controlled tuning manipulates the transition
-> the change is not reducible to primitive accuracy, calibration, or affine readout gain
```

这是最可能显著超过当前 LP4FM 稿件的方向，因为它不再把 cross-checkpoint failure 当 scope boundary，
而是把 failure 本身转化为机制对象。

### 更稳的独立主会分叉

Branch 3 可以独立发展为机制可识别性论文。它复用 Stage-3–43 的大部分负结果与数学资产，不依赖
Stage-52 在更多模型上复现，但必须加入外部任务和正式理论，避免成为项目复盘。

## 6. 最小下一步

1. 冻结当前 Qwen2.5 Base/Instruct 和 Qwen3 的 1,440-row composition benchmark。
2. 搜索本地或公开可获得的同一模型训练中间 checkpoint；优先同架构纵向轨迹，不再横向堆模型。
3. 先跑 3–5 个 checkpoint 的低成本 primitive + compound panel，寻找 residual change point。
4. 若存在 change point，再设计 controlled SFT；若不存在，转向 Branch 3 的 identifiability paper。
5. 当前 LP4FM 稿件保持 checkpoint-sensitive behavioral composition 的窄 claim，不等待新分支完成。

## 7. 2026-08-26：以 ICL 重新组织五个分支

在纳入付靖文、郑南宁关于 in-context learning、compositional generalization、
distributional compositional generalization 与 learning trajectory 的工作后，下一阶段
不再把“训练改变组合算子”作为唯一母对象。五个分支被进一步收束为
**Authority Compositional Generalization**：模型能否从 demonstrations 中归纳 typed
source relations，并在未见内容、表示和冲突拓扑中重新实例化这些关系。

旧的 Branch 1–5 继续分别承担 training trajectory、relation composition、
identifiability、graph topology 和 training objective 的角色；它们不再作为五篇互相
平行的主叙事。完整的新论文方案见
[下一阶段论文研究方案：上下文学习中的答案权威组合泛化](next_stage_icl_schema_rewriting_20260826_zh.md)
与 [English ACG Plan](next_stage_authority_compositional_generalization_20260826.md)。
五个分支与最近邻工作的统一比较、不可声称的新颖性和直接 baseline 见
[Related-Work Positioning](related_work_positioning_20260826.md) 与
[中文定位地图](related_work_positioning_20260826_zh.md)。

## 8. 2026-09-09：从 ACG 再升级为“冲突何时构成监督”

补查 AmbiBench、critical disambiguating-example attribution、demonstration conflict 和
context-faithful prompting 后，一个重要边界被关闭：不能再把“demonstrations 有歧义”
或“一个关键示例消除歧义”本身当成新发现。

当前最强的下一阶段对象变为：

```text
source-consensus correct examples
  -> candidate source policies remain observationally equivalent
  -> behavior follows checkpoint prior

target-consistent source-conflict example
  -> candidate policies predict different answers
  -> label identifies the governing relation

corrupted inter-demonstration conflict
  -> examples support incompatible policies
  -> relation learning is damaged or destabilized
```

这不是第六个平行分支，而是重新组织五个分支的母规律。ACG 负责“policy 已识别之后
能否迁移和组合”；training branch 负责 prior 与 update efficiency；topology/algebra
负责 policy-separation graph；mechanism branch 负责 typed transport claim；training
objective 负责偏好 diagnostic evidence、拒绝 corrupted evidence。

完整中英文方案见：

- [Conflict Identifies, Priors Decide：canonical 中文方案](best_paper_relation_to_commitment_20260911_zh.md)
- [Conflict Identifies, Priors Decide: canonical English plan](best_paper_relation_to_commitment_20260911.md)
- [当冲突成为监督：中文方案](best_paper_conflict_as_supervision_20260909_zh.md)
- [Conflict Is Supervision: English Plan](best_paper_conflict_as_supervision_20260909.md)
- [关系表征到答案承诺：项目级总图](next_stage_relation_preservation_20260909_zh.md)

## 9. 2026-09-11：从“关系—承诺断裂”收紧为 Identification → Commitment

进一步吸收《The Imperfective Paradox in Large Language Models》及其后续有效性讨论后，
研究对象再次收紧。不能把“模型可以读出关系但最终答错”单独当作新颖性，因为
ToM belief probing、context-faithful prompting、CoSToM 和 representation/decision
dissociation 已分别覆盖相邻现象。真正的升级是把两个阶段连成同一条可测量链：

```text
demonstrations
  -> policy identification: 哪个 source relation 被示例真正区分出来
  -> commitment competition: 该 relation 能否抵抗 default prior
  -> functional adaptation: 是否改变新情境中的沟通或行动
```

因此当前 canonical 下一篇论文是：

> **Conflict Identifies, Priors Decide: Identification to Commitment in Language Models**

主文只保留三个实验：

1. `Information, not example count`：比较 source-consensus 与 source-diagnostic 示例，
   用 policy-separation coverage / version-space reduction 解释示例信息量；
2. `Identification × Prior Pressure`：分离 independent relation readout 与 native final
   commitment，并在独立 calibration pool 上冻结 prior pressure；
3. `Functional partner adaptation`：在 held-out world、encoding 和 topology 中做
   partner-policy swap，要求模型改变协调行动。

LP4FM 是 Paper A，提供 typed source ontology、conflict grammar、answer-interface
scorer 和 checkpoint-sensitive boundary；Conflict Is Supervision 负责第一实验的
识别层；ACG 负责跨内容/编码/拓扑执行层。event semantics、checkpoint/training、
resource/attention 和 causal steering 降为 validity-screened supplement 或后续分支。

这一步不是把研究价值缩减为“可证伪的一个效应”，而是把研究单位从单一准确率改成
关系在 `specification -> identification -> readability -> commitment -> action` 链条
上的功能状态。所有新结论仍须标记为 `argument_shaped`，直到独立 world、模型族、
policy swap 和选择盲化证据完成。

完整 canonical 方案见：[Conflict Identifies, Priors Decide](best_paper_relation_to_commitment_20260911_zh.md)
及其 [English version](best_paper_relation_to_commitment_20260911.md)。
