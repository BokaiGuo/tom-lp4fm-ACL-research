# Next-Stage Paper Plan: Authority Compositional Generalization in Context

## Working Title

**What Does In-Context Learning Learn? Authority Compositional Generalization in Language Models**

Alternative titles:

- **Learning Who May Override Whom: Authority Composition in Context**
- **Who Gets to Write the Answer? In-Context Source-Relation Programs**
- **Beyond Task Mapping: Composing Answer Authority from Demonstrations**

Chinese companion: [下一阶段论文研究方案：上下文学习中的答案权威组合泛化](next_stage_icl_schema_rewriting_20260826_zh.md).

## 1. Central Thesis

The next paper should not present the five existing branches as five parallel
work packages, nor should it center on selecting a scalar composition operator.
Its candidate mother object is:

> In-context learning may induce a relational program that determines which
> represented information source is authorized to control an answer.

We call the resulting research object **Authority Compositional Generalization
(ACG)**: can a model infer from demonstrations which source may override which
competitor under a conflict topology, then re-instantiate that relation on
unseen content distributions, answer encodings, and joint topologies?

“Authority” is operational rather than metaphorical. Under a fixed latent
world and fixed candidate contents, a context can make the answer follow the
queried agent, reality, a reporter, a rule, another agent, or a distractor.

The submitted LP4FM manuscript establishes only the static discovery layer:
language form changes typed source arbitration, and compound behavior has
checkpoint-specific nonadditivity and transport boundaries. The new paper must
establish contextual induction and relation-level transfer with new evidence.

Current status: `argument_shaped`, not empirically validated.

## 2. Relationship to the Closest Work

### In-context examples and compositional generalization

[CoFe](https://arxiv.org/abs/2305.04835) shows that structural similarity,
diversity, simplicity, and coverage of required linguistic structures strongly
affect in-context compositional generalization. It asks which examples support
unseen combinations. ACG asks what relational object those examples induce:
do they only improve structural retrieval, or do they specify a source-
authority rule that survives content and encoding changes?

[A General Theory for Compositional Generalization](https://arxiv.org/abs/2405.11743)
provides a task-agnostic framing, a no-free-lunch result, and the notion of
generative effects. It prevents us from presuming that primitive authority
relations combine independently.

[On the Statistical Mechanisms of Distributional Compositional Generalization](https://proceedings.mlr.press/v267/fu25j.html)
is the closest theoretical neighbor. It separates insufficient supporting data
from a learning algorithm's knowledge-composition capacity and emphasizes
algorithm--rule compatibility and sensitivity. ACG instantiates its abstract
composition rule as `R(a,b|T)` and converts the distinction into three
experimental conditions: coverage-deficient, composition-heldout, and
compatibility-shift.

### Weights, context, and learning trajectories

[Breaking through the learning plateaus of in-context learning](https://proceedings.mlr.press/v235/fu24h.html)
conceptually separates a weights component from a context component.
[Learning Trajectories are Generalization Indicators](https://arxiv.org/abs/2304.12579)
uses update-level trajectory information to understand generalization. ACG
turns these broad components into a specific functional decomposition: a
weight-induced default authority prior, a context-induced authority update,
and their interaction.

The novelty is not the decomposition itself. It is testing whether the context
component changes who may control the answer and whether that policy transports.

### Language representation and schema

[Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
argues that representation design changes how knowledge is activated and
organized. ACG must be narrower than this position: “schema” is an explanatory
hypothesis, not the outcome label. The measurable object is a typed source
relation with cross-content and unseen-topology consequences.

### Instruction hierarchy and contextual conflict

[The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) installs an
explicit priority order among instruction channels. [Context-faithful Prompting](https://arxiv.org/abs/2303.11315)
changes reliance on contextual versus parametric knowledge. [Instruction--Data
Separation](https://arxiv.org/abs/2403.06833) formalizes whether a model treats
text as instructions or data.

ACG differs because no fixed privileged channel is supplied. Demonstrations
implicitly specify a relation graph, and the correct winner can change across
items and topologies. Hierarchy training and context-faithful prompting are
direct baselines, not interchangeable formulations.

### Belief routing and mechanistic work

[Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html),
[Brittle Minds](https://arxiv.org/abs/2406.17513), and [Language Models Use
Lookbacks to Track Beliefs](https://arxiv.org/abs/2505.14685) study internal
belief representations and causal interventions. Recent shared-router work on
[belief--reality separation](https://arxiv.org/abs/2607.11945) and [multiple
mental spaces](https://arxiv.org/abs/2607.10248) identifies a reusable internal
format for selecting a mental space.

ACG does not compete by proposing another router. It studies the functional
learning problem above the implementation: how context specifies which source
relation should be instantiated. A router mechanism would be an optional bridge,
not evidence that ACG has occurred.

The full literature matrix is available in [Related-Work Positioning](related_work_positioning_20260826.md).

## 3. Formal Object

Define a typed authority relation:

```text
R(a, b | T) = whether source a is authorized to defeat source b under topology T
```

Demonstrations expose pairs `(x,y)`, but ACG tests whether the model extracts the
latent relation rather than copies `y`:

```text
content generalization
  fixed R, new facts, entities, and latent worlds

representation generalization
  fixed R, new language realization, labels, and answer encoding

authority compositionality
  seen primitive R, unseen joint relation or conflict topology
```

For checkpoint `k`, demonstrations `D`, ontology `O`, and topology `T`, define a
functional authority schema:

```text
S_k(D,T) = (O, T, Pi_k(D,T), C_k(D,T))
```

where `Pi` is the induced source-authority policy and `C` describes how multiple
relations or constructions interact. A useful weights/context decomposition is:

```text
Pi_k(D,T) = Pi_weight,k(T)
          + Update_context,k(D,T)
          + Interaction_k(D,T)
```

This is a functional hypothesis, not a claim that the network contains three
separable modules.

## 4. Required Phenomena

### Authority inversion

Under a fixed latent world, candidate set, and answer encoding, demonstrations
switch the controlling source. Token frequency, position, length, and label
mapping must not explain the switch.

### Content--authority dissociation

The represented propositions remain available while source-specific margins and
argmax change. Conversely, new content is answered correctly under the induced
source priority.

### Relation transfer

An authority policy induced on one set of worlds or constructions transfers to
new worlds, encodings, realizations, or typed domains. Same-item response
reconstruction is not transfer.

### Authority compositional generalization

Primitive relations such as `belief > reality` and `rule > report` support an
unseen joint topology without a fixed source token always winning and without
an isomorphic compound appearing in the demonstrations.

## 5. How the Five Branches Fit

| Existing branch | Role in ACG | Claim that is no longer primary |
|---|---|---|
| Training-induced composition phase transition | Track default authority priors, plasticity, and change points | Endpoint Base/Instruct difference as training causality |
| Behavioral source-access algebra | Model relation composition and generative interaction | A preselected weakest-link or saturation law |
| Mechanistic identifiability | Separate content, authority, control, specificity, and transport | Response reconstruction as controller identification |
| Conflict-topology kernel | Represent source competition as a transferable graph | A universal kernel inferred from one domain |
| Source-accessibility objective | Selectively alter authority policy | Generic robustness or consistency gain as repair |

## 6. Experimental Program

### Study 0: Authority-schema benchmark

Generate latent worlds crossing:

- content: events, objects, states, and agent knowledge;
- sources: belief, reality, report, rule, other agent, distractor;
- topologies: single, pair, joint, and chain conflict;
- representations: natural language, structured slots, symbolic forms, and
  controlled rewrites;
- answers: independent permutations of order, token, label, and source identity.

Each target compound receives three supporting-distribution conditions:

```text
coverage-deficient
  at least one required primitive relation is absent

composition-heldout
  all primitive relations are present, but the joint rule is unseen

compatibility-shift
  data amount and primitive coverage are matched, but composition changes
```

Natural-language conditions require independent validation for grammaticality,
event equivalence, clear source attribution, and accidental relation leakage.

### Study 1: Context-induced source authority

Cross demonstration similarity, diversity, complexity, and source-policy
coverage. Compare label mapping, surface retrieval, content updating, and
relation induction using source capture, strongest-competitor margin, authority
entropy, off-policy leak, and cross-encoding identity accuracy.

### Study 2: Cross-content causal transport

Factorially cross the induced policy and world content. The primary estimand is
`cross-content source-policy preservation`, not same-item response error.
Behavioral transfer comes first; source-specific interventions are added after
the relation-level phenomenon is established.

### Study 3: Authority compositional generalization

Learn primitive authority edges, then predict unseen binary, ternary, and cross-
family topologies. Compare additive, scalar saturation, topology-conditioned
graph rewrite, context-dependent interaction, and surface-only models.

Diagnose three failures separately:

```text
data coverage deficit
authority-rule composition deficit
algorithm--authority-rule incompatibility
```

The leading predictive hypothesis combines:

```text
graph compatibility
  whether primitive authority edges are jointly satisfiable and identify a winner

model sensitivity
  whether each primitive edge causes a stable margin change across content
```

Only a satisfiable graph plus stable primitive sensitivity followed by joint
failure counts as a strict relation-composition deficit.

### Study 4: Training trajectory

Use a matched trajectory from base through continued pretraining, SFT, and
alignment. Track default policy, context-induced update, plasticity, transfer,
topology sensitivity, composition residual, and item-level geometry. External
models are anchors; they do not replace within-trajectory evidence.

### Study 5: Authority-aware training

Compare standard SFT, paraphrase consistency, typed routing, topology-balanced
training, and the full objective. Success means suppressing wrong-source capture
without flattening legitimate language sensitivity.

## 7. Identification Protocol

| Level | Question | Required evidence |
|---|---|---|
| Content readability | Are source propositions represented? | Held-out typed probes and content swaps |
| Authority accessibility | Does context change source margins? | Topology-crossed behavioral matrix |
| Authority controllability | Does intervention promote the intended source? | Source-specific intervention and matched wrong-channel controls |
| Relation transportability | Does the policy leave the original item and encoding? | Cross-content, cross-encoding, and cross-domain transfer |

The current construction intervention reconstructs same-item responses but does
not promote the intended typed source. It is therefore boundary evidence, not a
positive mechanism result for ACG.

## 8. Minimum High-Information Pilot

```text
2 domains: ToM + temporal/imperfective or deontic
3 policies: queried source, reality/report, distractor resistant
5 support regimes: none, coverage deficient, composition held out,
                   compatibility shift, direct-compound upper bound
20 independent latent worlds per regime
2 checkpoints: Qwen2.5-7B-Instruct + matched Base
```

The pilot must report:

1. same-item authority inversion;
2. cross-content source-policy transfer;
3. cross-encoding relation preservation;
4. primitive-to-unseen-topology composition;
5. graph-compatibility/model-sensitivity prediction;
6. answer-label, position, and source-token controls.

If only same-item inversion appears, the result is a bounded context effect. If
relations transfer across content and encoding, ACG merits full development. If
primitive relations also compose on unseen topology and the predictor beats
coverage/accuracy baselines, the study becomes oral-shaped. Cross-domain
transfer supports the broader functional-unit claim.

## 9. Relationship to the Submitted LP4FM Paper

```text
Paper A: language can alter which source controls the answer
Paper B: context can infer source relations and compose them on new distributions
```

Paper B's primary endpoints are:

```text
cross-content relation preservation
cross-encoding relation preservation
primitive-to-unseen-topology composition
coverage-versus-compatibility decomposition
```

Paper A supplies motivation, benchmark provenance, and boundary evidence only.
Paper B must not rename existing Stage-53--57 rows as ICL results.

## 10. Claim Ladder

### Bounded publishable version

Controlled demonstrations alter typed source arbitration under fixed semantics,
and the effect depends on structural coverage rather than answer labels.

### Oral-shaped version

ICL infers typed source relations and re-instantiates them on unseen content,
representations, and topologies; graph compatibility and model sensitivity
predict this ability beyond coverage and primitive accuracy.

### Broader theoretical version

The functional unit learned in context is not only a task mapping but a
relational authority program. Training determines which programs are compatible
with the model and how plastically context can instantiate them.

### Forbidden claims before evidence

- a universal epistemic institution across all LLMs;
- a single neural circuit identical to the behavioral schema;
- same-item reconstruction as cross-content transfer;
- instruction-tuning causality without a controlled trajectory;
- universal topology law from the current construction-specific evidence;
- ACG as validated before the new pilot is complete.

## 11. Current Status

```text
mother object: Authority Compositional Generalization
literature separation: explicit and argument-shaped
existing evidence: static motivation and boundaries only
next artifact: cross-content/cross-encoding/unseen-topology pilot
status: argument_shaped
```
