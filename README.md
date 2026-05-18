# Mirage Bench Local

Offline multimodal talking-head quality benchmark for gaze, expression, posture, and lip-sync coherence.

`Mirage Bench Local` is framed as an engineering instrument rather than a pitch deck: generate cases, break them, score them, and inspect the evidence.

## Operating question

Offline multimodal talking-head quality benchmark for gaze, expression, posture, and lip-sync coherence.

## Core mechanics

- Creates a clean/degraded `video clip` corpus sized for local iteration: 160 cases.
- Uses `gaze_stability`, `prosody_expression_fit`, `posture_coupling`, and `lipsync_grounding` to explain why a run passed, failed, or needs inspection.
- Turns `gaze_drift`, `late_smile`, `stiff_posture`, and `mouth_audio_desync` into executable cases with visible evidence trails.
- Carries the same `mirage-bench` result through JSON, dashboard, benchmark, and demo-pack outputs.

## Try it

```bash
uv sync --extra dev
uv run mirage-bench init-demo --force
uv run mirage-bench run-suite
uv run mirage-bench verify
uv run mirage-bench dashboard
uv run mirage-bench benchmark --iterations 100
uv run mirage-bench export-demo-pack
```

## Generated evidence

- `data/scenarios.json`
- `outputs/summary.json`
- `outputs/reports.json`
- `outputs/evidence_pack.md`
- `outputs/dashboard.html`
- `outputs/benchmark.json`
- `outputs/demo-pack.zip`

## Regression gate

```bash
uv run ruff check .
uv run pytest -q
uv run mirage-bench run-suite
uv run mirage-bench verify
uv run mirage-bench benchmark --iterations 100
```

## Scope

`Mirage Bench Local` is built for local reproduction: deterministic inputs enter the run, deterministic evidence comes out, and private data stays outside the repo.
