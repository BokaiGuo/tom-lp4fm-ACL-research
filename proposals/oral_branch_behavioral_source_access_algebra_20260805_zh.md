# Oral 分支二：来源可访问性的行为组合代数

## 暂定标题

**信念来源访问的行为代数**

备选标题：**当语言约束发生组合：检验 typed source access 的组合规律**。

## 一句话主张

语言构式可以被视为作用于共享 typed source-access route 的算子；其影响通常不满足简单加法，而可以通过冻结 primitive costs、预测未见 compounds 来识别组合类别。

## 为什么比当前 saturation claim 更强

Stage-52 已在 Qwen2.5-7B-Instruct 上确认正 subadditive residual，但 weakest-link 和 average-saturation predictor 都优于 additive null，且彼此无法区分。因此当前证据识别的是 saturation class，而非唯一 operator。本分支不掩盖这一 non-identification，而是直接把“哪个 operator 能预测未见组合”作为母问题。

## 可复用证据

Stage-48 验证 conflict-topology moderation；Stage-49 显示 topology kernel 的方向跨 realization 稳定，但 gain 对构式敏感；Stage-50 拒绝过窄的局部 reportative-scope mechanism；Stage-51 揭示 additive cost 对 held-out compound 的系统性 overshoot；Stage-52 在独立 latent worlds 上确认 subadditive residual。Base 与 Qwen3 则构成 operator portability 的重要边界。

## 形式化对象

令 `C(c)` 表示构式 `c` 的 item-level source-access cost。对三元 compound `c123`，候选算子包括：

```text
additive:          C1 + C2 + C3
weakest-link:      max(C1, C2, C3)
smooth saturation: f(C1, C2, C3; theta)
capacity sharing:  sum(Cj) - shared_capacity(C1, C2, C3)
topology mixture:  phi(C1, C2, C3, T)
```

所有分析必须在 typed、topology-moderated margin space 中进行，而不是 raw accuracy。operator 只能在 primitive cells 上拟合，然后预测完全未见的 compound cells。

## 研究问题

1. 三个及以上语言约束同时出现时，subadditivity 是否仍成立？
2. compound 是否由最弱 source route 决定，还是多个约束共享有限 accessibility capacity？
3. conflict topology 改变的是 operator 本身，还是只改变 gain 与方向？
4. reportative 与 factive 是否属于同一个 algebra？
5. 预测新 construction family 需要多少 primitive observations？

## 实验设计

### Factorial construction library

至少包含 attribution/evidentiality、voice/event-state、discourse order/locality、reference/coreference、commitment/negation 五类 primitive。生成所有可行 binary compounds 与一组具有区分力的 ternary compounds。在 item cluster 内固定 latent world、candidate answers、target source 和 context-length pair。

### Operator-level train/test split

关键 split 不仅要 item-disjoint，还必须 construction-disjoint。primitive families 用于拟合 operator；一个 held-out binary family 用于组合测试；另一个 held-out ternary family 用于外推。更强版本完全 withheld 一个 topology-construction interaction。

### 预注册模型比较

在查看 held-out compounds 前冻结 additive、weakest-link/max、average saturation、smooth-min、bounded-capacity、topology interaction、no-voice 和 shuffled-predicate models。评价指标包括 item-cluster bootstrap、predictive log score、rank correlation、held-out residual error 与 complexity penalty。

## 决定性实验

设计 primitive costs 为 high、medium、low 的三元组合。weakest-link 预测 high；shared-capacity 预测在第一个 bottleneck 后增量受限；additive 预测最大 suppression。该设计比再次观察一个 binary positive residual 更能区分理论。

## 控制

需要匹配 primitive margin、answer-token frequency、token length、perplexity、construction order；加入 no-voice、shuffled-predicate、no-source、same-item crossed topology、item-disjoint 与 family-disjoint splits，以及 affine/isotropic gain controls。

## 晋级门槛

**预测门槛：** selected operator 在 held-out binary 与 ternary compounds 上显著优于 additive 和 no-structure baseline。

**区分门槛：** 至少一个 held-out design 能区分 weakest-link、average saturation 与 capacity sharing；若仍无法区分，只能保留 class-level claim。

**拓扑门槛：** operator 或其参数必须随 typed conflict topology 可预测地变化，而非只随表面长度变化。

**跨 family 门槛：** 至少一个独立 construction family 保留预注册 ordering。

## Claim Ladder

当前可写：受控 Qwen2.5-7B-Instruct setting 中存在 subadditive saturation class。

三元确认后可写：一个命名的 operator class 能预测测试范围内的 unseen compounds。

跨 family transfer 后可写：该 operator 是 tested construction families 上可复用的 typed source-access primitive。

不能写：唯一 min operator、hidden neural accessibility unit、人类普遍规律或 unrestricted natural-language generality。

## Oral 叙事

论文应被组织成一个预测任务：测量 primitive access costs，冻结参数，预测 unseen compounds，让 ternary constructions 在 competing laws 之间做裁决。主图应展示 primitive fits、多个 operator 的分歧预测、held-out ternary outcome 与 topology-conditioned residual。

## 失败解释

若 additive 获胜，则当前 saturation 是 family-specific boundary；若所有 bounded saturation models 仍打平，则把 operator non-identification 写成正式结果；若 topology 只改变 gain，则保留 topology kernel 加 construction gain model；若三元 compound 破坏所有模型，则转向 context-dependent capacity。

## 最小下一步

构建一个包含五类 primitives、3–4 个 construction-disjoint held-out cells 的 ternary pilot。在 pilot 区分至少两个 operator families 之前，不增加更多模型。

