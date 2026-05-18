# Mirage Bench Local

Offline multimodal talking-head quality benchmark for gaze, expression, posture, and lip-sync coherence.

This is a local-first, synthetic-data prototype inspired by a company-specific project plan for **Captions / Mirage**. It is built to demonstrate the engineering shape of `mirage-bench` without private data, credentials, external APIs, or hosted services.

## Why it matters

Full-body video foundation models need benchmarks beyond lip-sync to prove natural eye contact, gesture, and expression timing.

## What it does

- Generates deterministic synthetic `video clip` scenarios.
- Scores each scenario against domain-specific quality gates.
- Produces evidence-backed findings for realistic failure modes.
- Writes a static dashboard, JSON reports, benchmark output, and a portable demo pack.
- Exposes a JSONL tool loop for local agent integration.

## Metrics

- `gaze_stability`
- `prosody_expression_fit`
- `posture_coupling`
- `lipsync_grounding`

## Failure modes

- `gaze_drift`
- `late_smile`
- `stiff_posture`
- `mouth_audio_desync`

## Quickstart

```bash
uv sync --extra dev
uv run mirage-bench init-demo --force
uv run mirage-bench run-suite
uv run mirage-bench verify
uv run mirage-bench dashboard
uv run mirage-bench benchmark --iterations 100
uv run mirage-bench export-demo-pack
```

## Expected outputs

- `data/scenarios.json`
- `outputs/summary.json`
- `outputs/reports.json`
- `outputs/evidence_pack.md`
- `outputs/dashboard.html`
- `outputs/benchmark.json`
- `outputs/demo-pack.zip`

## Validation

```bash
uv run ruff check .
uv run pytest -q
uv run mirage-bench run-suite
uv run mirage-bench verify
uv run mirage-bench benchmark --iterations 100
```

## Demo hook

A side-by-side clip leaderboard scores the axes where Mirage’s full-motion thesis actually lives.
