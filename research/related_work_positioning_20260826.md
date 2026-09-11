# Related-Work Positioning for the Next Research Stage

The current top-level entry point is [From Relation to Commitment](best_paper_relation_to_commitment_20260911.md).
The ICL, schema, ToM, memory, and steering neighbors below are positioned
against the narrower question of whether a contextual relation survives into
the final answer; they are not treated as evidence for one shared mechanism.

## Purpose

This document is a positioning argument, not a bibliography. It records what
the nearest research families already establish, which assumptions they share
with this project, what remains unidentified, and which evidence is required to
separate each proposed branch from its closest alternatives.

The project should not claim that in-context learning, compositional
generalization, belief representations, routing, or schema are unexplored. Its
candidate contribution is narrower: **inducing a typed source-relation program
from demonstrations and composing it over unseen answer-authority topologies**.

## 0. 2026-09-09: Required Narrowing After Nearest-Neighbor Search

Three easy novelty claims are closed by direct prior work:

- [AmbiBench](https://arxiv.org/abs/2212.10711) studies demonstrations that do
  not uniquely identify the intended task;
- [Ye et al. (2025)](https://aclanthology.org/2025.findings-acl.1092/) construct
  a critical disambiguating example and use it to audit ICL attribution;
- [Jiao et al. (2026)](https://arxiv.org/abs/2603.04464) study conflict caused by
  a corrupted-rule demonstration.

The project therefore cannot center novelty on task ambiguity, an aha example,
or the harm caused by conflicting demonstrations. Its stricter separation is:

```text
within-example source conflict
  candidate source policies predict different answers
  + a target-consistent label
  = diagnostic relational supervision

across-example policy conflict
  demonstrations support incompatible governing rules
  = corruption, noise, or unresolved normative ambiguity
```

ACG is the second half of this argument: once demonstrations identify a source
policy, test whether it transfers and composes. See the full
[Best-Paper candidate plan](best_paper_conflict_as_supervision_20260909.md).

## 1. Nearest-Neighbor Map

| Literature family | Representative work | Existing object | Required separation in this project |
|---|---|---|---|
| ICL example selection | [CoFe](https://arxiv.org/abs/2305.04835) | How similarity, diversity, complexity, and structural coverage affect unseen composition | Test whether examples induce a source-authority relation rather than only improving retrieval or accuracy |
| Components of ICL | [Learning Plateaus](https://proceedings.mlr.press/v235/fu24h.html), [task representations](https://aclanthology.org/2025.findings-acl.345/) | How weight-, context-, or task-vector components produce ICL | Instantiate the context component as an update to who may control the answer, then test transfer |
| Distributional compositional generalization | [Fu--Zheng 2025](https://proceedings.mlr.press/v267/fu25j.html), [general CG theory](https://arxiv.org/abs/2405.11743) | Supporting distributions, data coverage, and algorithm--rule compatibility | Define the rule as `R(a,b\|T)` and separate coverage, relation-composition, and compatibility failures |
| Long-context utilization | [Lost in the Middle](https://cs.stanford.edu/~nfliu/papers/lost-in-the-middle.arxiv2023.pdf), [IN2](https://arxiv.org/abs/2404.16811), [NoLiMa](https://arxiv.org/abs/2502.05167) | Position, length, lexical overlap, and supervision effects on context use | Measure changes in source authority rather than generic retrieval |
| ToM robustness | [Clever Hans](https://aclanthology.org/2024.eacl-long.138/), [FANToM](https://arxiv.org/abs/2310.15421), [DynToM](https://aclanthology.org/2025.acl-long.1171/) | Shortcut sensitivity, information asymmetry, and dynamic mental states | Use their controls while keeping typed source arbitration, not benchmark breadth, as the object |
| Belief representations | [Language Models Represent Beliefs](https://proceedings.mlr.press/v235/zhu24o.html), [Brittle Minds](https://arxiv.org/abs/2406.17513), [Lookbacks](https://arxiv.org/abs/2505.14685) | Whether beliefs are readable, bound to agents and states, and behaviorally editable | Separate readability, controllability, specificity, and transportability |
| Representation adequacy | [Standards for Belief Representations](https://arxiv.org/abs/2405.21030) | Accuracy, coherence, uniformity, and use | Operationalize these criteria at the source level and add cross-content relation preservation |
| Language representation and schema | [Shaping Schema](https://arxiv.org/abs/2605.09271) | How representation changes knowledge activation and organization | Replace broad schema language with computable source qualification and unseen-topology composition |
| Instruction and knowledge conflict | [Instruction Hierarchy](https://arxiv.org/abs/2404.13208), [Context-faithful Prompting](https://arxiv.org/abs/2303.11315), [Instruction--Data Separation](https://arxiv.org/abs/2403.06833) | Explicit priorities, contextual faithfulness, and instruction/data boundaries | Study implicit authority graphs induced by demonstrations without a fixed privileged channel |
| Steering and intervention | [Activation Steering](https://arxiv.org/abs/2410.12877), [ReFT](https://arxiv.org/abs/2404.03592), [Causal Abstraction](https://arxiv.org/abs/2301.04709) | Behavioral control and formal intervention semantics | Treat steering as a diagnostic; require relation-level transfer rather than a steering gain |
| Shared routers and mental spaces | [Belief--reality routing](https://arxiv.org/abs/2607.11945), [One mechanism for many mental spaces](https://arxiv.org/abs/2607.10248) | A shared router/value-slot implementation across mental spaces | Study contextual induction of the authority rule, not rediscovery of the router mechanism |

## 2. Branch-Specific Positioning

### Branch 1: Training-Induced Composition Phase Transition

The nearest work studies ICL phase changes, learning plateaus, learning
trajectories, and the data dependence of compositional generalization. This
branch shares their longitudinal analysis but changes the tracked variable:
each checkpoint indexes an operator over typed source-access constraints.

Its novelty cannot be “training dynamics matter.” A distinct result requires a
within-trajectory change in held-out source composition that survives matched
primitive accuracy, data coverage, calibration, answer encoding, and affine
gain controls. Endpoint Base/Instruct comparisons remain observational anchors.

### Branch 2: Behavioral Algebra of Source Accessibility

CoFe, COGS, and general CG theory ask whether familiar primitives can support
unseen combinations. This branch uses a different primitive: a typed source
relation measured as a conflict-conditioned answer margin. The operator is fit
before compound evaluation and tested on construction- and topology-held-out
combinations.

Subadditivity is not itself the novelty. The candidate contribution is a small,
pre-registered source-authority operator family that predicts unseen compounds
beyond additive, surface-only, and coverage-only baselines. Weakest-link and
smooth saturation remain live rivals until a ternary design separates them.

### Combined Branch: Learning the Algebra of Belief-Source Access

This proposal joins statistical compositional generalization with longitudinal
training analysis at one object:

```text
checkpoint k -> source-relation composition operator C_k
```

It is not a bundle of two papers. Its decisive evidence is the conjunction of
held-out operator prediction, a within-trajectory change point, and selective
controlled induction. Under the newer Authority Compositional Generalization
framing, this branch is the weights/training lane of the broader paper.

### Branch 3: Mechanistic Identifiability of Typed Controllers

The probing literature already establishes that readability does not imply
behavioral use. Belief-representation studies already decode and intervene on
mental-state features. Causal abstraction already formalizes patching,
steering, erasure, and mediation.

The distinct object is typed controller identification at a multiclass answer
interface: whether a method controls the intended source, is specific against
matched channels, and transports its causal semantics. Full-simplex vertex
non-identifiability, geometry--causality dissociation, and response-
reconstruction/controller dissociation are the candidate general results.
Cross-task replication is required before claiming a general audit theory.

### Branch 4: Conflict-Topology Kernel

Behavioral tests and ToM stress tests motivate controlled perturbations;
instruction hierarchy and contextual faithfulness study competing channels;
shared-router work proposes an internal implementation. This branch instead
asks whether a directed source-conflict graph predicts routing direction across
surface forms and domains.

The kernel must outperform surface similarity, lexical overlap, position,
explicit hierarchy labels, and raw accuracy on held-out prompt families. A
behavioral topology kernel is not evidence for a shared neural router without a
separate mechanism bridge.

### Branch 5: Source-Accessibility Training Objective

Instruction hierarchy, contextual-faithfulness methods, consistency training,
IN2, ReFT, and activation steering all alter how context or instructions affect
outputs. Their targets are fixed priority, context preference, invariance, or
efficient task control.

This branch targets conditional source authority. It must suppress wrong-source
capture while preserving sensitivity to legitimate changes in attribution,
reference, commitment, or evidential scope. The decisive comparison is a Pareto
frontier between wrong-source suppression and valid-language sensitivity, not
accuracy alone.

## 3. Paper A and Paper B

The submitted LP4FM manuscript is Paper A:

```text
language form -> typed source arbitration -> checkpoint-sensitive boundary
```

It supplies the source ontology, conflict-topology grammar, answer-interface
measurements, and negative transport boundaries.

The proposed ACG study is Paper B:

```text
demonstrations -> source-relation program -> unseen-content/topology composition
```

Paper B must use new demonstrations, cross-content factorials, answer-encoding
permutations, unseen topologies, and at least one new typed domain. Stage-53--57
can motivate the study but cannot count as its primary evidence.

## 4. Likely Novelty Objections

| Objection | Insufficient response | Required empirical separation |
|---|---|---|
| “This is prompt sensitivity.” | Rename the effect as schema | Preserve source relations across content, encoding, and topology while excluding labels, position, and retrieval |
| “This is standard CG.” | Emphasize the ToM domain | Show that the composition unit is typed authority relation, not task/output primitive |
| “This repeats belief probing.” | Report probe accuracy | Demonstrate use, specificity, and transport; separate same-item reconstruction |
| “This is instruction hierarchy.” | Note that sources also have priorities | Induce an implicit relation graph without a predefined privileged channel |
| “This applies schema shaping.” | Claim that the schema is more specific | Define `R(a,b\|T)`, supporting-distribution splits, and unseen joint-topology prediction |
| “This is a steering method.” | Report a larger gain | Make relation transfer and composition primary; use steering only as a diagnostic |

## 5. Evidence Boundary

```text
related work: verified background, precedent, contrast, and nearest neighbors
current LP4FM evidence: static typed arbitration and composition boundaries
next-stage hypothesis: contextual induction of a source-relation program
next-stage result: not yet validated
status: argument_shaped
```

The ACG object becomes a paper contribution only after cross-content,
cross-encoding, and unseen-topology transfer are demonstrated.

## Reference Entry Points

- [How Do In-Context Examples Affect Compositional Generalization?](https://arxiv.org/abs/2305.04835)
- [Breaking through the learning plateaus of in-context learning](https://proceedings.mlr.press/v235/fu24h.html)
- [Learning Trajectories are Generalization Indicators](https://arxiv.org/abs/2304.12579)
- [A General Theory for Compositional Generalization](https://arxiv.org/abs/2405.11743)
- [On the Statistical Mechanisms of Distributional Compositional Generalization](https://proceedings.mlr.press/v267/fu25j.html)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
- [Language Models Represent Beliefs of Self and Others](https://proceedings.mlr.press/v235/zhu24o.html)
- [Standards for Belief Representations in LLMs](https://arxiv.org/abs/2405.21030)
- [The Instruction Hierarchy](https://arxiv.org/abs/2404.13208)
- [Context-faithful Prompting](https://arxiv.org/abs/2303.11315)
- [Belief--reality separation lives in routing over a shared value slot](https://arxiv.org/abs/2607.11945)
- [One mechanism for many mental spaces](https://arxiv.org/abs/2607.10248)
