# Oral-shape 升级包：从关系识别到最终承诺

> **状态修订（2026-09-12）：** 本文档中的 `identification -> commitment -> partner
> adaptation` 已降级为条件性分支。当前更有证据基础的 oral 主线见
> [有证据支撑的 Oral 研究方案](evidence_grounded_oral_research_20260912_zh.md)。

更新日期：2026-09-12

## 0. 直接结论

本项目现在可以进入 `oral-shaped design`，但还不能写成经验上已经 oral-ready：

```text
设计状态       = oral_shaped
经验状态       = not_evaluated
论文状态       = argument_shaped
```

原因很明确：中心对象已经从一组机制现象收紧为一个可被三张主图检验的功能链，
但决定论文能否晋级的三组新数据尚未完成：

1. 诊断性示例是否比数量、相似度和多样性更能预测未见 policy 的执行；
2. 关系已经可读出时，最终答案是否仍被默认先验改写；
3. 模型是否根据伙伴的 latent policy 改变新内容和未见拓扑上的行动。

这不是把旧实验重新包装成 oral。它是把 LP4FM 的静态发现转成一篇具有新中心命题、
新测量单位和新功能后果的下一篇论文。

## 1. 唯一中心命题

论文暂定标题：

> **Conflict Identifies, Priors Decide: Identification to Commitment in Language Models**

中文主张：

> **上下文示例只有在排除竞争关系时才真正提供关系监督；关系一旦被识别，最终答案
> 仍可能在先验竞争处重新服从默认来源。只有关系识别、最终承诺和跨内容执行同时
> 成立，模型才表现出功能性的关系使用。**

最短的读者复述是：

```text
demonstrations -> policy identification -> final commitment -> partner adaptation
```

主文不声称“内部真的知道”、不声称 intrinsic ToM，也不声称发现一个统一神经模块。
`relation–commitment dissociation` 是本文的操作性描述，不是领域既有术语。

## 2. 与 LP4FM 的关系

LP4FM 是 Paper A：

```text
language form -> typed source arbitration under fixed latent worlds
             -> checkpoint-sensitive composition boundary
             -> same-item reconstruction != portable typed control
```

下一篇是 Paper B，不重复 LP4FM 的 saturation、affine transport 或 final-token 机制。
LP4FM 只承担三种作用：

| Paper A 提供 | Paper B 如何使用 | Paper B 不再声称 |
|---|---|---|
| typed source arbitration | 定义关系和竞争来源 | 语言形式本身是通用控制器 |
| primitive/compound boundary | 动机：关系可读不等于关系被兑现 | saturation law 已跨模型成立 |
| mechanism negative boundary | 约束 claim hygiene | 找到 hidden-state causal circuit |

## 3. 三个主实验

### Experiment 1：Information, not example count

固定词面、长度、答案频率、来源顺序和输出编码，构造：

```text
C1/C2/C4/C8/C16 consensus demonstrations
D1 target-policy diagnostic demonstration
D1 + rival-policy diagnostic demonstration
shuffled / corrupted / answer-permuted controls
```

候选 policy 集合为 `Pi`。对示例集 `D`，计算：

```text
C_sep(D) = 被至少一个示例区分的 policy-pair 比例
VR(D)    = log |Pi| - log |V(D)|
```

主 estimand 不是“多一个例子提高多少准确率”，而是：

```text
held-out policy performance ~ policy-separation coverage
```

并与 count、similarity、lexical diversity、structural coverage 做同协议比较。
只有在 prospective held-out split 上 `C_sep/VR` 增加预测力，selector 才进入主文；
否则 selector 留在 supplement。

### Experiment 2：Identification × Prior Pressure

先在不重叠的 calibration split 上估计默认 policy，不能根据主实验答案事后定义 prior：

```text
prior pressure = log P(y_prior | neutral context)
                - log P(y_relation | neutral context)
```

主实验采用匹配后的 2×2：

| | prior-aligned | prior-opposed |
|---|---:|---:|
| consensus / non-diagnostic | identification 未完成 | default baseline |
| diagnostic | control condition | **关键格：关系已读出但最终承诺可能被 prior 覆盖** |

每个 cell 同时报告：

1. independent relation readout；
2. native final answer；
3. explicit-rule oracle；
4. wrong-channel、shuffled、answer-permutation 和 direct-compound controls。

关键联合量为：

```text
RR = relation readability
FC = final relation commitment
PO = Pr(readout = relation, native answer = prior)
CD = Pr(readout = relation, native answer != relation)
```

只有 `RR` 在 prior-opposed cell 保持、而 `PO/CD` 显著高于所有测量与格式控制，才可
使用 relation–commitment dissociation。若 `RR` 本身失败，只能报告 identification 或
retention failure。

### Experiment 3：Functional partner adaptation

静态“回答 Alice 相信什么”不是功能性社会推理。模型先观察 partner 的 consensus 和
diagnostic interactions，再在新人物、新对象、新表达和未见 topology 中选择沟通或行动。

最重要的设计是 paired partner-policy swap：只改变 partner 的 governing source policy，
保持表面内容和奖励结构不变。主结果必须同时显示：

```text
partner policy inferred -> native action changes -> held-out coordination improves
```

控制包括：random partner、no-conflict partner、source-order、label-only 和
same-policy replay。没有 policy swap，不能把 partner 行为变化称为 policy adaptation。

## 4. Oral 晋级门槛

| 门槛 | 必须回答的审稿人问题 | 当前状态 | 晋级条件 |
|---|---|---|---|
| Importance | 为什么准确率不足以描述 ICL？ | design pass | 明确 identification/commitment 的新测量单位 |
| Discovery | 现象是否不是 prompt 偶然？ | pending | validity audit + 多个独立 latent worlds |
| Prediction | 是否有可提前计算的关系量？ | pending | `C_sep/VR` 在 held-out 上胜过 count/similarity |
| Identification | 中间读出是否不是最终答案的变体？ | pending | independent readout + oracle + wrong-channel controls |
| Consequence | 关系是否改变真实功能行为？ | pending | partner-policy swap 改变 held-out native action |
| Closure | 替代解释是否被系统排除？ | design pass | prior calibration、语义审核、置换和匹配控制全部冻结 |
| Compression | 是否能用一张图讲清楚？ | design pass | 三张主图分别对应信息、承诺、适应 |

当前最重要的事实是：前四个门槛中只有设计完成，数据门槛尚未通过。因此不能现在就
把项目写成“Best Paper result”。

## 5. 三张主图和一张表

### Figure 1：Policy information curve

- 横轴：example count 与 `C_sep/VR`；
- 纵轴：held-out policy-consistent decision；
- 颜色：consensus、diagnostic、rival-diagnostic；
- 误差：按 latent world 聚类的 bootstrap CI；
- 旁图：不同信息量指标的 prospective prediction gain；
- 非主张：不是在证明一个普适贝叶斯学习器。

### Figure 2：Identification does not guarantee commitment

- 左：independent relation readability；
- 中：native final commitment；
- 右：prior override / dissociation rate；
- 用 2×2 prior-aligned/opposed × consensus/diagnostic 排列；
- 关键 cell 必须是 prior-opposed + diagnostic；
- 非主张：不是 hidden-state causal localization。

### Figure 3：Partner-policy swap

- 横轴：partner policy A/B；
- 纵轴：held-out first action、message choice、coordination reward；
- 分面：new content、new representation、unseen topology；
- 只显示 native action 与 paired swap；
- 非主张：不是 intrinsic ToM 或人类同理心。

### Table 1：Relation pipeline

```text
validity -> identification -> readability -> retention -> commitment -> execution
```

每一格填入 estimand、sampling unit、控制和允许的最窄 claim，避免把 probe、steering、
same-item reconstruction 和跨内容行动混成一个“理解分数”。

## 6. 最小执行顺序

### Phase 0：冻结协议

冻结 stimulus manifest、candidate policies、答案编码、prior calibration split、
relation readout wording、随机种子和分析脚本。主实验前不再根据结果增删关键 cell。

### Phase 1：先跑 Experiment 1

优先验证 `C_sep/VR` 是否具有 prospective value。若没有超越 count/similarity，立即
把 selector 降为 supplement，不让一个分析量拖累主论文。

### Phase 2：跑 Experiment 2

只有在 validity 和 identification 通过后，才解释 prior-opposed cell。readout 与 native
answer 必须用不同协议、不同输出通道或规则 oracle，避免循环定义。

### Phase 3：跑 Experiment 3

只保留能通过 policy swap 的 partner panel。若模型能回答 source question 但不改变
held-out action，结论收紧为 relation/commitment boundary，不写 functional adaptation。

### Phase 4：最小复制

先在一个主模型完成三实验，再用一个匹配 checkpoint 或模型族复现主签名。不要在主结果
未稳定前扩展到 event semantics、local attention、memory、steering 或 controlled tuning。

## 7. 明确的停止和降级规则

| 结果 | 论文处理 |
|---|---|
| `C_sep/VR` 不预测 held-out use | 删除 selector 主张，保留 identification 现象 |
| readout 与 native answer 同步失败 | 不称 commitment dissociation，报告 relation access failure |
| readout 正确但 prior-opposed native answer 跟随 prior | 主结果：bounded relation–commitment dissociation |
| partner swap 不改变 action | 删除 functional adaptation，只保留识别/承诺边界 |
| 只在一个模板或同一 item 生效 | 作为 exploratory，不进主结论 |
| 机制干预失败 | 作为 boundary，不否定行为规律，也不由 probe 拯救行为规律 |

## 8. 与近邻研究的真正区别

- 与 Imperfective Paradox：同样借鉴“细粒度关系 + 中间判定/最终标签分离”，但本文的
  单位是 demonstration-set/target policy induction，并要求 held-out partner action。
- 与 ICL task identification：不把“关键示例”本身当新发现，而定义其价值为区分多少
  竞争 policy，并继续追问识别后是否能抵抗 prior。
- 与 belief probing/steering：不把 readable、steerable 或 same-item shift 当作使用；
  必须经过 native commitment 和 cross-content execution。
- 与 implicit adaptation：只使用“暴露后行为适应”这类可观察描述；不把它升级为无意识
  记忆或人类认知系统。

## 9. 最终 oral-shape 判断

这项研究真正可能成为 oral 的地方不是“LLM 又有一个失败 benchmark”，而是它改变了
研究者如何测量关系性 ICL：

```text
example count      -> policy-separation information
accuracy           -> relation commitment
probe readability  -> native use
same-item answer   -> held-out partner adaptation
```

若三张主图按上述门槛成立，论文可以提出一个清晰的、可迁移但有边界的更新：

> **上下文学习不是从示例直接跳到答案；它至少包含关系识别和最终承诺两个瓶颈，
> 而真正的功能性使用还要求对新内容中的伙伴策略采取行动。**

若其中任何一条失败，仍然有一篇清楚的普通论文路径，但不应以 Best Paper 或 intrinsic
understanding 的语言补偿证据不足。
