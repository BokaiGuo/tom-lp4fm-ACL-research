# Combined Oral Branch: Learning the Algebra of Belief-Source Access

## Working Title

**Learning the Algebra of Belief-Source Access: When Instruction Tuning Changes Linguistic Composition Laws**

Alternative titles:

- **How Training Rewrites the Composition of Linguistic Source Access**
- **From Primitive Constructions to Learned Composition Laws in Language Models**
- **Checkpoint-Sensitive Algebras of Belief-Source Access**

## One-Sentence Thesis

Language-model training can change not only the strength of individual linguistic
effects, but the operator by which independently measured source-access constraints
compose on unseen constructions.

## Mother Object

The unified research object is a **checkpoint-indexed source-access composition
operator**:

```text
phi_k(C_1, ..., C_m, T)
```

where:

- `k` identifies a checkpoint on a training trajectory;
- `C_j` is the primitive behavioral access cost of construction `j`;
- `T` is the typed conflict topology;
- `phi_k` predicts the cost or binding margin of a compound construction.

The central question is:

> Does training merely rescale a fixed composition operator, or does it change the
> operator class governing how linguistic constraints share a belief-source route?

This question joins the two original branches at the object level. The composition
algebra provides a falsifiable measurement of checkpoint change; the training
trajectory explains when and why the algebra changes.

## Why The Two Branches Must Be Combined

Studying checkpoint transition without an identified composition object risks
becoming another model-comparison paper. Studying composition algebra at one
checkpoint risks becoming a narrow behavioral regularity. The combined design
closes both weaknesses:

```text
primitive construction costs
-> frozen predictions for unseen compounds
-> operator identification within each checkpoint
-> operator trajectory across training
-> controlled exposure that induces or blocks the transition
```

The result is stronger than either component alone:

- the algebra supplies a compact, predictive object rather than a collection of
  prompt effects;
- the training trajectory supplies a causal route beyond single-checkpoint
  description;
- the matched Base/Instruct comparison supplies an architectural control;
- the affine-gain rejection supplies a null against simple response compression;
- typed conflict topology supplies source-level identifiability.

## Existing Evidence

### Behavioral source identification

Stage-2–6 establish that raw correctness is a mixed observation over
`queried_belief`, `reality`, `other_agent`, `object_distractor`, and policy-related
answer channels. Typed margins are therefore the correct measurement substrate.

### Topology-conditioned language effects

Stage-44–49 show that controlled linguistic constructions change typed source
arbitration, that conflict topology moderates those effects, and that the directional
kernel survives multiple realizations within the audited setting.

### Composition discovery and confirmation

Stage-50–52 show that primitive access costs fail additively on held-out compounds.
Twenty independent latent worlds confirm positive subadditive residuals in
Qwen2.5-7B-Instruct for reportative and factive construction families.

### Operator non-identification

Weakest-link and average-saturation predictors both improve over additivity, so the
current evidence identifies a saturating composition class but not a unique operator.

### Checkpoint boundary

Frozen Qwen3-8B and matched Qwen2.5-7B Base evaluations retain partial
control-relative predictor gains but not the positive directional residual.
Primitive-only affine transport fails, ruling out a simple positive scalar readout
gain explanation.

Primary repository artifacts:

- `docs/theory/source_accessibility_saturation_theory.md`
- `results/oral_readiness_pack_20260707/stage49_multitemplate_20260804/`
- `results/oral_readiness_pack_20260707/stage51_accessibility_budget_holdout_20260804/`
- `results/oral_readiness_pack_20260707/stage52_independent_saturation_20260804/`
- `results/oral_readiness_pack_20260707/qwen3_8b_independent_saturation_replication_20260804/`
- `results/oral_readiness_pack_20260707/qwen25_7b_base_independent_saturation_replication_20260805/`
- `results/oral_readiness_pack_20260707/affine_gain_null_audit_20260805/`

## Relationship to Related Work

### The two nearest literatures do not yet meet at our object

The first neighboring literature studies how compositional generalization
depends on demonstrations, data coverage, and the learning algorithm. [CoFe](https://arxiv.org/abs/2305.04835)
identifies structural similarity, diversity, simplicity, and linguistic-
structure coverage as important for in-context composition. [A General Theory
for Compositional Generalization](https://arxiv.org/abs/2405.11743) introduces
task-agnostic limits and generative effects. [Fu--Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html)
separates insufficient supporting data from knowledge-composition capacity and
emphasizes algorithm--rule compatibility.

The second literature studies when in-context computation and generalization
change during training. [Induction Heads](https://arxiv.org/abs/2209.11895)
links an ICL phase change to a candidate circuit; [Learning Plateaus](https://proceedings.mlr.press/v235/fu24h.html)
separates weights and context components; [Learning Trajectories](https://arxiv.org/abs/2304.12579)
uses update-path information to explain generalization.

The combined branch sits at their intersection but introduces a narrower state
variable:

```text
checkpoint k -> operator over typed source-access relations
```

The question is not simply whether training improves compositional accuracy.
It is whether training changes how independently measured source constraints
compose when several sources compete for the same answer interface.

### Relation to language representation and source-priority training

[Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
argues that task representation changes knowledge activation and organization.
[The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) shows that targeted
training can install an explicit channel-priority policy. These are close
precedents for representation- and training-induced changes, but neither
estimates a held-out composition operator from typed primitive source costs.

They also define two rival explanations that the study must retain: the observed
transition may be a broad schema/representation change, or a learned explicit
priority heuristic, rather than a change in composition capacity. Cross-
encoding tests, implicit-source tasks, and primitive-matched controls are needed
to separate these accounts.

### Relation to current LP4FM and the ACG successor

The submitted LP4FM paper establishes a static, checkpoint-sensitive behavioral
boundary: language form changes typed source arbitration, and compound effects
need not be additive. It does not establish a longitudinal operator transition.
This combined branch adds training trajectory and controlled induction, so its
main evidence cannot be a relabeling of Stage-51/52.

The newer [Authority Compositional Generalization plan](../research/next_stage_icl_schema_rewriting_20260826_zh.md)
pushes the object further: demonstrations may induce a relational authority
program that transfers across content, encodings, and unseen topologies. Under
that framing, this combined algebra branch becomes the **weights/training lane**
of a broader ACG paper rather than an obligatory standalone manuscript.

### Exact novelty boundary and direct baselines

The branch cannot claim novelty for ICL training dynamics, compositional
generalization, phase transitions, or schema shaping. Its separable contribution
would require all three of the following: held-out source-operator prediction,
a within-trajectory change point, and selective controlled induction after
primitive accuracy and data coverage are matched. The direct baseline suite is
CoFe-style example coverage, additive and flexible operator families,
primitive-accuracy/calibration trajectories, affine transport, explicit
instruction-hierarchy training, and no-compound exposure. See the [central
related-work map](../research/related_work_positioning_20260826_zh.md).

## Formal Framework

### Typed binding margin

For item `i`, checkpoint `k`, construction `c`, and typed topology `T`, define:

```text
M_i,k(c,T)
  = logit_k(intended source answer)
    - max logit_k(typed competitor answers)
```

For two instruction arms `a+` and `a-`, let:

```text
Delta_i,k(c,T) = M_i,k(a-,c,T) - M_i,k(a+,c,T)
```

The topology-moderated effect is:

```text
Gamma_i,k(c)
  = Delta_i,k(c, joint conflict)
    - mean Delta_i,k(c, single conflicts)
```

### Primitive access cost

For baseline construction `c0`:

```text
C_i,k(c) = Gamma_i,k(c0) - Gamma_i,k(c)
```

### Checkpoint-indexed composition operator

For primitives `c1,...,cm` and compound `c1...m`:

```text
C_i,k(c1...m)
  = phi_k(C_i,k(c1), ..., C_i,k(cm), T_i) + epsilon_i,k
```

Candidate operator families include:

```text
additive:
  phi_add = sum_j C_j

weakest-link:
  phi_max = max_j C_j

average saturation:
  phi_avg = alpha max_j C_j + (1-alpha) mean_j C_j

bounded capacity:
  phi_cap = B_k * h(sum_j C_j / B_k)

topology-conditioned mixture:
  phi_mix = sum_r pi_k,r(T) phi_r(C_1,...,C_m)
```

### Operator trajectory

For checkpoints ordered by training time `tau`:

```text
Phi(tau) = {
  operator family,
  operator parameters,
  directional residual,
  control-relative gain,
  topology dependence,
  itemwise residual geometry
}
```

The main scientific target is a change in `Phi(tau)` that cannot be reduced to:

- primitive margin scaling;
- output calibration;
- answer-token prior;
- isotropic or affine readout gain;
- construction-length differences.

## Main Hypotheses

### H1: Predictive composition

A low-complexity saturating operator predicts unseen binary and ternary compounds
better than additive and construction-only baselines within at least one checkpoint.

### H2: Checkpoint-dependent operator class

The best predictive operator or its directional residual changes across a matched
training trajectory, not merely across unrelated model families.

### H3: Non-affine transition

The checkpoint difference remains after primitive-scale matching, calibration
matching, and affine/isotropic gain controls.

### H4: Selective training induction

Controlled exposure to source-attribution compounds changes the held-out operator
more than primitive-only or answer-only training under equal token and optimizer
budgets.

### H5: Topology-conditioned emergence

The learned operator change is stronger when multiple typed competitors constrain
the same intended source route, rather than being a context-free prompt effect.

## Research Program

## Study A: Identify The Algebra Within A Checkpoint

### Construction library

Use at least five primitive constraints:

- reportative/factive attribution;
- active/passive voice or event-state marking;
- discourse order and embedding locality;
- reference/coreference form;
- commitment, negation, or evidential wording.

Generate:

- all feasible primitive cells;
- a balanced set of binary compounds;
- diagnostic ternary compounds where candidate operators disagree;
- same-item crossed single and joint conflict topologies.

### Operator-level split

Use three nested splits:

1. **Item split:** unseen latent worlds;
2. **Construction split:** unseen construction family or realization;
3. **Composition split:** unseen binary and ternary combinations.

Primitive costs and operator parameters must be frozen before compound evaluation.

### Model comparison

Pre-register additive, weakest-link, average saturation, bounded capacity,
topology mixture, no-voice, and shuffled-predicate models. Compare them using:

- item-cluster bootstrap residual intervals;
- held-out mean absolute error;
- predictive log score;
- compound rank correlation;
- complexity-penalized fit;
- model confidence sets when multiple operators remain tied.

### Study A promotion gate

At least one held-out ternary design must distinguish two candidate operator
families. If all saturating models remain observationally equivalent, the paper
must retain a class-level claim.

## Study B: Trace The Algebra Across Training

### Checkpoint trajectory

Prefer checkpoints from one continuous trajectory:

```text
Base
-> continued-pretraining
-> early SFT
-> late SFT
-> preference optimization
-> final aligned checkpoint
```

If the full trajectory is unavailable, run a controlled miniature trajectory with
the same tokenizer and source-routing benchmark.

### Frozen evaluation

For every checkpoint, freeze:

- latent items;
- answer candidate ordering;
- construction wording;
- primitive/compound split;
- scorer implementation;
- bootstrap unit;
- operator-selection criteria;
- promotion thresholds.

Do not re-estimate the positive reference law separately for each evaluation model
using its compound outcomes.

### Change-point analysis

Compare:

- no operator change;
- scalar amplitude trajectory;
- smooth operator-parameter drift;
- one global change point;
- predicate-specific change points;
- topology-dependent transition.

Select the change-point model on one item pool and confirm it on another.

### Study B promotion gate

At least one held-out trajectory must show an operator change or directional
residual transition that exceeds primitive-scale and calibration controls.

## Study C: Controlled Induction Of The Operator

### Training conditions

Train matched adapters or small models under equal budgets:

1. **Answer-only SFT:** final labels only;
2. **Primitive typed routing:** primitive constructions with source labels;
3. **Seen-pair composition:** primitives plus selected compounds;
4. **Topology-balanced composition:** selected compounds crossed with typed conflicts;
5. **Shuffled composition control:** identical tokens with mismatched source or
   topology labels.

### Critical held-out test

Withhold one construction family and one compound pair from all training conditions.
The decisive question is whether topology-balanced composition training predicts the
held-out compound operator better than primitive-only and answer-only training.

### Selectivity requirements

Training must not pass by:

- increasing all answer logits;
- flattening every construction contrast;
- memorizing answer-token identities;
- suppressing all non-target sources indiscriminately;
- changing only the final-token prior.

### Study C promotion gate

Controlled composition exposure must selectively alter the held-out operator or
residual relative to matched controls. This is the gate required for training-time
causality.

## Study D: Optional Mechanism Bridge

This study is optional and should not block the behavioral paper.

Use Stage-36 canonical typed contrasts and compare checkpoints immediately before
and after the behavioral operator transition. Test whether a change in local typed
response predicts compound residual changes on held-out items.

Required controls:

- final-token-only intervention;
- wrong-channel contrast;
- random and orthogonal directions;
- dose response;
- flip-budget eligibility;
- paired versus shuffled checkpoint alignment;
- geometry-only versus causal transport.

Without these controls, activation evidence remains descriptive and should not be
called the learned source-access mechanism.

## Decisive Ternary Design

Binary compounds currently leave weakest-link and average saturation tied. A ternary
design should deliberately separate them. For primitive costs:

```text
C1 = high
C2 = medium
C3 = low
```

the candidate predictions diverge:

```text
additive:        high + medium + low
weakest-link:    high
average model:   between high and the primitive average
capacity model:  depends on learned capacity B_k
```

Run this design at multiple checkpoints. The strongest result would show not merely
different residual amplitudes, but a checkpoint transition between predictive
operator classes.

## Necessary Controls

### Behavioral controls

- typed source identity and answer-target invariance;
- candidate-token counterbalancing;
- primitive margin matching;
- answer entropy and calibration;
- token length, perplexity, and lexical overlap;
- construction order and context-length counterbalancing;
- no-source, no-voice, and shuffled-predicate controls;
- item-disjoint and construction-disjoint evaluation.

### Checkpoint controls

- matched architecture and tokenizer;
- identical scoring code and prompt manifest;
- no checkpoint-specific prompt repair;
- primitive-scale and dynamic-range matching;
- affine and isotropic gain nulls;
- equal precision and decoding interface;
- contamination and memorization audit.

### Training controls

- equal token and optimizer budgets;
- multiple seeds;
- primitive-only exposure;
- answer-only exposure;
- shuffled source/topology labels;
- general task retention;
- valid linguistic sensitivity preservation.

## Result Matrix And Interpretation

| Algebra result | Training result | Safe interpretation | Oral potential |
|---|---|---|---|
| Ternary operator identified | Controlled induction succeeds | Training changes a predictive source-access composition operator | Highest |
| Saturation class only | Controlled induction succeeds | Training changes the strength/class of bounded subadditive composition | High |
| Operator identified | Longitudinal association only | Operator varies across training checkpoints; causal training source unresolved | Medium-high |
| Saturation class only | Checkpoint association only | Checkpoint-sensitive behavioral composition boundary | Current LP4FM tier |
| No held-out composition prediction | Any | Do not claim an algebra; retain typed prompt-effect diagnostics | Low |

## Unified Promotion Gates

### Gate 1: Typed identifiability

The effect must concern intended versus typed competitor sources, not raw accuracy.

### Gate 2: Held-out composition

Primitive observations must predict unseen compounds under frozen evaluation.

### Gate 3: Operator discrimination

At least one diagnostic design must separate candidate operator families, or the
claim must remain at the composition-class level.

### Gate 4: Longitudinal transition

The operator or residual must change within a matched training trajectory beyond
primitive-scale and calibration controls.

### Gate 5: Controlled induction

Matched training exposure must selectively change held-out composition behavior.
Only this gate supports training-time causality.

### Gate 6: Scope transfer

At least one new construction family or typed source domain must confirm the
predicted operator behavior before claiming broad linguistic generality.

## Claim Ladder

### Tier 0: Existing bounded result

Qwen2.5-7B-Instruct exhibits independently confirmed subadditive composition for
the tested reportative and factive families. Base and Qwen3 delimit portability.

### Tier 1: Predictive algebra

A pre-registered operator class predicts unseen binary and ternary compounds in the
tested checkpoint.

### Tier 2: Learned operator trajectory

The operator changes systematically along a matched training trajectory and cannot
be reduced to primitive margin or affine readout gain.

### Tier 3: Training-time causal result

Controlled source-attribution composition exposure selectively induces, removes,
or changes the held-out operator relative to matched controls.

### Tier 4: General training principle

The controlled result transfers to another construction family or typed routing
domain while preserving valid linguistic distinctions.

## Forbidden Claims

Do not claim, without additional evidence:

- instruction tuning universally creates subadditivity;
- the operator is uniquely weakest-link, min, or tropical;
- a behavioral operator is a localized hidden-state circuit;
- all foundation models share one source-access algebra;
- the result is a human psycholinguistic universal;
- robustness improvement is internal belief-mechanism repair.

## Paper Story

The manuscript should follow an identification chain rather than a stage chronology:

```text
accuracy cannot identify the controlling source
-> typed topology exposes source-routing effects
-> primitive linguistic costs define competing composition laws
-> unseen ternary compounds identify the operator class
-> training checkpoints reveal when the operator changes
-> controlled exposure tests whether training induces the change
-> affine gain, calibration, and final-token alternatives are rejected
```

## Proposed Main Figures

### Figure 1: Typed source-access algebra

Show the latent world, typed conflict topology, primitive constructions, and
competing compound predictions.

### Figure 2: Held-out operator identification

Show primitive fits and unseen binary/ternary outcomes for additive, weakest-link,
average saturation, and capacity models.

### Figure 3: Training trajectory

Plot operator class, directional residual, primitive scale, and control-relative
gain over checkpoints with a held-out change-point interval.

### Figure 4: Controlled induction

Compare answer-only, primitive-only, composition, topology-balanced, and shuffled
training on the unseen compound.

### Figure 5: Alternative-explanation closure

Summarize affine-gain, calibration, lexical, contamination, and optional mechanism
controls.

## Proposed Tables

- Construction and topology factorial;
- frozen train/discovery/confirmation split;
- operator definitions and preregistered predictions;
- checkpoint artifact and training-stage provenance;
- promotion-gate results;
- claim-safe and forbidden interpretations.

## Minimal Viable Version

The smallest paper-changing experiment is:

1. Build one diagnostic ternary construction panel;
2. evaluate Qwen2.5-7B Base and Instruct under the unchanged typed interface;
3. identify whether the two checkpoints prefer different operator classes;
4. run a small Base-model LoRA with primitive-only versus composition exposure;
5. evaluate one unseen compound pair and one unseen latent-item pool.

This version can establish whether the combined branch is viable before a full
checkpoint census or activation study.

## Full Oral Version

The full oral target requires:

1. a predictive ternary operator result;
2. a matched longitudinal checkpoint transition;
3. controlled training induction on an unseen compound;
4. exclusion of primitive-scale, calibration, affine-gain, and lexical alternatives;
5. at least one construction-family or typed-domain transfer;
6. an explicit claim boundary separating behavioral algebra from hidden mechanism.

## Execution Roadmap

### Milestone 1: Freeze operator benchmark

- Extend Stage-52 primitives to a ternary panel;
- assign discovery, operator-selection, and confirmation splits;
- freeze prompt, scorer, and bootstrap hashes.

### Milestone 2: Base/Instruct viability test

- run both checkpoints on the ternary panel;
- compare operator model confidence sets;
- decide whether there is evidence of class change or only amplitude change.

### Milestone 3: Controlled LoRA pilot

- train answer-only, primitive-only, and composition conditions;
- evaluate unseen compounds and valid semantic contrasts;
- stop if all conditions collapse to generic margin amplification.

### Milestone 4: Longitudinal expansion

- add intermediate checkpoints only after the viability test passes;
- preregister change-point alternatives;
- confirm on independent latent worlds.

### Milestone 5: Scope extension

- add one independent construction family or typed routing domain;
- rerun the frozen operator and induction gates;
- finalize the strongest safe claim tier.

## Decision Rule

Proceed as a combined oral project only if the ternary pilot provides operator
discrimination or the controlled training pilot selectively changes held-out
composition. If neither occurs, keep the current LP4FM paper and return to the
mechanistic-identifiability branch rather than expanding the model matrix.
