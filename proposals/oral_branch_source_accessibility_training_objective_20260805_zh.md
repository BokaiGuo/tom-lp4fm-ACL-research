# Oral 分支五：Source-Accessibility 训练目标

## 暂定标题

**在语言压力下训练模型维持 Typed Source Routing**

备选标题：**既鲁棒又不失明：校准语言变化下的来源访问**。

## 一句话主张

有效的 robustness objective 应在避免无关 wording pressure 把答案路由到错误 typed source 的同时，保留模型对真正语言语义差异的敏感性。

## 为什么标准鲁棒训练不够

普通 consistency training 鼓励 paraphrases 给出相同答案，但这可能通过抹平 attribution、reference、commitment 或 evidential scope 的真实差异来提高 benchmark accuracy。当前项目提供了更精细的训练目标：明确应该由哪个 source 控制答案、错误时被哪个 competitor 捕获、构式如何改变 source margin、多个约束如何组合，以及 off-policy leak 是否增加。

目标不是不变性，而是 calibrated source routing。

## 可复用证据

Stage-2/6 提供 typed channels 与 arbitration metrics；Stage-44–49 提供 controlled linguistic axes 与 topology-conditioned effects；Stage-50–52 提供 primitive costs 和 compound residual；Stage-13–20 说明 final-token success 不能自动解释为内部 source correction；Base/Instruct/Qwen3 提供自然的训练前后边界。

## 与 Related Work 的关系

### 与 instruction priority 和 context faithfulness 的关系

[The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) 训练模型遵守显式 privilege
ordering；[Context-faithful Prompting](https://arxiv.org/abs/2303.11315) 用 opinion-based
prompt 和 counterfactual demonstrations 提高知识冲突时的上下文忠实性；[instruction–data
separation](https://arxiv.org/abs/2403.06833) 则形式化模型能否区分待执行和待处理文本。

这些都是直接 baseline，因为它们都改变 competing text sources 对答案的影响。但它们
优化固定 hierarchy、context preference 或 instruction/data boundary；本分支的正确
source 随 item 和 topology 改变。目标是 **conditional source authority**，而不是无条件
“更听上下文”。

### 与 consistency 和 representation adaptation 的关系

paraphrase consistency 可能通过抹平所有 construction effect 获胜。[IN2 training](https://arxiv.org/abs/2404.16811)
证明定向数据可增强长上下文的信息感知和跨片段整合；[ReFT](https://arxiv.org/abs/2404.03592)
与 [activation steering](https://arxiv.org/abs/2410.12877) 则说明 representation-level
intervention 可以高效改变行为。这些方法并不保证 intended typed source 而非 label 或
generic instruction feature 获得控制，因此必须在相同 source margin、answer
permutation、wrong-channel 和 off-policy leak 指标下比较。

### 与 schema shaping 和组合训练的关系

[Shaping Schema](https://arxiv.org/abs/2605.09271) 强调表示方式可重组知识激活；
[Fu–Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html) 区分数据不足与
algorithm–rule compatibility。这意味着训练成功可能只是增加 primitive coverage 或
改变 representation compatibility，不等于学习了普遍 composition law。

本分支不能声称 instruction robustness、context faithfulness 或 representation
finetuning 是新的。可成立的新颖性是：在 unseen construction 上改善 typed routing，
同时保留合法 linguistic source change 的敏感性。决定性结果应是 wrong-source
suppression 与 valid-language sensitivity 的 Pareto frontier，并与 standard SFT、
consistency、hierarchy training、context-faithful prompting 和 ReFT/steering 匹配比较。
完整定位见 [Related Work 定位地图](../research/related_work_positioning_20260826_zh.md)。

## 训练目标

对 item `i`、construction `c`、topology `T`、intended source `s*`，定义 typed margin：

```text
M_i(c,T) = logit(s*) - max_{s != s*} logit(s)
```

总目标可写为：

```text
L = L_task
  + lambda_route L_route
  + lambda_topology L_topology
  + lambda_comp L_composition
  + lambda_leak L_leak
```

`L_route` 提高 intended-source margin，但保留完整 competitor distribution；`L_topology` 约束 matched worlds 中 single/joint conflict 的预注册 ordering；`L_composition` 校准 unseen compounds 上的 operator class，但不能把当前 Qwen2.5-Instruct residual 硬编码成普遍真理；`L_leak` 惩罚 wrong-channel 与 distractor source 的非特异性增长。

## 研究问题

1. typed routing 能否在 unseen constructions 上改善，同时不抹平有效 linguistic effects？
2. source margin、topology consistency、composition calibration 与 leak control 中哪些是必要成分？
3. 改善能否迁移到独立 latent worlds，而非记忆 templates？
4. objective 是否跨 epistemic、temporal、deontic source routing 迁移？
5. 行为修复对应更早的 non-final controller，还是更强的 final-readout policy？

## 实验条件

设置五个等预算条件：standard answer-only SFT；paraphrase consistency；typed routing loss；topology-balanced routing；加入 composition calibration 与 leak penalty 的 full objective。只有当 representation-level regularization 击败 final-token-only controls，才允许把 Stage-3/36 的表征对象用于训练。

## 数据划分

必须同时使用 latent-item、template/realization、construction-family、compound-pair、topology、可选 domain 和 checkpoint-seed splits。primary test 至少同时 item-disjoint 与 construction-disjoint；随机 prompt split 不足以支持 generalization claim。

## 评价指标

报告 intended-source margin、typed source accuracy、controller confusion、off-policy leak、topology interaction sign agreement、primitive/compound calibration error、held-out operator residual、对有效语义差异的敏感性，以及标准 task utility 与 calibration。

## 核心科学张力

真正对象是以下 Pareto frontier：

```text
source-routing robustness
versus
linguistic-semantic sensitivity
```

如果模型在所有 constructions 下都给出相同答案，它可能看似鲁棒，却失去了对 source attribution 的有效敏感性。成功 objective 应减少无关 capture，但在语言真实改变来源或 commitment structure 时保留差异。

## 决定性实验

第一，训练时 withheld 一个 construction family，评价 unseen construction repair。第二，只训练 primitives 与部分 compounds，评价一个 withheld compound pair。第三，构造 wording 改变但 intended source 不变，以及微小语言变化真正改变 source attribution 的 semantic counterfactual pairs。第四，比较训练后行为与 final-token-only、wrong-channel intervention signature，防止把 output prior 强化误写成 source-binding repair。

## 晋级门槛

**Typed improvement：** intended-source routing 改善且特定 competitor capture 减少，而非只提高 final accuracy。

**语言敏感性保持：** 有效语义 contrasts 仍可区分；全局 flattening 失败。

**组合泛化：** 改善必须迁移到 unseen compound 或 construction family。

**Baseline separation：** full objective 在等预算下优于 standard SFT、paraphrase consistency 与 typed-margin training。

**Scope honesty：** non-final causal evidence 未击败 final-token controls 时，只能声称 behavioral source-routing repair。

## Claim Ladder

当前可写：repo 已提供 typed source-routing diagnostics 与 checkpoint-sensitive composition。

训练成功后可写：typed source-access objective 改善 held-out routing，同时保留有效语言差异。

跨 domain 后可写：该 objective 是 tested source-routing domains 上可复用的训练原则。

机制验证后才可写：训练改变了一个能预测行为修复的 named non-final source-control object。

不能写：模型因此获得 human-like ToM、prompt sensitivity 被消除，或内部 belief mechanism 已修复。

## Oral 叙事

论文从 naive robustness 的失败开场：当语言改变说话者、commitment 或 governing source 时，强制不变性本身可能是错的。主图应展示 robustness–sensitivity Pareto frontier、训练前后的 typed controller confusion 与 unseen compound performance。

## 最小下一步

在一个 Base checkpoint 上运行四个 adapter/LoRA 条件：standard SFT、paraphrase consistency、typed routing、typed routing plus leak control。先在 Stage-49/52 的 item-disjoint 与 construction-disjoint cells 上评价，再决定是否加入 composition regularization。
