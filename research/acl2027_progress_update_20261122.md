# ACL 2027 research progress update — 2026-11-22

This snapshot records the current research status. It is deliberately
separate from the submitted LP4FM manuscript and does not change manuscript
claims.

## Completed engineering work

- Restored CUDA execution on an NVIDIA GeForce RTX 5060 Ti with PyTorch
  `2.8.0+cu128`.
- Completed four-model GPU discovery runs (Qwen2.5-7B-Instruct, Llama-3.2-3B-
  Instruct, Mistral-7B-Instruct-v0.3, and Phi-3.5-mini-instruct) over the
  frozen 120-world discovery panel. These runs are explicitly exploratory and
  claim-ineligible; they do not update a confirmatory gate.
- Added a model × task-description × topology interaction audit. The audit
  shows that source-complete task descriptions reduce hidden failures for
  Qwen, Llama, and Phi, while Mistral does not follow the same P/F direction.
  The confirmatory estimand therefore retains the interaction instead of
  making a pooled prompt-repair claim.
- Rechecked the Phi-3.5 snapshot through the ModelScope mirror. The existing
  local snapshot is complete and reusable; no weights were modified.
- Verified a real Phi-3.5 bf16 forward pass on `cuda:0`. The remaining Phi
  issue is a wrapper/Transformers `DynamicCache.seen_tokens` incompatibility in
  `generate()`, not a GPU or mirror failure.

## Current interpretation

The discovery evidence supports a bounded measurement statement: task
description and interface are potential effect modifiers of candidate-set
vulnerability. It does **not** support a universal prompt repair, a
checkpoint-invariant source-routing law, or a confirmatory claim.

## Remaining gates

The following remain open and are intentionally not represented as completed:

1. external immutable timestamping and scoring authorization;
2. claim-eligible frozen-input scoring and locked-test activation;
3. independent human validation for natural benchmark candidate sets;
4. cross-dataset generation prediction and decision-utility tests.

The full-size discovery JSONL files and model weights remain in the local
workspace rather than this GitHub research-planning repository. Their local
paths and hashes are recorded in the ACL2027 workspace manifests.
