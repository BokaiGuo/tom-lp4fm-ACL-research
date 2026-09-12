# 有证据支撑的 Oral 研究方案：平均规律保留，实例效应重排

更新日期：2026-09-12

## 0. 先给结论

前一版 `identification -> commitment -> partner adaptation` 适合作为待验证的研究
假设，但目前不能作为本项目的中心问题：LP4FM 尚未提供“关系已经被读出、但最终答案
被先验改写”的独立证据，更没有 partner-policy swap 的行为证据。继续围绕这个前提
扩展实验，会把叙事先于事实。

基于仓库中已经完成的结果，当前最有根据的 oral 候选应改为：

> **语言模型的语义组合行为具有尺度依赖的可迁移性：跨构式或跨 checkpoint，群体平均
> 的效应方向可以保留，但实例级效应、组合算子和 item 排序可以同时重排。**

英文暂定标题：

> **Laws Without Shared Programs: Scale-Dependent Transfer in LLM Semantic Composition**

这里的“law”是行为层的统计规律，“program”不是指某个已经被证明存在的隐藏程序。
论文不声称找到了神经机制，也不声称模型完全没有共享表示。

## 1. 这个问题从哪里来，而不是凭空提出

### 1.1 已验证现象 A：平均方向可以跨变体保留

在同一 latent world、同一 typed conflict topology 和匹配长度下，Stage-48/49 发现：

- description order interaction 在 crossed topology 和多种 realization 中保持正方向；
- coreference/pronoun interaction 保持负方向；
- negation/evidential interaction 的总体方向保留，但在 passive realization 中反转；
- 因此最窄的结论是 `direction-stable conflict-topology kernel with construction-sensitive gain`。

这不是“所有语言形式都稳定”，而是平均层面的方向性具有部分可迁移性。

### 1.2 已验证现象 B：实例级对齐不随平均方向一起迁移

Stage-53/54 的 hierarchical audit 显示：

- 两个 linguistic factor 都有正的 pooled topology contrast；
- joint interaction 的 item-level 跨阶段相关只有 `0.008`；
- role-only 和 object-only 也只有 `0.165/0.168`；
- Stage-54 的 pooled effect 保留，但 matched-item localization 没有超过 item-shuffled control。

所以“平均效应复现”不能推出“同一批实例以同样方式产生效应”。

### 1.3 已验证现象 C：checkpoint 改变的不只是标量增益

Stage-52 到 Qwen3、Qwen2.5-7B Base 和 Llama 3.1 的冻结迁移显示：

- 部分 control-relative shape 可以保留；
- directional saturation residual 的方向和幅度不稳定；
- primitive-only affine transport 不能解释变化；
- itemwise residual correlation 很低。

因此不能把跨 checkpoint 差异简化为“同一个规律只是整体变弱”。

### 1.4 这三个结果共同指向的对象

```text
mean direction      = 可以迁移
effect magnitude    = construction/checkpoint-sensitive
item-level ranking  = 可能重排
operator identity   = 尚未识别
```

这比“模型识别了 policy 但没有承诺”更稳，因为每一部分都直接连接到已完成的结果。

## 2. 真正的研究问题

### 主问题

> **当语言模型在新的语言构式或 checkpoint 上保留一个语义效应的平均方向时，这种
> 保留究竟是同一实例级规律的迁移，还是一种由不同实例组合出来的群体平均规律？**

### 三个子问题

1. **Scale question**：aggregate sign、effect magnitude、item ranking 和 operator class
   哪些层级可以分别迁移？
2. **Boundary question**：构式替换、source topology 替换和 checkpoint 替换分别在哪一层
   破坏可迁移性？
3. **Prediction question**：在 source construction 上学到的 item-level effect，能否在
   未见 construction、未见 topology 或未见 checkpoint 上前瞻性预测，而不是事后解释？

## 3. 中心科学主张

本文只争取一个主张：

> **LLM 语义组合的“泛化”不是单一属性，而是一个分层的 transfer profile；在本研究的
> 测试范围内，群体平均方向的保留可以与实例级效应不迁移、组合算子不确定同时出现。**

更强的形式是一个待验证的经验规律：

```text
aggregate transfer  >  magnitude transfer  >  itemwise transfer
```

这不是预设所有模型都满足的定律，而是本项目要在冻结数据和独立 confirmation 上检验
的 transfer ordering。若 ordering 不成立，就报告哪一层先失效，不强行维持标题。

## 4. 与已有研究的明确区别

### 与 Imperfective Paradox

该工作把一个语言学上清楚的最小语义关系与最终 NLI 判断分开，并发现表征区分不保证
推理决策忠实。本文吸收其“先证明关系有效、再分析模型行为”的方法，但不重复体义
现象。本文研究的是：**同一个关系效应在新的 construction/checkpoint 上是否由同一批
实例、同一组合结构和同一方向模式产生。**

### 与 Shaping Schema

Shaping Schema 论证语言表示会改变知识激活和组织，并观察不同语言表示下的行为/激活
变化。本文不把宽泛的 `schema` 当作贡献，而是给出可测量的 transfer profile：平均方向、
幅度、item ranking 和 operator class 分层报告，并使用前瞻性 held-out prediction 区分
“表示改变”与“规律迁移”。

### 与 belief probing / ToM benchmark

belief representation 工作主要问信念是否可读或可干预；ToM benchmark 工作关注任务覆盖、
shortcut 和 partner adaptation。本文不把 probe、steering 或静态准确率当作同一证据，
也不把研究包装成更大的 ToM benchmark。ToM 只是最成熟的 source-arbitration 实例。

### 与 compositional generalization

已有组合泛化研究关注数据覆盖、组合规则和任务分布。本文的增量不是再提出一个 coverage
指标，而是检验一个常被默认却少被直接分解的命题：**平均层面的组合规律复现，是否意味着
实例级行为程序也复现。**

## 5. 决定性实验设计

### Experiment 1：Transport profile，而不是单一 replication score

用已验证的 typed source-arbitration 构式作为 discovery source，并冻结：

```text
latent worlds
candidate answers
source topology
token length and answer encoding
scorer and item-cluster bootstrap
```

在每个 source→target transfer pair 上同时计算四个层级：

```text
T_sign      = aggregate effect direction agreement
T_mag       = normalized effect-magnitude agreement
T_rank      = item-level effect-rank correlation
T_operator  = held-out operator-family agreement
```

对照不是只报告 target accuracy，而是比较：

- source construction → held-out construction；
- role/object topology → crossed topology；
- Qwen2.5-7B-Instruct → matched checkpoint；
- same latent worlds → new latent worlds。

主图应该直接显示 transfer profile，而不是把所有结果压成一个分数。

### Experiment 2：前瞻性 item-level transport

这是本论文最关键的新数据。不能只复述 Stage-53/54 的相关系数，必须真正做
selection-blinded prediction：

1. 在 source construction 的训练 items 上估计每个 item 的 effect；
2. 用固定的 hierarchical model、surface-similarity model、random-effect model 和
   aggregate-only model；
3. 在 target construction 的全新 items 上预测 effect direction/rank；
4. 用 item-shuffled、predicate-shuffled 和 latent-world-shuffled controls 检验预测是否
   来自实例身份泄漏；
5. 只在 target pool 解盲后计算 prediction error。

决定性比较是：

```text
same-item / matched prediction
    versus
surface similarity / aggregate-only / shuffled prediction
```

如果 aggregate direction 稳定但 item-level prediction 不超过 shuffled baseline，就得到
真正有区分度的结果：**群体规律保留并不等于实例规律迁移。**

### Experiment 3：外部语义关系的确认，而不是随意增加领域

只有 Experiment 1/2 的 ToM transfer profile 稳定后，才加入一个结构清晰的外部关系，
优先使用 Imperfective NLI 的 process/result distinction：

- 先做 human/专家语义 validity audit，排除 telic/atelic 歧义和 endpoint 未规定问题；
- 使用最小 premise–hypothesis pairs，固定词汇和答案编码；
- 把“关系方向”“中间语义分类”“最终 NLI label”分别报告；
- 测量与 ToM 相同的 `T_sign/T_mag/T_rank/T_operator`，但不假设两域共享机制；
- 如果只有平均方向相似而 item-level 不相似，写跨域的 transfer-profile analogy；
- 如果连平均方向都不相似，不把 ToM 结果外推到一般语言理解。

这个实验的功能是检验研究对象是否超出 ToM，而不是为了堆一个“跨领域”标题。

## 6. 主图和主表

### Figure 1：The transfer profile

四个纵向层级：`sign / magnitude / item rank / operator`；横轴为 construction、topology、
checkpoint 的 transfer pair。读者一眼看到哪些层级保留、哪些层级破裂。

### Figure 2：Same mean, different items

左侧显示 source/target 的 pooled effect；右侧显示 item-level prediction 与 shuffled
baseline。核心视觉不是“两个均值都为正”，而是“均值相似但实例排序不相似”。

### Figure 3：Prospective boundary map

矩阵行是 transfer boundary，列是 new construction、new topology、new checkpoint、
external semantic relation。每个 cell 使用同一四层指标，不显示内部 Stage 编号。

### Table 1：Claim-safe evidence ladder

```text
validity -> aggregate effect -> magnitude -> itemwise transport -> operator prediction
```

每一列写清 sampling unit、控制、区间和最窄允许 claim。

## 7. 最强替代解释及其关闭方法

| 替代解释 | 为什么可能成立 | 决定性关闭方法 |
|---|---|---|
| pooled effect 只是 stimulus heterogeneity | item-level effect 由少数 item 驱动 | hierarchical model、item bootstrap、leave-one-item-out |
| target effect 只是 lexical similarity | construction 替换保留了表面词汇 | exact-length、surface-matched、lexical-similarity baseline |
| item correlation 低只是测量噪声 | score variance 可能过大 | repeated scoring、reliability upper bound、noise-corrected correlation |
| checkpoint 只是 logit scale 改变 | 这是最简单的 transport 解释 | primitive-only affine null、normalized residual、itemwise residual audit |
| semantic validity 不清楚 | construction 可能同时改变 scope/reference | independent blind annotation、endpoint/source invariants、ambiguous-item exclusion |
| 结果只是一个 ToM 模板 | 单一任务不能支持 broader law | 新 latent worlds、cross-realization、一个外部关系确认 |

关闭这些解释后，仍然不能写“没有共享机制”；只能写行为层 transfer profile 和其边界。

## 8. 为什么它有真正的 Oral 潜力

它改变的不是一个 benchmark 数字，而是一个常被混用的概念：

```text
replication of mean effect != replication of instance-level behavior
representation change != law transfer
cross-checkpoint similarity != compositional portability
```

如果 Experiment 2 的前瞻性结果成立，社区在报告“组合泛化”“prompt robustness”或
“跨模板复现”时，就不能只报告 pooled accuracy/effect；至少要区分方向、幅度、实例
排序和组合算子。这是一个可迁移的方法论后果。

它也能自然吸收现有五个分支：

- Branch 1/2：checkpoint 与 composition law 成为 transfer boundary，不再强行声称训练
  因果或唯一算子；
- Branch 3：机制分析只作为“行为迁移不能推出机制迁移”的边界证据；
- Branch 4：conflict topology 是 transfer profile 的一个轴；
- Branch 5：训练目标只有在明确改善 held-out itemwise transport 后才进入后续工作。

## 9. 旧主线的处理

`identification -> commitment -> partner adaptation` 降级为 conditional branch，原因是：

1. LP4FM 当前证据支持 readable belief 与 answer-controlling state 的边界，但不支持
   “关系已读出后被先验覆盖”的新联合测量；
2. partner adaptation 尚无 paired policy-swap 结果；
3. 其中心现象来自理论推演，不是本仓库已经完成的 observation。

它可以在未来作为 Experiment 4，但不能继续充当当前 oral proposal 的母问题。

## 10. 晋级条件与降级条件

### 可以进入 oral candidate 的最低组合

1. Experiment 1 在至少两个 independently generated transfer pairs 上复现
   `T_sign > T_mag > T_rank` 的部分阶梯，或发现一个稳定而不同的 transfer ordering；
2. Experiment 2 的 held-out itemwise prediction 显著超过 aggregate-only 与 shuffled
   controls，或者得到稳定的“均值保留、实例不迁移”边界；
3. 至少一个 checkpoint 或外部语义关系确认同一层级结构；
4. 所有核心结论使用 item/world-clustered uncertainty 和独立 validity audit；
5. 主文只保留一个 transfer profile，不再并列堆 saturation、steering、memory、schema
   和 partner game。

### 必须降级的情况

- 只有 pooled direction，没有 selection-blinded item-level prediction：普通 behavioral
  result，不称 oral-level transfer law；
- item-level prediction 只在同一 item 重建中成立：same-item reconstruction，不称 transport；
- 外部域失败：保留 ToM-specific transfer boundary，不写 general language law；
- 语义 validity 不通过：暂停跨构式 claim，先修 stimulus；
- operator family 不能区分：写 nonadditivity/transfer profile，不命名唯一代数。

## 11. 当前状态

```text
mother object       = scale-dependent transport of semantic composition behavior
validated base      = pooled-vs-item specificity boundary; checkpoint shape rewiring
new decisive work   = selection-blinded prospective itemwise transport
strongest rival     = pooled heterogeneity / measurement noise / surface similarity
current status      = argument_shaped, not_evaluated for the new prospective gate
```

因此，这是一份真正由现有证据推出来的 oral 研究方案；它不是把尚未运行的
identification、commitment 或 partner adaptation 写成既成事实。
