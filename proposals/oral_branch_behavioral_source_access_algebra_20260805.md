# Oral Branch 2: Behavioral Algebra of Source Accessibility

## Working Title

**A Behavioral Algebra of Belief-Source Access**

Alternative title:

**When Linguistic Constraints Compose: Testing the Algebra of Typed Source Access**

## One-Sentence Thesis

Linguistic constructions behave like operators over a shared typed source-access
route: their effects are not generally additive, and the composition class can be
identified by frozen primitive costs and predictions on unseen compounds.

## Why This Is Stronger Than the Current Saturation Claim

Stage-52 validates a positive subadditive residual in Qwen2.5-7B-Instruct, but
weakest-link and average-saturation controls both pass. The present evidence
therefore identifies a class, not a unique operator. This branch makes that
non-identification the research question instead of hiding it.

The object becomes:

```text
which operator family predicts unseen linguistic compounds from primitive access costs?
```

This is a genuine behavioral theory if it predicts new combinations rather than
describing already observed prompt effects.

## Existing Evidence To Reuse

- Stage-48 establishes conflict-topology moderation within crossed constructions.
- Stage-49 shows a direction-stable topology kernel across audited realizations,
  while gain remains construction-sensitive.
- Stage-50 rejects a narrow local reportative-scope mechanism.
- Stage-51 shows that additive accessibility-cost composition overshoots held-out
  compounds and motivates a saturation model class.
- Stage-52 confirms the subadditive residual on 20 independent latent worlds for
  reportative and factive families in the reference checkpoint.
- Qwen3 and Base define an important boundary: the operator class is not yet
  broad model-family law.

Primary artifacts:

- `results/oral_readiness_pack_20260707/stage48_same_item_topology_20260804/`
- `results/oral_readiness_pack_20260707/stage49_multitemplate_20260804/`
- `results/oral_readiness_pack_20260707/stage50_evidential_scope_factorial_20260804/`
- `results/oral_readiness_pack_20260707/stage51_accessibility_budget_holdout_20260804/`
- `results/oral_readiness_pack_20260707/stage52_independent_saturation_20260804/`

## Formal Object

Let `C(c)` denote the item-level behavioral access cost of construction `c`.
For a compound `c123`, candidate operators include:

```text
additive:          C1 + C2 + C3
weakest-link:      max(C1, C2, C3)
smooth saturation: f(C1, C2, C3; theta)
capacity sharing:  sum(Cj) - shared_capacity(C1, C2, C3)
topology mixture:  phi(C1, C2, C3, T)
```

The analysis must be performed in the typed, topology-moderated margin space,
not raw accuracy. Each operator is fit using primitive cells only, then predicts
unseen compound cells.

The target is not to select the most flexible post-hoc curve. It is to identify
the smallest operator class that survives held-out compound prediction.

## Research Questions

1. Is subadditivity stable for three or more simultaneous constraints?
2. Does the weakest source route determine the compound, or do constraints share
   a finite accessibility capacity?
3. Does conflict topology change the operator, or only its gain and sign?
4. Are reportative and factive constructions instances of one algebra or two?
5. How many primitive observations are needed to predict a new construction family?

## Experimental Design

### Factorial construction library

Use a balanced library with at least five primitive construction types:

- attribution/evidentiality;
- voice or event-state marking;
- discourse order/locality;
- reference/coreference;
- commitment or negation.

Generate all binary compounds and a selected set of ternary compounds. Hold the
latent world, candidate answers, target source, and context-length pair fixed
within each item cluster.

### Train/test split at the operator level

The key split must not merely be item-disjoint. It must be construction-disjoint:

```text
primitive families -> fit operator parameters
held-out binary family -> test composition
held-out ternary family -> test extrapolation
```

A stronger split withholds one topology-construction interaction entirely.

### Pre-registered model comparison

Before seeing the held-out compounds, specify:

- additive null;
- weakest-link/max;
- average saturation;
- smooth-min or bounded-capacity models;
- topology-dependent interaction model;
- no-voice and shuffled-predicate controls.

Use item-cluster bootstrap intervals, predictive log score, rank correlation,
held-out residual error, and model complexity penalty. A model that wins only on
in-sample fit does not pass.

## Decisive Experiment

The most informative experiment is a ternary design where the candidate operators
make different predictions:

```text
C1 = high, C2 = medium, C3 = low
```

Weakest-link predicts the high cost. A shared-capacity model predicts a bounded
increment after the first bottleneck. Additivity predicts the largest suppression.
The ternary item-level ordering is more diagnostic than another binary positive
residual.

## Controls

- primitive margin matching across construction families;
- answer-token and label-frequency matching;
- token-length and perplexity controls;
- construction-order counterbalancing;
- no-voice, shuffled-predicate, and no-source controls;
- same-item crossed topology;
- source identity and answer target invariance;
- item-disjoint and family-disjoint held-out splits;
- affine-gain transport and isotropic gain baselines;
- model-selection nested inside discovery data only.

## Promotion Gates

### Gate A: Prediction

The selected operator must predict held-out binary and ternary compounds with a
pre-registered margin over additive and no-structure baselines.

### Gate B: Discrimination

At least one held-out design must separate weakest-link from average saturation
and capacity sharing. If all saturating models remain tied, the claim stays at
the class level.

### Gate C: Topology interaction

The operator or its parameter must vary predictably with typed conflict topology,
not merely with surface construction or length.

### Gate D: Cross-family portability

At least one independent construction family must preserve the predicted ordering.
Cross-checkpoint transfer is a separate scope gate, not silently pooled with this one.

## Claim Ladder

**Safe current claim:** a bounded Qwen2.5-7B-Instruct setting exhibits a
subadditive saturation class.

**After ternary confirmation:** a named operator class predicts unseen compounds
within the tested construction algebra.

**After family transfer:** the operator is a reusable behavioral primitive for
typed source access across construction families.

**Not allowed:** a unique min operator, a hidden neural accessibility unit,
human psycholinguistic universality, or unrestricted natural-language generality.

## Oral Argument

The paper should replace a table of prompt effects with a prediction game:

```text
Measure primitive access costs.
Freeze them.
Predict unseen compounds.
Let ternary constructions decide between competing composition laws.
```

The central figure should show primitive fits, competing operator predictions,
held-out ternary outcomes, and topology-conditioned residuals.

## Failure Interpretation

- If additive wins, the current saturation result was family-specific and the
  contribution becomes an explicit negative boundary.
- If all bounded saturation models tie, report the identifiable class and make
  operator non-identification a formal result.
- If topology changes only gain, retain a topology kernel plus a construction
  gain model.
- If ternary compounds break all models, the next object is context-dependent
  capacity rather than a fixed algebra.

## Minimal Next Action

Build a ternary 5-primitive pilot with 3–4 construction-disjoint held-out cells.
Do not add more models until the ternary pilot has separated at least two
operator families.

