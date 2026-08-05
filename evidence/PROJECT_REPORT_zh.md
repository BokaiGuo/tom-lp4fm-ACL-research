# Pressure-Stable Belief Binding 中文总报告

更新日期：2026-08-04

用途：论文重写、组会汇报、oral-readiness 讨论的 paper-facing 底稿。主文叙事已经避免内部实验编号式表达；内部编号只保留在最后的复现索引中。

## 2026-08-05 跨 checkpoint 结果与 affine-gain 理论边界

冻结的 Qwen3-8B replication 已完成全部 `1,440` 个 rows，manifest、scorer 和
statistical audit 的 SHA256 均与 Qwen2.5-7B-Instruct confirmation 一致；没有修改
prompt、重新估计 primitive cost 或改变 promotion gate。结果是严格的 partial
replication：

- pooled saturation residual 为 `0.250 [-0.095, 0.580]`，directional law 未复制；
- weakest-link 与 average-saturation 的 pooled control-relative gains 仍通过；
- reportative residual 为 `0.391 [-0.032, 0.817]`，factive 为
  `0.108 [-0.397, 0.606]`，两类 predicate gate 都未通过。

因此当前不能写 cross-model saturation law。更准确的 insight 是：

```text
amplitude collapse with partial control-relative shape transfer
```

进一步的理论分解排除了“只是 logits 整体缩小”这一解释。若两个 checkpoint 仅满足
正仿射 readout gain，saturation residual 必须同比缩放，其符号与归一化 residual
应保持。实际 primitive dynamic range 只缩小约 `6.2x`，residual 却缩小
`11.8x–34.9x`；itemwise residual correlation 仅 `0.057–0.197`。使用 primitive
forms 拟合的五折 affine transport 在 reportative 和 factive 上均未通过 matched
mean、shuffled、zero-residual 与 wrong-predicate controls。因此可以写：

```text
cross-checkpoint variation changes the downstream construction-composition
shape and cannot be reduced to scalar readout-gain compression
```

这仍是 behavioral functional boundary，不是 hidden-state circuit localization。

matched Qwen2.5-7B Base/Instruct 对照已经完成全部 `1,440` rows。Base pooled
residual 为 `-0.184 [-0.453, 0.107]`；reportative 为
`-0.126 [-0.467, 0.238]`，factive 为 `-0.241 [-0.638, 0.205]`。两个 directional
predicate gates 均失败，但 pooled weakest-link/average-saturation controls 均通过。
结合 Qwen3，最终判决是：

```text
instruction-tuning-associated amplitude and direction shift
```

即强方向性 saturation law 只在 Qwen2.5-7B-Instruct 上成立；Base 与 Qwen3 保留
partial control-relative shape，但不保留正 residual。该结果排除了“Qwen2.5 架构本身
自动产生该 law”，但只能写与 instruction tuning 相关，不能写 instruction tuning
因果地产生了该机制。模型扩展到此停止。

最新 artifacts：

```text
results/oral_readiness_pack_20260707/cross_checkpoint_saturation_decomposition_20260805/
results/oral_readiness_pack_20260707/affine_gain_null_audit_20260805/
results/oral_readiness_pack_20260707/qwen25_7b_base_independent_saturation_replication_20260805/
```

## 2026-08-04 LP4FM 投稿主线与最后两个 identification gates

当前论文不再以 activation-trajectory localization 或跨 checkpoint transport 为
主贡献。LP4FM-facing 的母问题已经收敛为：

```text
When several linguistic constructions restrict access to the same described
source, do their behavioral costs add independently, or saturate at a shared
belief-binding bottleneck?
```

研究 story 是一条递进推理链，而不是实验目录：

```text
accuracy does not identify the controlling source
-> typed conflict topology reveals source selection
-> linguistic form changes that selection under the same latent world
-> primitive construction costs fail additively on unseen compounds
-> independent latent worlds confirm a source-accessibility saturation class
```

在 Qwen2.5-7B-Instruct 的独立 20-item confirmation 上，reportative 与 factive
compound 的 saturation residual 分别为 `4.609 [2.646, 6.447]` 和
`3.771 [2.274, 5.228]`；pooled residual 为 `4.190 [2.931, 5.401]`。weakest-link
与 average-saturation control 都优于 additive prediction，但二者不能相互区分，
所以论文只识别 `saturating composition class`，不声称 unique min/tropical
operator。

当前只剩两个真正会改变投稿 claim tier 的 identification holes：

1. **Cross-checkpoint portability。** 已冻结原 1,440-row manifest、scorer 与
   statistical gate，并在 Qwen3-8B 上原样复现；不修改 prompt、不重估 primitive
   cost、不改变 gate。后台任务为 `tom-qwen3-independent-saturation.service`。
   全 gate 通过才允许写 tested-checkpoint cross-model saturation；部分通过写
   architecture/training-sensitive boundary；全部失败则标题和 claim 改为单数
   checkpoint scope，并停止继续追加模型。
2. **Linguistic manipulation validity。** 已生成 120-pair 双盲 stimulus audit，
   覆盖 voice、reportative/factive attribution、commitment、event-state、answer
   target、source identity、reference 与 discourse coherence。至少两名独立标注者
   完成并通过 preservation/grammaticality gates 前，只能写 annotation pack ready，
   不能写 linguistic validity validated。

当前最准确 verdict：论文已经是 **single-checkpoint behavioral oral-shaped**；还不是
cross-checkpoint oral-ready，也不是 hidden-state mechanism paper。旧 final-readout、
writer localization 与 checkpoint transport 结果保留为机制和外推边界，不再作为
平行主线。

最新执行与审稿防御 artifacts：

```text
results/oral_readiness_pack_20260707/qwen3_8b_independent_saturation_replication_20260804/
results/oral_readiness_pack_20260707/linguistic_validity_annotation_20260804/
results/oral_readiness_pack_20260707/lp4fm_reviewer_attack_audit_20260804/
paper/lp4fm2026/main.pdf
```

## 2026-07-30 论文主线总重构

最新 canonical research story 已不再按历史实验阶段组织，而是围绕一个母问题：

```text
Which semantic channel causally controlled the answer, and does the evidence
actually identify that channel?
```

论文 identity 已固定为：

```text
an answer-control identifiability paper
+ a theory-derived typed intervention
+ a bounded activation-trajectory rewiring finding
```

三项主贡献为：

1. 形式化 answer-control identifiability，并证明 competitor-complete full-simplex intervention 对 target channel 不可识别；
2. 从 symmetry assumptions 推导 canonical centered one-vs-rest typed contrast；
3. 在六个 named Qwen2.5-7B Base/Instruct conditions 中识别 activation-trajectory-dominated rewiring，并用 scale/checkpoint/compression/decoder boundaries 限定其范围。

Behavioral arbitration、readability-control dissociation、flip budget、stitching 和跨模型负结果不再作为平行贡献，而是分别承担“提出问题、排除替代解释、识别机制、限制外推”的证据职责。

完整叙事、逐节结构、结果验证映射和审稿人异议表位于：

```text
results/oral_readiness_pack_20260707/paper_story_20260730/
```

### 2026-07-30 causal stitching 双向最终结果

双向 aligned、raw 与 shuffled 真实 arms 均已完成。六个高剂量方向×层 rows 全部达到
coverage；旧 mean-level closeness gate 为 `5/6`，但 10,000 次 item-paired bootstrap
严格 causal-rescue gate 仅为 `2/6`。两个通过项都位于 Base-to-Instruct 的 2k 层位；
Instruct-to-Base 为 `0/3`。因此最终 insight 是：

```text
checkpoint geometry is bidirectionally alignable, but causal portability is
direction- and layer-conditioned
```

进一步的 120-row item-level audit 显示，paired stitch 的几何 success 为 `120/120`，
causal success 仅为 `32/120`。Base-to-Instruct 为 `24/60`，Instruct-to-Base 为
`8/60`；六个 named groups 的 exact paired dissociation gates 全部通过。当前可写的
理论边界是：

```text
Euclidean representation alignment does not imply downstream-functional causal
portability, although coordinate alignment can rescue a strict subset.
```

对应 local affine theory 中，causal residual 是 `a_t^T e`，所以正确 metric 应由
`G_t = a_t a_t^T` 定义，而不是普通 cosine。下一机制对象因此转为 downstream
functional covector：检验 `|a_t^T e|` 是否比 Euclidean residual、wrong-channel 与
norm-matched random controls 更能预测真实 causal transport error。

## 0. 当前 Verdict

### 0.1 2026-07-19 最新主线修正：把负结果写成正贡献

最新实验口径不是“跨模型复制失败，所以机制线失败”。更强的 paper-facing 解释是：

```text
The negative rows form a controlled identifiability boundary map.
They show when readable belief cannot be promoted to portable answer control.
```

因此，本研究的正贡献应该从 `broad mechanism transfer` 改写为：

```text
typed answer-interface identifiability boundary
```

审稿人版本：

```text
False-belief accuracy and hidden-state readability do not identify answer
control. The paper contributes an intervention-gated boundary map showing when
typed answer-interface geometry exists, fails to transfer, or collapses under
matched controls.
```

这让小模型和跨锚点负结果成为正证据，而不是失败清单：每个 negative gate 都关闭一个 reviewer attack，包括 fixed-layer portability、wrong-layer rescue、eligible-only rescue、shared low-rank compression、decoder-family generality、以及 second-anchor overgeneralization。当前仍不能写 internal-causal mechanism oral-ready，但可以写 paper-facing mechanism-boundary contribution。

最新可复现 synthesis artifact：

```text
results/oral_readiness_pack_20260707/answer_interface_boundary_contribution_20260719/
results/oral_readiness_pack_20260707/negative_result_positive_contribution_20260719/
```

这项工作现在最强的版本不是“我们做了很多 ToM 实验”，也不是“长上下文让 ToM 下降”。它应该被讲成：

```text
False-belief accuracy is non-identifying.
Under pressure, LLM ToM answers are selected by typed answer-controller
arbitration, not by a single portable belief variable.
```

当前论文形态：

```text
behavioral oral-shaped
+ strong mechanism-boundary paper
+ paper-facing capacity-thresholded answer-interface geometry

not yet:
internal-causal mechanism oral-ready
```

最短 thesis：

```text
False-belief accuracy hides which controller produced the answer. Under role,
object, context, and policy pressure, LLMs arbitrate between belief, reality,
other-agent, object-distractor, and policy answer channels. The queried belief
is readable in hidden states, but tested causal gates show it is not the
portable answer controller. The defensible mechanism object is therefore a
typed answer-interface geometry: stable, held-out replicated, and functionally
layer-stable inside Qwen2.5-7B-Instruct, and reproduced by a same-checkpoint
anchor rerun, but not reproduced in tested smaller/intermediate scales, not
reproduced by Qwen2.5-7B base, Qwen3-8B, DeepSeek-R1-Distill-Qwen-7B, or
Qwen2.5-VL-7B text-decoder anchors, and not compressible into the tested shared
rank-3 controller.
```

审稿人应该记住的一句话：

```text
This paper is not about whether LLMs "have beliefs"; it is about when a readable
belief can win, lose, or be bypassed at the answer interface under typed
counterfactual pressure.
```

## 1. 主线转向

### 1.1 旧主线为什么不够 oral

弱版本是：

```text
LLMs fail false-belief tasks under long-context or pressure.
```

这个版本会被审稿人自然归入 benchmark / prompt sensitivity / instruction following。即使结果很全，也容易被看成实验拼接。

真正要避免的叙事是：

```text
accuracy drops
→ hidden state has belief
→ steering sometimes moves margins
→ many controls
```

这条线的问题是：accuracy、probe readability、margin movement 都不是同一个科学对象。

### 1.2 新主线：从 belief tracing 转向 answer-controller arbitration

新的母对象是：

```text
typed answer-interface arbitration geometry
```

它包含四个层次：

| 层次 | paper-facing 问题 | 当前状态 |
| --- | --- | --- |
| 行为层 | 同一个 false-belief question 到底由哪个 typed controller 产出答案？ | 已有 counterfactual pressure-world evidence |
| 表征层 | 被查询 belief 是否在 hidden state 中可读？ | 可读 |
| 因果层 | 可读 belief 是否是答案的可迁移 causal controller？ | 当前 gates 阻止该 claim |
| 几何层 | 答案接口是否形成 typed answer-channel geometry？ | 7B named rows positive，多个边界 negative |

这就是主线转向：

```text
from: "Does the model represent the false belief?"
to:   "Which typed answer controller wins at the answer interface, and under
      what capacity / control / transfer boundaries?"
```

## 2. 核心 Insight

### Insight A：false-belief accuracy 是混合观测量

一个看似正确或错误的 false-belief answer，可以由不同 controller 产生：

| controller | 含义 |
| --- | --- |
| belief | 被查询角色的 mental-state answer |
| reality | 真实世界位置 |
| other-agent | 另一个角色的 belief / answer |
| object-distractor | 干扰物体绑定的 answer |
| policy | instruction 或 in-context policy 要求的 answer |

所以 accuracy 不是机制变量。它只是 controller competition 的最终观测。

论文第一层贡献应写成：

```text
We turn false-belief evaluation from correctness scoring into typed controller
arbitration under pressure.
```

### Insight B：readable belief 不等于 causal answer controller

反直觉点是：

```text
query-local belief can be readable, while still failing guarded causal rescue
and answer-control tests.
```

这不是一个消极结果；它是论文的机制转折：

```text
The model may encode the queried belief without using it as the decisive answer
controller under pressure.
```

这个 distinction 是区分本工作和普通 probe paper 的关键。

### Insight C：正确机制对象是 answer-interface budget，而不是单一 hidden belief direction

旧机制对象是：

```text
find a belief direction and steer it
```

当前证据不支持这个对象。更好的对象是：

```text
typed controller flip budget at the answer interface
```

即：某个 answer channel 要从输变赢，需要跨过多少 logprob budget。这个对象解释了为什么 hidden-state steering 可以移动 margin，却不能稳定跨过 argmax boundary。

### Insight D：负结果组成的是边界图谱，而不是失败清单

最重要的研究设计升级是：

```text
negative transfer is not a failed replication story;
it is a scale/control boundary map for answer-interface geometry.
```

当前贡献不是 broad transfer，而是：

```text
capacity-thresholded named answer-interface geometry
```

这条主线能把正负结果都统一起来：

```text
7B named geometry exists
→ held-out items preserve it
→ answer-simplex controls sharpen it
→ smaller/intermediate scales do not reproduce it under repaired gates
→ within-7B it does not compress into one shared low-rank controller
→ a high-capacity VL text-decoder fallback also does not reproduce it
```

## 3. 主要证据链

### 3.1 Typed pressure worlds expose controller arbitration

设计逻辑：

```text
same underlying ToM item
+ typed pressure operators
+ typed candidate answers
→ identify which controller wins
```

当前结果支持：false-belief accuracy 会隐藏 policy capture、semantic retention、reality substitution、other-agent substitution 和 object-distractor substitution 的差异。

允许写：

```text
Counterfactual typed pressure worlds expose answer-controller arbitration that
is invisible to raw false-belief accuracy.
```

不能写：

```text
Counterfactual world flips prove an internal causal belief controller.
```

### 3.2 Hidden belief is readable, but causal control does not follow

关键证据：

| 证据面 | 结果含义 |
| --- | --- |
| query-local probe positive | queried belief is readable |
| description/random controls near null | readability is site-specific rather than a trivial surface artifact |
| story/query patching and steering fail strict control | readable site is not a portable answer controller |
| final-state random calibration blocks selective belief-control language | margin movement is not enough |

允许写：

```text
Readable belief states and answer-controlling states dissociate under pressure.
```

不能写：

```text
The query-binding direction is the causal answer controller.
```

### 3.3 Answer-interface budget explains why steering fails

机制转向后的证据链：

| 观察 | 解释 |
| --- | --- |
| steering can move margins | hidden state contains answer-relevant directions |
| argmax often does not flip | movement does not cross typed answer-channel budget |
| flip-budget audit explains failed cells | the right unit is budget crossing, not raw vector effect |

允许写：

```text
Typed flip budgets provide a sharper answer-interface mechanism object than
single-direction belief steering.
```

### 3.4 Multi-site interventions collapse to a final-readout shortcut

这个部分不要写成“multi-site mediation 成功”。真正贡献是：

```text
raw multi-site effects are accounted for by a direct final-readout shortcut
under matched controls.
```

证据链：

| 设计 | 结果解释 |
| --- | --- |
| multi-site policy/final intervention | raw effect can be large |
| final-token-only decomposition | direct final readout reproduces the effect |
| no-final residualized multi-site | non-final policy/query contribution collapses |
| cross-cell generalization | final-readout shortcut is a stable mechanism boundary |
| component / bundle localization | no tested single branch or branch bundle localizes the writer |

允许写：

```text
The current causal evidence supports a bounded final-readout shortcut boundary,
not policy-channel mediation.
```

### 3.5 Named 7B answer-interface geometry is real but narrow

7B positive evidence has three layers：

| check | result |
| --- | --- |
| item-stability | positive rows are not single-item artifacts |
| held-out item replication | same-model new items preserve the named rows |
| typed answer-simplex controls | effect is not only a 1D target-vs-current competitor artifact |
| same-checkpoint functional layer sweep | 3/3 named rows pass split-half functional selection/evaluation gates |
| same-checkpoint anchor rerun | 3/3 named rows pass the all-model high-capacity anchor gate |

允许写：

```text
Qwen2.5-7B-Instruct contains named, item-stable, held-out-replicated,
same-checkpoint functionally stable, and same-checkpoint anchor-rerun stable
typed answer-interface geometry rows under matched controls.
```

不能写：

```text
This proves broad internal belief control or model-family transfer.
```

### 3.6 Scale and anchor boundaries are now part of the result

### 3.6a 2026-07-29 control semantics re-audit and checkpoint transport pivot

Stage-20 的 control-semantics 审计发现，旧 wrong-channel readout projection 没有
真正使用 `control_source_channel`。该问题现已通过 corrected 120-row real re-audit
解决：复用 360 条可信 target/native、orthogonal、random rows，并只重算 120 条
true wrong-channel basis rows。三个 named cell-layer rows 全部重新通过 coverage、
budget、argmax、specificity 和 leak gates，因此 `3/3 controlled positive` 已恢复，
不再只是 legacy-control positive。

| named row | target budget | true wrong budget | target-control gap | target/wrong argmax | gate |
| --- | ---: | ---: | ---: | ---: | ---: |
| 1024 tokens, layer -2 | 3.0116 | -1.8567 | 0.8686 | 0.825 / 0.075 | pass |
| 2048 tokens, layer -2 | 4.3434 | 1.5580 | 1.0579 | 0.775 / 0.275 | pass |
| 2048 tokens, layer -1 | 4.6021 | 1.9442 | 2.6580 | 0.925 / 0.325 | pass |

Stage-34 和 Stage-35 真实双向实验现已完成，并迫使机制对象再次升级。

Stage-34 的结果是高度方向不对称：Base basis transport 到 Instruct 时 3/3
transport/native rows 通过；Instruct basis transport 到 Base 时 0/3 通过。但两个
checkpoint 的 full answer-simplex subspace 几乎重合，mean subspace cosine 为
`0.999673`，最大 principal angle 仅 `1.7185°`。因此不能把方向不对称解释成
readout subspace 被 instruction tuning 大幅旋转。

Stage-35 进一步交换 activation-delta source 与 readout-basis source，共完成 1440
条真实结果和 6/6 coverage。结果是：

- basis-source main effect 在六个 named direction-row 上均接近零；
- delta×basis interaction 也接近零；
- Instruct delta -> Base 在三个 rows 上提高 budget/argmax；
- Base delta -> Instruct 在三个 rows 上降低 budget/argmax；
- external target basis 与 external wrong-channel basis 在固定同一 delta 时产生相同
  argmax，平均 budget 差仅 `0.000403`。

这里暴露了一个定义级 identifiability boundary：对 K 个候选 answer-token vectors，
从任一指定 vertex 到其余 K-1 个 vertices 的差向量，都张成同一个 K-1 维 affine
simplex tangent space。因此 full answer-simplex projector 可以识别“候选答案输出
空间”，却不能识别“哪个 typed channel 是 causal”。240 个 geometry pairs 的最小
principal cosine 为 `0.999962`，240 个固定 delta 的 target/wrong effect pairs 中
argmax 差为零。

所以新的母问题是：

```text
Instruction tuning leaves the candidate-output simplex nearly invariant but
rewires the counterfactual activation trajectory through it. Can a genuinely
typed one-vs-rest contrast, under same-delta wrong/random controls and dose
response, identify which channel carries that asymmetric trajectory?
```

Stage-36 已将 full-simplex projector 替换为一维 one-vs-rest typed contrast，使用
相同 activation delta 比较 target/wrong/random directions，并加入 `alpha=1,2`。
双向真实实验已于 2026-07-30 完成，共 `2400` 行、`12/12` coverage，六个 named
direction-layer rows 的高剂量 gate 全部 promotion。所有六个 promoted rows 的 dominant
factorial mechanism class 都是 `activation_trajectory_rewiring`：delta-source target-logit
main effect 显著大于 contrast-source main effect 和 delta×contrast interaction。

进一步的 20-item paired bootstrap 验证表明：

- `6/6` named rows 的 trajectory-dominance 95% 下界严格大于零；
- `24/24` target-logit dose tests 通过；
- `23/24` target-versus-wrong/random paired specificity tests 通过；
- 唯一未过 test 是 Instruct -> Base、2048、layer -1 的 native-delta/external-contrast
  argmax 下界恰为 `0`，但该 named row 的其他 contrast arms、budget、dose 和 trajectory
  dominance 均通过。

因此当前可写的机制结论已经升级为：在六个 named Qwen2.5-7B Base/Instruct rows 上，
canonical typed answer contrasts 选择性控制目标 channel，而 checkpoint 差异主要由
activation trajectory source 决定，不是由 readout contrast source 或二者 coadaptation
决定。它仍然不是 broad internal belief controller 或跨模型家族机制。

### 3.6b 2026-07-30 mathematical identification and trajectory theory

新的理论对象不再是泛泛的“checkpoint difference”，而是两个可证明命题和一个可检验
动力学分解。

第一，若目标 channel `c` 的线性子空间包含从 `w_c` 到所有其他候选读出向量的差，
则该子空间必然等于所有 pairwise differences 的全局 contrast span。因此所有 target
vertices 的 full `K-1` simplex projector 完全相同。换言之，competitor completeness
与 linear channel identifiability 不可兼得。这解释了 Stage-36 的 240 对 target/wrong
projectors 和固定-delta effects 为什么等效；它不是偶然 negative，而是结构性
non-identifiability。

第二，在“线性、全局平移不变、对非目标 labels 置换不变”的自然条件下，唯一的非零
typed rank-one direction，差一个尺度，是

```text
q_c = w_c - mean_{j != c}(w_j).
```

因此正在运行的 one-vs-rest contrast 不是 ad hoc repair，而是满足这些对称性条件的
canonical identifiable relaxation。

第三，令 `r = delta_instruct - delta_base`。局部线性响应下，Instruct delta transport
到 Base 测得 `L_B(r)`，Base delta transport 到 Instruct 测得 `-L_I(r)`。现有六个
Stage-35 main effects 重写后，budget 与 argmax 的符号一致均为 `3/3`；两个 checkpoint
的 budget gain vector cosine 为 `0.936506`，argmax gain vector cosine 为
`0.985932`。在 240 个真实 item-level factorial rows 上，6/6 个方向×层组合的共同
trajectory gain 95% bootstrap 区间均严格大于零。Stage-36 的 typed contrast 结果又
进一步验证：六个高剂量 rows 全部 promotion，且 `6/6` trajectory-dominance bootstrap
下界为正。这把 first-order antisymmetric transport 从解释候选推进为 named
answer-channel mechanism，但尚未唯一排除 hidden-coordinate gauge mismatch。

为区分 coordinate gauge 与真正 downstream rewiring，Stage-37 已使用未进入 causal
evaluation 的 20 个 items 拟合 paired low-rank linear stitch，并在另外 20 个 held-out
items 上验证。正确配对 stitch 在 6/6 个方向×层组合中都显著优于 shuffled-pair stitch：
raw cosine 约 `0.566-0.580`，aligned cosine 提升到 `0.834-0.893`，且 paired bootstrap
specificity gates 全部通过。双向 causal discriminator 随后完成：120/120 held-out
item rows 几何对齐成功，但 causal success 仅 32/120，六个 exact dissociation gates
全部通过；严格 item-paired bootstrap rescue 为 2/6，且只出现在 Base-to-Instruct 的
两个 2k rows。因此结果排除全局 coordinate-gauge 解释，但保留方向与层位条件下的局部
coordinate rescue。

完整定理、证明、有限样本审计、竞争理论与参考文献见：

`results/oral_readiness_pack_20260707/stage37_mathematical_transport_theory_20260730/answer_interface_geometry_theory_zh.md`

当前边界证据：

| tested boundary | result | claim meaning |
| --- | --- | --- |
| 1.5B fixed-layer transfer | negative | not direct anatomical transfer |
| 1.5B functional layer sweep | negative | not just wrong layer index |
| 1.5B eligible-only rescue | negative | not explained by no flippable budget |
| 3B functional scale test | negative | intermediate scale does not rescue |
| 3B eligible-only rescue | negative | not explained by eligibility |
| 7B shared rank-3 compression | negative | not one global low-rank controller |
| DeepSeek-R1-Distill-Qwen-7B | negative | clean text-only high-capacity anchor boundary |
| Qwen3-8B | negative | high-capacity Qwen-family specificity/leak boundary |
| Qwen2.5-7B base | negative | architecture alone does not explain the Instruct positive |
| Qwen2.5-VL-7B text decoder | negative | high-capacity VL decoder-family boundary |

最新 high-capacity anchor 状态：

```text
DeepSeek-R1-Distill-Qwen-7B clean text-only anchor:
  artifact: complete local weights
  missing shard repaired: model-00001-of-000002.safetensors = 8606596466 bytes
  real rows: 480
  complete cell-layer rows: 3/3
  passed high-capacity anchor gates: 0/3
  decision: not replicated

Qwen2.5-VL-7B text-decoder-only audit:
  real rows: 480
  complete cell-layer rows: 3/3
  passed high-capacity anchor gates: 0/3
  decision: not replicated

Qwen3-8B text-only anchor:
  artifact: complete local weights
  complete cell-layer rows: 3/3
  passed high-capacity anchor gates: 0/3
  decision: not replicated
  failure signature: strong but nonspecific channel movement; target does not beat matched controls and leak is high

Qwen2.5-7B base anchor:
  artifact: complete local weights
  complete cell-layer rows: 3/3
  passed high-capacity anchor gates: 0/3
  decision: not replicated
  failure signature: target effect exists but specificity gap is insufficient
```

VL fallback 的边界很重要：

```text
This is a VL decoder-family negative boundary.
It is not evidence that all text-only 7B checkpoints fail.
```

## 4. 文本方向最新判断

### 4.1 推荐标题方向

当前投稿转向：LP4FM-facing 的 linguistic source-control paper。当前推荐标题为

```text
Descriptions as Controllers: Identifiability and Saturation of Belief-Source Access in Language Models
```

论文不再把自己包装成普通 ToM benchmark，而是研究语言描述作为控制界面，如何改变
belief/reality/other-agent/object-distractor 的 typed arbitration。核心新增语言轴是
description order、coreference/pronoun ambiguity、negation/evidential wording，以及
role label versus name。机制上只写 final-readout shortcut boundary，不写 broad hidden-state
causal mediation。完整 submission spine 见
`docs/proposals/lp4fm_pressure_stable_belief_binding_20260801.md`。

Stage-44 已将该转向落实为可运行的语言学 pressure manifest：以 `role_decoy` 和
`object_decoy` 的 80 个 parent rows 为基础，构造 description order、
coreference/pronoun ambiguity、negation/evidential wording 三个 P0 主轴，以及
role label versus name 辅助轴。每个 parent 有 11 个 arms，包括 original-description
anchor，共 `880` rows。所有 rows 均重新进行 neutral-padding length rebalance，达到 `880/880`
exact target length；所有变体保持 `instruction_target_kind=other_agent`，因此不会把
语言形式效应与 target semantic change 混为一谈。当前只能写 manifest-ready，尚不能写
语言轴存在显著 behavioral 或 causal effect。

Stage-43 已完成 `120` paired external groups，但 `object_decoy` 与 `role_decoy` 两个
family 的 frozen isotropic-gain gates 均未通过；median prediction error 分别为
`0.2058` 与 `0.1591`。该结果定义 pressure-family portability boundary，不能通过
重新拟合 gamma 救回，也不能写 broad cross-family transfer。

Stage-44 的 `44-row` Qwen2.5-7B-Instruct paired linguistic smoke 已通过 coverage、
finite score 和完整 parent pairing gates。`n=4` 的方向信号为：description order
`-0.252`、negation/evidential `+4.660`、coreference/pronoun `+1.648`、role
label/name `-1.643`；这些仍是 smoke 方向信号，不是稳定语言学效应。

Stage-44 full audit 已完成。80-parent paired results 中，description-order 的
`state_first - role_first` binding-margin delta 为 `-2.132`，95% bootstrap CI
`[-2.787,-1.486]`；coreference/pronoun ambiguity 的 `pronoun_ambiguous - name_explicit`
delta 为 `+0.705`，CI `[0.282,1.128]`。这两个轴进入主文正结果。`negated_exclusion -
affirmative_direct` 的 CI `[-0.384,1.396]` 跨 0，role-label/name 的 CI `[-1.145,0.482]`
也跨 0；source-explicit 相对 affirmative direct 为 `+1.685`，但不与 negation 合并为
单一效应。当前最干净的 LP4FM contribution 是 discourse order 与 reference explicitness
改变 typed role-state arbitration，而不是所有语言轴都有效。

最稳标题：

```text
False-Belief Accuracy Is Non-Identifying:
Typed Answer-Controller Arbitration in LLM Theory-of-Mind
```

更机制向：

```text
Pressure-Stable Belief Binding:
Answer-Interface Arbitration in Long-Context Theory-of-Mind
```

不建议：

```text
Long-Context LLMs Fail Theory-of-Mind
```

因为它把 paper 拉回普通 benchmark。

### 4.2 Introduction 应该怎么开

第一段要抓住 contradiction：

```text
False-belief tasks are usually treated as correctness tests. For LLMs, this is
not identifiable: the same answer can be produced by belief tracking, reality
tracking, another agent, object distractors, or instruction policy.
```

第二段给 primitive shift：

```text
We recast false-belief evaluation as typed answer-controller arbitration under
pressure.
```

第三段给 surprise：

```text
The queried belief is readable, but it is not the portable causal answer
controller under our guarded interventions.
```

第四段给 bounded consequence：

```text
The supported mechanism object is a capacity-thresholded answer-interface
geometry: stable in named Qwen2.5-7B rows, bounded by smaller-scale,
compression, high-capacity decoder-family negatives, and a clean text-only
high-capacity anchor negative.
```

### 4.3 主文结果顺序

不要按内部实验顺序写。按概念写：

1. False-belief accuracy is non-identifying.
2. Typed pressure worlds reveal controller arbitration.
3. Query-local belief is readable but not sufficient causal controller.
4. Answer-interface flip budgets explain failed steering.
5. Multi-site effects reduce to a final-readout shortcut under controls.
6. Named Qwen2.5-7B-Instruct answer-interface geometry survives item, answer-simplex, same-checkpoint functional-layer, and same-checkpoint anchor-rerun tests.
7. Smaller/intermediate scales, Qwen2.5-7B base, Qwen3-8B, high-capacity decoder-family fallback, and a clean DeepSeek text-only anchor define the boundary.

### 4.4 主图方向

主图应该是一个四面板 figure，而不是流程图或实验编号表。

| panel | 内容 | 读者应该记住 |
| --- | --- | --- |
| A | typed pressure worlds and answer channels | accuracy is a controller mixture |
| B | readable belief vs failed causal rescue | readability is not answer control |
| C | answer-interface budget / final-readout shortcut | control concentrates at late answer interface |
| D | capacity boundary map | 7B named geometry exists but does not broadly transfer |

这个 figure 是 oral 叙事的中心 artifact。

当前 canonical source pack：

```text
results/oral_readiness_pack_20260707/main_story_figure_pack/main_story_figure_pack.md
```

这个 pack 把主图压成四个 stage-free panel：accuracy non-identifying、
readable belief/control dissociation、budgeted answer-interface boundary、以及
capacity/compression/anchor boundary map。后续论文重写和组会汇报应优先从这个
pack 出发；旧的实验编号式 figure/table 只作为 provenance。

## 5. 论文可写 / 不能写的 Claim

### 5.1 可以写

- Raw false-belief accuracy is non-identifying for LLM ToM under pressure.
- Typed pressure worlds expose answer-controller arbitration.
- Query-local belief variables are readable under controls.
- Readable belief does not imply portable causal answer control.
- Typed answer-channel flip budget is a defensible mechanism object.
- Current intervention evidence supports a final-readout shortcut boundary, not policy-channel mediation.
- Qwen2.5-7B-Instruct has three named, held-out-replicated candidate-output-subspace intervention rows under corrected activation/control pairings.
- Full K-1 answer-simplex projection is channel-nonidentifying: target and wrong-channel vertex choices span effectively the same subspace and produce the same effect under a fixed delta.
- Cross-checkpoint activation-delta transport is directionally asymmetric while the candidate-output simplex is nearly invariant.
- Competitor-complete full-simplex linear interventions are provably channel-nonidentifying.
- The centered one-vs-rest vector is the unique typed linear contrast, up to scale, under translation and non-target permutation symmetry.
- The observed bidirectional transport is consistent with a common checkpoint trajectory increment under a first-order antisymmetric decomposition; all 6/6 item-level bootstrap gain intervals are positive.
- Canonical one-vs-rest interventions identify typed answer-channel control in six named Base/Instruct direction-layer rows under wrong/random, dose, argmax, and leak controls.
- Activation-trajectory source is the bootstrap-stable dominant factorial mechanism in all 6/6 named rows; 24/24 dose tests and 23/24 paired specificity tests pass.
- Held-out paired linear stitching gives geometry success in 120/120 item rows, but causal success in only 32/120; all 6/6 exact geometry-function dissociation gates pass.
- Strict item-paired causal rescue holds in only 2/6 direction-layer rows, both Base-to-Instruct 2k rows, establishing partial directional coordinate rescue rather than a global gauge account.
- Tested 1.5B and 3B scales fail fixed, functional, and eligible repair gates.
- The 7B positives do not collapse into one shared rank-3 answer-interface basis.
- Qwen2.5-7B base, Qwen3-8B, and DeepSeek-R1-Distill-Qwen-7B do not reproduce the named Qwen2.5-7B-Instruct rows under unchanged high-capacity anchor gates.
- Qwen2.5-VL-7B text decoder does not reproduce the named Qwen2.5-7B rows under unchanged gates.
- The paper contribution is a capacity-thresholded mechanism-boundary map for typed answer arbitration.

### 5.2 不能写

- 不能写 LLMs lack belief representations.
- 不能写 task descriptions protect a portable belief representation.
- 不能写 query-binding is the causal answer controller.
- 不能写 final-token steering proves selective causal belief control.
- 不能写 policy-channel mediation or multi-site controller causality is demonstrated.
- 不能写 component / branch audits localize the internal writer.
- 不能写 named 7B rows prove broad model-family transfer.
- 不能写 smaller/intermediate negative results prove every smaller model lacks related geometry.
- 不能写 Qwen2.5-VL negative proves all 7B text-only checkpoints fail.
- 不能把 full answer-simplex projection 写成 typed channel-specific mediation。
- 不能把 Stage-36 named answer-channel mechanism 扩写成 broad internal belief controller、全层 writer localization 或跨模型家族普遍机制。
- 不能把 held-out stitching geometry rescue 直接写成 causal coordinate-gauge mechanism；必须通过 aligned versus shuffled typed-contrast interventions。
- 不能写 first-order antisymmetric decomposition 是唯一机制；非线性余项和 hidden-coordinate gauge 仍需判别。
- 不能写 the result is internal-causal mechanism oral-ready.

## 6. Oral 风险

| risk | 为什么危险 | 应对 |
| --- | --- | --- |
| 看起来像实验堆叠 | 内部编号太多会让审稿人觉得是流水账 | 主文完全按 mother object 和 attack closure 写，编号只进复现索引 |
| negative transfer 被看成失败 | 审稿人可能期待 broad transfer | 明确说 paper object 是 capacity-thresholded boundary map |
| typed mechanism 被误写成 belief controller | Stage-36 识别的是 answer-interface channel control，不是完整内部 belief state | 主文明确区分 typed answer-channel causality 与 broad internal belief controller |
| Qwen2.5-7B 过窄 | DeepSeek、Qwen3-8B、Qwen2.5-7B base 和 VL text-decoder anchors 均已完成但未复现 | 把它写成 checkpoint / instruction-alignment / decoder-family boundary；不能写 broad transfer |
| final-readout shortcut 被看成 trivial | 读者可能觉得“当然最后一层控制答案” | 强调 surprise 是 readable belief 和 answer controller 的 dissociation |
| 数学部分被看成事后包装 | full-simplex 等式本身是初等线性代数 | 主张 identification consequence、canonical contrast uniqueness 和 falsifiable transport decomposition，不冒充新的纯数学定理 |
| stitching 被质疑为 adapter overfit | 任意高容量映射都可能改善几何 | train/eval item 隔离，并要求正确配对显著超过 shuffled map，最终以 typed causal rescue 而非 cosine promotion |
| 主图不够记忆点 | 没有一个不可忽视 artifact | Panel D boundary map 加 Panel B dissociation 是主图核心 |

当前最大 oral 风险不是缺结果，而是叙事过宽。越想写“内部因果机制”，越容易被 controls 反杀。更稳的 oral 版本是：

```text
We identify and bound the answer-interface arbitration geometry that false-belief
accuracy hides.
```

## 7. 下一步优先级

### 第一优先级：验证 downstream functional metric

Stage-37/38 已完成并给出混合机制：几何可双向对齐，但严格 causal rescue 仅为 2/6。
下一步不再重试 Euclidean stitch，而是抽取 target-checkpoint 的局部 typed covector
`a_t`，检验 functional residual `|a_t^T e|` 是否比 Euclidean residual、wrong-channel
covector 与 norm-matched random covector 更能预测真实 causal transport error。只有
Phase-A predictor gate 通过，才进入 minimum-norm functional correction intervention。

截至 2026-07-30，Stage-39 已从 protocol-ready 推进到 runner-ready：360-row 双向计划、
真实 autograd VJP extractor、signed causal-error export、item-cluster bootstrap audit 与
单模型顺序下载的可恢复队列均已实现。当前正在运行双向各一个 group 的真实 smoke；在
smoke 完成前只能写 runner-ready / smoke queued，不能写 functional metric validated。

2026-07-31 审计进一步发现：第一版完整 Stage-39 虽完成 120 groups / 360 residual rows，
并出现 Base-to-Instruct 通过、Instruct-to-Base 未通过的 `1/2` 结果，但 predictor 使用了
完整 activation delta residual，而真实 Stage-36/37 干预使用的是 `alpha` 缩放后的 typed
projection。两者 estimand 不一致，因此旧 `1/2` 只保留为 full-delta diagnostic，不进入
claim ladder。修正版已改为 applied-intervention residual，并在 evaluator 中强制检查
`residual_space=applied_typed_projection`；修正后的完整双向实验正在后台运行。

修正版已于 2026-07-31 完成：120 groups、360 applied-residual rows 全覆盖，但双方向
predictor gate 均未通过。Base-to-Instruct 的 target Spearman 为 `0.5519`、cluster
bootstrap 下界 `0.4009`，Instruct-to-Base 为 `0.1743`、下界 `0.0128`；两个方向的
effect-sign accuracy 都是 `1.0`。失败来自 specificity：target covector 未能在 cross-fit
error 上稳定优于 wrong-channel 与 Euclidean controls。因此不能运行 scalar functional
correction。

这个结果把机制对象推进为新的 identifiability boundary：实际 residual 全被 typed
projection 压到一维族 `e=s q_target`，任意非正交 scalar covector 都主要继承同一个
`|s|` 排序，单轴数据不能识别 functional identity。下一对象已升级为 rank-up-to-three
typed response operator。Stage-40 已生成 1920-row 计划，实现三条 Helmert
answer-simplex 轴、一条 norm-matched random 轴、四个正负对称剂量、真实 runner、operator
audit 和 fail-closed smoke-to-full controller。完整实验现已完成：120 groups / 1920 rows
全覆盖，Base-to-Instruct target 为 `60/60` 通过，Instruct-to-Base target 为 `59/60`
通过。两个方向的 median even ratio 为 `0.0086/0.0164`，minimum symmetric-dose
cosine 中位数为 `0.999996/0.999950`，simplex/random gain ratio 为 `31.1/33.9`，
有效秩均为三。这验证了稳定、局部线性、typed-specific 的 rank-3 response operator；
但仅凭该结果不能写 active-set correction 或 global checkpoint equivalence。

Stage-41 随后在相同 60 个 held-out item/layer 对上完成配对 gain-shape decomposition。
两个 checkpoint 的 typed operator 形状高度一致：matched operator cosine 中位数
`0.9957`，95% bootstrap CI 为 `[0.9926, 0.9992]`；语义轴循环错配 control 的中位
cosine 为 `-0.0347`。最优标量校正把跨 checkpoint relative residual 从 `0.3701`
降到 `0.0929`，解释比例中位数为 `75.6%`。同时 Instruct/Base gain ratio 为
`1.5556`，95% CI `[1.5120, 1.5961]`。两个算子距各自最优标量恒等映射的 residual
仅为 `0.0793/0.1304`；整体 gate 和三个 named row gate 均通过。

由此形成当前最强统一机制：**typed answer interface 在语义 simplex 上近似各向同性，
instruction tuning 主要重标定局部响应增益，而不是产生一个稳定的 channel-selective
轴。** 这把 full-simplex channel nonidentifiability、单 scalar covector 不可识别和
multi-axis operator 正例统一为一个可检验的结构定律。该定律是 final-answer interface
上的局部结果，不授权 broad belief control、全隐藏空间等价或 non-final policy mediation。

Stage-42 对该定律做了 leave-one-item-out predictive compression：每次只用同一 stratum
其余 19 个 item 估计 `gamma`，再由 held-out Base operator 预测 Instruct typed response。
各向同性增益模型的 median/mean relative error 为 `0.1175/0.1221`，相对未缩放 Base、
item-shuffled scaled Base 和 cyclic wrong-axis controls 的 paired bootstrap improvement
下界分别为 `0.2516/0.0531/1.2677`，predictive gate 通过。训练集 Instruct mean baseline
误差更低 (`0.1043`)，因此结论是低维 stratum-level predictive law，而不是 item-specific
Base operator 必不可少。

Stage-43 已把独立 gate 落实为跨 pressure-family 外推，而不是继续切分原 60 items。
发现集只包含 `role_object_decoy`；新实验冻结 Stage-39 的三组 native dose median 与
Stage-41 的三组 `gamma`，在新生成、精确长度、单 token 合格的 `role_decoy` 和
`object_decoy` 上运行双 checkpoint full-vector response。真实 audit 已完成 `120`
paired groups；两个 family gate 均失败，median frozen-gain error 为 `0.2058` 和 `0.1591`。
这不是 calibration 可调问题，而是当前 isotropic law 的 pressure-family portability
boundary；因此不再 refit gamma。

Stage-45 已完成 `880` rows，但后置 source-independence audit 发现其 80 个 parent 与
Stage-44 是同一 identity pool：故事、候选答案、agent、length、pressure condition 与
original-description anchor 为 `80/80` 重合。因此两个阶段数值完全一致只能证明 runner
reproducibility，不能写 held-out replication。该行已从 promotion evidence 降级为
`replay-equivalent-not-independent`。

Stage-46 已完成修正后的 item-disjoint gate：40 个与 Stage-44 `item_id` 零重叠的新故事，
`L3-mis-other-agent`，`1024/2048` 两个长度，共 80 parents 和 `880` 个 finite score rows。
结果否定了 pressure-invariant linguistic main effect：description order 从 `-2.132` 反转为
`+4.678`；coreference 从 `+0.705` 变为 `-0.227` 且 CI 跨 0；negation 从 null 变为
`+4.143`；role-label/name 从 null 变为 `-1.238`。预注册 replication signature 为
`0/2` primary axes、`0/2` negative controls，因此不能写语言学泛化。

这个负结果产生了新的主研究对象：**语言形式不是 pressure-invariant 主效应，而是由 typed
conflict topology 门控的条件控制信号。** Stage-47 用 item-cluster bootstrap 将三种压力族
拆开，发现 `3/4` 语言轴存在 composite-vs-single interaction，且 `4/4` 在 1024/2048
上保持同方向。description-order、coreference、negation 的 interaction 分别为
`+6.807/-0.931/+3.600`。但 role/object/combined family 仍与 item pool 混杂，因此当前只能
写 conflict-topology moderation candidate，不能写因果 moderation。

Stage-48 已启动 same-item crossed-topology confirmatory gate：30 个全新 latent items 在
`role_decoy/object_decoy/role_object_decoy` 三种 topology 下共享 agents、objects、四个候选
答案和两种长度，再施加冻结的 11 个语言 arms，总计 `1980` real score rows。source preflight
为 `30/30` candidate-invariant blocks、`180/180` exact rows、`0` invariant failures、`0`
prior latent overlap。预注册方向是 description order 正、coreference 负、negation 正；必须
`3/3` 通过且在两个长度同方向，才允许写 stimulus-level causal topology moderation。

Stage-48 真实结果已经完成并通过 promotion：`1980` rows、`180` parent groups、`30`
crossed latent items，三个预注册轴为 `3/3 pass`。joint-minus-single interaction 分别是
description order `+6.722`（CI `[5.697,7.747]`）、coreference/pronoun `-1.062`
（CI `[-1.724,-0.409]`）、negation/evidential `+3.706`（CI `[2.875,4.527]`），并且
全部在 1024/2048 上保持预注册方向。现在可以写：在该 same-item crossed construction 内，
typed role/object conflict topology 因果调节测试过的语言形式效应。仍不能写 hidden-state
mediation、human psycholinguistic universality 或 broad model-family transfer。

为扩展更多元数据样本，Stage-49 构建了四种语义保持的 story realization：canonical
narrative、structured event log、passive-voice report、compact indirect report。balanced
pilot 已完成 `12` items、`288` parents、`3168` real rows，并通过预注册 promotion gate：
三个 confirmatory axes 的 aggregate directional CI 为 `3/3 pass`，description order、
coreference、negation 在四种模板上的预期方向分别为 `4/4`、`4/4`、`3/4`。aggregate
interaction 分别为 `+5.060`（CI `[4.455,5.662]`）、`-1.239`（CI
`[-1.738,-0.749]`）、`+1.632`（CI `[0.917,2.328]`）。这支持一个更精确的语言学结论：
conflict-topology interaction 存在跨 realization 的方向稳定核，但 construction family
调节效应强度；passive voice 下 negation/evidential interaction 反转为 `-1.481`，因此不能写
模板无条件不变性。

30-item full cross-realization replication 现已完成：`720` parents、`7920/7920` real rows、
三个 aggregate axes `3/3 pass`。description order 为 `+4.551`（CI `[3.907,5.152]`，
`4/4` 模板同方向），coreference 为 `-1.006`（CI `[-1.474,-0.530]`，`4/4` 同方向），
negation/evidential 为 `+1.396`（CI `[0.915,1.896]`，`3/4` 同方向）。pilot 到 full 的
方向和边界均复现；passive voice 下 negation 仍显著反转为 `-1.052`（CI
`[-1.756,-0.330]`）。正式 promotion 是：可以写 **cross-realization direction-stable
conflict-topology kernel with construction-sensitive gain**，但不能写 unrestricted natural
language generality、human psycholinguistic universality 或 broad model-family transfer。

随后对异常 construction 做机制判别时发现，旧名 `passive_voice_report` 实际不是干净的
passive voice，而是每个事件都被 `It was reported that` 局部嵌套。由此研究对象从“模板
敏感性”升级为 **source accessibility**。六条件 factorial 将 genuine passive、global/local
reportative、global/local factive 与 active baseline 分离。结果否定了单一 local-reportative
解释，却显示三类可分解损失：genuine passive 相对 active 为 `-2.952`，reportative locality
为 `-3.061`，factive locality 为 `-2.032`，且 reportative-specific locality interaction 为
`-1.030`（CI `[-1.945,-0.097]`）。

把这些损失按线性加法组合后，held-out passive-by-embedding cells 的方向大体可预测，但幅度
被系统性高估；compound responses 全部停在接近零的窄区间，而非继续下降到 additive model
预测的 `-1` 到 `-3.4`。post-hoc model comparison 中，weakest-link MAE 为 `1.453`，优于
additive `2.260`（paired improvement CI `[0.363,1.247]`）和 no-voice `2.153`（CI
`[0.406,1.028]`），但未显著优于 average-saturation `1.599`。因此当前理论对象不是唯一的
min operator，而是更保守的 **subadditive accessibility saturation**。

该定律随后在 `20` 个全新 latent items 上独立确认，和先前 crossed pool 的 latent-signature
overlap 为 `0`。完整 `1440/1440` rows、两种 predicate families 均通过预注册 gate。
compound 相对 additive prediction 的 pooled saturation residual 为 `+4.190`（CI
`[2.931,5.401]`）；reportative 为 `+4.609`，factive 为 `+3.771`。weakest-link 与
average-saturation 模型都显著优于 additive，且两个 predicate family 分别通过。因此论文
现在可以写：**在受控 typed belief-binding worlds 中，多个语言 accessibility constraints
以次加性方式饱和；一旦某个 source-binding route 成为瓶颈，额外 construction pressure 不再
线性累加。** 仍不能把该行为定律写成 hidden-state circuit、唯一 min-law、人类 source
monitoring 机制或 broad model-family universality。

### 第二优先级：同步完成 paper-facing rewrite

当前最有价值的推进是写作和图表重排：

1. 用四面板主图重写 Introduction 和 Results。
2. 把内部编号全部移入 supplement / reproduction index。
3. 把正负结果写成 attack closure，而不是 chronology。
4. 把 claim ladder 放进 supplement。

### 第三优先级：把已完成 text-only 7B+ fallback anchors 写成失败签名边界

DeepSeek、Qwen3-8B 和 Qwen2.5-7B base 已经给出一组 completed
high-capacity negative boundaries。目标不再是“继续堆模型”，而是把它们写成
可解释的 answer-interface boundary：DeepSeek 关闭 clean second text-only anchor
解释，Qwen3-8B 关闭更大 Qwen-family text model 自动复现解释，Qwen2.5-7B
base 关闭纯 architecture 解释。

```text
additional text-only high-capacity anchors with complete local weights
```

要求：

- 模型 artifact 必须完整可加载，下载/队列状态本身不能当科学证据；
- 沿用相同 held-out items、answer-simplex arms、coverage、budget、argmax、leak、specificity gates；
- 结果无论正负都必须写窄；
- promotion 只来自完整 Stage-33 audit，不来自 partial effects 或 smoke rows。

当前 preflight / queue 状态：

```text
results/oral_readiness_pack_20260707/second_text_anchor_preflight/second_text_anchor_preflight.md
results/oral_readiness_pack_20260707/high_capacity_anchor_queue_status/high_capacity_anchor_queue_status.md
```

结论已经从 `model_artifact_blocked` 推进到
`validated_real_negative_second_high_capacity_anchor`：

- `deepseek-ai/DeepSeek-R1-Distill-Qwen-7B` 已经成为当前 clean text-only / high-capacity primary anchor：缺失 shard 已补齐，final safetensors byte size 与 expected bytes 一致。
- DeepSeek Stage-33 已完成：`runs/stage33_high_capacity_anchor_deepseek_hf_mirror_curl_20260713_115406`，480 real rows，3/3 complete cell-layer rows，0/3 high-capacity anchor gates passed。
- 这允许写“tested clean text-only high-capacity anchor did not reproduce the named Qwen2.5-7B rows”，但不能写 all 7B text-only checkpoints fail，也不能写 Qwen2.5-7B positive 被推翻。
- `Qwen/Qwen2.5-7B` 同家族 base fallback 已完成标准 Stage-33 和 functional-layer diagnostic，结果均未复现 named Qwen2.5-7B-Instruct rows；这支持“instruction/alignment answer-interface boundary”，但不能写 base model 没有任何相关 latent geometry。
- `Qwen/Qwen3-8B` 已通过 Transformers + offload 路线完成标准 Stage-33 和 functional-layer diagnostic，结果均未复现；主要 failure signature 是 nonspecific control/leak，而不是简单没有 logit movement。
- `Qwen/Qwen3.5-9B` 和 `Qwen/Qwen3.5-9B-base` 当前只有不可读缓存，没有本地 config / snapshot。
- `Qwen/Qwen2.5-7B-Instruct` 是 reference model，不能作为第二 anchor。
- `Qwen/Qwen2.5-VL-7B-Instruct` 已跑过，但它是 VL decoder-family boundary，不是 clean text-only anchor。
- `Qwen/Qwen2.5-3B-Instruct` 低于 high-capacity anchor 阈值。

代码层面已经把这个执行路径接进同一实验 gate，并已用于完成 Qwen3-8B anchor：

```bash
DEVICE_MAP=auto \
MAX_MEMORY=0=14GiB,cpu=48GiB \
OFFLOAD_FOLDER=runs/offload_qwen3_8b \
LOAD_IN_8BIT=1 \
MODEL=Qwen/Qwen3-8B \
ANCHOR_LABEL=qwen3_8b \
bash scripts/queue_stage33_high_capacity_anchor.sh
```

这只改变模型加载/显存调度，不改变 answer-simplex intervention、held-out stimuli、coverage/budget/argmax/leak/specificity gates。Qwen3-8B 的完成结果是 negative anchor boundary，不是 transfer evidence。

若后续需要把新模型排在现有队列后自动运行，仍使用同类 watcher：

```bash
WAIT_FOR_PID=<previous_queue_pid> bash scripts/queue_qwen3_8b_offload_after_pid.sh
```

所以当前可以把 completed text-only anchors 写成 validated negative boundaries：

```text
the tested DeepSeek-R1-Distill-Qwen-7B, Qwen3-8B, and Qwen2.5-7B base
anchors did not reproduce the three named Qwen2.5-7B-Instruct
answer-interface rows under unchanged held-out answer-simplex gates.
```

但仍不能写 all model-family transfer fails，也不能写这些 base / Qwen3 /
DeepSeek 模型完全没有相关 latent geometry；当前只支持“在这套 typed
answer-interface gates 下不复现 named Instruct rows”。

### 第四优先级：只有在 reviewer 逼问时才做 clustered / per-condition basis

共享 rank-3 basis 已经失败。不要重复相同实验。

可选的更窄 follow-up 是：

```text
per-condition or clustered local basis
```

它的作用只能是补充解释，不应成为主 claim。

### 不建议继续：除非换新对象，否则不要追旧 non-final mediation

不要再重复：

```text
same-length patching retry
final-token random-direction retry
raw multi-site retry
1.5B/3B fixed/functional layer retry
global shared-basis retry
```

如果还要冲 internal-causal mechanism，必须换对象，例如：

```text
training-time intervention, causal circuit discovery with learned local probes,
or a new non-final site with a falsifiable gate that beats final-readout controls.
```

这不是当前 submission 的必要条件。

## 8. 组会汇报版讲稿

```text
我现在不会把这个项目讲成 ToM benchmark。主线是：false-belief accuracy 在
LLM 上不可识别，因为同一个答案可能来自 belief、reality、other-agent、
object distractor 或 policy。我们因此构造 typed pressure worlds，把问题从
答对没答对改成哪个 answer controller 赢。

最关键的机制发现是 readable belief 和 answer control 分离。query-local belief
能被 probe 读出来，但 patching/steering 和 final-token controls 显示它不是
可迁移的答案控制器。更好的机制对象是 answer interface 上的 typed flip
budget，也就是不同 answer channels 要赢得输出需要跨过的预算。

后面的实验不是散乱加 controls，而是逐步关闭 reviewer attack。7B 的 named
answer-interface geometry 经 item stability、held-out item 和 answer-simplex
controls 支撑，并且 Qwen2.5-7B-Instruct self functional sweep 3/3 通过；
1.5B 和 3B 在 fixed、functional、eligible repairs 下不复现；7B 内部也不能
压成一个 shared rank-3 controller；Qwen2.5-7B base、Qwen3-8B、Qwen2.5-VL-7B
text decoder fallback 和 clean text-only DeepSeek high-capacity anchor 都没有
复现。最终可写的贡献仍是 typed answer-interface identifiability boundary map，
不是 broad transfer 或 internal belief controller。
```

## 9. 复现索引

以下内部编号和路径只用于复现，不建议出现在主文叙事中。

| artifact | role |
| --- | --- |
| `results/oral_readiness_pack_20260707/oral_submission_gate/oral_submission_gate.md` | submission gate |
| `results/oral_readiness_pack_20260707/paper_outline.md` | paper outline draft |
| `results/oral_readiness_pack_20260707/main_story_figure_pack/main_story_figure_pack.md` | main story / Figure 1 source pack |
| `results/oral_readiness_pack_20260707/answer_interface_boundary_contribution_20260719/answer_interface_boundary_contribution_report_zh.md` | negative-to-positive boundary contribution synthesis |
| `results/oral_readiness_pack_20260707/negative_result_positive_contribution_20260719/negative_results_as_positive_contribution_zh.md` | failure-signature ledger turning controlled negative gates into a positive boundary contribution |
| `results/oral_readiness_pack_20260707/second_text_anchor_preflight/second_text_anchor_preflight.md` | second text-only anchor preflight |
| `results/oral_readiness_pack_20260707/high_capacity_anchor_queue_status/high_capacity_anchor_queue_status.md` | high-capacity anchor queue / artifact status |
| `results/oral_readiness_pack_20260707/all_model_all_experiments_20260718/all_model_all_experiments_status.md` | all complete text-only model x model-general experiment queue status |
| `results/oral_readiness_pack_20260707/all_model_vl_supported_experiments_20260718/all_model_vl_supported_experiments_status.md` | VL-model supported fallback queue status |
| `results/oral_readiness_pack_20260707/claim_ladder.csv` | allowed / forbidden claims |
| `results/oral_readiness_pack_20260707/paper_table_plan.csv` | table / figure source list |
| `results/oral_readiness_pack_20260707/oral_critical_experiment_queue.csv` | next experiment queue |
| `results/oral_readiness_pack_20260707/stage14_flip_budget_intervention_protocol/stage30_capacity_threshold_oral_pack_audit.md` | capacity boundary pack |
| `results/oral_readiness_pack_20260707/stage14_flip_budget_intervention_protocol/stage31_capacity_boundary_figure_pack.md` | boundary figure pack |
| `results/oral_readiness_pack_20260707/stage14_flip_budget_intervention_protocol/stage32_shared_answer_interface_basis_audit.md` | shared-basis negative boundary |
| `results/oral_readiness_pack_20260707/stage14_flip_budget_intervention_protocol/stage33b_vl_text_decoder_anchor/stage33_high_capacity_anchor_audit.md` | VL text-decoder negative boundary |
| `runs/stage33_high_capacity_anchor_deepseek_hf_mirror_curl_20260713_115406/audit/stage33_high_capacity_anchor_audit.md` | clean text-only high-capacity negative boundary |
| `results/oral_readiness_pack_20260707/stage37_mathematical_transport_theory_20260730/answer_interface_geometry_theory_zh.md` | identification theorems, trajectory decomposition, stitching discriminator, and references |
| `results/oral_readiness_pack_20260707/stage37_mathematical_transport_theory_20260730/stage37_mathematical_transport_decision.json` | real item-level first-order transport validation |
| `results/oral_readiness_pack_20260707/stage37_mathematical_transport_theory_20260730/stitching/stage37_heldout_geometry_summary.csv` | held-out paired versus shuffled stitching geometry |
| `results/oral_readiness_pack_20260707/stage36_typed_contrast_factorial_20260730/bootstrap_validation/stage36_bootstrap_report.md` | item-paired specificity, dose, and mechanism-class validation |
| `results/oral_readiness_pack_20260707/stage37_mathematical_transport_theory_20260730/figure_answer_interface_trajectory_theory.png` | paper-facing Figure 2: identifiability, trajectory dominance, and held-out stitching geometry |

基本验证：

```bash
python -m py_compile scripts/build_stage29_scale_conditioned_mechanism_synthesis.py \
  scripts/build_stage30_capacity_threshold_oral_pack.py \
  scripts/build_stage31_capacity_boundary_figure_pack.py \
  scripts/build_main_story_figure_pack.py \
  scripts/build_second_text_anchor_preflight.py \
  scripts/build_stage32_shared_answer_interface_basis_plan.py \
  scripts/run_stage32_shared_answer_interface_basis.py \
  scripts/build_stage32_shared_answer_interface_basis_audit.py \
  scripts/run_same_length_patching.py \
  scripts/build_second_text_anchor_candidate_rank.py \
  scripts/build_high_capacity_anchor_queue_status.py \
  scripts/build_stage33_high_capacity_anchor_plan.py \
  scripts/build_stage33_high_capacity_anchor_audit.py \
  scripts/build_answer_interface_boundary_contribution.py \
  scripts/run_stage14_flip_budget_interventions.py \
  scripts/run_stage17_component_writer_interventions.py \
  scripts/run_stage18_distributed_writer_interventions.py \
  scripts/run_stage20_readout_subspace_interventions.py \
  scripts/build_oral_submission_gate.py
```

```bash
bash -n scripts/queue_stage33_high_capacity_anchor.sh \
  scripts/queue_second_text_anchor_hf_mirror_curl_then_stage33.sh \
  scripts/queue_qwen3_8b_offload_after_pid.sh \
  scripts/queue_all_model_all_experiments.sh \
  scripts/queue_vl_supported_after_all_model_experiments.sh \
  scripts/resume_process_group_after_pid.sh \
  scripts/refresh_high_capacity_anchor_after_pid.sh
```
