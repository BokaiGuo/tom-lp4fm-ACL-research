# New Research Proposal: Who Gets to Be Right?

## Learning Source-Selection Policies from Context

Updated: 2026-09-12

Status: `not_evaluated`

This is a new research design, not a reinterpretation of LP4FM results. LP4FM
only contributes the source ontology, typed conflict-topology grammar, and
lessons about auditing answer-source claims. All core data, protocols, and
claims below require new generation and new runs.

## 1. Research question

> **Can a language model learn from demonstrations which information source is
> entitled to determine an answer when sources conflict, and execute that
> source-selection policy on new facts, new realizations, and unseen conflict
> structures?**

The object is not fact memorization or accuracy on one false-belief template. It
is a small functional learning problem:

```text
source values + conflict topology + contextual demonstrations
                 -> inferred source-selection policy
                 -> answer or action on a new world
```

Proposed title:

> **Who Gets to Be Right? Learning Source-Selection Policies from Context**

`Source-selection policy` is an operational definition of the task structure,
not a claim that the model contains a module with that name.

## 2. Why this is a distinct object

Prior work separately studies ICL demonstrations, schema and language
representation, belief representations, ToM shortcuts, partner adaptation, and
fine-grained semantic relations whose representations may not control final
judgments. This proposal defines a new measurable unit:

> **A demonstration may specify not only what answer is correct, but which source
> has answer authority under conflict. The study tests whether that policy is
> induced and executed in counterfactual worlds.**

Unlike ordinary task identification, the task label may remain fixed while the
source that governs the answer changes. Unlike a larger ToM benchmark, the
protocol explicitly generates competing sources, policies, and held-out policy
compositions.

## 3. Formal task

Each latent world `w` contains sources such as:

```text
S(w) = {belief, reality, testimony, memory, instruction, ...}
```

Each source gives a possibly different value `z_s(w)` for a query. A conflict
topology `T` specifies which sources conflict and how binary or ternary
competition is formed.

```text
pi(T, q) -> selected source
y_pi(w, T, q) = z_{pi(T,q)}(w)
D_pi = {(w_i, T_i, q_i, y_pi(w_i,T_i,q_i))}_{i=1}^n
```

The model must infer `pi` from `D_pi` and output `y_pi(w*, T*, q*)` on an
unseen world. Separate four outcomes:

```text
fact recall        = repeating facts seen in demonstrations
policy reading     = naming the source currently preferred
policy execution   = selecting that source in a new world
policy composition = combining learned source relations on unseen topology
```

Only policy execution is the core behavioral result; composition is the stronger
extension. Reading cannot substitute for execution.

## 4. Data-generation design

### Same world, different policies

For the same latent world, generate paired targets under different policies:

```text
belief-first / reality-first / testimony-first / recency-first / instruction-first
```

Policy changes must leave world facts, people, objects, surface templates, and
answer spaces fixed. This creates a genuine paired policy swap.

### Disjoint pools

Use separate policy-identification, content-holdout, encoding-holdout,
topology-holdout, and policy-swap-confirmation pools. Freeze item selection,
prior calibration, and template choices before confirmation is unblinded.

### Shortcut prevention

Randomize source names, people, positions, labels, order, lengths, lexical
similarity, and demonstrations per policy. Every source must be selected and
rejected under multiple policies. Partner identity and answer tokens must not
uniquely reveal policy.

## 5. Core studies

### Study 1: What do demonstrations teach?

Compare matched contexts:

```text
many-consensus       = many examples that rarely separate policies
one-diagnostic       = one example that separates the target policy
balanced-diagnostic  = a minimal set separating multiple policy pairs
corrupted-policy     = demonstrations supporting incompatible policies
```

Define:

```text
C_sep(D) = fraction of candidate policy pairs separated by D
```

Test execution on completely new worlds and compare `C_sep`/version-space
reduction against count, similarity, lexical diversity, and structural coverage.
The study does not assume `C_sep` is universal; it tests whether it predicts
held-out execution better than existing example-level descriptions.

### Study 2: Counterfactual policy interchange

This is the decisive study. For the same world, query, surface form, and source
values, change only which policy the demonstrations specify:

```text
same world + same query + same surface form + policy A
same world + same query + same surface form + policy B
```

Collect source-choice output, native final answer, and first action where a
coordination task is used. Include fact-swap, label-swap, demonstration-order,
source-name permutation, explicit-rule upper bound, and replay versus new-world
transfer controls.

If source-choice changes but the native answer does not, that is policy-execution
failure, not evidence that the policy was mastered. Functional policy execution
requires native answer or action sensitivity to the swap.

### Study 3: Compositional source-policy transfer

Train on binary conflicts and test on an unseen ternary conflict:

```text
training: belief/reality, testimony/reality, belief/testimony
test:     belief/reality/testimony
```

Compare pairwise composition, majority/recency heuristics, explicit-rule,
direct-label memorization, random, and source-order controls. The question is
whether binary source relations compose into an unseen policy rather than become
a new pattern-matching task.

## 6. Functional consequence: partner policy inference

Partner interaction is a downstream consequence, not the starting assumption.
After observing a partner in conflict worlds, the model chooses clarification or
coordination actions in new worlds. A paired partner-policy swap changes only the
partner's governing policy while holding identity, tone, reward, and surface
frequency fixed.

This enters the main paper only after native policy-swap execution succeeds.

## 7. Hypotheses and rivals

H1: after matching example count and token budget, policy-separation coverage
predicts held-out execution better than count or surface similarity.

H2: when only governing policy changes, native answer/action changes with policy,
not with source name, label, or demonstration order.

H3: if a source policy is learned rather than memorized pairwise, it composes on
the unseen ternary topology.

The strongest rivals are surface-cue following, fact memorization, source-order
heuristics, answer-label association, and partner-identity memorization. Each
must have an independent control.

## 8. Main-paper artifact

Use three displays:

1. **Policy information curve:** whether `C_sep` predicts execution on new worlds.
2. **Counterfactual policy-swap matrix:** whether native answer/action changes when
   only the governing policy changes.
3. **Compositional transfer map:** binary training to ternary testing and the
   functional partner consequence.

The contribution is a change in the measurement unit of relational ICL: whether
examples specify a source-selection policy, and whether that policy executes in
counterfactual and unseen structures.

## 9. Success and boundary paths

The strongest result has prospective `C_sep` value, native policy-swap effects,
unseen ternary composition, held-out partner adaptation, and replication on one
independent checkpoint or model family.

Valuable boundary results include policy reading without native execution,
content transfer without encoding/topology transfer, aggregate policy-swap
effects without item generalization, or partner adaptation no better than
source-choice readout. Each outcome narrows the claim rather than being hidden.

## 10. Relation to LP4FM

LP4FM provides source ontology, conflict-topology grammar, typed answer auditing,
and negative examples against mechanism overclaim. It is not confirmation data
for this study. The new proposal requires new latent worlds, demonstrations,
policy-swap pairs, and held-out targets.

The narrow object is:

> **context-induced source-policy learning and counterfactual execution**

Until new scoring is completed, the status remains `not_evaluated`.
