# Best-Paper Research Plan: From Relation to Commitment

Chinese counterpart: [Best Paper 级下一阶段方案：从关系到答案承诺](best_paper_relation_to_commitment_20260911_zh.md).

Updated: 2026-09-11

Status: `not_evaluated`. This is a research projection grounded in the submitted
LP4FM paper, the existing branches, and *The Imperfective Paradox in Large Language
Models*. It is not evidence that the proposed law already holds.

## 0. Executive decision

The most transferable idea in the Imperfective Paradox paper is not its event-semantic
benchmark. It is the separation between:

```text
whether a model can represent a relation
whether the model uses that relation in its final judgment
```

This upgrades the current program:

```text
Conflict Is Supervision   = identifying a relation from demonstrations
ACG                       = executing the relation across content and topology
Relation to Commitment    = preserving the relation at final answer time
```

The strongest candidate paper is therefore:

> **Language models can read out a relation yet commit to a competing default prior.
> Diagnostic conflict can identify a governing source policy, but identification does not
> guarantee final commitment.**

Recommended reader-facing title:

> **From Relation to Commitment: When Language Models Represent a Relation but Follow a Prior**

“Commitment Gap” is only a provisional internal shorthand. The paper should first use the
descriptive term **relation–commitment dissociation** and define its measurement explicitly.

## 1. What to borrow, and what not to copy

The ACL paper provides four design lessons:

1. choose a theoretically sharp relation rather than a broad ability label;
2. use matched minimal contrasts;
3. separate intermediate relational judgments from final labels;
4. treat prompt repair as a possible redistribution of errors.

Its concurrent critique adds a non-negotiable validity lesson: if the stimulus does not
uniquely determine the relation, an apparent model failure may be benchmark ambiguity.

Our paper must therefore make semantic validity and selection blinding a first study, not
an afterthought.

The direct distinction is structural. The Imperfective paper studies premise–hypothesis
event entailment. We study demonstration sets, competing typed source policies, target
transfer, prior pressure, and final answer commitment. Event semantics is an external
replication domain, not the novelty claim.

## 2. Scientific object

For an instance define:

```text
w        latent world/task instance
R        intended typed relation
S        candidate information sources
z_s      answer implied by source s
T        source/conflict topology
D        demonstrations or contextual evidence
P        default prior or surface-favored answer policy
M        intermediate relation readout
Y        final answer/label/first action
k        checkpoint
```

The candidate policy is `pi : (x,T) -> S`, with `h_pi(x,T)=z_pi(x)(x)`.

The program separates five possible breaks:

```text
validity        relation/gold label is underspecified
identification  demonstrations do not separate candidate policies
retention       the relation is not accessible under interference/resources
commitment      the relation is readable but absent from the final answer
execution       an explicit relation fails on new content/encoding/topology
```

The key operational quantities are:

```text
RR = Pr(M = R | valid, held-out readout protocol)
FC = Pr(Y = R | valid, prior-conflicting target)
PO = Pr(M = R, Y = P, P != R | valid, prior conflict)
CD = Pr(M = R, Y != R | valid, independent readout agreement)
DG = FC(diagnostic demonstrations) - FC(consensus demonstrations)
```

`M` is a proxy, not a synonym for a true hidden state. Primary conclusions should rely on
paired native answers, independent readouts, oracle controls, and wrong-channel controls.

## 3. Candidate law and decisive design

The candidate functional law is:

> Under semantically validated, selection-blinded matched contrasts, final sensitivity to
> a contextual relation requires both diagnostic evidence that separates candidate policies
> and sufficient relation retention against a competing default prior.

The first main figure is a 2×2:

| | Prior aligned with intended relation | Prior opposed to intended relation |
|---|---:|---:|
| Source-consensus demonstrations | easy but non-identifying | prior-dominated |
| Source-diagnostic demonstrations | diagnostic gain | **critical relation–commitment cell** |

The key signature is joint, not singular:

1. diagnostic demonstrations improve source-policy identification;
2. an independent relation readout is correct under prior conflict;
3. the native final answer still follows the prior;
4. primitive accuracy, length, tokenization, answer frequency, and affine-gain controls do
   not explain the effect.

If (2) fails, the evidence is an identification/access failure. If (2) holds and (3) holds,
the commitment-dissociation claim becomes appropriate.

## 4. Exact separation from nearest work

- **Imperfective Paradox:** event process/result entailment; ours adds demonstration-level
  relation induction, typed source competition, prior pressure, and cross-content transfer.
- **AmbiBench / critical examples / corrupted demonstrations:** ambiguity, informative
  examples, and conflict are known objects; ours measures policy distinguishability and then
  asks whether the identified relation survives final commitment.
- **Context-faithful prompting:** counterfactual context can overcome parametric priors;
  ours separates context responsiveness, relation identification, and final commitment.
- **Schema/task-representation work:** broad latent task structure is not a substitute for
  an enumerated candidate relation and matched source-policy reversal.
- **CoSToM and steering:** behavior can be changed in a setting; this does not establish a
  transportable typed controller. Steering is a later mechanism arm.
- **Memory/local-attention work:** these inform relation retention and expressivity, not a
  unified semantic mechanism.

## 5. Experimental program

### Study 0: validity and selection audit

Use three domains:

- ToM: queried belief versus reality/other-agent/distractor;
- event: process versus culmination with lexically matched minimal pairs;
- context: local context versus parametric default.

For every domain include human or expert blind annotation, explicit uncertainty, lexical and
answer-frequency matching, relation-preserving paraphrases, source swaps, label shuffles, and
non-overlapping discovery/holdout/confirmation worlds. Primary gold labels cannot be supplied
by an LLM judge alone.

Source-consensus examples count as non-diagnostic only under approximately matched selection:

```text
q(x,T,surface | pi_a) = q(x,T,surface | pi_b)
```

### Study 1: ToM 2×2 main experiment

Factors:

```text
diagnosticity = consensus vs. source-diagnostic demonstrations
prior pressure = aligned vs. opposed target prior
```

Use matched latent worlds where belief and reality are sometimes identical and sometimes
separated. The target asks for the typed source. Run three separate arms:

1. native final answer only;
2. independent relation readout on a separate replication set;
3. explicit relation/oracle upper bound.

Do not use one chain-of-thought to prove both relation recognition and native execution.

### Study 2: conflict as supervision

Compare:

```text
C1/C4/C8/C16       source-consensus correct demonstrations
D1 + C7            one target-consistent diagnostic example
D1 + X1 + C6       matched diagnostic examples supporting rival policies
explicit policy    rule upper bound
```

With `V(D)={pi:h_pi(d)=y_d for all d}`, report policy identification, diagnostic gain, and
inter-demonstration conflict cost. The latter must not be confused with within-example source
conflict.

### Study 3: prior boundary across domains

Compare domain-specific defaults:

```text
ToM       reality-copying/narrator salience
context   parametric knowledge
event     goal completion/sufficiency
report    narrator/world fact
```

Each domain gets consensus, diagnostic, prior-aligned, prior-opposed, explicit-rule, and
held-out-content conditions. The target comparison is the 2×2 interaction, not a pooled
“reasoning score.”

### Study 4: repair and calibration

Compare strict definitions, counterfactual prompts, explicit source rules, diagnostic demos,
and optional activation interventions. Report target gain, neighboring-valid-relation loss,
prior-override reduction, calibration/abstention changes, and cross-content transfer.

A target improvement accompanied by broad rejection of valid neighboring relations is a
decision shift, not structural relation learning.

### Study 5: unseen execution

Hold out people, objects, lexical realizations, answer encodings, ternary source topologies,
and unseen conflict graphs. This is the ACG half and the main separation from a static event
benchmark. Same-item reconstruction is not cross-content generalization.

### Study 6: checkpoints and mechanisms

Only after the behavioral dissociation replicates, compare available Base → SFT → alignment
checkpoints on prior strength, diagnostic responsiveness, relation readability, override, and
execution. Controlled SFT is needed before making a training-causal claim.

Resource and local-attention arms test retention, not source choice. Causal tracing/steering
requires final-token-only, wrong-channel, specificity, and transportability controls.

## 6. Predictions and interpretation branches

- More source-consensus examples will add less policy information than one matched diagnostic
  example under selection blinding.
- A diagnostic example will produce a held-out policy gain, not merely same-item repetition.
- Under prior conflict, relation readout can remain correct while native commitment follows
  the default prior.
- Two diagnostic examples supporting incompatible policies will incur a different cost from
  one within-example source conflict that identifies the target policy.
- Prompt repair can reduce one prior-driven error while increasing a neighboring valid error.
- At least two domains should share the functional 2×2 shape, without requiring a common
  internal vector or attention head.

Interpretation is conditional. If relation readout fails, do not call the result commitment
dissociation. If only ToM transfers, retain a ToM-specific boundary. If an oracle leaves a
large residual, report an execution/answer-interface boundary.

## 7. Best-Paper evidence gates

1. **Validity:** the main relation is independently and unambiguously specified.
2. **Identification:** diagnosticity changes candidate-policy version sets, not just labels.
3. **Dissociation:** independent readout and native final answer separate under prior conflict.
4. **Transfer:** the effect survives new content, encoding, or topology.
5. **Repair:** interventions do more than replace one label preference with another.
6. **Cross-domain:** an independent relation domain shows the same functional structure.
7. **Mechanism boundary:** causal language is used only if specificity and transport controls
   pass.

## 8. Relation to LP4FM and the existing branches

```text
LP4FM              Paper A: language changes typed source arbitration
Conflict           identification: diagnostic conflict teaches source policy
ACG                execution: transfer across content/encoding/topology
New headline       commitment: identified relation can still lose at final answer time
Mechanism branch   probe/control/transportability boundary
Training branch    later selective repair of prior override
```

The project-level claim becomes:

> Context does not merely provide an answer; it determines which relation is eligible to
> control the answer, and that relation may still be rewritten at final commitment.

## 9. Minimal pilot

Start with 96 independent ToM worlds, four matched 2×2 conditions per world, two lexical
realizations, non-overlapping holdout worlds, native final outputs, independent relation
readouts, and an explicit-rule oracle. Add small validity-screened event/context replications
with independent templates.

Proceed to the full paper only if:

1. diagnosticity yields held-out policy gain;
2. prior-opposed targets show reproducible readout/final separation;
3. lexical, position, answer-frequency, and leakage controls survive;
4. repair shows selective improvement or an explicit calibration frontier;
5. at least one independent domain reproduces the functional shape.

Otherwise keep the narrower ACG or conflict-supervision paper and do not inflate the claim.

## 10. Suggested paper skeleton

1. A matched counterexample where the model reads the relation but follows the prior.
2. Why correct consensus demonstrations do not identify a source policy.
3. The 2×2 diagnosticity × prior-pressure protocol.
4. Independent readout versus native final commitment.
5. Cross-content and unseen-topology execution.
6. Prompt-repair trade-offs and boundary conditions.
7. An independent semantic domain and a claim-bounded discussion.

Avoid claims that LLMs “do not understand semantics,” that a probe found a belief circuit,
or that every domain shares one teleological mechanism.

## 11. References

- [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/)
- [The Imperfective Paradox Is Not Necessarily in Large Language Models](https://arxiv.org/abs/2608.25005)
- [AmbiBench](https://arxiv.org/abs/2212.10711)
- [Critical Example Attribution](https://aclanthology.org/2025.findings-acl.1092/)
- [Demonstration Conflict](https://arxiv.org/abs/2603.04464)
- [Context-Faithful Prompting](https://aclanthology.org/2023.findings-emnlp.968/)
- [CoSToM](https://aclanthology.org/2026.acl-long.421/)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
- [Characterizing the Expressivity of Local Attention](https://aclanthology.org/2026.acl-long.1739/)
- [Memory Efficiency and Resource-Rational Encoding](https://aclanthology.org/2026.acl-long.1550/)
- [ImplicitMemBench](https://aclanthology.org/2026.acl-long.1301/)
