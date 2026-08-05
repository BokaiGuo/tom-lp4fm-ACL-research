# Source-Accessibility Saturation Theory 中文版

## 范围

本文形式化当前论文的行为对象。代数恒等式在定义给定后严格成立；saturation residual 的符号则是经验命题，目前仅在受审计模型与 construction family 范围内得到验证。

## 记号

令 `i` 表示 latent story item，`a` 表示 linguistic instruction contrast，`T` 表示 typed conflict topology。`M_i(a,T)` 表示 intended source 相对 strongest typed competitor 的 answer-interface binding margin。对 instruction arms `a+` 与 `a-`：

```text
Delta_i(T) = M_i(a-,T) - M_i(a+,T)
```

定义 topology-moderated effect：

```text
Gamma_i(c)
  = Delta_i(role+object,c)
    - 1/2 [Delta_i(role,c) + Delta_i(object,c)]
```

`Gamma_i(c)>0` 表示该语言 contrast 在 joint typed conflict 下比 single conflicts 的平均效应更正。以 unembedded active baseline `c0` 定义 access cost：

```text
C_i(c) = Gamma_i(c0) - Gamma_i(c)
```

## 定义一：Additive Null

对 primitive constraints `c1,c2` 与 compound `c12`，独立成本假设预测：

```text
C_i^add(c12) = C_i(c1) + C_i(c2)
```

等价地：

```text
Gamma_i^add(c12)
  = Gamma_i(c1) + Gamma_i(c2) - Gamma_i(c0)
```

## 定义二：Saturation Residual

定义：

```text
S_i(c1,c2)
  = Gamma_i(c12) - Gamma_i^add(c12)
```

总体 residual 为 `S=E_i[S_i]`。论文的主要经验命题是：当两个 construction 都限制 intended source access 时，`S>0`。

## 命题一：成本—残差恒等式

对每个 item 与 compound construction：

```text
S_i(c1,c2) = C_i(c1) + C_i(c2) - C_i(c12)
```

因此 `S_i>0` 当且仅当 compound access cost 严格次可加。

证明由定义直接展开：

```text
S_i
= Gamma_i(c12)-Gamma_i(c1)-Gamma_i(c2)+Gamma_i(c0)
= [Gamma_i(c0)-Gamma_i(c1)]
  +[Gamma_i(c0)-Gamma_i(c2)]
  -[Gamma_i(c0)-Gamma_i(c12)]
= C_i(c1)+C_i(c2)-C_i(c12)
```

## 命题二：Saturating Aggregator 的界

假设 primitive costs 非负，compound operator `phi` 满足：

```text
max(C1,C2) <= phi(C1,C2) <= C1+C2
```

则：

```text
0 <= S <= min(C1,C2)
```

因为 `S=C1+C2-phi(C1,C2)`。`phi` 的上界给出 `S>=0`，下界给出 `S<=C1+C2-max(C1,C2)=min(C1,C2)`。

## 推论：Additive Boundary Overshoot

如果 additive prediction 给出 `Gamma_add(c12)<0`，且 `S>-Gamma_add(c12)`，则 additive model 预测负 interaction，而实际 compound interaction 仍为正。一般而言，正 residual 会把 compound response 从 additive prediction 拉回 baseline 一侧。

## 命题三：Subadditivity 不识别唯一算子

经验条件 `S>0` 无法区分 weakest-link 与 smooth saturation。例如：

```text
phi_lambda(C1,C2)
  = lambda max(C1,C2) + (1-lambda)(C1+C2)
```

当两个 primitive costs 都非零时，对一系列 `lambda` 均产生严格 subadditivity。因此当前证据只能确认 saturating class。要声称 unique min、max、soft-min 或 tropical operator，必须设计新的独立 held-out predictions。

## 估计与检验

所有 quantity 在 latent item 内先跨两个 context lengths 平均。对 `n` 个独立 items：

```text
S_hat = 1/n sum_i S_i
```

置信区间以 latent item 为 cluster 重采样，在 item 内保持 topology、length、instruction arm 与 construction cells 完整。confirmatory gate 要求 reportative 与 factive family 的 95% item-cluster bootstrap 下界均大于零。

## 经验实例

20 个独立 latent items 上：

```text
S_hat_reportative = 4.609, 95% CI [2.646, 6.447]
S_hat_factive      = 3.771, 95% CI [2.274, 5.228]
S_hat_pooled       = 4.190, 95% CI [2.931, 5.401]
```

weakest-link 与 average-saturation predictor 都优于 additive null，因此验证的是 saturation class，而不是 exact aggregation operator。

## Claim Boundary

该理论描述 controlled language interventions 下 observable typed binding margins 的组合行为。它不证明 hidden-state circuit、人类 source-monitoring mechanism、unrestricted language generality 或 broad model-family transfer。

