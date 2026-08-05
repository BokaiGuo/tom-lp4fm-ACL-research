# Source-Accessibility Saturation Theory

## Scope

This note formalizes the paper's behavioral object. The algebraic statements
are exact given their definitions. The sign of the saturation residual is an
empirical claim validated only for the audited model and construction family.

## Notation

Let `i` index a latent story item, `a` a linguistic instruction contrast, and
`T` a typed conflict topology. Let `M_i(a,T)` denote the answer-interface
binding margin between the instructed source and its strongest typed competitor.
For two instruction arms `a+` and `a-`, define

```text
Delta_i(T) = M_i(a-,T) - M_i(a+,T).
```

The topology-moderated language effect is

```text
Gamma_i(c) = Delta_i(role+object,c)
             - 1/2 [Delta_i(role,c) + Delta_i(object,c)],
```

where `c` denotes a construction condition. Positive `Gamma_i(c)` means that
the linguistic contrast has a more positive effect under joint typed conflict
than under the average single-conflict condition.

Let `c0` be the unembedded active baseline. Define the accessibility cost

```text
C_i(c) = Gamma_i(c0) - Gamma_i(c).
```

For two constraints `c1` and `c2`, let `c12` denote their compound construction.

## Definition 1: Additive Null

The independent-cost null predicts

```text
C_i^add(c12) = C_i(c1) + C_i(c2),
```

or equivalently

```text
Gamma_i^add(c12)
  = Gamma_i(c1) + Gamma_i(c2) - Gamma_i(c0).
```

## Definition 2: Saturation Residual

Define

```text
S_i(c1,c2)
  = Gamma_i(c12) - Gamma_i^add(c12).
```

The population residual is `S = E_i[S_i]`. The paper's primary empirical law is
`S > 0` when both constructions suppress access to the instructed source.

## Proposition 1: Exact Cost-Residual Identity

For every item and compound construction,

```text
S_i(c1,c2) = C_i(c1) + C_i(c2) - C_i(c12).
```

Therefore, `S_i > 0` if and only if the compound accessibility cost is strictly
subadditive.

### Proof

By the definitions of `C_i` and `Gamma_i^add`,

```text
S_i
= Gamma_i(c12)
  - Gamma_i(c1) - Gamma_i(c2) + Gamma_i(c0)
= [Gamma_i(c0)-Gamma_i(c1)]
  + [Gamma_i(c0)-Gamma_i(c2)]
  - [Gamma_i(c0)-Gamma_i(c12)]
= C_i(c1) + C_i(c2) - C_i(c12).
```

The sign equivalence follows immediately.

## Proposition 2: Bounds for a Saturating Cost Aggregator

Assume nonnegative primitive costs and a compound-cost operator `phi` satisfying

```text
max(C1,C2) <= phi(C1,C2) <= C1+C2.
```

Then the saturation residual obeys

```text
0 <= S <= min(C1,C2).
```

### Proof

From Proposition 1, `S=C1+C2-phi(C1,C2)`. The upper bound on `phi` gives
`S>=0`. The lower bound gives

```text
S <= C1+C2-max(C1,C2) = min(C1,C2).
```

## Corollary 1: Additive Boundary Overshoot

If `Gamma_add(c12) < 0` and `S > -Gamma_add(c12)`, then the additive model
predicts a negative interaction while the observed compound interaction remains
positive. More generally, positive `S` moves the compound response toward the
baseline side of the additive prediction.

This is the paper's explanation for why additive composition correctly predicts
broad suppression direction but systematically exaggerates its magnitude.

## Proposition 3: Subadditivity Does Not Identify a Unique Operator

The empirical condition `S>0` cannot distinguish a weakest-link operator from a
smooth saturation operator. For positive costs, both

```text
phi_max(C1,C2) = max(C1,C2)
```

and, for suitable `lambda` in `(0,1)`,

```text
phi_lambda(C1,C2)
  = lambda max(C1,C2) + (1-lambda)(C1+C2)
```

satisfy strict subadditivity whenever both primitive costs are nonzero.

### Consequence

The validated claim is a subadditive saturation class. A unique `min`, `max`,
soft-min, or tropical operator requires additional independently held-out
predictions and is not claimed in the paper.

## Estimator and Test

All quantities are computed per latent item after averaging the two context
lengths. For `n` independent items,

```text
S_hat = 1/n sum_i S_i.
```

The confidence interval resamples latent items, preserving every topology,
length, instruction arm, and construction cell within an item cluster. The
confirmatory gate requires the lower 95% item-cluster bootstrap bound of
`S_hat` to exceed zero in both reportative and factive families.

## Empirical Instantiation

On 20 independent latent items with zero discovery-pool signature overlap:

```text
S_hat_reportative = 4.609, 95% CI [2.646, 6.447]
S_hat_factive      = 3.771, 95% CI [2.274, 5.228]
S_hat_pooled       = 4.190, 95% CI [2.931, 5.401]
```

Both a weakest-link predictor and an average-saturation predictor improve over
the additive null. This validates the saturation class while leaving the exact
aggregation operator unidentified.

## Claim Boundary

The theory concerns observable typed binding margins under controlled language
interventions. It does not prove a hidden-state circuit, human source-monitoring
mechanism, unrestricted language generality, or broad model-family transfer.
