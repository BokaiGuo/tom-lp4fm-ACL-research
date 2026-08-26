# 合并 Oral 研究：学习信念来源访问的组合代数

## 暂定标题

**Learning the Algebra of Belief-Source Access：当指令微调改变语言组合定律**

中文标题可写为：**学习信念来源访问的代数：训练何时改变语言约束的组合规律**。

## 一句话主张

语言模型训练改变的可能不仅是单个语言构式效应的强弱，还包括 independently measured source-access constraints 在 unseen compounds 上的组合算子。

## 母对象

统一研究对象是一个 **checkpoint-indexed source-access composition operator**：

```text
phi_k(C_1, ..., C_m, T)
```

其中 `k` 表示训练轨迹上的 checkpoint，`C_j` 表示 primitive construction 的 behavioral access cost，`T` 表示 typed conflict topology，`phi_k` 预测 compound construction 的 cost 或 binding margin。

核心问题是：

> 训练只是缩放一个固定 composition operator，还是改变了多个语言约束共享 belief-source route 时所遵循的 operator class？

组合代数提供可证伪的 checkpoint-change measurement；训练轨迹则解释这种代数何时、为何变化。

## 为什么必须合并

只研究 checkpoint transition 容易退化为模型对比；只研究单 checkpoint algebra 容易变成窄行为规律。合并后的链条是：

```text
primitive construction costs
-> 冻结 unseen compound predictions
-> 每个 checkpoint 内识别 operator
-> 沿训练轨迹追踪 operator
-> controlled exposure 诱导或阻止 transition
```

代数部分提供紧凑的 predictive object，训练部分提供从 association 走向 causality 的路线，matched Base/Instruct 提供架构控制，affine-gain rejection 排除简单幅度解释，typed topology 提供 source-level identifiability。

## 已有证据位置

Stage-2–6 证明 raw accuracy 是多个 typed controllers 的混合观测。Stage-44–49 证明语言构式改变 source arbitration，且 conflict topology 调节效应。Stage-50–52 将 primitive costs 冻结后预测 compound，并在独立世界上确认 Qwen2.5-7B-Instruct 的 subadditivity。weakest-link 与 average saturation 暂时无法区分。Qwen3 与 Qwen2.5 Base 不保留正 directional residual，affine-gain audit 又排除了统一读出压缩。

## 与 Related Work 的关系

### 两个最近邻文献簇尚未在本对象上汇合

第一类工作研究 compositional generalization 如何依赖示例、数据覆盖和学习算法。
[CoFe](https://arxiv.org/abs/2305.04835) 识别 structural similarity、diversity、
simplicity 和语言结构覆盖；[general CG theory](https://arxiv.org/abs/2405.11743) 给出
task-agnostic limit 与 generative effect；[Fu–Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html)
区分 supporting-data 不足和 knowledge-composition capacity。

第二类工作研究训练中 ICL 和 generalization 何时变化。[Induction Heads](https://arxiv.org/abs/2209.11895)
把 ICL 突变与候选 circuit 联系起来；[Learning Plateaus](https://proceedings.mlr.press/v235/fu24h.html)
区分 weights/context component；[Learning Trajectories](https://arxiv.org/abs/2304.12579)
用 update path 解释 generalization。

本合并分支位于二者交叉处，但定义了更窄的状态变量：

```text
checkpoint k -> typed source-access relation 上的 composition operator
```

问题不是训练是否提高一般 composition accuracy，而是多个 source 竞争同一 answer
interface 时，训练是否改变 independently measured constraints 的组合方式。

### 与 schema shaping 和 source-priority training 的关系

[Shaping Schema](https://arxiv.org/abs/2605.09271) 提出语言表示改变知识激活与组织；
[Instruction Hierarchy](https://arxiv.org/abs/2404.13208) 表明显式训练可安装 channel
priority policy。它们是表示/训练诱导变化的近邻，但没有从 typed primitive source cost
估计 held-out composition operator。它们也构成必须保留的 rival：所谓 transition
可能只是 broad schema change 或显式 priority heuristic。

### 与当前 LP4FM 和 ACG 后继路线的关系

已投稿 LP4FM 只建立静态、checkpoint-sensitive 的 behavioral boundary，不建立纵向
operator transition。本分支必须增加 training trajectory 与 controlled induction，不能
重新命名 Stage-51/52。[Authority Compositional Generalization 方案](../research/next_stage_icl_schema_rewriting_20260826_zh.md)
则进一步研究 demonstrations 是否诱导可跨 content、encoding 和 unseen topology 的
authority relation program；在该框架下，本合并分支是 ACG 的 weights/training lane，
不一定需要独立成稿。

本分支不能声称 ICL dynamics、组合泛化、phase transition 或 schema shaping 本身是新
颖性。独立贡献必须同时具备 held-out source-operator prediction、within-trajectory
change point 和 primitive/data-coverage matched 的 selective induction。直接 baseline
包括 CoFe-style coverage、additive/flexible operators、accuracy/calibration trajectory、
affine transport、instruction-hierarchy training 与 no-compound exposure。完整定位见
[Related Work 定位地图](../research/related_work_positioning_20260826_zh.md)。

## 形式化框架

对 item `i`、checkpoint `k`、construction `c` 与 topology `T`，定义 intended-source 对 strongest typed competitor 的 margin `M_i,k(c,T)`。通过 instruction arms 构造 `Delta_i,k(c,T)`，再以 joint conflict 相对 single conflicts 的差异构造 `Gamma_i,k(c)`。相对 baseline construction `c0` 的 primitive cost 为：

```text
C_i,k(c) = Gamma_i,k(c0) - Gamma_i,k(c)
```

compound 满足：

```text
C_i,k(c1...m)
  = phi_k(C_i,k(c1), ..., C_i,k(cm), T_i) + epsilon_i,k
```

候选 family 包括 additive、weakest-link、average saturation、bounded capacity 与 topology-conditioned mixture。训练轨迹 `Phi(tau)` 同时记录 operator family、parameters、directional residual、control-relative gain、topology dependence 与 itemwise residual geometry。

## 核心假设

**H1 Predictive composition：** 低复杂度 saturating operator 在至少一个 checkpoint 上优于 additive，能预测 unseen binary 与 ternary compounds。

**H2 Checkpoint-dependent operator：** matched training trajectory 上的 operator 或 residual 发生变化，而不仅是 unrelated models 的差异。

**H3 Non-affine transition：** 变化在 primitive-scale matching、calibration matching 和 affine/isotropic gain controls 后仍成立。

**H4 Selective training induction：** controlled compound exposure 对 held-out operator 的影响大于 primitive-only 与 answer-only training。

**H5 Topology-conditioned emergence：** transition 在多个 typed competitors 竞争同一 intended source 时更强，而不是 context-free prompt effect。

## Study A：checkpoint 内识别组合代数

建立至少五类 primitive constraints：reportative/factive attribution、voice/event-state、discourse order/locality、reference/coreference、commitment/negation。生成 balanced binary compounds 与能区分理论的 ternary compounds，并交叉 single/joint conflict topology。

使用 item split、construction split、composition split 三层 held-out。primitive costs 与 operator parameters 必须在 compound evaluation 前冻结。预注册 additive、weakest-link、average saturation、bounded capacity、topology mixture、no-voice 与 shuffled-predicate models。

晋级要求是至少一个 held-out ternary design 区分两个 operator families；若所有 saturation models 仍 observationally equivalent，claim 保留在 class level。

## Study B：沿训练轨迹追踪代数

优先使用 Base、continued pretraining、early/late SFT、preference optimization 与 final aligned checkpoint 的连续轨迹。若不可得，则训练一个相同 tokenizer 与 source-routing benchmark 的 controlled miniature trajectory。

所有 checkpoint 必须使用相同 latent items、candidate ordering、construction wording、primitive/compound split、scorer、bootstrap unit、operator criteria 与 promotion thresholds。比较无变化、纯 scalar amplitude、smooth drift、global change point、predicate-specific 与 topology-dependent transition。

晋级要求是 held-out 轨迹出现超过 primitive scale 与 calibration controls 的 operator change 或 residual transition。

## Study C：受控诱导 operator

设置等预算的 answer-only SFT、primitive typed routing、seen-pair composition、topology-balanced composition 与 shuffled composition control。训练中 withheld 一个 construction family 与一个 compound pair。

决定性问题是 topology-balanced composition training 是否比 primitive-only 和 answer-only 更好地预测 held-out compound。训练不能通过提高所有 logits、抹平构式差异、记忆 answer token、无差别压制非目标来源或只改变 final-token prior 获胜。

只有 controlled exposure 相对 matched controls 选择性改变 held-out operator，才支持 training-time causality。

## Study D：可选机制桥接

该部分不应阻塞 behavioral paper。可在行为 transition 前后 checkpoint 上应用 Stage-36 canonical typed contrast，检验 local typed response change 是否预测 held-out compound residual。必须包含 final-token-only、wrong-channel、random、orthogonal、dose response、flip-budget、paired/shuffled alignment 与 geometry-versus-causal transport controls。

## 决定性三元实验

设置 primitive costs 为 high、medium、low。additive 预测三者之和，weakest-link 预测 high，average saturation 位于 high 与 primitive average 之间，capacity model 由 learned capacity 决定。将同一 ternary design 运行于多个 checkpoints。最强结果不是 residual amplitude 不同，而是不同 checkpoint 由不同 operator class 最好预测。

## 统一晋级门槛

1. **Typed identifiability：** 效应必须是 intended source 对 typed competitors 的变化，而非 raw accuracy。
2. **Held-out composition：** primitive observations 必须预测 unseen compounds。
3. **Operator discrimination：** 至少一个设计区分 competing families，否则保持 class-level claim。
4. **Longitudinal transition：** matched trajectory 上的变化超过 primitive scale 与 calibration。
5. **Controlled induction：** matched training exposure 选择性改变 held-out composition；只有此门槛支持训练因果。
6. **Scope transfer：** 至少一个新 construction family 或 typed domain 确认 operator behavior，才能声称较广泛语言规律。

## Claim Ladder

**Tier 0：当前 LP4FM 结果。** Qwen2.5-7B-Instruct 在 tested reportative/factive families 上独立确认 subadditivity；Base 与 Qwen3 限定 portability。

**Tier 1：Predictive algebra。** 预注册 operator class 预测 tested checkpoint 上 unseen binary 与 ternary compounds。

**Tier 2：Learned operator trajectory。** operator 沿 matched training trajectory 系统变化，且不能由 primitive margin 或 affine gain 解释。

**Tier 3：Training-time causal result。** controlled source-attribution composition exposure 选择性诱导、移除或改变 held-out operator。

**Tier 4：General training principle。** controlled result 迁移到另一个 construction family 或 typed routing domain，同时保留有效语言差异。

## 禁止性 Claim

没有额外证据时不能写：instruction tuning 普遍地产生 subadditivity；operator 唯一是 weakest-link/min/tropical；behavioral operator 等于 localized hidden circuit；所有 foundation models 共享一个 algebra；结果是人类 psycholinguistic universal；robustness improvement 已修复 internal belief mechanism。

## 论文叙事

```text
accuracy 无法识别 controlling source
-> typed topology 暴露 source-routing effects
-> primitive costs 定义 competing composition laws
-> unseen ternary compounds 识别 operator class
-> training checkpoints 显示 operator 何时变化
-> controlled exposure 检验训练是否诱导变化
-> affine gain、calibration 与 final-token alternatives 被排除
```

## 主图规划

Figure 1 展示 typed source-access algebra；Figure 2 展示 held-out binary/ternary operator identification；Figure 3 展示训练轨迹上的 operator class、residual、primitive scale 与 change point；Figure 4 展示 controlled induction；Figure 5 汇总 affine gain、calibration、lexical、contamination 与 optional mechanism controls。

## 最小可行版本

先构建一个 diagnostic ternary panel，在 Qwen2.5 Base 与 Instruct 上运行完全相同的 typed interface，判断两个 checkpoint 是否偏好不同 operator class；随后在 Base 上运行 primitive-only 与 composition-exposure LoRA，评价一个 unseen compound pair 与独立 latent pool。

## 完整 Oral 版本

需要同时满足：predictive ternary operator、matched longitudinal transition、controlled induction、排除 primitive scale/calibration/affine/lexical alternatives、至少一个 construction-family 或 typed-domain transfer，以及明确区分 behavioral algebra 与 hidden mechanism 的 claim boundary。

## 执行路线

第一步扩展 Stage-52 为 ternary benchmark 并冻结哈希；第二步完成 Base/Instruct viability test；第三步运行 controlled LoRA pilot；只有 pilot 通过后才加入 intermediate checkpoints；最后增加一个独立 construction family 或 typed routing domain。

## 决策规则

只有当 ternary pilot 能区分 operator，或 controlled training 能选择性改变 held-out composition，才继续作为合并 oral 项目。若两者都失败，应保留当前 LP4FM 稿件，并转向 mechanism-identifiability 分支，而不是继续扩展模型矩阵。
