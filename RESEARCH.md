# Research And Plan Review

Project: Gaze Sync Meter Local
Project: `gaze-sync-meter`

## Refined Thesis

Full-body video foundation models need benchmarks beyond lip-sync to prove natural eye contact, gesture, and expression timing.

The implementation is intentionally local and synthetic, but the test harness is shaped around the real operating question: can the proposed artifact create evidence a founder, CTO, or product leader would immediately recognize as useful?

## Plan Excerpt Used

## The Gap

Talking-head video quality is still commonly measured on older lip-sync axes such as LSE-D, LSE-C, and Sync-Net confidence. Those metrics do not answer newer product-quality questions: did the eye contact land, is the micro-expression congruent with prosody, and does upper-body posture match the speech act. The right artifact is a multimodal talking-head benchmark and harness that measures gaze, expression, posture, and lip-sync coherence together.

## The Project - `gaze-sync-meter`

> A public benchmark for full talking-head video quality: eye-contact stability, micro-expression/prosody congruence, upper-body-posture appropriateness, and classic lip-sync grounding.

**What it is.** A Python package plus a curated eval set of reference and generated talking-head clips. Each clip gets scored on eye-gaze stability, prosody-expression congruence, posture-speech-act coupling, and classic lip-sync grounding. A vendor adapter API lets any talking-head model produce a leaderboard row.

**Why it solves the gap.** The field needs a benchmark on newer full-motion axes instead of only lip-sync. A public benchmark makes quality differences legible on metrics that lip-sync-only models structurally cannot cover.

**The wow moment.** A leaderboard shows six systems scored across four axes. Each row clicks through to a side-by-side video grid with the same audio and identity image. Eye-gaze heatmaps overlay in real time so model strengths and weaknesses are visible instead of argued.

## Prototype Plan

- **CLI surface.** `gaze-sync-meter eval --vendor local-adapter --split common-voice-en-test --out ./report`; `gaze-sync-meter compare ./report --plot radar`.
- **Five representative demo inputs.** (1) Conversational anecdote — long sentence with emphasis on the punchline, tests posture-coupling. (2) Question — rising intonation, tests gaze-toward-camera at the end. (3) Bulleted list — three emphasis peaks, tests rhythmic head/hand cues. (4) Laughter mid-sentence — tests AU-prediction-from-audio (laughter spike) congruence with detected AU6+AU12 (Duchenne smile). (5) Apology — soft prosody, tests subdued micro-expressions vs over-acted competitor outputs.
- **Demo screen.** Hosted leaderboard. Click a row → side-by-side 6-pane video grid; click an axis → radar chart per phrase across the five demo inputs. Gaze heatmap toggle overlays per-frame fixation cones.
- **Proof metrics.** (1) Inter-rater agreement between automated AU-congruence score and human raters on a held-out 60-clip subset: Spearman ρ ≥ 0.65. (2) Bootstrap CI on per-axis leaderboard score < 0.04 (rank reliability). (3) Audio→AU classifier on real humans hits Pearson r ≥ 0.6 (calibration gate). (4) End-to-end harness: a single 30-second clip scored across all four axes in <20 s on a single A10 GPU.


## Build Acceptance Criteria

- Deterministic local fixtures.
- Domain-specific metrics and failure modes.
- Passing unit tests.
- Passing CLI verifier.
- Static dashboard generated locally.
- Benchmark output under the project `outputs/` folder.
- Public-safe README: no founder emails, no private outreach text, no credentials.
