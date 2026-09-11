# Best-Paper Candidate: When Conflict Becomes Supervision

Chinese counterpart: [Best Paper 级下一阶段候选：当冲突成为监督](best_paper_conflict_as_supervision_20260909_zh.md).

The higher-level upgrade is [From Relation to Commitment](best_paper_relation_to_commitment_20260911.md).
This document now serves as the relation-identification half of that program:
it specifies when demonstrations distinguish candidate source policies, while
the upgraded paper studies whether an identified relation survives final answer
commitment.

Updated: 2026-09-09

Status: `argument_shaped`. This document specifies a research candidate with
Best Paper potential. It does not claim an award, an oral decision, or an
empirically established result.

## 0. Verdict

The current Authority Compositional Generalization (ACG) plan is shaped like a
strong main-conference paper, but it does not yet contain a Best-Paper-level
scientific update. “Learning a source-authority relation from demonstrations”
can still be read as a new task family rather than a result that changes how the
field understands demonstrations.

The closest literature also closes several easy novelty claims:

- AmbiBench already shows that examples may not uniquely identify a task;
- Ye et al. (2025) already construct a critical example that disambiguates an
  inductive rule;
- Jiao et al. (2026) already study corruption-induced conflicts among
  demonstrations;
- context-faithful prompting already uses counterfactual demonstrations to
  overcome parametric priors.

The candidate thesis must therefore be narrower and stronger:

> **In relational tasks, a correct demonstration supervises the governing
> relation only where candidate source policies predict different answers.
> Arbitrarily many source-consensus examples may leave the task unidentified,
> while one target-consistent source conflict can provide decisive relational
> supervision.**

The memorable distinction is:

> **Conflict is supervision when it separates candidate source policies;
> conflict is noise when demonstrations disagree about the policy itself.**

This separates two phenomena that are usually grouped together:

1. **Within-example source conflict:** belief, reality, report, local context,
   or result state imply different answers, and the gold response reveals which
   source governs the task. This is potentially useful supervision.
2. **Across-example policy conflict:** different demonstrations support
   incompatible governing policies. This is corruption, noise, or genuine
   normative uncertainty.

If this distinction yields a cross-domain law and a prospective demonstration
selector, the contribution is not another benchmark. It changes the effective
unit of supervision from the number of correct examples to the candidate
relations those examples distinguish.

## 1. Why This Can Exceed the Current ACG Plan

ACG asks whether demonstrations induce a source-authority relation that
transfers across content, answer encoding, and unseen conflict topologies. That
is already sharper than generic prompt sensitivity, but it remains vulnerable
to four readings: task induction in a new domain, a ToM compositional-
generalization benchmark, context faithfulness, or instruction hierarchy.

The proposed law can instead explain several otherwise disconnected findings:

```text
Random labels sometimes matter little
  -> many demonstrations convey format, label space, or input distribution
     without separating the target relation.

One "aha" example can matter greatly
  -> it is the first example that separates plausible rules.

A corrupted example can be disproportionately harmful
  -> it introduces incompatible evidence across demonstrations.

Counterfactual demonstrations improve context faithfulness
  -> context and prior knowledge finally predict different answers.

True-belief examples do not establish belief tracking
  -> follow-belief and follow-reality are observationally equivalent when the
     sources agree.
```

A Best-Paper candidate requires four aligned contributions:

1. a surprising but necessary law about when labels contain relational
   information;
2. a precise object, source-policy distinguishability, rather than broad
   schema language;
3. a decisive signature in which one matched diagnostic example outperforms
   many consensus examples, while corrupted policy conflict has the opposite
   effect;
4. a consequence for benchmark construction, demonstration selection, and the
   attribution of reasoning failures.

## 2. Formal Object

### 2.1 Sources and policies

For input `x`, let

```text
S = {s_1, ..., s_m}             candidate information sources
z_s(x) in Y                     answer implied by source s
Pi = {pi_1, ..., pi_K}          candidate source-selection policies
pi(x, T) in S                   selected source under topology T
h_pi(x, T) = z_{pi(x,T)}(x)     answer implied by policy pi
```

Examples include:

```text
ToM       queried belief / reality / other agent / distractor
factual   local context / parametric or default knowledge
event     process occurrence / culmination or result state
reporting reporter commitment / narrator or world state
```

A demonstration `d=(x,T,y)` is consistent with policy `pi` iff
`h_pi(x,T)=y`.

### 2.2 Three kinds of demonstrations

**Source-consensus demonstration.** Every candidate policy predicts the gold
answer. The example can be perfectly correct while providing no evidence about
which policy generated it.

**Policy-diagnostic demonstration.** At least two remaining policies predict
different answers, and the gold response eliminates at least one. Sources
conflict within the item, but the item is fully consistent with the target
policy.

**Policy-inconsistent demonstration.** Its label cannot be jointly explained
with target-consistent diagnostic demonstrations by one candidate policy. It is
an across-example conflict, not diagnostic source disagreement. A single
corrupted diagnostic item plus source-consensus items is *not* an
across-example conflict, because consensus items support no competing policy.
The actual conflict condition must contain at least one target-consistent
diagnostic item and at least one matched diagnostic item supporting a rival
policy.

### 2.3 Version set

For demonstrations `D`, define

```text
V(D) = {pi in Pi : h_pi(x_i,T_i) = y_i for every d_i in D}.
```

The target `pi*` is identified iff `V(D)={pi*}`. This version-space statement is
not presented as a new theorem; it is inherited from concept learning and
machine teaching. The candidate novelty is its instantiation as typed natural-
language source policies and the empirical claim that source-conflict topology
predicts relational ICL beyond count, correctness, similarity, and ordinary
structural coverage.

### 2.3.1 Necessary condition: selection-blinded demonstrations

“Source-consensus examples are relationally zero-information” is not an
unconditional statement about arbitrary natural-language prompts. It holds only
under a controlled intervention: conditional on observable length, lexicon,
construction, label frequency, position, and source mentions, the teacher or
generator's distribution over selected source-consensus items must not vary by
target policy:

```text
q(x, T, surface | pi_a) = q(x, T, surface | pi_b)
for every competing pi_a, pi_b in Pi.
```

Otherwise selection itself leaks the target policy. For example, a teacher could
systematically choose different names, lexical items, ordering, or extreme
worlds for follow-belief than follow-reality prompts. A model could then update
without learning from the label relation. Study 0 must audit and balance `q`,
while Study 1 randomizes selection and order and confirms results on a new
surface pool.

Under that condition, if `h_pi_a(d)=h_pi_b(d)=y`, the observed label has equal
likelihood under the two policies. “Zero information” here means **zero
label-likelihood separation within the preregistered policy class**. The same
example can still convey output format, label space, task family, grammar,
entity type, and general pragmatic information.

### 2.3.2 Bayesian language is a comparator, not a mechanism claim

The notation

```text
P(pi | D) proportional to P(D | pi) P(pi)
```

explains why source-consensus labels cannot update the *relative* posterior of
two equivalent policies. The paper does not assume that actual LLMs implement
exact Bayesian inference. The implicit-Bayesian and latent-variable ICL accounts
of Xie et al. and Wang et al. provide useful normative comparators; Pan et al.
separate task recognition from task learning; Falck et al. show that observed
LLM ICL fails necessary behavioral conditions for a general Bayesian learner.
This paper measures prior attraction and contextual update as behavior, not as
evidence that an internal posterior is literally represented.

### 2.4 Policy-separation coverage

Define target-relative coverage

```text
C_sep(D, pi*) =
  sum_{pi != pi*} w(pi) * 1[exists d in D: h_pi(d) != h_pi*(d)]
  ------------------------------------------------------------
                    sum_{pi != pi*} w(pi).
```

Use both:

- `w(pi)=1`, a model-independent measure;
- weights estimated from an independent zero-shot or non-diagnostic condition,
  a prior-weighted measure.

The model-independent measure must remain primary to avoid explaining a model
with a variable circularly derived from that same model.

### 2.5 Demonstration distinguishability graph

Construct `G_D` with candidate policies or typed sources as nodes. Add an edge
between two policies iff at least one target-consistent demonstration makes
them predict differently.

```text
no edges
  -> all examples are source-consensus

partial graph
  -> demonstrations identify only an equivalence class

target separated from every competitor
  -> target policy is identifiable

edges a>b and b>c, query a>c
  -> relation composition rather than mere identification
```

This separates surface or structural coverage from policy-separation coverage.

### 2.6 Limited propositions

**Controlled consensus non-identifiability.** If two policies make identical
predictions on selection-blinded demonstrations, observed labels cannot
distinguish them without an extra prior or cue.

**Target separation.** In a deterministic finite policy class, `pi*` is uniquely
identified iff every alternative policy is separated from `pi*` by at least one
target-consistent demonstration.

**Identification is not execution.** Even when `V(D)={pi*}`, the model may fail
to execute that policy on new content or topology. Experimentally estimate:

```text
identification gap = explicit-policy upper bound - nondiagnostic ICL
execution gap      = validated task ceiling - explicit-policy upper bound
```

These are functional contrasts, not claims about modular Transformer internals.

## 3. Nearest-Neighbor Boundary

| Closest work | Established result | Insufficient novelty here | Required separation |
|---|---|---|---|
| [Task Ambiguity in Humans and Language Models](https://arxiv.org/abs/2212.10711) | AmbiBench controls ambiguity among classification features and varies examples/instructions | Showing again that demonstrations underdetermine a task | Manipulate typed source agreement, predict transfer and graph composition |
| [Can Input Attributions Explain Inductive Reasoning in ICL?](https://aclanthology.org/2025.findings-acl.1092/) | One critical example disambiguates a synthetic inductive rule and serves as attribution ground truth | Another one-aha-example task | Make information structure the primary object across natural-language sources, multiple policies, and held-out topology |
| [Understanding the Dynamics of Demonstration Conflict](https://arxiv.org/abs/2603.04464) | A corrupted rule example harms ICL and induces competing representations | Another noisy-demonstration study | Directly contrast helpful within-item source conflict with harmful across-item policy conflict |
| [Context-faithful Prompting](https://aclanthology.org/2023.findings-emnlp.968/) | Counterfactual demonstrations improve use of supplied context | Replicating that intervention | Explain it by policy separation, transfer the law across domains, and derive a selector |
| [CoFe](https://aclanthology.org/2023.acl-long.618/) | Similarity, diversity, simplicity, and structure coverage affect compositional ICL | Adding one correlated coverage score | Matched factorial and prospective tests where policy separation adds predictive power |
| [Rethinking Demonstrations](https://aclanthology.org/2022.emnlp-main.759/) | Correct labels are sometimes less important than format, label space, and input distribution | Reporting that labels sometimes do not matter | Predict that correct labels matter specifically on policy-separating items |
| [Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/) | Novel in-context tasks may compose tasks learned during pretraining | Calling source routing a structured task | Separate contextual task identification from execution of the identified relation |
| [Debiasing ICL Demonstrations](https://aclanthology.org/2024.findings-acl.430/) | Multiple input-label mappings create semantic ambiguity and order sensitivity | Another mapping-ambiguity intervention | Compute source-state disagreement and test directed relation composition |

The novelty claim is therefore not “demonstrations are ambiguous” or “critical
examples exist.” It is the distinction between two levels of conflict and the
claim that a demonstration's relational teaching value is governed by which
latent source policies it separates.

## 4. Six Preregistered Predictions

### P1: More-correct-examples flat curve

Increasing source-consensus demonstrations from `1` to `2/4/8/16` should not
reliably improve relation accuracy on source-conflict queries. Predictions
should remain close to the checkpoint's default policy prior.

### P2: Single-diagnostic-example jump

Replacing one consensus example with a length-, label-, structure-, and
lexicon-matched diagnostic example should produce a much larger held-out gain
than adding several consensus examples.

### P3: Prior attraction under non-identification

When `V(D)` contains multiple policies, behavior should track an independently
estimated checkpoint prior. As policy-separation coverage rises, that prior
should explain less variance.

### P4: Graph coverage beats count

At fixed example count, label balance, and structural coverage, unseen-
topology performance should vary with `C_sep(D,pi*)` or remaining `|V(D)|`.
Examples that separate different policy pairs should outperform repeated
coverage of one pair.

### P5: Two conflicts have opposite effects

Replacing a source-consensus item with a target-consistent within-example source
conflict should improve policy identification. Replacing a target-consistent
diagnostic item with a state- and surface-matched diagnostic item labeled by a
rival policy should impair or destabilize it. The latter must co-occur with at
least one diagnostic item supporting the target policy to create an actual
across-example conflict. Compare both in the same generator, token budget, and
position.

### P6: Identification does not guarantee execution

Some conditions should remain difficult even after a diagnostic set uniquely
identifies the policy. An explicit-policy upper bound distinguishes relation
identification from cross-content or compositional execution.

## 5. Experimental Program

### Study 0: Semantic and policy audit

Before model evaluation:

1. specify a finite candidate-policy set without inventing alternatives from
   model errors after the fact;
2. store each source state `z_s(x)` for every item;
3. deterministically validate source agreement, disagreement, topology, label
   balance, and policy signatures;
4. independently annotate intended relations, intermediate source states, and
   plausible alternative interpretations;
5. audit whether the selection distribution leaks lexicon, entities, order,
   source mentions, structural coverage, or answer frequency across competing
   policies;
6. remove non-unique items and report exclusions;
7. freeze discovery, holdout, and confirmation pools.

The event domain must follow the warning raised by the critique of
ImperfectiveNLI: template membership is not sufficient evidence for aspectual
class or non-culmination.

### Study 1: The decisive matched factorial

Use two-source tasks and construct:

```text
Z0          zero demonstrations
C1/C2/C4/C8/C16
            source-consensus, target-consistent demonstrations only
D1+C7       one diagnostic plus seven consensus demonstrations
D2+C6       two diagnostic plus six consensus demonstrations
D8          eight diagnostic, target-consistent demonstrations
D1+X1+C6    one target-consistent diagnostic plus one matched diagnostic labeled
            by a rival policy, plus six consensus demonstrations; the only true
            across-demonstration conflict condition
E           explicit-policy upper bound
```

Every evaluation query must make candidate sources predict different answers.
Match demonstration count, token budget, label frequency, lexical and syntactic
content, source mentions, position, order, query similarity, output format, and
answer-token frequency.

Primary contrasts:

```text
diagnostic replacement gain > matched consensus-count gain

first diagnostic source-conflict effect
  performance(D1+C7) - performance(C8) > 0

across-demo conflict penalty, holding two diagnostic slots fixed
  performance(D1+X1+C6) - performance(D2+C6) < 0
```

### Study 2: Checkpoint prior and contextual update

For matched Base/Instruct checkpoints and at least one independent family,
estimate:

```text
default policy prior       = Z0 and consensus-only conflict queries
diagnostic responsiveness  = matched change after a D1 replacement
execution ceiling          = explicit-policy condition E
```

Plot models on two axes, prior quality and update efficiency, rather than rank
them only by mean accuracy. This offers a sharper interpretation of LP4FM's
checkpoint-sensitive boundary without treating endpoint differences as causal
evidence about instruction tuning.

### Study 3: Multi-source graph and unseen composition

Extend to three or four sources. Compare equal-size demonstration sets that:

- repeatedly separate one policy pair;
- separate the target from several competitors;
- show primitive edges but hold out a compound topology;
- provide a transitive chain and test an inferred edge;
- contain ambiguous or cyclic evidence.

This study asks whether graph coverage predicts source choice and whether a
uniquely identified policy is composable on an unseen topology.

### Study 4: Cross-domain confirmation

**ToM source arbitration**

```text
consensus  = true-belief worlds, belief == reality
diagnostic = false-belief worlds, belief != reality
target     = follow queried belief, reality, or another specified source
```

**Local context versus default knowledge**

```text
consensus  = local context agrees with the default answer
diagnostic = controlled counterfactual context disagrees with the default
target     = follow context or default source for the task family
```

The primary confirmation should use closed-world or fictional facts so that
unknown parametric knowledge does not define the gold state.

**Process versus result relation**

```text
consensus  = process occurred and culmination is independently established
diagnostic = process occurred while culmination is explicitly blocked
target     = infer occurrence, result, or the specified event relation
```

Use lexical matched pairs, multi-step judgments, and independent native-speaker
validation. If validity fails, this domain becomes a benchmark case study and
does not count as cross-domain confirmation.

### Study 5: Prospective demonstration selection

Compute each candidate item's policy signature from gold source states before
observing test labels. Compare greedy model-independent and prior-weighted
policy-separation selection against:

- random selection;
- semantic-similarity retrieval;
- diversity;
- CoFe-style simplicity and structural coverage;
- model uncertainty;
- cross-entropy-difference selection;
- a test-label oracle used only as an upper bound.

The scientific target is not another average-score selector. The method must
predict teaching efficiency under fixed token budgets on new content,
encodings, and preferably a held-out domain.

### Study 6: Optional mechanism bridge

Only after Studies 1--5 establish the behavioral law:

- compare policy readability under consensus and diagnostic support;
- test whether attribution targets policy-separating information rather than
  position;
- include wrong-channel, item-shuffled, norm-matched, and final-token controls;
- require cross-content and cross-topology transport;
- compare CoSToM-style steering and demonstration-conflict heads.

Mechanistic failure does not invalidate a behavioral law. Conversely, a probe
or head-ablation result cannot rescue a failed behavioral law.

## 6. Estimands and Analysis

Primary quantities:

```text
diagnostic replacement gain = performance(D1+C7) - performance(C8)
consensus count gain         = performance(C16) - performance(C1)
conflict sign contrast       = helpful source conflict - penalty from replacing
                               one valid diagnostic with a rival-policy diagnostic
prior retention              = agreement with independently estimated prior
identification gap           = performance(E) - best nondiagnostic performance
execution gap                = validated ceiling - performance(E)
transfer ratio               = cross-content gain / same-template gain
```

Use deterministic source identity, target-versus-competitor log-probability
margins, and off-policy leakage as primary outcomes. Do not rely on an LLM judge
as the sole endpoint.

The sampling unit is an independent latent world, predicate, or closed-world
fact. Cluster paraphrases, answer permutations, and source rows within items.
Use mixed-effects models or item-clustered bootstrap intervals, with frozen
discovery and confirmation pools.

Consensus items must be sampled by the generator rather than the task policy.
Explicitly test whether a surface-only prompt predicts policy above chance.

Competitively evaluate these predictors on held-out data:

```text
example count
correct-label count
semantic similarity
lexical overlap
diversity
structural coverage
token count and position
model uncertainty
policy-separation coverage
remaining version-set size
```

The last two must add predictive value in matched factorials and out-of-domain
tests, not merely correlate within one synthetic generator.

## 7. Main Figures and Table

**Figure 1: More correct examples can teach nothing.** Plot the consensus-only
shot curve, one diagnostic replacement, two valid diagnostic demonstrations, a
genuine across-demonstration conflict, and the explicit-policy upper bound. A
reader should immediately see that demonstration count and relational
supervision diverge.

**Figure 2: Demonstration distinguishability graph.** Show source states,
candidate-policy signatures, the remaining version set, and the held-out query.
Do not depict a neural circuit without causal evidence.

**Figure 3: Cross-domain law.** Plot precomputed policy-separation coverage
against cross-content relation transfer, with domains rather than model ranks as
the visual organization.

**Table 1: Prospective predictor comparison.** Compare count, similarity,
diversity, structural coverage, uncertainty, and policy separation under
leave-template-out and leave-domain-out evaluation.

## 8. Best-Paper Evidence Gates

### Gate A: Decisive phenomenon

- `D1+C7 > C8` is directionally stable in at least two model families and three
  domains;
- `C16-C1` is materially smaller than the diagnostic replacement gain;
- position, length, labels, surprisal, and lexical controls do not explain it;
- confirmation uses unseen latent worlds.

### Gate B: Beyond ambiguity and the aha example

- source-state agreement is directly manipulated;
- evidence extends beyond binary synthetic feature classification;
- graph coverage predicts multi-source and unseen-topology behavior;
- explicit-policy controls separate identification from execution.

### Gate C: One experiment reconciles prior findings

The same matched generator must show that target-consistent source conflict
helps, replacing a valid diagnostic with a rival-policy diagnostic harms,
consensus-example labels can be uninformative, and counterfactual evidence is
valuable specifically when candidate policies disagree.

### Gate D: Field consequence

At least one must hold:

1. a diagnostic selector improves held-out-domain teaching efficiency;
2. auditing an existing benchmark reveals that most training examples are
   non-diagnostic for its claimed relation and a corrected teaching set changes
   the conclusion;
3. explicit-policy controls reclassify some failures as underdetermination and
   others as genuine execution failures.

### Gate E: Prospective theory

The formal object must decide before experimentation which examples have zero
relational information, which replacement should cause a jump, which policies
remain equivalent, and which query requires composition. A post-hoc correlation
is insufficient.

## 9. Relationship to LP4FM and the Five Branches

The submitted LP4FM manuscript is Paper A:

```text
language form
-> typed source arbitration under fixed latent worlds
-> checkpoint-sensitive composition
-> same-item reconstruction != typed controller transport
```

It supplies the source ontology, conflict generator, answer-simplex scorer,
topology, checkpoint boundaries, and intervention claim ladder. It does not
contain demonstrations or manipulate demonstration-level policy
distinguishability, so it cannot count as primary evidence for this paper.

ACG becomes the execution and composition half of Paper B:

```text
diagnostic demonstrations uniquely identify a source policy
-> the identified policy must transfer across content and encoding
-> primitive source relations must compose on unseen topology
```

The five older branches now serve one chain:

| Branch | Role here |
|---|---|
| Training-induced phase transition | Separate checkpoint prior from diagnostic responsiveness |
| Behavioral source-access algebra | Graph composition and held-out topology |
| Mechanistic identifiability | Claim ladder and transport controls |
| Conflict-topology kernel | Policy-separation graph and cross-domain prediction |
| Source-accessibility objective | Later training to favor diagnostic evidence and reject policy-inconsistent evidence |

## 10. Minimum High-Information Pilot

Ask only:

> Does one matched diagnostic demonstration reliably outperform eight
> source-consensus correct demonstrations on held-out false-belief conflict
> queries?

```text
models       = Qwen2.5-7B Base and Instruct; Qwen3 as a frozen boundary
policies     = follow queried belief vs follow reality
support      = Z0, C1, C4, C8, D1+C7, D2+C6, D1+X1+C6, explicit-policy upper bound
worlds       = independent discovery and confirmation pools
encodings    = natural names plus permuted source and answer labels
queries      = belief-reality conflict only
metrics      = source identity, typed log-probability margin, off-policy leak
```

Move forward only if the diagnostic replacement survives placement, label,
source-token, lexical-twin, selection-blinding, and new-world controls, and
`D1+X1+C6` incurs the preregistered penalty relative to `D2+C6`. If it is
instruction-checkpoint specific, narrow to a prior/plasticity boundary. If
surface cues or answer copying explain it, return to the narrower ACG plan.

## 11. Paper Narrative

Open with eight correct true-belief demonstrations followed by a false-belief
query. The examples look sufficient but are exactly equivalent under
follow-belief and follow-reality policies. Replace one with a matched
false-belief demonstration and show the predicted change on new content.

The argument then follows:

```text
correct demonstrations can be relationally non-identifying
-> source disagreement creates policy-separating supervision
-> disagreement across demonstrations instead corrupts the policy
-> a distinguishability graph predicts transfer beyond count and similarity
-> explicit-policy controls separate identification from execution
-> diagnostic selection changes how ICL data should be built
```

Avoid claims that LLMs “do not truly understand language,” that five cognitive
papers establish one hidden mechanism, or that a broad conflict circuit has
been found.

## 12. Titles and Abstract Spine

Recommended title:

**Conflict Is Supervision: Identifying Latent Source Policies in In-Context Learning**

Alternatives:

- **When Agreement Teaches Nothing: Diagnostic Demonstrations for Relational ICL**
- **Correct Examples Need Not Identify the Task**
- **What Makes a Demonstration Informative? Source Conflict and Policy Identification**

Abstract spine:

1. ICL demonstrations are usually characterized by count, correctness,
   similarity, or structural coverage.
2. These measures omit whether competing relational policies make different
   predictions on the examples.
3. Formalize typed source-policy distinguishability and construct matched
   consensus, target-diagnostic, and rival-policy-diagnostic demonstration sets.
4. Test whether one diagnostic replacement produces a cross-content jump while
   additional consensus examples do not and replacing a valid diagnostic with a
   rival-policy diagnostic harms.
5. Test graph-based predictions across relational domains and unseen topology.
6. Evaluate a prospective diagnostic selector under fixed token budgets.
7. Use explicit-policy controls to separate task identification from relation
   execution.

Items 4--6 remain planned hypotheses until new experiments are complete.

## 13. Final Position

```text
program question
  how relations survive from context to decision

candidate law
  a demonstration teaches a source policy only where candidate policies disagree

decisive distinction
  within-example source conflict = potentially diagnostic supervision
  across-example policy conflict = corruption or ambiguity

formal object
  policy distinguishability, remaining version set, source-conflict graph

empirical burden
  one diagnostic example beats many matched consensus examples across domains

practical consequence
  audit and select demonstrations by relation-diagnostic coverage

current evidence
  LP4FM supplies ontology and boundaries, not validation of this law

status
  argument_shaped; requires a frozen pilot and independent confirmation
```

## 14. Reference Entry Points

### Direct neighbors

- [Task Ambiguity in Humans and Language Models](https://arxiv.org/abs/2212.10711)
- [Can Input Attributions Explain Inductive Reasoning in In-Context Learning?](https://aclanthology.org/2025.findings-acl.1092/)
- [Understanding the Dynamics of Demonstration Conflict in In-Context Learning](https://arxiv.org/abs/2603.04464)
- [Debiasing In-Context Learning by Instructing LLMs How to Follow Demonstrations](https://aclanthology.org/2024.findings-acl.430/)
- [Context-faithful Prompting for Large Language Models](https://aclanthology.org/2023.findings-emnlp.968/)
- [Rethinking the Role of Demonstrations](https://aclanthology.org/2022.emnlp-main.759/)
- [How Do In-Context Examples Affect Compositional Generalization?](https://aclanthology.org/2023.acl-long.618/)
- [What Do Language Models Learn in Context? The Structured Task Hypothesis](https://aclanthology.org/2024.acl-long.669/)
- [Active Example Selection for In-Context Learning](https://aclanthology.org/2022.emnlp-main.622/)
- [The Teaching Dimension of Linear Learners](https://proceedings.mlr.press/v48/liua16.html)
- [An Explanation of In-context Learning as Implicit Bayesian Inference](https://arxiv.org/abs/2111.02080)
- [Large Language Models Are Latent Variable Models](https://arxiv.org/abs/2301.11916)
- [What In-Context Learning “Learns” In-Context](https://aclanthology.org/2023.findings-acl.527/)
- [Is In-Context Learning in Large Language Models Bayesian?](https://proceedings.mlr.press/v235/falck24a.html)

### 2026 papers absorbed by the broader program

- [The Imperfective Paradox in Large Language Models](https://aclanthology.org/2026.acl-long.689/)
- [The Imperfective Paradox Is Not Necessarily in Large Language Models](https://arxiv.org/abs/2608.25005)
- [Characterizing the Expressivity of Local Attention in Transformers](https://aclanthology.org/2026.acl-long.1739/)
- [Memory efficiency and resource-rational encoding in sentence processing](https://aclanthology.org/2026.acl-long.1550/)
- [CoSToM](https://aclanthology.org/2026.acl-long.421/)
- [ImplicitMemBench](https://aclanthology.org/2026.acl-long.1301/)
- [Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
