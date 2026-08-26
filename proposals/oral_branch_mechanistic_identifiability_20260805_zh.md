# Oral 分支三：Typed Controller 的机制可识别性

## 暂定标题

**一个信念控制器何时可识别？Probe、Answer Simplex 与 Causal Transport**

备选标题：**Readable 不等于 Controllable：语言模型机制探测的可识别性边界**。

## 一句话主张

一个表征是否可线性读取、能否控制输出、是否具有 source specificity、能否跨 checkpoint transport，是四个可独立成立或失败的性质；机制 claim 必须分别通过对应 identification gate。

## 为什么不是负结果汇总

项目中多次出现强机制表象：query-local belief information 可读，却不是稳定 steering site；multi-site intervention 被 final-token shortcut 吸收；full answer-simplex projector 看似 target-specific，实际对不同 vertex 代数等价；Base/Instruct 的候选输出子空间高度重合，但 causal transport 不对称；linear stitching 提高几何对齐，却无法全局恢复 causal effect。

这些现象共同说明：实验对象没有识别研究者想宣称的机制性质。本分支的贡献应是一套正式 identifiability framework、一般定理和可复用 audit protocol。

## 可复用证据

Stage-3/4 提供 behavior–probe–patch dissociation；Stage-5/13 展示 flip-budget calibration；Stage-14–16 揭示 final-token shortcut；Stage-17–20 关闭多个 writer/readout localization 假设；Stage-23/25/33 给出 held-out 与 model-family negative boundaries；Stage-34/35 揭示 full-simplex non-identifiability；Stage-36 提供 canonical one-vs-rest relaxation；Stage-37 显示 geometry rescue 与 causal rescue 分离。

## 与 Related Work 的关系

### probe 可读性与因果使用

[Probing Classifiers](https://arxiv.org/abs/2102.12452) 系统总结了 decoder accuracy
的识别局限；[Amnesic Probing](https://arxiv.org/abs/2006.00995) 更直接指出普通 probe
表现与 task importance 不必相关，并用信息移除检验行为使用。因此，“readable 不等于
causal”不是本分支可以单独宣称的新颖性，而是必须继承的起点。

本分支进一步把问题放到 typed multiclass answer interface：干预究竟控制 intended
source、答案 identifier，还是 final-token readout；它是否优于 wrong-channel controls；
相同 causal semantics 能否跨 item 和 checkpoint transport。

### 与 belief representation 工作的关系

[Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html)
和 [Brittle Minds, Fixable Activations](https://arxiv.org/abs/2406.17513) 已经提供 belief
decoding 与 intervention 的直接先例；[Standards for Belief Representations](https://arxiv.org/abs/2405.21030)
要求 accuracy、coherence、uniformity、use 四项条件。因此本分支不能只重复“probe
不够”，而必须给出 typed-controller 特有的识别结果：full-simplex vertex
non-identifiability、subspace overlap 与 causal transport 分离，以及 same-item
reconstruction 与 intended-source promotion 分离。

### 与 causal abstraction 和 steering 的关系

[Causal Abstraction](https://arxiv.org/abs/2301.04709) 为 patching、mediation、erasure、
steering 和 distributed alignment 提供统一形式语言；[ReFT](https://arxiv.org/abs/2404.03592)
及 [activation steering](https://arxiv.org/abs/2410.12877) 表明 representation
intervention 可以高效改变行为。这些是可直接比较的方法，但 steering success 与 typed
identifiability 不是同一 estimand。

[Cross-Encoding Steering Evaluation](https://arxiv.org/abs/2608.22985) 是尤其接近的
近邻：它显示 steering 可能跟随 extraction index 而不是 semantic label。本框架将其
视为 answer-encoding specificity failure，并进一步要求 source specificity 与
checkpoint transport。

本分支不能声称一般 mechanistic interpretability no-go 或唯一 belief circuit。要成为
独立贡献，必须补 multiclass theorem、至少两个非 ToM 外部任务，以及一套能实际改变
claim assignment 的审计协议。完整定位见
[Related Work 定位地图](../research/related_work_positioning_20260826_zh.md)。

## 四层识别对象

### Readability

能否从表征 `z` 在 held-out items 上预测 typed state `y`？

### Controllability

对 `z` 的受控 intervention 是否在足够 flip budget 下改变 target answer margin 或 argmax？

### Specificity

target intervention 是否显著优于 wrong-channel、orthogonal、random 和 final-token controls？

### Transportability

同一对象是否在 items、layers、checkpoints 或 tasks 间保留 causal semantics？

任何一层都不能在没有额外条件时推出下一层。probe accuracy 高不推出 causal control，subspace overlap 高也不推出 transportable causal semantics。

## 核心理论任务

### 定理族一：Vertex non-identifiability

对 `K` 个 answer vectors，任何包含目标 vertex 到其余所有 vertex 差向量的 target-specific subspace，都等于同一个全局 `K-1` contrast space。因此 competitor-complete full-simplex projector 无法识别 target vertex。

### 定理族二：Canonical typed relaxation

在线性、全局平移不变、对非目标 labels 置换不变的条件下，canonical rank-one typed contrast 与下式成比例：

```text
q_c = w_c - mean_{j != c}(w_j)
```

### 定理族三：Readability–control separation

构造 nuisance subspace 完美编码 `y`、但 downstream Jacobian 对该 subspace 为零的反例；同时构造全局 decodability 低、却因 item-conditional gating 具有强 output control 的方向。

### 定理族四：Geometry–causality separation

证明低误差或可逆 alignment 可以提高 cosine similarity，却不能保证 downstream response operator 被保留。causal transport 需要 state coordinates 与 local response map 同时对齐。

## 跨任务实证

理论必须至少在两个非 ToM typed-controller setting 上验证，例如 temporal source selection、deontic authority selection、spatial perspective 或 multi-document authority routing。每个任务都必须暴露多个共享输出界面的候选 controller，不能只记录二元正确率。

## 标准审计协议

第一步定义 typed candidates 与 controller confusion；第二步以 held-out、shuffled labels 和 matched-capacity probes 检验 readability；第三步在 flip-budget eligibility 下检验 causal effect；第四步加入 final-token、wrong-channel、orthogonal、random controls；第五步冻结对象进行 item/layer/checkpoint/task transport；最后按 readable、controllable、source-specific、held-out transportable、cross-checkpoint causal semantics 分层赋予 claim。

## 决定性反例

论文至少需要展示：相同 probe accuracy 但 causal effects 不同；相同 answer subspace 但 checkpoint transport 相反；multi-site effect 很强但扣除 final-token 后 residual 为零；geometry alignment 改善但 causal rescue 失败；target 与 wrong-channel effect 同样强。至少两个反例必须在 ToM 之外复现。

## 晋级门槛

**一般定理：** 至少一个结论适用于一般 multiclass readout，而非当前 Qwen answer tokens。

**跨任务复制：** 至少两个非 ToM tasks 重现两个 identification tiers 之间的预言分离。

**协议效用：** 新 audit 至少改变一个标准 probe/patching analysis 给出的机制 verdict。

**正对象：** 至少识别一个能通过更强控制的 canonical object；只有失败清单难以形成 oral-level methodology。

## Claim Ladder

当前可写：在受测 ToM setting 中，readability、controllability、specificity 和 transportability 多次分离。

理论与外部任务通过后可写：这些分离实例化了一般 multiclass mechanism analysis 的 identifiability limits。

协议验证后可写：该 audit 能阻止特定 false-positive mechanism claims，并识别更强的 canonical alternatives。

不能写：所有 probes 都无用、不存在 hidden mechanism、linear methods 永远失败，或 one-vs-rest 在非线性系统中全局唯一。

## Oral 叙事

主线应是：机制证据常通过更换 measurement object 被过早晋级，却没有验证新对象是否识别宣称的 controller。主图采用 readability、controllability、specificity、transportability 四列矩阵，标出每个反例被哪一道 gate 改变 verdict。

## 最小下一步

先写出一般 vertex non-identifiability theorem，再在 temporal-source 与 deontic-authority 两个任务上实例化四层 audit；不要先扩展大模型矩阵。
