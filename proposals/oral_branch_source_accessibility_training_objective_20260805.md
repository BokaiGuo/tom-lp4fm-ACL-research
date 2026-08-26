# Oral Branch 5: Source-Accessibility Training Objectives

## Working Title

**Training Language Models to Preserve Typed Source Routing Under Linguistic Pressure**

Alternative title:

**Robust Without Becoming Blind: Calibrating Source Access Under Linguistic Variation**

## One-Sentence Thesis

A useful robustness objective should preserve the model's sensitivity to genuine
linguistic distinctions while preventing irrelevant wording pressure from routing
answers to the wrong typed source.

## Why Standard Robustness Objectives Are Insufficient

Ordinary consistency training encourages the same answer across paraphrases. That
can improve benchmark accuracy by flattening all language effects, including valid
effects of attribution, reference, commitment, or evidential scope. The current
project provides a richer target:

- which source should control the answer;
- which competing source captures it when routing fails;
- how language construction changes the source margin;
- whether multiple constraints compose additively or saturate;
- whether off-policy or wrong-channel leakage increases.

The goal is therefore not invariance. It is calibrated source routing.

## Existing Evidence To Reuse

- Stage-2/6 provide typed answer channels and behavioral arbitration metrics.
- Stage-44–49 provide controlled language axes and topology-conditioned effects.
- Stage-50–52 provide primitive access costs and compound composition residuals.
- Stage-13–20 provide intervention controls showing why final-token success alone
  is not evidence of internal source correction.
- Base/Instruct/Qwen3 comparisons provide natural pre-training and post-training
  boundary conditions.

Primary artifacts:

- `prompt_packs/tom_task_info_frozen_prompt_pack_qwen2.json`
- `results/oral_readiness_pack_20260707/stage49_multitemplate_20260804/`
- `results/oral_readiness_pack_20260707/stage52_independent_saturation_20260804/`
- `results/oral_readiness_pack_20260707/qwen25_7b_base_independent_saturation_replication_20260805/`

## Relationship to Related Work

### Instruction priority and context faithfulness

[The Instruction Hierarchy](https://arxiv.org/abs/2404.13208) trains models to
respect an explicit privilege ordering and selectively ignore lower-priority
instructions. [Context-faithful Prompting](https://arxiv.org/abs/2303.11315)
uses opinion-based prompts and counterfactual demonstrations to make models rely
more strongly on supplied context when it conflicts with parametric knowledge.
[Can LLMs Separate Instructions From Data?](https://arxiv.org/abs/2403.06833)
formalizes a related safety property and shows that prompt engineering or
fine-tuning can trade separation against utility.

These works are direct baselines because they all alter how competing text
sources influence an answer. Their target, however, is a fixed hierarchy,
context preference, or instruction/data boundary. Our target varies by item:
the queried belief may legitimately defeat reality in one topology, while a
system rule or current state should dominate in another. The objective must
learn **conditional source authority**, not unconditional context faithfulness.

### Consistency and representation-level adaptation

Paraphrase consistency is a natural robustness baseline, but it can succeed by
flattening all construction effects. [IN2 training](https://arxiv.org/abs/2404.16811)
shows that targeted data can improve fine-grained context awareness and
multi-segment integration. [ReFT](https://arxiv.org/abs/2404.03592) learns
low-rank representation interventions, while instruction-specific [activation
steering](https://arxiv.org/abs/2410.12877) improves adherence and composes
multiple steering vectors.

These methods establish that both data and representation interventions can
change context use. They do not guarantee that the intended typed source, rather
than a label or generic instruction feature, gains control. They should be
compared under identical source-margin, answer-permutation, wrong-channel, and
off-policy-leak measurements.

### Schema shaping and compositional training

[Shaping Schema via Language Representation](https://arxiv.org/abs/2605.09271)
argues that language representation can reorganize knowledge activation without
changing scale. [Fu--Zheng's DCG analysis](https://proceedings.mlr.press/v267/fu25j.html)
separates data insufficiency from algorithm--rule compatibility. Together they
imply that a successful training objective may work by changing representation
compatibility rather than by learning a universal composition law.

The full objective must therefore report whether it improves primitive coverage,
changes topology sensitivity, or truly improves held-out relation composition.
An accuracy gain alone cannot distinguish these mechanisms.

### Exact novelty boundary and decisive trade-off

This branch cannot claim that instruction robustness, contextual faithfulness,
or representation finetuning is new. Its separable contribution would be an
objective that improves unseen typed routing while preserving sensitivity to
legitimate linguistic source changes. The decisive display is a Pareto frontier
between wrong-source suppression and valid-language sensitivity, with standard
SFT, paraphrase consistency, hierarchy training, context-faithful prompting,
and ReFT/steering as matched baselines. See the [central related-work map](../research/related_work_positioning_20260826_zh.md).

## Training Objective

For item `i`, construction `c`, topology `T`, intended source `s*`, and competitor
sources `s`, define the typed margin:

```text
M_i(c,T) = logit(s*) - max_{s != s*} logit(s)
```

The total objective can contain four terms:

```text
L = L_task
  + lambda_route L_route
  + lambda_topology L_topology
  + lambda_comp L_composition
  + lambda_leak L_leak
```

### Routing loss

Increase the intended-source margin while preserving the full typed competitor
distribution rather than only the final label.

### Topology consistency loss

For matched latent worlds, enforce the preregistered ordering of language effects
across single and joint conflict topologies.

### Composition calibration loss

Penalize pathological compound behavior relative to a target operator class. The
target should be learned or selected on discovery data and tested on unseen
construction pairs; it must not hard-code the current Qwen2.5-Instruct residual as
universally correct.

### Off-policy leak loss

Penalize gains in wrong-channel or distractor sources, especially when the target
margin improves through nonspecific logit inflation.

## Research Questions

1. Can typed routing improve on unseen linguistic constructions without flattening
   meaningful construction effects?
2. Which objective component is necessary: source margin, topology consistency,
   compound calibration, or leak control?
3. Does training improve independent latent worlds or only memorized templates?
4. Does the objective transfer across epistemic, temporal, and deontic source routing?
5. Does behavioral repair correspond to an earlier non-final controller or only a
   stronger final-readout policy?

## Experimental Conditions

### Baseline 1: Standard supervised fine-tuning

Train on correct final answers only.

### Baseline 2: Paraphrase consistency

Force matched prompts to produce similar final-answer distributions.

### Baseline 3: Typed routing loss

Use the intended-source margin and wrong-source labels.

### Baseline 4: Topology-balanced routing

Add single/joint conflict ordering constraints.

### Baseline 5: Full source-access objective

Add held-out composition calibration and off-policy leak penalties.

### Optional mechanism-aware condition

Apply representation-level regularization only if it beats final-token-only
controls and is justified by a frozen causal object. Do not assume that Stage-3
probe directions are valid training targets.

## Data Splits

Use orthogonal generalization splits:

- latent-item split;
- template/realization split;
- construction-family split;
- compound-pair split;
- topology split;
- optional domain split;
- checkpoint seed split.

The primary test should combine item-disjoint and construction-disjoint evaluation.
Random train/test prompt splits are insufficient.

## Success Metrics

- intended-source margin;
- typed source accuracy and controller confusion;
- off-policy leak;
- topology interaction sign agreement;
- primitive and compound calibration error;
- held-out operator residual;
- semantic sensitivity to valid construction contrasts;
- lexical/paraphrase invariance only where the latent semantics are unchanged;
- standard task utility and calibration.

## The Central Tradeoff

The strongest scientific object is the Pareto frontier between:

```text
source-routing robustness
versus
linguistic-semantic sensitivity
```

A model that answers identically under every construction may appear robust but
has lost valid sensitivity to source attribution. A successful objective should
reduce irrelevant capture while preserving effects when language changes the
intended source or commitment structure.

## Decisive Experiments

### Experiment 1: Unseen construction repair

Train without one construction family and evaluate typed routing on it. Improvement
must exceed standard SFT and paraphrase consistency.

### Experiment 2: Unseen compound prediction

Train on primitives and selected compounds, then test a withheld compound pair.
Measure whether the learned model preserves or improves the target composition
class without simply amplifying all primitive margins.

### Experiment 3: Semantic counterfactual

Create pairs where wording changes but intended source does not, and pairs where a
small linguistic change genuinely changes source attribution. The model should be
invariant only in the first case.

### Experiment 4: Mechanism boundary

Compare behavioral repair with final-token-only and wrong-channel intervention
signatures. If the trained model only strengthens the output prior, report a
behavioral training result, not internal source-binding repair.

## Controls

- matched token count and lexical overlap;
- answer-token frequency balancing;
- calibration and entropy matching;
- random typed labels;
- shuffled topology labels;
- no-leak and wrong-channel baselines;
- data contamination audit;
- equal training-token and optimizer budgets;
- multiple random seeds;
- held-out worlds and constructions;
- general task retention and catastrophic-forgetting checks.

## Promotion Gates

### Gate A: Typed improvement

The objective must improve intended-source routing and reduce specific competitor
capture, not only final accuracy.

### Gate B: Linguistic sensitivity preservation

Valid semantic contrasts must remain distinguishable. A universal flattening of
construction effects fails this gate.

### Gate C: Composition generalization

Improvement must transfer to an unseen compound or construction family under
frozen evaluation.

### Gate D: Baseline separation

The full objective must beat standard SFT, paraphrase consistency, and typed-margin
training under equal budgets.

### Gate E: Scope honesty

Unless non-final causal evidence beats final-token controls, claim only behavioral
source-routing repair.

## Claim Ladder

**Safe current claim:** the repository provides typed source-routing diagnostics
and identifies checkpoint-sensitive behavioral composition.

**After training success:** a typed source-access objective improves held-out
source routing while preserving valid linguistic distinctions.

**After cross-domain transfer:** the objective is a reusable training principle
for tested source-routing domains.

**After mechanism validation:** training changes a named non-final source-control
object that predicts behavioral repair.

**Not allowed:** the objective gives human-like ToM, eliminates prompt sensitivity,
or repairs an internal belief mechanism without causal localization.

## Oral Argument

The paper should center on a failure of naive robustness:

```text
Invariance can be wrong when language changes who is speaking, what is committed,
or which source should govern the answer.
```

The main figure should show the robustness-sensitivity Pareto frontier, typed
controller confusion before and after training, and performance on unseen compounds.

## Risks and Fallbacks

- **Standard SFT matches the full objective:** the diagnostic remains valuable,
  but the new objective is not necessary.
- **Robustness causes semantic flattening:** publish the tradeoff as a negative
  boundary and search for conditional invariance objectives.
- **Only current ToM tasks improve:** keep the scope narrow and do not claim a
  general training principle.
- **Behavior improves through output bias:** frame as answer-interface calibration,
  not source-binding repair.
- **Composition target is checkpoint-specific:** train against a bounded operator
  class or uncertainty interval rather than a fixed positive residual.

## Minimal Next Action

Run a small adapter or LoRA comparison on one Base checkpoint with four conditions:
standard SFT, paraphrase consistency, typed routing, and typed routing plus leak
control. Evaluate on item-disjoint and construction-disjoint Stage-49/52 cells
before adding composition regularization.
