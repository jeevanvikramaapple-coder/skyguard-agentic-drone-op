# SkyGuard AI

Secure multi-agent incident intelligence for **simulated** drone operations.

> **Status: Day 1 of 7.** Only the project skeleton, data schemas and the synthetic
> telemetry simulator exist. Everything else in the plan is not built yet.
> No part of this project controls a real aircraft. All data is synthetic.

## What works today

- Pydantic schemas for telemetry frames, mission plans and ground-truth labels.
- A deterministic simulator that generates 10 labelled missions: normal, noisy-normal,
  five fault types, a compound fault, an ambiguous case and a prompt-injection case.
- A CLI that writes the dataset with SHA-256 hashes in a manifest.
- 50 pytest tests.

## Setup

Requires Python 3.11 or newer.

```bash
python -m venv .venv
# macOS/Linux:
source .venv/bin/activate
# Windows (PowerShell):
.venv\Scripts\Activate.ps1

pip install -e ".[dev]"
cp .env.example .env        # Windows: copy .env.example .env
```

## Run

```bash
pytest                                  # all tests
ruff check .                            # lint
python -m skyguard.simulator.cli        # writes data/generated/
python -m skyguard.simulator.cli --seed 7 --out data/generated_seed7
```

## Layout

```
src/skyguard/
  config.py              settings from environment variables
  schemas/telemetry.py   data contracts (TelemetryFrame, MissionPlan, labels)
  simulator/
    scenarios.py         the 10 scenarios and their fault windows
    generator.py         turns a scenario + seed into labelled telemetry
    cli.py               writes the dataset to disk
tests/                   pytest suite
docs/PROGRESS.md         what is done, what is next
```
