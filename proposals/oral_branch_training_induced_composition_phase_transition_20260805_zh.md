# Oral 分支一：训练诱导的组合定律相变

## 暂定标题

**当指令微调改变信念来源访问的组合代数**

备选标题：**从原子边际到组合定律：checkpoint 如何重写语言来源访问**。

## 一句话主张

语言模型训练改变的可能不只是单个语言构式效应的幅度，还包括多个语言约束在共享 typed source-access interface 上的组合方式。

## 新研究对象

现有结果不支持普遍的 saturation law，却揭示了一个更值得研究的现象：Qwen2.5-7B-Instruct 在独立世界上表现出正的 subadditive residual；同架构 Qwen2.5-7B Base 与 Qwen3 保留部分 control-relative predictor gain，却不保留 residual 的正方向。primitive-only affine transport 同样失败，因此差异不能被解释为固定定律经过统一 logit 压缩后的结果。

新的母对象不是“saturation 能否跨模型复制”，而是：

> 在一条连续训练轨迹上，source-access composition law 在何处、以何种方式发生类别变化？

## 可复用证据

Stage-48/49 已确认 typed conflict topology 对语言效应具有调节作用，并在受控 realization 中得到方向稳定的 kernel。Stage-51/52 已经把 primitive cost 冻结后用于预测未见 compound，并在 Qwen2.5-7B-Instruct 上确认 subadditivity。Qwen3 与 Base 的冻结复制限定了可移植性；affine-gain audit 排除了简单幅度压缩；Stage-34–43 则提示 checkpoint 差异更可能位于 counterfactual activation trajectory，而非候选答案子空间的整体旋转。

主要证据目录包括：

- `results/oral_readiness_pack_20260707/stage52_independent_saturation_20260804/`
- `results/oral_readiness_pack_20260707/qwen3_8b_independent_saturation_replication_20260804/`
- `results/oral_readiness_pack_20260707/qwen25_7b_base_independent_saturation_replication_20260805/`
- `results/oral_readiness_pack_20260707/affine_gain_null_audit_20260805/`

## 形式化对象

对 checkpoint `k`、latent item `i`、typed topology `T` 与语言构式 `c`，定义 topology-moderated typed binding margin `Gamma_i,k(c,T)`。对两个 primitive 构式 `c1,c2` 及其 compound `c12`，定义：

```text
S_i,k(c1,c2,T)
  = Gamma_i,k(c12,T)
    - Gamma_i,k(c1,T)
    - Gamma_i,k(c2,T)
    + Gamma_i,k(c0,T)
```

checkpoint 的组合签名不应只包含平均 residual，而应包含 residual 方向、control-relative gain、primitive dynamic range、itemwise residual geometry，以及 topology-by-construction interaction。核心假设是：训练会改变这一整体签名，而不是对固定签名乘以一个正标量。

## 研究问题

1. 同一训练轨迹上是否存在可重复的 composition-law change point？
2. reportative 与 factive 构式是否同时转变，还是由不同训练信号分别学习？
3. 变化主要关联 SFT、preference optimization、source-attribution 数据，还是一般 calibration？
4. 行为算子变化是否伴随 Stage-36 所定义的 typed activation trajectory 变化？
5. controlled exposure 是否能在不普遍提高 primitive accuracy 的情况下诱导该变化？

## 实验计划

### 阶段一：checkpoint 纵向普查

优先收集同一模型家族、同一训练流程中的 3–8 个 checkpoint：Base、continued pretraining、early/late SFT、early/late preference optimization 和最终 aligned checkpoint。若公开中间 checkpoint 不可得，则在相同 tokenizer、答案界面和 stimulus generator 下训练一个受控的小型轨迹。

### 阶段二：冻结组合评测

沿用 Stage-52 的 latent item split、prompt、construction contrast、answer candidates、primitive-cost estimator、cluster bootstrap 和 directional gate。先对每个 checkpoint 跑低成本 5-item screen，只扩展接近潜在转变点的 checkpoint。

### 阶段三：change-point identification

预注册无变化、纯幅度缩放、平滑单调变化、单一 change point、predicate-specific change point 和 topology-dependent change point。转变点选择与最终确认必须使用不同 item pool。

### 阶段四：controlled induction

设置两个严格匹配的训练条件。composition exposure 条件提供 primitive 与 compound source-attribution construction，并保留 typed source label；primitive-only 条件匹配 token 与 primitive margin，但不出现 compound。更强设计是随机化训练中出现的 construction pairs，并评估完全未见的 pair。

## 必要控制

必须控制 primitive answer margin、calibration、output entropy、token length、prompt embedding similarity、lexical overlap、source-label frequency、answer-token frequency、construction order、latent-world leakage、no-voice、shuffled-predicate、wrong-source 与 affine/isotropic gain baseline。

## 晋级门槛

**纵向变化门槛：** 至少一条 held-out 训练轨迹中出现 residual 方向或算子参数变化，并且不能由 primitive scale 解释。

**选择性诱导门槛：** composition exposure 对未见 compound 的影响显著大于 primitive-only exposure，同时不通过普遍扩大答案 margin 获胜。

**机制连接门槛：** 若加入 activation analysis，trajectory contrast 必须在 held-out items 上预测行为 residual，并击败 final-token-only、wrong-channel、random 和 calibration controls。

## Claim Ladder

当前可写：被测试 checkpoint 的组合形状差异超过 scalar affine gain。

纵向确认后可写：composition-law change 与同一训练轨迹中的特定训练阶段相关。

controlled SFT 通过后可写：受控构式暴露改变了 held-out composition behavior。

没有更强证据时不能写：instruction tuning 普遍地产生该 law、存在唯一 hidden operator，或人类 discourse processing 具有相同转变。

## Oral 叙事

论文应从一个悖论开场：相同 primitive linguistic costs 在不同 checkpoint 上产生不同 compound law，即使候选答案几何仍高度相似。主图应同时展示训练轨迹、primitive margins、compound residual、affine-null prediction 与 held-out transition。

## 风险与后备路线

若没有中间 checkpoint，则以 controlled miniature SFT 作为 intervention study；若只观察到幅度变化，则降级为 calibration boundary；若只有一个 predicate 改变，则写 predicate-specific learning；若 controlled induction 失败，则转向组合代数分支，把 checkpoint variation 保留为 scope boundary。

## 最小下一步

先冻结一个 5-checkpoint pilot，只运行 primitive 与 binary compound scoring。在任何 activation 工作之前，先判断纵向 residual change 是否存在。

