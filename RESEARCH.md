# Research And Plan Review

Company: Captions / Mirage
Project: `mirage-bench`

## Refined Thesis

Full-body video foundation models need benchmarks beyond lip-sync to prove natural eye contact, gesture, and expression timing.

The implementation is intentionally local and synthetic, but the test harness is shaped around the real operating question: can the proposed artifact create evidence a founder, CTO, or product leader would immediately recognize as useful?

## Fresh Sources Checked

- https://help.mirage.app/api-reference/api
- https://captions.ai/mirage
- https://captions.ai/help/docs/api/overview

## Plan Excerpt Used

## The Gap

Mirage's stated thesis is **"the AI lip sync era is over"** — they generate *full talking-head video with eye contact, micro-expressions, and upper-body motion*, not just mouth-keypoint warping ([captions.ai blog on Mirage foundation model](https://www.captions.ai/blog-post/mirage-worlds-first-foundation-model-for-ugc-video)). They've now opened a Mirage API and a text-message product (Cappy) that puts the model in front of non-technical users. But **the public benchmark landscape for talking-head video quality is still measured on the *old* lip-sync axis** — LSE-D, LSE-C, Sync-Net confidence — metrics that LatentSync, MuseTalk, and Diff2Lip optimize. **There is no public benchmark for "did the eye contact land," "is the micro-expression congruent with the prosody," "does the upper-body posture match the speech-act."** Mirage's stated quality lead is on the very axes nobody measures, which means it cannot be cited, recruited against, sold against, or defended against a copycat — exactly the moat problem Gaurav talks about. The right artifact is a **multimodal talking-head benchmark + harness that measures the axes Mirage's thesis lives on.**

## The Project — `mirage-bench`

> A public benchmark for *full talking-head video quality* — eye-contact stability, micro-expression-prosody congruence, upper-body-posture appropriateness — that scores any talking-head generator (Mirage API, LatentSync, MuseTalk, HeyGen, Synthesia) on the axes the field is actually moving toward and that Mirage's thesis is staked on.

**What it is.** A Python package (`pip install mirage-bench`) plus a curated 600-clip eval set: 200 reference talking-head clips (real humans, paired with their speech) + 400 generated clips across the open-source landscape. Each gets scored on **(a) eye-gaze stability** via a per-frame gaze-vector estimator + temporal smoothness regulariser; **(b) prosody-expression congruence** via a small audio→AU (Action Unit) cross-modal classifier — does the smile peak align with the laugh in the audio? does brow-furrow track stress in the speech? **(c) posture-speech-act coupling** — does the speaker lean forward on emphasis? **(d) classic lip-sync** (LSE-D / LSE-C) for grounding against the existing field. A vendor adapter API (`class TalkingHeadModel: def generate(audio, identity_image) -> Video`) lets Mirage and competitors run their model and produce a leaderboard row.

**Why it solves the gap.** It maps 1:1 onto Mirage's stated thesis (and Gaurav's LinkedIn quote). The closed-model bet is *only* defensible if the axes it wins on are visible to the market. Today they're not — and the open-source landscape (LatentSync, MuseTalk) is converging fast on the *old* lip-sync axis where Mirage's edge is hardest to show. A public benchmark on the *new* axes makes the moat legible: Mirage wins, citably, on metrics that lipsync-only models structurally can't.

**The wow moment.** 60 seconds in, Gaurav sees a hosted leaderboard at `mirage-bench.org` showing six systems scored across the four axes. Mirage leads on eye-gaze stability and prosody-expression congruence; LatentSync wins lip-sync-D; HeyGen middles everything. Each row clicks through to a side-by-side video grid: same audio, same identity image, six generated clips. Eye-gaze heatmap overlaid in real-time. The slide: "This is the benchmark your next press release gets to cite, and your Series D narrative gets to be staked on."

## Prototype Plan

- **CLI surface.** `mirage-bench eval --vendor mirage-api --split common-voice-en-test --out ./report`; `mirage-bench compare ./report --plot radar`.
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
