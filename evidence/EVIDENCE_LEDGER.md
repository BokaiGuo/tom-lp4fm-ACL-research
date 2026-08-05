# Evidence Ledger: Believing is Seeing Claim Boundary

Audit date: 2026-07-07.

## Source Boundary

Publicly verified facts:

| Source | Verified fact | Allowed use in this project |
|---|---|---|
| [ACL 2026 SRW accepted papers](https://2026.aclweb.org/program/srw_papers/) | Lists **Believing is Seeing: How Token Inflation Mechanistically Erodes Theory of Mind in Large Language Models** by Zhizhi Wang and Ruochen Zhang. | Bibliographic motivation only. |
| [ACL 2026 SRW call](https://2026.aclweb.org/calls/student_research_workshop/) | SRW accepts archival and non-archival work; accepted work may be presented orally or as posters. | Explains why an accepted listing does not guarantee an available public full text. |
| [ACL Anthology 2026 ACL SRW volume](https://aclanthology.org/volumes/2026.acl-srw/) | Searched for the title/authors on 2026-07-07; no matching full-text entry was found. | Full text is treated as unavailable for current claim-making. |
| Local run report: `runs/pilot_report_20260707.md` | Documents local Qwen pilot artifacts and summary tables. | Current project evidence, with pilot-only scope. |

Full-text status: as of this audit, no public full text/PDF for **Believing is Seeing** was found in the checked official ACL/SRW/ACL Anthology pages, nor by exact-title web searches over arXiv/OpenReview/ACL Anthology. Therefore poster-derived details remain unverified.

## Poster-Only Readouts

The following appear in exported notes and should not carry project claims until the paper/poster source is available and checked:

| Poster-derived readout | Files where it appears | Current status |
|---|---|---|
| Token inflation corrupts early belief-state representations and short-context belief activations recover ToM. | `chatgpt-export_论文解析 Theory of Mind.md`, `chatgpt-export_论文查找与摘要.md`, `chatgpt-export_07_07 15_02 长上下文社会推理失效机制.md` | Hypothesis/motivation only. |
| Specific numeric recovery: 256 to 2048, about 66% to 82%, +16 pp. | same exported notes | Poster-only; not load-bearing. |
| Best rescue in layers 0-8. | same exported notes | Poster-only; not load-bearing. |
| Belief-token patching helps while reality/random patching does not. | same exported notes | Hypothesis to test locally. |
| Back-patching finds no late belief signal above the 2048-token baseline. | same exported notes | Hypothesis to test locally. |
| ToM-critical heads lose selective attention; belief attention down, entropy up, output norm flat. | same exported notes and prior plan language | Hypothesis to test locally; not established. |
| Cross-lingual recovery in English, Chinese, Japanese, Korean, including any language ranking. | same exported notes and prior plan language | Deferred extension; not a current claim. |
| Padding-format controls show context length, not filler artifact, drives the mechanism. | same exported notes and prior plan language | Local robustness control to run, not a verified result. |

## Verified Local Evidence

Local pilot facts from `runs/pilot_report_20260707.md`:

| Item | Verified local fact | Claim boundary |
|---|---|---|
| Scoring path | Real-logprob scoring ran on local Qwen models. | Validates the code path, not the full research hypothesis. |
| Data volume | Both local model runs scored 1080 rows; 900 exact-length rows after excluding `256 x 4-shot`. | Pilot size only. |
| Qwen3-0.6B | Shows token-inflation fragility in some 0-shot cells, but L3 does not protect behavior. | Does not support the L3-protection hypothesis. |
| Qwen2.5-7B-Instruct | Near ceiling on the current templates and does not show a long-context drop. | Current templates are too easy for this model. |
| 4-shot Qwen2.5-7B-Instruct | Exact-length 1024/2048 cells score at ceiling for L0-L3 and misdirection variants. | Not usable as main interaction evidence without harder items. |

## Load-Bearing Rule

Do not use poster-only numeric readouts such as `66% -> 82%`, `+16 pp`, `layers 0-8`, or any language-ranking claim to set effect sizes, sample sizes, success thresholds, or current conclusions. Context lengths such as `256/1024/2048` may be used as project design choices and local-run conditions, but not as proof that the poster's reported effect has been replicated.
