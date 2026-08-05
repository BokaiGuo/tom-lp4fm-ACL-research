# Oral Branch 1: Training-Induced Composition Phase Transition

## Working Title

**When Instruction Tuning Changes the Algebra of Belief-Source Access**

Alternative title:

**From Primitive Margins to Composition Laws: How Checkpoints Rewire Linguistic Source Access**

## One-Sentence Thesis

Instruction tuning may change not only the amplitude of language-model responses,
but the way independently measured linguistic constraints compose at a shared
typed source-access interface.

## Why This Is a New Research Object

The current evidence does not support a universal saturation law. It does support
a sharper observation: Qwen2.5-7B-Instruct shows a positive held-out saturation
residual, while matched Qwen2.5-7B Base and Qwen3 preserve some control-relative
predictor gains but not the directional residual. Primitive-only affine transport
also fails, so the difference is not explained by a scalar readout compression.

The new object is therefore not `whether saturation transfers`, but:

```text
the checkpoint trajectory at which a source-access composition law changes class
```

This turns the current portability failure into the central phenomenon.

## Existing Evidence To Reuse

- Stage-48/49: typed conflict topology moderates language-form effects and a
  cross-realization kernel is direction-stable within the audited setting.
- Stage-51/52: frozen primitive costs predict subadditive compound behavior in
  Qwen2.5-7B-Instruct, but weakest-link and average-saturation predictors remain
  observationally tied.
- Qwen3-8B and Qwen2.5-7B Base: pooled control-relative gains partly transfer,
  but positive directional residuals do not.
- Affine-gain audit: primitive-scale compression cannot explain the residual
  collapse or the weak itemwise residual correlations.
- Stage-34–43: answer-simplex geometry is not enough; counterfactual activation
  trajectories are checkpoint-sensitive.

Primary artifacts:

- `results/oral_readiness_pack_20260707/stage52_independent_saturation_20260804/`
- `results/oral_readiness_pack_20260707/qwen3_8b_independent_saturation_replication_20260804/`
- `results/oral_readiness_pack_20260707/qwen25_7b_base_independent_saturation_replication_20260805/`
- `results/oral_readiness_pack_20260707/affine_gain_null_audit_20260805/`

## Formal Object

For checkpoint `k`, latent item `i`, typed topology `T`, and construction `c`,
let `Gamma_i,k(c,T)` be the topology-moderated typed binding margin. For two
primitive constructions `c1,c2` and their compound `c12`, define:

```text
S_i,k(c1,c2,T)
  = Gamma_i,k(c12,T)
    - Gamma_i,k(c1,T)
    - Gamma_i,k(c2,T)
    + Gamma_i,k(c0,T)
```

The checkpoint-level composition signature is not only `mean(S)`. It is the
tuple:

```text
C_k = {
  mean residual,
  residual direction by predicate,
  control-relative predictor gains,
  primitive dynamic range,
  itemwise residual geometry,
  topology-by-construction interaction
}
```

The main hypothesis is that training changes `C_k` discontinuously or
non-monotonically, rather than applying a single positive scalar to a fixed law.

## Research Questions

1. Does a single training trajectory contain a reproducible composition-law
   change point?
2. Does the change occur for reportative and factive constructions together,
   or are they independently learned interfaces?
3. Is the change associated with instruction tuning, preference optimization,
   source-attribution data, or generic calibration?
4. Does the transition alter only the behavioral operator, or also the
   typed activation trajectory identified by Stage-36?
5. Can controlled exposure induce the transition without improving primitive
   task accuracy in general?

## Experimental Program

### Phase 1: Checkpoint census

Collect 3–8 checkpoints from one model family and one training trajectory:

```text
base -> continued-pretraining -> SFT early -> SFT late
     -> preference/alignment early -> preference/alignment late
```

If public intermediate checkpoints are unavailable, train a small controlled
Qwen-compatible model with identical tokenizer, answer interface, and latent
world generator. The primary analysis must remain within-trajectory; unrelated
models are secondary anchors only.

### Phase 2: Frozen composition screen

Run the existing primitive and compound panel without changing:

- prompt wording;
- latent item split;
- construction contrasts;
- answer candidates;
- primitive-cost estimation;
- cluster bootstrap;
- directional promotion thresholds.

Use a low-cost 5-item screen per checkpoint first, then expand only checkpoints
near a possible transition.

### Phase 3: Change-point identification

Fit pre-registered alternatives:

- no change, scalar amplitude only;
- smooth monotonic gain change;
- one change point in residual direction;
- separate reportative/factive change points;
- topology-dependent change point.

The change point must be chosen from a training-independent screen and evaluated
on held-out latent items. Do not use the positive Qwen2.5-7B-Instruct checkpoint
as the selection target after seeing the full result.

### Phase 4: Controlled induction

Construct two matched SFT interventions:

1. **Composition exposure:** show primitive and compound source-attribution
   constructions with labels that preserve typed source distinctions.
2. **Primitive-only exposure:** match primitive margins and token statistics,
   but never expose compounds.

The prediction is that composition exposure changes the held-out compound residual
more than primitive-only exposure. A stronger test randomizes which construction
pairs are seen during training and evaluates an unseen pair.

## Necessary Controls

- Primitive answer margin and calibration;
- output entropy and token-length effects;
- affine and isotropic readout-gain baselines;
- prompt embedding similarity and lexical overlap;
- source-label frequency and answer-token frequency;
- no-voice and shuffled-predicate controls;
- construction order counterbalancing;
- same latent world with crossed typed topology;
- training-data leakage audit;
- item-disjoint change-point confirmation.

## Promotion Gates

### Gate A: Longitudinal change

At least one held-out checkpoint trajectory must show a directional residual
change that survives item-cluster bootstrap and is not explained by primitive
scale.

### Gate B: Selective induction

Composition exposure must alter compound residuals on unseen pairs more than
primitive-only exposure, while preserving source-specific routing and not merely
raising all answer margins.

### Gate C: Mechanism linkage

If activation analysis is included, the trajectory contrast must predict the
behavioral residual on held-out items. It must beat final-token-only, wrong-channel,
random, and calibration controls. It is not enough to find a readable direction.

### Gate D: Negative result value

If no change point appears, the paper should report that composition laws are
checkpoint-local and not obviously induced by the tested training stages. That is
still informative if the null is well powered and the benchmark is frozen.

## Claim Ladder

**Safe now:** tested checkpoints differ in behavioral composition shape beyond
scalar affine readout gain.

**After longitudinal confirmation:** composition-law change is associated with
training stage within one model trajectory.

**After controlled SFT:** controlled construction exposure changes held-out
composition behavior.

**Not allowed without stronger evidence:** instruction tuning universally causes
the law, a unique hidden operator exists, or human discourse processing follows
the same transition.

## Oral Argument

The paper should open with a paradox:

```text
The same primitive linguistic costs produce different compound laws at different
checkpoints, even when the candidate answer geometry remains similar.
```

The main figure should show a checkpoint trajectory with primitive margins,
compound residual, affine-null prediction, and the first held-out transition.
The contribution is a training-sensitive composition law, not another prompt
robustness table.

## Risks and Fallbacks

- **No intermediate checkpoints:** run controlled miniature SFT and frame the
  result as an intervention study, not a historical training claim.
- **Only amplitude changes:** retain the affine-null result but demote to a
  checkpoint calibration paper; do not call it a phase transition.
- **Only one predicate changes:** write predicate-specific composition learning,
  not a general source-access algebra.
- **No causal induction:** pivot to Branch 2 and treat checkpoint variation as
  a boundary condition.

## Minimal Next Action

Freeze a 5-checkpoint pilot manifest and run primitive plus binary-compound
scoring before doing any activation work. The first decision is whether a
longitudinal residual change exists at all.

