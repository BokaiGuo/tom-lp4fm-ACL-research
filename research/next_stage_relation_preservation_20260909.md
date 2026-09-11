# Updated Next-Stage Research Plan: From Representation to Commitment

Chinese counterpart: [下一阶段研究方案更新：从关系表征到答案承诺](next_stage_relation_preservation_20260909_zh.md).

Updated: 2026-09-09

This document places the submitted LP4FM paper, the five proposed branches, and
the newly reviewed ACL 2026 work in one claim-bounded program. It is a research
map, not evidence for a unified neural mechanism.

## 0. Executive Decision

The five ACL 2026 papers should not be merged into a story about one common
mechanism spanning memory, attention, causality, ToM, and semantics. Their useful
commonality is functional:

```text
valid task or relation specification
-> relation-bearing representation
-> access and retention under constraints
-> arbitration among competing sources
-> commitment in the final answer or first action
```

The project should therefore operate at three levels:

1. **Program-level question:** when is a relation preserved from context to
   decision?
2. **Best-Paper candidate law:** a demonstration identifies a latent source
   policy only where candidate policies disagree. See
   [Conflict Is Supervision](best_paper_conflict_as_supervision_20260909.md).
3. **Immediate execution object:** once a source policy is identified, can it
   transfer across content, answer encoding, and unseen conflict topology? This
   is the narrower ACG object.

The new strongest candidate is not a sixth parallel branch. It reorganizes the
existing branches around a distinction between **task identification** and
**relation execution**.

Current status: `argument_shaped`. LP4FM motivates and instruments the program;
it does not validate contextual policy induction or a cross-domain law.

## 1. Functional Chain

### Specification

Is the intended relation, intermediate state, and gold response uniquely and
validly defined? The critique of ImperfectiveNLI shows that a benchmark can
attribute a semantic failure before its relation labels are secure.

### Encoding

Can the relation-relevant source, role, event state, or rule be decoded? A
readable representation is evidence for availability, not behavioral use.

### Access and retention

Does that information remain usable under distance, interference, attention
topology, or resource constraints? The local-attention and resource-rational
memory papers inform this boundary without establishing semantic mechanisms.

### Arbitration

When belief, reality, context, report, or result state disagree, which source is
allowed to control the answer? This is LP4FM's strongest existing object.

### Commitment

Which relation or source is enacted by the final answer or first action?
CoSToM-style steering, ImplicitMemBench-style first-attempt protocols, and the
project's answer-simplex controls measure different parts of this step and must
not be conflated.

These are functional distinctions, not claims that each stage maps to a unique
Transformer module.

## 2. Evidence Audit of the New Papers

| Work | Established level | Useful import | Unsafe extrapolation |
|---|---|---|---|
| [Imperfective Paradox](https://aclanthology.org/2026.acl-long.689/) | Event-semantic behavior and representation/decision dissociation | Separate intermediate event relations from final NLI decisions | Universal deep semantic failure before benchmark validity is secured |
| [Imperfective benchmark critique](https://arxiv.org/abs/2608.25005) | Substantial ambiguity and relation-assignment concerns | Put semantic validity before model diagnosis | Treat the critique as a final resolution of aspect understanding |
| [Local Attention](https://aclanthology.org/2026.acl-long.1739/) | Formal expressivity under specified precision, depth, and positional assumptions | Use architecture as an access/expressivity boundary | Infer that local attention produces semantic understanding |
| [Memory Efficiency](https://aclanthology.org/2026.acl-long.1550/) | Resource-constrained encoding and partial human reading-time fit | Separate retrieval limits from representation quality | Equate injected noise with human memory or general understanding |
| [CoSToM](https://aclanthology.org/2026.acl-long.421/) | Causal tracing plus supervised activation steering on strategic ToM tasks | Reuse intervention controls and require transport | Treat steering gains as intrinsic ToM or a cross-domain typed controller |
| [ImplicitMemBench](https://aclanthology.org/2026.acl-long.1301/) | Exposure--interference--first-attempt behavioral protocol | Test persistence without explicit recall prompts | Infer human-like unconscious memory from an operational analogy |
| [Shaping Schema](https://arxiv.org/abs/2605.09271) | Position paper plus representation-dependent logic-circuit results | Representation can alter knowledge organization and activation | Use broad schema language as an identified source mechanism |

## 3. Updated Paper Architecture

### Paper A: Submitted LP4FM

```text
language form
-> typed source arbitration under fixed latent worlds
-> checkpoint-sensitive subadditive composition boundary
-> same-item response reconstruction != typed controller transport
```

LP4FM contributes a static behavioral discovery and a mechanism boundary. It
supplies the typed source ontology, conflict grammar, answer-interface scorer,
frozen primitive/compound protocol, affine-gain audit, and checkpoint limits.

### Paper B: Conflict as supervision

```text
candidate source policies
-> source-consensus versus source-diagnostic demonstrations
-> contextual policy identification
-> cross-content and unseen-topology execution
```

The central candidate law is that correct examples carry relational supervision
only where candidate policies disagree. This is the Best-Paper-level bet.

### Paper C: Cross-domain representation-to-commitment framework

Only if at least two independent domains exhibit a reproducible separation
between relation readability, retention, arbitration, and final commitment:

```text
valid specification
-> readable relation
-> retained relation
-> source arbitration
-> answer or action commitment
```

Paper C must remain conditional. Cross-domain analogy is not evidence for one
neural circuit.

## 4. How the Five Older Branches Fit

| Branch | Updated role | Claim to avoid |
|---|---|---|
| Training-induced composition phase transition | Track checkpoint prior and responsiveness to diagnostic evidence | Base/Instruct endpoints establish instruction-tuning causality |
| Behavioral algebra of source accessibility | Test graph composition after policy identification | Binary subadditivity uniquely identifies a universal operator |
| Mechanistic identifiability | Enforce readability/control/specificity/transport tiers | A probe or same-item patch identifies a controller |
| Conflict-topology kernel | Define policy-separation and held-out source graphs | A behavioral graph proves a shared neural router |
| Source-accessibility training objective | Prefer diagnostic evidence while suppressing wrong-source capture | Accuracy or consistency alone proves repair |

## 5. Core Experimental Sequence

### Study 0: Relation validity

- independently annotate intended relations and intermediate source states;
- report ambiguity and exclusion rates;
- use deterministic validators for source agreement and topology;
- freeze discovery, holdout, and confirmation pools.

### Study 1: Demonstration policy identification

Compare zero-shot, increasing source-consensus demonstrations, a matched single
diagnostic replacement, two valid diagnostic demonstrations, a genuine
across-demonstration policy conflict, and an explicit-policy upper bound. The
conflict condition must contain one target-consistent diagnostic item and one
matched diagnostic item labeled by a rival policy; source-consensus items alone
do not create policy conflict. Every evaluation query must make candidate
sources disagree.

### Study 2: Cross-content and cross-encoding transfer

Change latent worlds, entities, source names, answer labels, constructions, and
positions. Relation transfer, not same-item response inversion, is the target.

### Study 3: Unseen conflict-topology composition

Freeze primitive source relations, hold out binary and ternary compounds, and
separate insufficient policy identification from failure to execute a uniquely
identified policy.

### Study 4: Resource and architecture boundary

Only after the primary behavioral law is stable, manipulate interference,
position, encoding precision, and local/global attention in a controlled
substrate. These variables explain access limits; they do not establish source
semantics by themselves.

### Study 5: Mechanism and training

Use aligned, wrong-channel, shuffled, norm-matched, and final-token controls.
Require cross-content policy transport. Longitudinal checkpoint analysis or
controlled tuning is a separate causal claim and should not block Paper B.

## 6. Related-Work Boundary

The closest ICL work already covers task ambiguity, disambiguating examples,
demonstration conflict, semantic mapping ambiguity, structural coverage, task
recognition, and counterfactual prompting. The new work must therefore show
more than any one of those phenomena.

Its distinct object is:

```text
which candidate typed source policies are behaviorally equivalent on D
-> which within-item source conflicts separate them
-> whether the identified relation transfers and composes
```

Key direct neighbors:

- [Task Ambiguity in Humans and Language Models](https://arxiv.org/abs/2212.10711)
- [Can Input Attributions Explain Inductive Reasoning in ICL?](https://aclanthology.org/2025.findings-acl.1092/)
- [Understanding the Dynamics of Demonstration Conflict](https://arxiv.org/abs/2603.04464)
- [Debiasing ICL Demonstrations](https://aclanthology.org/2024.findings-acl.430/)
- [Context-faithful Prompting](https://aclanthology.org/2023.findings-emnlp.968/)
- [Rethinking the Role of Demonstrations](https://aclanthology.org/2022.emnlp-main.759/)
- [CoFe](https://aclanthology.org/2023.acl-long.618/)
- [Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/)

## 7. Evidence Ladder

| Claim | Minimum evidence | Safe wording |
|---|---|---|
| Relation is valid | Independent annotation plus ambiguity report | controlled relation stimulus |
| Relation is readable | Held-out probe or intermediate judgment | relation information is decodable |
| Demonstrations identify a policy | Matched consensus/diagnostic manipulation and held-out conflict query | contextual policy identification observed |
| Relation transfers | New content, encoding, and source labels | relation-level transfer observed |
| Relation composes | Frozen primitives and held-out topology | bounded authority composition |
| Typed causal control | Specificity controls plus cross-content transport | bounded typed control |
| Training causality | Matched trajectory and controlled exposure | training changes behavior under stated scope |

## 8. Minimum Pilot and Decision Rule

Start with the smallest decisive ToM contrast:

```text
one diagnostic false-belief demonstration
versus
eight matched true-belief source-consensus demonstrations
```

Evaluate Qwen2.5-7B Base/Instruct with Qwen3 as a frozen boundary, independent
latent worlds, answer/source permutations, and target-versus-competitor margins.
Directly compare a target-consistent diagnostic replacement with a matched
rival-policy diagnostic replacement, while auditing selection-blinding.

Proceed to cross-domain work only if the diagnostic replacement survives
position, label, lexical-twin, and confirmation-world controls. Otherwise keep
the narrower ACG program.

## 9. Current Position

```text
program object
  relation preservation from context to decision

strongest candidate law
  source-policy disagreement determines relational teaching information

immediate empirical object
  identify a source policy, then test ACG transfer and composition

LP4FM role
  static typed arbitration, instruments, and checkpoint/mechanism boundaries

status
  argument_shaped; no contextual or cross-domain law has been validated
```
