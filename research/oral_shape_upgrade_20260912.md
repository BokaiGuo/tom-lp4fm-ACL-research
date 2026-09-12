# Oral-Shape Upgrade: From Relation Identification to Final Commitment

> **Status revision (2026-09-12):** `identification -> commitment -> partner adaptation`
> is now a conditional branch, not the current oral headline. The evidence-grounded main
> proposal is [Evidence-Grounded Oral Research](evidence_grounded_oral_research_20260912_zh.md).

Updated: 2026-09-12

## Verdict

The project can now enter an `oral-shaped design` state, but it is not empirically oral-ready:

```text
design status    = oral_shaped
empirical status = not_evaluated
paper status     = argument_shaped
```

The object is now one functional chain rather than a collection of mechanism findings:

```text
demonstrations -> policy identification -> final commitment -> partner adaptation
```

The three required evidence blocks are still missing: policy-separation prediction on held-out
tasks, independent relation readability versus native commitment under prior pressure, and a
paired partner-policy swap that changes held-out action.

## Central claim

> **Relational in-context learning has at least two separable bottlenecks: identification and
> commitment. Demonstrations identify a governing relation only when competing policies make
> different predictions; even after the relation is readable, a competing prior can override it
> at final commitment. Functional use additionally requires policy-sensitive action on unseen
> content.**

`Relation–commitment dissociation` is an operational description introduced by this paper, not
an established field term. The paper does not claim intrinsic ToM, a hidden-state circuit, or a
universal Bayesian learner.

## Relation to LP4FM

LP4FM is Paper A: typed source arbitration under fixed latent worlds, checkpoint-sensitive
composition, and a boundary between same-item reconstruction and portable typed control.
Paper B reuses the typed-source object but does not repeat saturation, affine transport, or
final-token mechanism claims.

| Paper A provides | Paper B uses it for | Paper B does not claim |
|---|---|---|
| typed source arbitration | defining competing relations and policies | language form is a universal controller |
| primitive/compound boundary | motivation for separating readability from use | saturation is model-universal |
| negative mechanism boundaries | claim discipline | a localized causal circuit |

## Three main experiments

### 1. Information, not example count

Use matched consensus, target-diagnostic, rival-diagnostic, shuffled, corrupted, and
answer-permuted demonstrations. For candidate policies `Pi`, define:

```text
C_sep(D) = fraction of policy pairs separated by at least one demonstration
VR(D)    = log |Pi| - log |V(D)|
```

The primary estimand is prospective held-out policy performance predicted by `C_sep/VR`, compared
with count, similarity, lexical diversity, and structural coverage. A greedy selector remains a
main-paper consequence only if it wins prospectively; otherwise it moves to the supplement.

### 2. Identification × prior pressure

Calibrate each model's default policy on a disjoint neutral-context split. Then use a matched
2×2 of non-diagnostic/diagnostic evidence and prior-aligned/opposed targets. Report independent
relation readability, native final commitment, an explicit-rule oracle, and wrong-channel and
permutation controls.

The critical signature is:

```text
relation readout = target relation
native answer    = competing prior
```

If the readout fails, the result is an identification or retention failure, not a commitment
dissociation. Only a stable prior-opposed dissociation supports the bounded claim.

### 3. Functional partner adaptation

After observing consensus and diagnostic partner interactions, the model acts on new people,
objects, expressions, and unseen conflict topologies. A paired policy swap changes only the
partner's governing source policy. The strongest result is:

```text
partner policy inferred -> native action changes -> held-out coordination improves
```

Without the swap, a change in partner behavior is not policy adaptation.

## Oral gates

| Gate | Reviewer question | Current state | Promotion condition |
|---|---|---|---|
| Importance | Why is accuracy insufficient? | design pass | identification/commitment as the unit of analysis |
| Discovery | Is the effect more than a prompt artifact? | pending | semantic audit and independent latent worlds |
| Prediction | Is there a pre-computable information quantity? | pending | `C_sep/VR` beats count/similarity prospectively |
| Identification | Is the readout independent of the answer? | pending | separate readout, oracle, wrong-channel controls |
| Consequence | Does the relation control useful behavior? | pending | policy swap changes held-out native action |
| Closure | Are alternatives closed? | design pass | frozen calibration, matching, and permutations |
| Compression | Can the paper be told in one visual arc? | design pass | information, commitment, and adaptation figures |

## Main display plan

1. **Figure 1 — Policy information curve:** held-out policy performance against count and
   `C_sep/VR`, with world-clustered uncertainty.
2. **Figure 2 — Identification does not guarantee commitment:** independent readability, native
   commitment, and prior override in the matched 2×2.
3. **Figure 3 — Partner-policy swap:** held-out action, message choice, and coordination under
   swapped partner policies across new content and topology.
4. **Table 1 — Relation pipeline:** validity → identification → readability → retention →
   commitment → execution, with estimand, controls, and claim boundary for every cell.

## Execution order

1. Freeze stimuli, candidate policies, answer encodings, prior calibration, readout protocol,
   seeds, and analysis scripts.
2. Run Experiment 1 first; drop the selector from the main story if it has no prospective value.
3. Run Experiment 2 only after semantic validity and identification pass.
4. Run Experiment 3 only with a paired policy swap and held-out content/topology.
5. Replicate the complete signature on one matched checkpoint or model family before adding
   event semantics, memory, local attention, steering, or controlled tuning.

## Stop and downgrade rules

- No prospective gain from `C_sep/VR`: remove selector claim.
- No independent readout in the prior-opposed cell: report access failure, not dissociation.
- Readout correct but native answer follows prior: promote the bounded dissociation claim.
- No partner-policy action change: retain identification/commitment only.
- Single-template or same-item effect: exploratory only.
- Mechanism failure: report a boundary; do not use it to rescue a behavioral claim.

## Final calibration

The oral-level update is not “another failure benchmark.” It is a change in the measurement unit:

```text
example count      -> policy-separation information
accuracy           -> relation commitment
probe readability  -> native use
same-item answer   -> held-out partner adaptation
```

If the three main figures pass their gates, the paper can argue that contextual learning does not
jump directly from examples to answers: relation identification, final commitment, and functional
adaptation are distinct stages. If a gate fails, the claim must narrow with it.
