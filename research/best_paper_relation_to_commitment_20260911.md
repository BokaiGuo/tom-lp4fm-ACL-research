# Best-Paper Research Plan: Conflict Identifies, Priors Decide

Chinese counterpart: [Best Paper 级下一阶段方案：Conflict Identifies, Priors Decide](best_paper_relation_to_commitment_20260911_zh.md).

Updated: 2026-09-11

Status: `not_evaluated`. This is a research projection grounded in the submitted
LP4FM paper, the existing branches, and *The Imperfective Paradox in Large Language
Models*. It is not evidence that the proposed law already holds.

> **2026-09-11 canonical revision:** this version absorbs the external review and
> sharpens the paper into a two-stage theory, `identification -> commitment`. The main
> paper has exactly three experiments: information rather than example count,
> identification × prior pressure, and functional partner adaptation. The former Study
> 0–6 details remain as supporting protocols or conditional extensions rather than seven
> parallel contributions.

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

> **Conflict Identifies, Priors Decide: Identification to Commitment in Language Models**

“Commitment Gap” is only a provisional internal shorthand. The paper should first use the
descriptive term **relation–commitment dissociation** and define its measurement explicitly.
The claim to pursue is not merely “the model knows but does not use it.” It is sharper:
demonstration information is determined by whether examples separate competing policies,
and successful identification is followed by a distinct prior competition at final answer
time.

### The single main-paper claim

> **Relational in-context learning contains two separable bottlenecks: identification
> and commitment. Demonstrations identify a governing relation only where competing
> policies make different predictions; even after the relation becomes identifiable and
> behaviorally accessible, a competing prior can override it at final commitment.**

The Chinese formulation is: **demonstrations provide relational supervision only when they
exclude competing relations; after identification, the final answer may still return to a
default source under prior pressure.**

### Three main experiments

1. **Information, not example count:** compare `C1/C2/C4/C8/C16` source-consensus examples,
   one source-diagnostic example, and a target diagnostic example plus a rival-policy
   diagnostic example under matched lexical, length, answer-frequency, and selection
   distributions. Test whether policy-separation coverage or version-space reduction
   predicts held-out policy performance better than count, similarity, or diversity.
2. **Identification × Prior Pressure:** cross `consensus/diagnostic` demonstrations with
   `prior-aligned/opposed` targets and plot independent relation readout against native
   final commitment. The decisive cell is high readout with prior-following native output;
   native, independent-readout, and explicit-rule-oracle arms must be fully separated.
3. **Functional partner adaptation:** move from passive “where does Alice believe?” to a
   coordination game. The model infers a partner's latent source policy from diagnostic
   behavior, then chooses a message or action for new content and unseen topologies.

Add only one small consequence section: a greedy demonstration selector that maximizes
pairwise policy separation or version-space reduction, compared with random, similarity,
diversity, and larger-budget baselines. Do not relabel active example selection as the
paper's novelty.

### What is cut or demoted

- LP4FM saturation, affine transport, and checkpoint geometry remain Paper-A discovery
  and boundary evidence, not a second Paper-B mainline.
- Mechanistic interpretability enters only after behavioral dissociation and remains a
  boundary/optional section; current evidence does not support a universal typed controller.
- Event semantics becomes a validity-screened external replication or supplement, not the
  main cross-domain novelty.
- Replace global claims about “true understanding” with measurable identification,
  readability, commitment, retention, and execution layers.

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

### Prior calibration must be independent

“Prior pressure” cannot be assigned from the observed test answer. Before the main
experiment, use a disjoint calibration split with neutral instructions and no diagnostic
demonstrations to estimate the model/checkpoint's default policy:

```text
P_k(y | x,T, neutral context)
prior policy      = argmax_y P_k(y | neutral context)
prior pressure    = log P_k(y_prior) - log P_k(y_relation)
```

Freeze this calibration before selecting the confirmation items. The primary analysis uses
the pre-registered aligned/opposed bins; a continuous pressure score is a secondary analysis.
If the calibration prior is unstable across paraphrases, the item is not a valid prior-
pressure test. Narrative salience, source order, and answer frequency may be manipulated only
as matched controls, not used to define the prior itself.

Example information is not defined by quantity. For a candidate policy set `Pi`, let
`h_pi(d)` be policy `pi`'s prediction on demonstration `d`:

```text
V(D) = {pi in Pi : h_pi(d) = y_d for every d in D}

C_sep(D) = 1 / choose(|Pi|, 2)
           * sum_{a<b} 1[there exists d in D such that h_pi_a(d) != h_pi_b(d)]

VR(D) = log |Pi| - log |V(D)|
```

`C_sep` measures how many candidate-policy pairs are separated by the demonstrations;
`VR` measures version-space reduction. Neither is presented as a new mathematical
theorem. The empirical question is whether either predicts held-out relation use better
than example count, similarity, lexical diversity, or format coverage.

### Terminology ledger

| Term | Operational use | Claim status |
|---|---|---|
| relation identification | demonstrations eliminate competing source policies | functional description |
| relation readability | an independent protocol reads out the target relation | proxy, not use |
| final commitment | native label, answer token, or first action | operational definition |
| relation–commitment dissociation | readout supports the relation while native output follows a rival prior | operational definition |
| policy-separation coverage | fraction of candidate policy pairs separated by demonstrations | explicit analysis quantity |
| partner adaptation | inferring a partner policy and changing communication/action | functional social reasoning, not intrinsic ToM proof |

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

- **ToM benchmarks and partner adaptation:** [ToMBench](https://aclanthology.org/2024.acl-long.847/)
  and OpenToM broaden task and narrative coverage, so a larger false-belief benchmark is
  not our contribution. [Theory of Mind Benchmarks are Broken](https://proceedings.mlr.press/v267/riemer25a.html)
  argues that static QA is insufficient if a model does not adapt to a partner's behavior;
  this motivates the third main experiment. [Language Models Represent Beliefs of Self and
  Others](https://proceedings.mlr.press/v235/zhu24o.html) occupies the belief-readout space,
  while [CoSToM](https://aclanthology.org/2026.acl-long.421/) studies internal-to-behavior
  misalignment and steering. Our separation is policy-separation analysis followed by a
  test of whether the relation controls native answers and coordination actions.

## 5. Main paper and supporting protocol

The main text contains only three experiments:

```text
Experiment 1  Information, not example count
Experiment 2  Identification x Prior Pressure
Experiment 3  Functional partner adaptation
Small consequence  Policy-separation demonstration selector
```

Semantic validity is a prerequisite for all three. Cross-domain prior pressure, prompt
repair, checkpoints, resource constraints, and mechanism analyses are supporting material
or later work, not parallel headline contributions.

### Prerequisite: validity and selection audit

Use ToM source arbitration as the primary domain:

- ToM: queried belief versus reality/other-agent/distractor.

Context-versus-parametric-default and event process-versus-culmination tasks are optional,
validity-screened stress tests. They are not required headline domains and cannot be used to
rescue an unstable ToM result.

For every included domain, use human or expert blind annotation, explicit uncertainty, lexical
and answer-frequency matching, relation-preserving paraphrases, source swaps, label shuffles,
and non-overlapping discovery/holdout/confirmation worlds. Primary gold labels cannot be
supplied by an LLM judge alone.

Source-consensus examples count as non-diagnostic only under approximately matched selection:

```text
q(x,T,surface | pi_a) = q(x,T,surface | pi_b)
```

The decisive 2×2 is specified before the upstream dose-response for design clarity; the paper
should present the results as Experiment 1 → 2 → 3.

### Design-first specification: Identification × Prior Pressure (Main Experiment 2)

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
Estimate prior pressure on a separate neutral calibration pool, then freeze the target split;
do not label an item “prior-opposed” because the model happened to answer it incorrectly in
the main test.

### Main Experiment 1: Information, not example count

Compare:

```text
C1/C2/C4/C8/C16    source-consensus correct demonstrations
D1                  one target-consistent diagnostic example
D1 + C7             one diagnostic example plus consensus examples
D1 + incompatible D1  diagnostic examples supporting rival policies
explicit policy     rule upper bound
```

With `V(D)={pi:h_pi(d)=y_d for all d}`, report policy identification, diagnostic gain, and
inter-demonstration conflict cost. The latter must not be confused with within-example source
conflict. Compare `C_sep(D)` and `VR(D)` as predictors while controlling demonstration count,
embedding similarity, lexical/semantic diversity, label balance, position, length, and format
cues. The memorable signature, if it survives held-out confirmation, is:

```text
one diagnostic example > sixteen matched consensus examples
adding a rival diagnostic example reverses part of the gain
performance tracks policy separation better than example count
```

### Supporting boundary: prior pressure across domains

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

### Supporting diagnosis: repair and calibration

Compare strict definitions, counterfactual prompts, explicit source rules, diagnostic demos,
and optional activation interventions. Report target gain, neighboring-valid-relation loss,
prior-override reduction, calibration/abstention changes, and cross-content transfer.

A target improvement accompanied by broad rejection of valid neighboring relations is a
decision shift, not structural relation learning.

### Main Experiment 3: Functional partner adaptation

Static QA only shows that a model can emit a label about another person's state. It does not
show that the model changes its behavior according to the partner's policy. Use a minimal
partner-adaptation game:

```text
source state       = {partner belief, reality, report, distractor}
partner policy     = latent mapping from source topology to partner action
observer evidence  = consensus and diagnostic partner interactions
model decision     = message to partner or first coordination action
outcome            = coordination success / payoff
```

Consensus interactions leave candidate policies behaviorally equivalent. A diagnostic
interaction makes their predicted actions diverge, allowing the partner's action to identify
the governing policy. On held-out episodes, change people, objects, lexical forms, answer
encodings, and conflict topologies. Then perform a paired policy swap: only the partner's
source policy changes, so a policy-sensitive model should change its message or action.

The partner's action vocabulary, frequency, timing, and surface description are matched across
policies. The diagnostic action is not accompanied by an explicit source label; otherwise the
game measures label following rather than policy inference. A policy-swap pair keeps the world,
goal, action set, and partner identity fixed while changing only the source that governs the
partner's action.

Report coordination success, policy-consistent action, policy-swap sensitivity, verbal-report
versus native-action dissociation, and the gain from one diagnostic interaction over many
consensus interactions. Match action frequency and add identity/order swaps, source-label
permutations, no-conflict controls, and a random-partner baseline. This is evidence for
functional adaptation, not proof of intrinsic or human-like ToM.

The ACG execution audit remains part of this experiment: same-item reconstruction is not
cross-content generalization, and failure on unseen topology is an execution/commitment
boundary rather than a failed identification claim.

### Conditional supplement: checkpoints, resources, and mechanisms

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
- The ToM QA and partner-action settings should share the identification/commitment logic,
  without requiring a common internal vector or attention head. An external semantic domain
  is optional and cannot substitute for the functional action test.

Interpretation is conditional. If relation readout fails, do not call the result commitment
dissociation. If only ToM transfers, retain a ToM-specific boundary. If an oracle leaves a
large residual, report an execution/answer-interface boundary.

## 7. Best-Paper evidence gates

1. **Validity:** the ToM relation and partner-action protocol are independently and
   unambiguously specified; an external semantic domain is optional.
2. **Identification:** diagnosticity changes candidate-policy version sets, not just labels.
3. **Dissociation:** independent readout and native final answer separate under prior conflict.
4. **Transfer:** the effect survives new content, encoding, or topology.
5. **Model-family replication:** the identification × prior-pressure interaction appears in
   at least two independent model families or checkpoints; otherwise narrow the title.
6. **Functional consequence:** a partner-policy swap changes native communication/action and
   improves held-out coordination, not merely verbal policy reports.
7. **Selective repair:** interventions preserve neighboring valid relations or explicitly
   expose a calibration frontier; broad label replacement is not repair.
8. **Mechanism boundary:** causal language is used only if specificity and transport controls
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
readouts, and an explicit-rule oracle. Add 48 partner-adaptation episodes with consensus,
diagnostic, policy-swap, and seen/unseen-topology conditions. Event/context replication is
optional supplement work and must use independent templates.

Proceed to the full paper only if:

1. diagnosticity yields held-out policy gain;
2. prior-opposed targets show reproducible readout/final separation;
3. lexical, position, answer-frequency, and leakage controls survive;
4. partner-policy swap changes native action and improves coordination on held-out episodes;
5. at least two model families or checkpoints show the same identification × prior-pressure
   interaction;
6. the policy-separation selector beats a random/similarity baseline at one frozen budget,
   or its failure is recorded without making it a headline contribution.

If (2) fails, return to the narrower ACG/conflict-supervision paper. If (1) fails, do not
call the result relation induction. If (4) fails, retain a passive relation/commitment
boundary without claiming functional social reasoning; if (5) fails, make the result
model- or checkpoint-specific.

## 10. Suggested paper skeleton

1. A matched counterexample where the model reads the relation but follows the prior.
2. Why correct consensus demonstrations do not identify a source policy.
3. Information value: policy separation versus example count.
4. Identification × prior pressure: independent readout versus native commitment.
5. Partner adaptation and the paired policy swap.
6. Cross-content/topology execution and claim-bounded diagnostics.
7. Optional repair or external semantic validation in the supplement.

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
