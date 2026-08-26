# Oral Branch 3: Mechanistic Identifiability of Typed Controllers

## Working Title

**When Is a Belief Controller Identifiable? Probes, Answer Simplexes, and Causal Transport**

Alternative title:

**Readable Is Not Controllable: Identifiability Limits for Mechanistic Probing of Language Models**

## One-Sentence Thesis

A representation can be linearly readable, output-controlling, source-specific,
and checkpoint-transportable in different combinations; mechanistic claims require
separate identification gates for each property.

## Why This Is Not Merely a Negative-Results Paper

The project repeatedly found strong-looking mechanism signals that failed under
stronger controls:

- query-local belief information was readable but not a stable steering site;
- multi-site interventions were absorbed by a final-token shortcut;
- whole answer-simplex projectors appeared channel-specific but were algebraically
  identical across target vertices;
- Base and Instruct checkpoints had almost identical candidate-output subspaces
  but asymmetric causal transport;
- paired linear stitching improved geometric alignment without globally rescuing
  causal effects.

These are instances of one general problem: the experimental object did not
identify the mechanism property being claimed. The contribution should be a
formal identifiability framework and a reusable audit protocol.

## Existing Evidence To Reuse

- Stage-3: behavior, probe, and patching evidence do not align.
- Stage-4: query-binding readability coexists with story/query patch failure and
  strong final-token transfer.
- Stage-5/13: steering failures depend on flip budget and causal calibration.
- Stage-14–16: apparent multi-site effects collapse to final-token-only control.
- Stage-17–20: component, distributed-writer, layer-path, and readout-subspace
  localization do not yield a broad mechanism.
- Stage-23/25/33: held-out and model-family transfer fail under frozen gates.
- Stage-34/35: full answer-simplex subspaces are nearly identical, yet typed
  transport is asymmetric; full vertex projectors are structurally non-identifying.
- Stage-36: canonical one-vs-rest directions restore typed discrimination.
- Stage-37: geometric stitching and causal rescue dissociate.

Primary artifacts:

- `results/oral_readiness_pack_20260707/stage3_paper_decision.json`
- `results/oral_readiness_pack_20260707/stage4_probe_site_dissociation/`
- `results/oral_readiness_pack_20260707/stage14_flip_budget_intervention_protocol/`
- `results/oral_readiness_pack_20260707/stage34_checkpoint_basis_transport_20260729/`
- `results/oral_readiness_pack_20260707/stage37_mathematical_transport_theory_20260730/`

## Relationship to Related Work

### Probing does not identify causal use

[Probing Classifiers: Promises, Shortcomings, and Advances](https://arxiv.org/abs/2102.12452)
reviews why decoder accuracy alone does not establish how a representation is
used. [Amnesic Probing](https://arxiv.org/abs/2006.00995) makes the point
interventional: conventional probing performance need not correlate with task
importance, so information removal is used to test behavioral use. Our
readability--controllability distinction is therefore not itself novel; it is a
required starting point inherited from this literature.

This branch extends the separation to a typed multiclass answer interface. It
asks whether an intervention controls the intended source rather than merely an
answer identifier, whether it beats wrong-channel and final-token controls, and
whether its causal semantics transport across items and checkpoints.

### Belief representations and adequacy criteria

[Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html)
and [Brittle Minds, Fixable Activations](https://arxiv.org/abs/2406.17513)
provide direct precedents for decoding and intervening on belief-related
representations. [Standards for Belief Representations in LLMs](https://arxiv.org/abs/2405.21030)
argues that belief-like status requires accuracy, coherence, uniformity, and
use rather than any single signal. These works make a broad claim such as
"probes do not prove mechanisms" insufficient as a contribution.

Our exact wedge is an identifiability result for **typed controller claims**:
full competitor-complete answer-simplex projectors can be vertex-nonidentifying;
high subspace overlap can coexist with asymmetric causal transport; and
same-item response reconstruction can coexist with failure to promote the
intended source. These are stronger, source-specific counterexamples to common
mechanistic inferences.

### Causal abstraction, steering, and intervention methods

[Causal Abstraction](https://arxiv.org/abs/2301.04709) supplies a unified formal
language for patching, mediation, erasure, steering, and distributed alignment.
[ReFT](https://arxiv.org/abs/2404.03592) and instruction-specific [activation
steering](https://arxiv.org/abs/2410.12877) show that learned or contrastive
representation interventions can efficiently change behavior. These methods
are applicable baselines, but intervention success is not the same estimand as
typed identifiability.

The protocol must therefore evaluate each method with the same answer-encoding
permutations, source-specific argmax, wrong-channel controls, and cross-content
transport. The recent [Cross-Encoding Steering Evaluation](https://arxiv.org/abs/2608.22985)
is especially close: it demonstrates that steering can follow extraction
indices rather than semantic labels. Our proposed framework subsumes this as an
answer-encoding specificity failure and adds source and checkpoint transport.

### Exact novelty boundary

The paper cannot claim a general no-go theorem for mechanistic interpretability
or a unique belief circuit. A defensible contribution requires (i) a general
multiclass readout theorem, (ii) at least two non-ToM tasks, and (iii) a protocol
that changes which mechanistic claims survive. Without cross-task replication,
the current evidence remains a ToM-specific mechanism boundary. See the [central
related-work map](../research/related_work_positioning_20260826_zh.md).

## Four Distinct Identification Targets

For representation or intervention object `z`, define four claims:

### Readability

Can a decoder recover typed state `y` from `z` on held-out items?

```text
I_read(z) = predictive information about y
```

### Controllability

Does a bounded intervention on `z` change the target output under a calibrated
flip budget?

```text
I_control(z) = causal effect on target answer margin or argmax
```

### Specificity

Does the intervention affect the intended typed channel more than wrong-channel,
orthogonal, random, and final-token controls?

```text
I_specific(z) = target effect - strongest matched control effect
```

### Transportability

Does the same object preserve its causal semantics across items, layers,
checkpoints, or model families?

```text
I_transport(z_a -> z_b) = held-out causal semantic preservation
```

No implication among these properties should be assumed without theorem-level
conditions. In particular, high readability does not imply controllability, and
subspace overlap does not imply transportable causal semantics.

## Core Theoretical Results To Develop

### Theorem family 1: Vertex non-identifiability

For `K` answer vectors `w_1,...,w_K`, any target-specific subspace that contains
all differences `w_c-w_j` for `j != c` spans the same global `K-1` contrast space.
Therefore, a competitor-complete full-simplex projector cannot identify the
target vertex.

The existing Stage-35 result should be generalized beyond one model and written
as a theorem about multiclass linear readouts.

### Theorem family 2: Canonical typed relaxation

Under linearity, invariance to global translation, and permutation symmetry over
non-target labels, the canonical nonzero rank-one typed contrast is proportional to:

```text
q_c = w_c - mean_{j != c}(w_j)
```

The paper should state when this relaxation is sufficient and when it remains
vulnerable to nonlinear or active-set effects.

### Theorem family 3: Readability-control separation

Construct representations where a nuisance subspace perfectly encodes `y`, while
the downstream Jacobian annihilates that subspace. Conversely, construct an
output-controlling direction with low global decodability due to item-conditional
gating. These examples formalize why probes and interventions target different
objects.

### Theorem family 4: Geometry-causality separation

Show that an invertible or low-error alignment can improve cosine similarity while
failing to preserve the downstream response operator. Causal transport requires
alignment of both state coordinates and the local response map.

## Cross-Task Empirical Program

The theory must be validated beyond false-belief tasks. Use at least two external
typed-controller settings:

1. **Temporal source selection:** answer from event time, report time, or current
   state.
2. **Deontic authority selection:** answer from system rule, user preference,
   quoted authority, or distractor rule.
3. Optional: spatial perspective, multi-document authority, or tool-state routing.

Each task must expose multiple candidate controllers sharing a common output
interface. Binary correctness is insufficient.

## Standard Audit Protocol

### Step 1: Typed behavioral decomposition

Enumerate candidate source channels and report margins, capture rates, leaks,
and controller swaps.

### Step 2: Readability gate

Use held-out items, shuffled labels, random sites, and matched-capacity probes.

### Step 3: Calibrated causal gate

Estimate flip-budget eligibility before interpreting null interventions. Report
continuous margin effects and argmax changes separately.

### Step 4: Specificity gate

Include final-token-only, wrong-channel, orthogonal, random-direction, and
same-source controls. The target must beat the strongest matched control.

### Step 5: Transport gate

Freeze the object and test item-, layer-, checkpoint-, and task-level transport.
Do not refit after observing evaluation outcomes.

### Step 6: Claim assignment

Assign the strongest supported tier:

```text
readable
-> controllable
-> source-specific
-> held-out transportable
-> cross-checkpoint causal semantics
```

## Decisive Empirical Counterexamples

The paper needs compact demonstrations that invalidate common inference patterns:

1. Same probe accuracy, different causal effects;
2. Same answer subspace, opposite checkpoint transport;
3. Strong multi-site effect, zero residual after final-token subtraction;
4. Better geometric alignment, no causal rescue;
5. Same target effect, wrong-channel control equally strong.

At least two counterexamples must be reproduced outside ToM.

## Baselines and Controls

- linear and nonlinear probes with matched capacity;
- random-label and random-feature baselines;
- final-token and wrong-channel interventions;
- dose response and erasure tests;
- flip-budget eligibility conditioning;
- full-simplex versus one-vs-rest contrasts;
- paired versus shuffled stitching;
- geometric versus downstream-functional alignment;
- item-disjoint and task-disjoint evaluation.

## Promotion Gates

### Gate A: General theorem

At least one theorem must apply to general multiclass readouts rather than only
the current Qwen answer tokens.

### Gate B: Cross-task replication

At least two non-ToM tasks must reproduce a predicted separation between two
identification tiers.

### Gate C: Protocol utility

The proposed audit must change at least one mechanism verdict relative to a
standard probe or patching analysis.

### Gate D: Positive object

The paper must identify at least one canonical object that survives stronger
controls, such as one-vs-rest typed contrast under held-out specificity. A paper
containing only failures will be harder to position as oral-level methodology.

## Claim Ladder

**Safe current claim:** tested ToM mechanisms exhibit multiple readability,
controllability, specificity, and transport dissociations.

**After theorem and external tasks:** these dissociations instantiate general
identifiability limits for multiclass language-model mechanism analysis.

**After protocol validation:** the audit protocol prevents specific false-positive
mechanism claims and identifies stronger canonical alternatives.

**Not allowed:** all probes are useless, no hidden mechanism exists, linear methods
can never work, or the identified contrast is globally unique under nonlinear models.

## Oral Argument

The paper should be framed around a methodological failure mode:

```text
Mechanistic evidence is often promoted by changing measurement objects without
checking whether the new object identifies the claimed controller.
```

The main figure should be a four-column matrix—readability, controllability,
specificity, transportability—with concrete counterexamples and the exact gate
that changes each verdict.

## Risks and Fallbacks

- **Theory too narrow:** broaden from answer tokens to arbitrary multiclass linear
  readouts before adding more experiments.
- **External tasks do not replicate:** keep the work as a ToM-specific mechanism
  audit; do not claim general interpretability methodology.
- **No positive canonical object:** position as a benchmark for false mechanistic
  promotion, likely lowering oral potential.
- **Nonlinear methods rescue everything:** the contribution becomes conditions
  under which linear identification fails and nonlinear local response models help.

## Minimal Next Action

Write the general vertex non-identifiability theorem and instantiate the full
four-tier audit on one temporal-source and one deontic-authority task before
running any new large model suite.
