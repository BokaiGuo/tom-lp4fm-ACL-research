# Oral 分支四：作为通用诊断的 Conflict-Topology Kernel

## 暂定标题

**超越 Prompt Sensitivity：用于来源路由诊断的冲突拓扑核**

备选标题：**谁必须击败谁？Typed Conflict Topology 预测语言模型的来源路由错误**。

## 一句话主张

许多表面不稳定的 wording effects 可以被分解为一个由 typed source conflict 决定的方向稳定 kernel，以及一个由具体语言构式调节的 gain。

## 独立研究对象

传统 prompt robustness 通常按 wording、template 或 task 汇总结果。Stage-48/49 暗示更稳定的对象是“目标来源必须击败哪些 typed competitors”。语言形式主要改变冲突的表达强度，而 topology 决定 source routing 的方向。

本分支要识别：

> 一个能够跨 prompt family 预测 controller changes 的 conflict-topology kernel。

只有当 kernel 能预测新的 domain 与 prompt family，而不是拟合现有 false-belief matrix，它才构成通用 behavioral diagnostic。

## 可复用证据

Stage-2/6 建立 typed candidate space；Stage-44/46 表明语言效应需要独立 latent sources；Stage-47/48 验证 language-form × topology interaction；Stage-49 显示 directional topology pattern 跨四个 controlled realizations 稳定，但 gain 依赖构式；Stage-50–52 进一步证明 topology-moderated margin 可作为 accessibility 与 composition 分析的测量底座。

## 与 Related Work 的关系

### 与行为 stress test 的关系

[CheckList](https://aclanthology.org/2020.acl-main.442/) 和 [contrast sets](https://aclanthology.org/2020.findings-emnlp.117/)
建立了 controlled perturbation 与局部 decision-boundary 测试范式；[FANToM](https://arxiv.org/abs/2310.15421)、
[Clever Hans](https://aclanthology.org/2024.eacl-long.138/) 和 [DynToM](https://aclanthology.org/2025.acl-long.1171/)
则表明 ToM 表现会在信息不对称、对抗改写和动态状态下失效。这些工作提供实验纪律，
但主要输出仍是 correctness、consistency 或 error rate。

本分支提出的候选 invariant 是有向图：**哪个 typed source 必须击败哪些 competitors**。
只有当该图能在 surface realization、domain 和 candidate wording 改变后预测 source
capture 方向，kernel 才超出一般 behavioral testing。

### 与 instruction hierarchy 和知识冲突的关系

[The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) 规定 system/user/third-party
的显式优先级；[Context-faithful Prompting](https://arxiv.org/abs/2303.11315) 研究参数知识
与上下文冲突；[instruction–data separation](https://arxiv.org/abs/2403.06833) 研究模型
是否区分待执行和待处理文本。本分支不替代这些 taxonomy，而是将它们抽象成共同的
behavioral topology，并检验该对象能否跨 epistemic、temporal、deontic、spatial 和
document-authority domain 迁移。

### 与 shared-router 机制工作的关系

[Belief–reality routing](https://arxiv.org/abs/2607.11945) 和 [One mechanism for many
mental spaces](https://arxiv.org/abs/2607.10248) 提出共享 router/value-slot 机制，是最
接近的机制替代解释。它们定位“mental-space selection 如何在内部实现”；本分支只
定位“何时、朝哪个 source 发生 routing failure”。behavioral kernel 不能直接写成
shared neural router，除非另有独立 mechanism bridge。

本分支不能声称 prompt robustness、source conflict 或 graph representation 是新的。
可成立的新颖性只能是：一个 compact topology kernel 在 held-out prompt family 和新
domain 上优于 surface similarity、lexical overlap、context position、显式 hierarchy
label 与 raw accuracy。Stage-49 目前只支持 within-setting candidate。完整定位见
[Related Work 定位地图](../research/related_work_positioning_20260826_zh.md)。

## 形式化对象

将候选来源视为图节点 `V={v_1,...,v_K}`。任务实例定义有向 conflict graph `T`，边 `v_a -> v_b` 表示 intended source `v_a` 必须击败 competitor `v_b`。对构式 `c`，定义跨 topology cells 的 effect vector：

```text
Delta_i(c) = [Delta_i(T_1,c), ..., Delta_i(T_m,c)]
```

kernel 假设为：

```text
Delta_i(c) ≈ g_i(c) K_i(T) + epsilon_i(c,T)
```

其中 `K_i(T)` 是方向稳定的 conflict-topology kernel，`g_i(c)` 是 construction-sensitive gain，`epsilon` 是不能被 factorization 解释的 residual interaction。

## 跨领域任务

至少覆盖两个非 ToM domains。候选包括：temporal routing 中的 prior/report/current/future state；deontic routing 中的 system rule、user preference、quoted authority 与 distractor policy；spatial routing 中的 observer/actor/world/distractor frame；multi-document authority routing 中的 primary source、correction、quoted secondary source 与 non-authoritative retrieval。

## 实验计划

首先在每个 domain 预先定义 typed source ontology，确保每个 answer 唯一映射到一个 channel，并对 candidate tokens 做角色 counterbalance。随后固定 latent world，交叉 queried source、competitor set、single/joint conflict、language construction 与 distractor pressure。

使用 discovery prompt families 估计 sign-only、rank-one kernel plus gain、low-rank domain-specific gain 和 fully saturated table。只有在 held-out prompt families 上，低秩 kernel 仍接近或优于 saturated table，kernel 才具有解释价值。

冻结 `K(T)` 后，依次预测同 domain 新 realization、新 construction family、新 latent pool、新 domain，以及可选的新 checkpoint。主要目标不是预测最终对错，而是预测哪一个错误来源会取得控制权。

## Baselines

比较 raw accuracy、prompt length、perplexity、sentence embedding similarity、lexical overlap、template ID、answer-token prior、construction-only model、fully saturated table 与 shuffled topology labels。

## 指标

报告 typed source capture、target-versus-strongest-competitor margin、controller swap confusion matrix、kernel sign agreement、held-out topology rank correlation、source-identity log score、超越 construction-only baseline 的 explained variance 与 cross-domain alignment error。

## 决定性预测

1. topology kernel 的 sign pattern 比 wording magnitude 更可迁移；
2. construction 主要缩放 kernel，而 topology 改变 source ordering；
3. joint conflict 相对 single conflicts 产生可预测偏离；
4. accuracy 相似的模型可以具有不同 kernel rank 或 leak topology；
5. Base 与 Instruct 可能共享部分 topology structure，但在 gain 与 compound composition 上不同。

## 晋级门槛

**域内预测：** frozen kernel 在 held-out prompt family 上优于 length、perplexity、embedding 和 construction-only baselines。

**跨域 transfer：** 至少两个非 ToM domains 保留有意义的 kernel alignment 或共享低秩 conflict factorization。

**错误来源预测：** 必须预测具体 typed competitor，而非只预测正确率下降。

**简约性：** kernel 必须在 complexity penalty 或严格 held-out evaluation 下优于 saturated table，否则只是重新参数化。

## Claim Ladder

当前可写：typed conflict topology 在受控 false-belief worlds 中调节语言效应。

域内预测后可写：conflict-topology kernel 能预测 audited domain 的 held-out prompt-family routing。

跨域确认后可写：conflict topology 是 tested domains 上可复用的 source-routing diagnostic。

不能写：所有 prompt sensitivity 都由 topology 解释、kernel 是 hidden neural circuit、人类推理遵循同一图结构或结果可无限制跨任务迁移。

## Oral 叙事

论文应展示：词面差异很大的 prompts 可以共享同一个 routing kernel，而几乎相同的 prompts 可能因为 source conflict 不同而产生不同 controller。主图包括 typed conflict graph、learned kernel、held-out predictions、cross-domain confusion matrices 和 construction-specific residual failure cases。

## 最小下一步

实现一个 temporal-routing 与一个 deontic-routing stimulus family，保持与 Stage-48/49 相同的四通道 typed interface，然后检验 topology factorization 是否比 prompt embeddings 更好地预测 held-out construction effects。
