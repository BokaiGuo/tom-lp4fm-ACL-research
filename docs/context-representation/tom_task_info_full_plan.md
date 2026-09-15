# Does Task Information Protect Belief Representations?
### Task-Description Informativeness × Token Inflation in Theory-of-Mind ICL — Full Research Plan

> Scope note. This expands the narrowed one-page proposal. It keeps a single main line
> (interaction hypothesis + mechanistic validation) and defers the "Effective Information"
> concept, the dual-bottleneck framework, and the long experiment menu to future work.
> Paper 1 details are cited only where publicly verifiable (abstract / intro / index snippets
> of *Information Fusion* 2025, DOI 10.1016/j.inffus.2025.103116). Paper 2 (Believing is Seeing,
> ACL 2026 SRW) is currently verified only as an accepted-paper listing; full text/PDF was not
> found in the checked public sources on 2026-07-07. Its poster readouts are motivation only;
> no numeric or mechanistic detail from the poster enters a load-bearing claim. See
> `EVIDENCE_LEDGER.md`.

---

## 1. Core research questions

**RQ1 (behavioral, interaction).** Under token inflation on false-belief tasks, does the
*informativeness* of the task description modulate how much long context degrades ToM? Concretely:
does a high-information, belief-aware description flatten the length-induced drop, and is a
low-information description worse than no description (mirroring Paper 1's transition effect)?

**RQ2 (mechanistic).** If a high-information description helps, does the behavioral gain co-occur
with (a) higher early-layer linear decodability of the queried agent's belief location and
(b) recovered selective attention of ToM-critical heads to the belief token — rather than being
pure instruction-following at the output layer?

## 2. Hypotheses

- **H1 (interaction).** `info_level × context_length` interaction is significant. Ordering of
  reality-bias rate: `low-info + long > none + long > mid-info + long > high-info + long`.
  High-info description shows the smallest slope over length.
- **H2 (mechanism, not instruction-following).** The behavioral benefit of the high-info
  description is accompanied by a rise in early-layer belief-probe accuracy and belief-token
  attention. In the **misdirection control** (Section 6.3), behavior follows the (wrong)
  instruction while the belief probe for the *queried* agent stays intact — dissociating
  "protecting the belief representation" from "obeying the instruction."
- **H3 (patching).** Denoising patch of short-context belief-token activations into the
  long-context run recovers the belief margin; recovery magnitude is *largest* under
  low-info + long (most-degraded representation) and *smallest* under high-info + long.

Define the continuous behavioral readout as the belief-minus-reality answer margin
\[
\Delta \;=\; \log p(\text{belief-answer token}) \;-\; \log p(\text{reality-answer token})
\]
evaluated at the answer position. \(\Delta>0\) means belief-consistent. Using \(\Delta\)
(not just accuracy) gives graded signal and avoids ceiling/floor artifacts.

---

## 3. Stimulus generator (false-belief items)

A templated first-order Sally–Anne generator. Every item fixes: two agents \(A_1,A_2\), one object,
a *source* container and a *target* container, and the canonical event chain
(place → \(A_1\) leaves → \(A_2\) moves object source→target → \(A_1\) returns → query).

Design constraints:
- **Belief answer ≠ reality answer, always.** Belief = source container; reality = target.
- **Single-token answers.** Pick container nouns that tokenize to one token in the chosen
  tokenizer (e.g. for Llama-3: `box`, `basket`, `drawer`, `cup`, `jar`, `bag`). If unavoidable,
  compare on the *first* answer token. This is required for clean logit/patching alignment.
- **Lexical randomization.** Sample agent names, object, and containers per item from disjoint
  pools for train/probe-train vs test (Section 5) to block memorization.
- **Balanced roles.** Counterbalance which physical container is source vs target so "belief
  answer" is not confounded with a fixed word.

Canonical target block (kept fixed; only irrelevant context around it varies):

```
{A1} put the {object} in the {source}.
{A1} left the room.
While {A1} was away, {A2} moved the {object} from the {source} to the {target}.
Later, {A1} came back.
Question: Where will {A1} look for the {object}?
Answer:
```

Control item types (for the example-usage and dissociation analyses, Sections 5–6):
- **True-belief control:** \(A_1\) witnesses the move ⇒ correct answer = target. Tests whether the
  model tracks *knowledge access*, not a "always answer source" heuristic.
- **Second-order (optional, defer if time-limited):** query \(A_2\)'s belief about \(A_1\)'s belief.
- **Knowledge-access probe items:** vary whether \(A_1\) is *told* the new location vs *sees* it.

---

## 4. Independent variables and prompt templates

### 4.1 Factor A — Task-description informativeness (length-matched)

Four levels. **Critical control:** pad L0–L2 with topic-neutral, rule-*non*-informative filler so
all four levels have (near-)identical token counts; otherwise "more informative" is confounded
with "longer description."

**L0 — none**
```
Answer the question. [neutral length-matching filler]
```

**L1 — low information**
```
Read the story and answer where the character will look. [filler]
```

**L2 — medium information (hints false belief, no rule)**
```
Answer based on what the character knows, not necessarily where the object really is. [filler]
```

**L3 — high information (explicit belief-aware rule)**
```
Track each character's belief. A character only knows events they observed or were told.
If an object is moved while a character is absent, that character still believes it is in
the original location. When asked where a character will look, answer using that character's
belief, not the object's real location.
```

All descriptions state a **rule**, never an answer. This is what lets probe/patching evidence,
not leaked answers, carry the mechanistic claim.

### 4.2 Factor B — Context length / token inflation

Total prompt length \(\in \{256, 1024, 2048\}\) tokens (extend to 4096 if compute allows).
Irrelevant context \(C\) is inserted **before** the fixed target block, and the block is kept
**immediately adjacent to the query**. Rationale: this controls the story-to-query distance while
varying prefix length. It reduces lost-in-the-middle confounds without claiming that absolute
position effects have been fully eliminated.

Padding types (poster-motivated robustness control, not a verified prior result here): `dots`,
`coherent Wikipedia`, `shuffled Wikipedia`. Report all three; any claim that the length effect is
not a filler artifact must come from local results.

### 4.3 Factor C — Number of examples (secondary; tests Paper 1's mitigation)

\(k\)-shot \(\in \{0, 2, 8, 16\}\), examples being *other* false-belief items with answers.
Primary use: test whether many examples rescue low-info descriptions under long context
(Paper 1's mitigation (i)), and whether that rescue weakens as context inflates.

Full crossing of A×B×C is large; run A×B fully, and C only at the two extreme lengths to control
cost.

---

## 5. Dependent variables and metrics

1. **ToM accuracy** — \(\mathbb{1}[\Delta>0]\), i.e. belief token out-logits reality token.
2. **Reality-bias rate** — fraction of items with reality token > belief token. (ToM errors are
   systematic toward reality, so this is more diagnostic than raw accuracy.)
3. **Belief margin** \(\Delta\) — continuous, primary regression outcome.
4. **Example-usage score** — accuracy on *edge-case* items whose rule is only inferable from the
   in-context examples (e.g. "told vs saw" distinction). Low score ⇒ model rode the description,
   ignored examples.
5. **Belief-probe accuracy** (Section 7) and **belief-token attention / entropy** (Section 8).

---

## 6. Confound controls (design-level, not afterthoughts)

### 6.1 Instruction-following vs representation-protection — the headline confound
A high-info description that improves accuracy might just be obeyed, with no change to the internal
belief representation. Two design moves dissociate them:

- **Misdirection condition (L3-mis).** Same length and specificity as L3, but the rule is *wrong*:
  it instructs the model to answer with the object's *real* current location (L3-mis-reality) or
  to answer about the *other* agent (L3-mis-other-agent). Prediction if the effect is mere
  instruction-following: behavior tracks the wrong instruction. The dissociation claim is narrower:
  if behavior shifts while the queried-agent belief probe stays intact, then output policy and
  internal belief representation are separable. The stronger "representation protection" claim
  still requires the normal L3 condition to improve both behavior and belief-probe/attention
  read-outs.
- **Probe/patching requirement.** Any mechanistic claim must be carried by Section 7–8 evidence,
  not by accuracy alone.

### 6.2 Length vs position
Story block pinned adjacent to the query (Section 4.2); only \(C\) length varies.

### 6.3 Description length confound
Length-matched descriptions (Section 4.1).

### 6.4 Cross-run token alignment for patching
Clean (short) and corrupted (long) runs use the **identical** target-block text; only \(C\)
differs. Activations are indexed by **story-relative offset from the query**, not absolute
position — so belief/reality/agent tokens map one-to-one across runs.

### 6.5 White-box constraint
Probe + patching need open weights ⇒ all mechanistic work on a **fixed open-weight model family**,
chosen up front. GPT-4-style closed models can only appear in a behavioral-only replication
(no patching), and are optional.

### 6.6 Prior-work delineation
Frame the contribution strictly as *how description informativeness modulates the token-inflation
mechanism*, not as "long context hurts reasoning" (crowded: lost-in-the-middle, distractibility).

---

## 7. Probe training details

**Goal.** Show that early layers linearly encode the *queried agent's belief location*, and that
this decodability (a) drops with inflation and (b) is preserved by high-info descriptions.

**Activations.** Residual-stream vectors \(h^{(\ell)}_{pos}\) for every layer \(\ell\), at a small
set of story-relative positions: the queried agent-name token in the question, the final
pre-answer token, and the source/target container mention tokens. Extract with frozen weights.

**Targets (four probes).**
- P1 — queried-agent belief location as a **role-coded binary variable**: source container vs
  target container. *This is the central probe.* Avoid a lexical multiclass head over concrete
  container words, because the leakage-control split deliberately uses disjoint train/test
  container vocabularies.
- P2 — reality location (control: should stay decodable even when P1 degrades).
- P3 — "answer-by-belief vs answer-by-reality" (binary task-identity probe).
- P4 — agent knowledge access: did the queried agent witness/get told the move (binary).

**Probe model.** Logistic regression (linear) on standardized activations ⇒ supports the
"*linearly* decodable representation" claim. Robustness: a 1-hidden-layer MLP; report both.

**Splits (leakage control).** Train and test items use **disjoint** agent/object/container
vocabularies and disjoint templates, so the probe cannot succeed by memorizing lexical identity.
Report accuracy on the held-out vocabulary only.

**Selectivity control (Hewitt–Liang).** Train each probe on **shuffled labels**; a valid probe
must show high selectivity (real-label acc − control-label acc). Report selectivity per layer.

**Two decodability curves.**
- *Within-length:* train and test at the same context length; plot P1 accuracy vs layer for each
  length. Prediction: early-layer P1 accuracy falls as length grows.
- *Transfer:* train probe on short context, test on long. A drop indicates the belief subspace is
  *disrupted*, not merely relocated (complements Paper 2's back-patching logic).

**Cross with Factor A.** Fit P1 accuracy as a function of `info_level × length`. H2 predicts high-info
descriptions keep early-layer P1 accuracy up under long context.

---

## 8. Activation-patching protocol

**Setup.** Two denoising directions.

1. **Same-length cross-info patch (primary mechanism test):** clean = L3-long; corrupted =
   L1-long. Because L0–L3 are length-matched and use the same padding/story position, this tests
   whether task-description informativeness changes belief representations without importing a
   short-vs-long absolute-position difference.
2. **Short→long patch (replication/bridge test):** clean = short-context run (belief intact);
   corrupted = long-context run (degraded). Same target block in both (Section 6.4), aligned by
   story-relative offsets rather than absolute positions.

**Grid.** For each (layer \(\ell\), token-type \(\tau\)) patch clean→corrupted, \(\tau \in\)
{belief token, reality token, agent-name, question, random}. Recovery per cell:
\[
R(\ell,\tau) \;=\; \frac{\Delta_{\text{patched}} - \Delta_{\text{corrupt}}}{\Delta_{\text{clean}} - \Delta_{\text{corrupt}}}
\]
Poster-motivated prediction to test locally: belief-token patching in early layers gives the
largest \(R\); reality/random ≈ 0. This is not treated as established until local patching
evidence exists.

**Cross with Factor A (the novel bit).** Repeat the patch under L0–L3. H3: recovery is largest
under low-info + long and smallest under high-info + long (less to fix when the description already
protected the representation).

**Attention read-out.** Identify ToM-critical heads (top heads by belief-token attention in short
context, and/or by patching effect). For those heads track, vs length and info-level:
belief-token attention weight, attention entropy, and output-vector norm. The planned
"attention dilution" signature is belief attention down, entropy up, and output norm roughly flat,
but this is a local hypothesis rather than a verified prior-work fact.

---

## 9. Models and data volume

- **Open-weight, fixed early:** e.g. Llama-3.1-8B (base + Instruct) as primary; Qwen2.5-7B as a
  second family for generality; one smaller model (e.g. 1–3B) for exhaustive layer sweeps.
  Instruct vs base contrast is itself informative for the instruction-following confound.
- **Items:** ≥ 2–3k unique test items per condition cell; ≥ 20k probe-training items with the
  disjoint vocabulary split.
- **Multilingual (defer):** exported poster notes mention cross-lingual recovery, but this remains
  poster-only. Treat multilingual work as an extension only after the English mechanism is nailed,
  since tokenizer efficiency changes effective length.

---

## 10. Analysis plan

Primary model (mixed effects), per model family:
\[
\Delta_{i} \;=\; \beta_0 + \beta_1\,\text{info} + \beta_2\,\text{len} + \beta_3\,(\text{info}\times\text{len}) + u_{\text{template}} + \varepsilon_i
\]
H1 lives in \(\beta_3\). Report the interaction with bootstrap CIs; repeat across padding types as
robustness. For probes, report per-layer accuracy + selectivity curves with CIs. For patching,
report the \(R(\ell,\tau)\) heatmap and its shift across info-levels. Pre-register H1–H3, the
answer-token scoring rule, and the misdirection dissociation before running.

---

## 11. What success / failure looks like

- **Clean positive:** significant `info × length` interaction (H1); high-info keeps early-layer P1
  up and belief attention up (H2); patch-recovery ordering matches H3; misdirection dissociates
  behavior from probe. ⇒ "Task information protects belief representations under inflation."
- **Instructive null:** high-info helps behavior but P1/attention don't move and misdirection shows
  behavior fully tracks instruction ⇒ the effect is instruction-following, *not* representation
  protection. This is still publishable as a caution against reading accuracy gains as mechanism.

---

## 12. Explicitly deferred (future work, not in the main claim)

Effective-Information = Semantic × Accessibility; Task-Belief Information Bottleneck; the full
A×B×C×padding×language grid; second-order ToM; closed-model patching. None of these are load-bearing
for RQ1–RQ2.
