# Oral Branch 4: Conflict-Topology Kernels as General Diagnostics

## Working Title

**Beyond Prompt Sensitivity: Conflict-Topology Kernels for Source-Routing Diagnostics**

Alternative title:

**What Must Defeat What? Typed Conflict Topology Predicts Language-Model Routing Errors**

## One-Sentence Thesis

Many apparent wording effects can be decomposed into a direction-stable kernel
defined by typed source conflict and a construction-sensitive gain that controls
how strongly the kernel is expressed.

## Why This Is a Distinct Research Object

Prompt-robustness studies usually group examples by wording, template, or task.
The current Stage-48/49 evidence suggests a different invariant: the relevant
structure is which source must beat which typed competitors. Language form changes
the gain of this conflict, while topology predicts the direction of source routing.

The proposed object is:

```text
a conflict-topology kernel that predicts controller changes across prompt families
```

The contribution becomes a general behavioral diagnostic only if the kernel
predicts new domains and prompt families, rather than merely fitting the existing
false-belief matrix.

## Existing Evidence To Reuse

- Stage-2/6: typed candidates expose qualitatively different error controllers.
- Stage-44/46: language-form effects require independent latent sources and cannot
  be inferred from replay-equivalent prompt splits.
- Stage-47/48: language-form effects interact with typed conflict topology in
  same-item crossed designs.
- Stage-49: the directional topology pattern survives four controlled realizations,
  while construction-sensitive gain remains variable.
- Stage-50–52: topology-moderated margins are the measurement substrate for the
  later accessibility-cost and composition analyses.

Primary artifacts:

- `results/oral_readiness_pack_20260707/stage44_linguistic_pressure_manifest_20260801/`
- `results/oral_readiness_pack_20260707/stage46_independent_linguistic_20260801/`
- `results/oral_readiness_pack_20260707/stage47_language_pressure_interaction_20260804/`
- `results/oral_readiness_pack_20260707/stage48_same_item_topology_20260804/`
- `results/oral_readiness_pack_20260707/stage49_multitemplate_20260804/`

## Relationship to Related Work

### Behavioral stress tests and contrastive evaluation

[CheckList](https://aclanthology.org/2020.acl-main.442/) and [contrast sets](https://aclanthology.org/2020.findings-emnlp.117/)
establish the value of controlled perturbations and local decision-boundary
tests. ToM evaluations such as [FANToM](https://arxiv.org/abs/2310.15421),
[Clever Hans](https://aclanthology.org/2024.eacl-long.138/), and [DynToM](https://aclanthology.org/2025.acl-long.1171/)
show that apparent social reasoning can fail under information asymmetry,
adversarial changes, or evolving mental states. These works motivate the
factorial stimulus discipline, but their primary outputs remain correctness,
consistency, or error rates.

Our proposed invariant is different: the directed graph specifying **which
typed source must defeat which competitors**. The kernel is only useful if this
graph predicts the direction of source capture after surface realization,
domain, and candidate wording change.

### Instruction hierarchy and contextual knowledge conflict

[The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) formalizes an
explicit priority order among system, user, and third-party instructions.
[Context-faithful Prompting](https://arxiv.org/abs/2303.11315) studies conflicts
between parametric knowledge and supplied context, while [instruction--data
separation](https://arxiv.org/abs/2403.06833) asks whether models can distinguish
text to execute from text to process. All three concern competing information
channels and are direct conceptual neighbors.

This branch does not replace those taxonomies. It abstracts them into a common
behavioral topology and asks whether the same graph-level diagnostic transfers
across epistemic, temporal, deontic, spatial, and document-authority settings.
Unlike instruction hierarchy, source priority need not be predeclared; unlike
context faithfulness, the intended source can vary by item and topology.

### Shared routers and mental spaces

[Belief--reality separation lives in routing over a shared value slot](https://arxiv.org/abs/2607.11945)
and [One mechanism for many mental spaces](https://arxiv.org/abs/2607.10248)
provide a particularly close mechanistic alternative: a reusable router/slot
format can separate belief, reality, temporal, fictional, and counterfactual
spaces. Those papers identify internal routing structure and causal transfer.
Our kernel is a behavioral diagnostic over source conflicts; it must not be
presented as evidence for the same neural router without an independent
mechanism bridge.

The two programs are complementary. The topology kernel predicts **when and in
which direction routing should fail**; shared-router work proposes **where and
how mental-space selection is implemented**. A strong cross-domain result could
use router/slot interventions as an external validation, but behavioral transfer
must stand on its own.

### Exact novelty boundary and baselines

This branch cannot claim that prompt robustness, source conflict, or graph
representations are new. Its separable contribution would be a compact topology
kernel that predicts held-out prompt families and domains better than surface
similarity, lexical overlap, context position, explicit hierarchy labels, and
raw task accuracy. Stage-49 currently supports only a within-setting candidate,
not a universal kernel. See the [central related-work map](../research/related_work_positioning_20260826_zh.md).

## Formal Object

Let candidate sources be nodes `V={v_1,...,v_K}`. A task instance defines a
directed conflict graph `T`, where an edge `v_a -> v_b` means the instructed
source `v_a` must defeat competitor `v_b` for the intended answer.

For construction `c`, define an item-level language effect vector over topology
cells:

```text
Delta_i(c) = [Delta_i(T_1,c), ..., Delta_i(T_m,c)]
```

The kernel hypothesis factorizes:

```text
Delta_i(c) approximately equals g_i(c) K_i(T) + epsilon_i(c,T)
```

where:

- `K_i(T)` is the direction-stable conflict-topology kernel;
- `g_i(c)` is construction-sensitive gain;
- `epsilon` is residual interaction not explained by the factorization.

The research question is whether `K(T)` transfers across domains more reliably
than surface prompt features or raw accuracy.

## Cross-Domain Task Families

### Epistemic routing

Candidate sources:

- queried agent belief;
- reality;
- another agent belief;
- object or location distractor.

### Temporal routing

Candidate sources:

- prior event state;
- report-time state;
- current state;
- anticipated future state.

### Deontic routing

Candidate sources:

- system rule;
- user preference;
- quoted authority;
- distractor policy.

### Spatial perspective routing

Candidate sources:

- observer-relative location;
- actor-relative location;
- map/world coordinates;
- salient distractor frame.

### Multi-document authority routing

Candidate sources:

- primary source;
- later correction;
- quoted secondary source;
- retrieved but non-authoritative distractor.

At least two non-ToM families are required for a general diagnostic claim.

## Experimental Program

### Phase 1: Typed ontology construction

For each domain, define candidate source channels before generating prompts.
Every answer must map uniquely to one channel, and candidate tokens should be
counterbalanced across roles.

### Phase 2: Topology crossing

Hold the latent world fixed while crossing:

- source to be queried;
- competitor set;
- single versus joint conflict;
- language construction;
- context length or distractor pressure.

This separates topology from item difficulty and surface form.

### Phase 3: Kernel estimation

Estimate a low-rank topology kernel using discovery prompt families. Candidate
models include:

- sign-only kernel;
- rank-one kernel plus construction gain;
- low-rank kernel with domain-specific gains;
- fully saturated construction-by-topology table.

The kernel is useful only if the low-rank model predicts held-out prompt families
nearly as well as the saturated table.

### Phase 4: Predictive transfer

Freeze `K(T)` and predict:

- a new realization within the same domain;
- a new construction family;
- a new latent item pool;
- a new domain with an aligned source ontology;
- optionally, a new checkpoint.

The main test is error migration: can the kernel predict which wrong source gains
control under a new prompt condition?

## Baselines

- raw prompt accuracy;
- prompt length and perplexity;
- sentence or prompt embedding similarity;
- lexical-overlap and template-ID models;
- generic task difficulty;
- answer-token prior;
- fully saturated prompt-family table;
- no-topology construction-only model;
- random or shuffled topology labels.

## Metrics

- typed source capture rate;
- target-versus-strongest-competitor margin;
- controller swap confusion matrix;
- kernel sign agreement;
- held-out rank correlation over topology cells;
- held-out log score for source identity;
- variance explained beyond construction-only baselines;
- cross-domain alignment error.

## Decisive Predictions

1. Topology kernel sign patterns transfer better than raw wording-effect magnitudes.
2. Construction changes mostly rescale the kernel, while topology changes its
   direction or source ordering.
3. Joint conflict produces predictable deviations from the average single-conflict
   cells.
4. Models with similar accuracy can have different kernel rank or leak topology.
5. Instruction-tuned and Base checkpoints may share some topology structure while
   differing in gain and compound composition.

## Promotion Gates

### Gate A: Within-domain prediction

A frozen kernel must predict a held-out prompt family better than length,
perplexity, prompt embeddings, and construction-only baselines.

### Gate B: Cross-domain transfer

At least two non-ToM domains must preserve a meaningful kernel alignment or a
shared low-rank source-conflict factorization.

### Gate C: Error-source prediction

The model must predict which typed competitor captures the answer, not only
whether the final answer is correct.

### Gate D: Parsimony

The kernel model must beat a fully saturated table after complexity penalty or
on strictly held-out prompt families. Otherwise it is only a reparameterization.

## Claim Ladder

**Safe current claim:** typed conflict topology moderates tested linguistic effects
within controlled false-belief worlds.

**After within-domain prediction:** a conflict-topology kernel predicts held-out
prompt-family source routing in the audited domain.

**After cross-domain confirmation:** conflict topology is a reusable diagnostic
for source-routing behavior across tested domains.

**Not allowed:** all prompt sensitivity is topology, the kernel is a hidden neural
circuit, human reasoning follows the same graph, or unrestricted task transfer.

## Oral Argument

The paper should demonstrate that two prompts with low lexical similarity can
share the same routing kernel, while two nearly identical prompts can differ
because they instantiate different source conflicts.

The main figure should contain:

1. a typed source-conflict graph;
2. the learned low-rank kernel;
3. held-out prompt-family predictions;
4. cross-domain source-confusion matrices;
5. failure cases where construction-specific residuals remain.

## Risks and Fallbacks

- **Kernel is ToM-specific:** frame it as a typed ToM diagnostic, not a general
  foundation-model principle.
- **Cross-domain ontology alignment is ambiguous:** require domain-local kernels
  and test only shared invariants such as rank, sign, and joint-conflict curvature.
- **Prompt embeddings perform equally well:** show whether embeddings can predict
  typed error identity; if they can, the topology contribution is weaker.
- **Kernel needs high rank:** abandon the stable-kernel claim and study domain-specific
  conflict geometry instead.

## Minimal Next Action

Implement one temporal-routing and one deontic-routing stimulus family with the
same four-channel typed interface, then test whether the Stage-48/49 factorization
predicts held-out construction effects better than prompt embeddings.
