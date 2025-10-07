# Codebase Overview

## Domain and Purpose
- The project delivers a simulation and optimization workbench for pressurized water reactor (PWR) control strategies, coupling plant physics with grid behaviour and multiple controller families.【F:README.md†L1-L59】
- Configuration is centralized under `config/parameters.py`, which provides plant constants, safety limits, controller defaults, and reinforcement-learning curriculum metadata used across the stack.【F:config/parameters.py†L3-L141】

## Core Packages
- **environment** – hosts `PWRGymEnvUnified`, a Gymnasium-compatible environment that wraps reactor, turbine, and grid models, applies normalization, tracks scenario state, and evaluates reward signals with curriculum overrides.【F:environment/pwr_gym_env.py†L1-L200】
- **controllers** – supplies PID, fuzzy logic, and RL controller implementations plus loader utilities (see `analysis/controller_factory.py`) to instantiate tuned artifacts for evaluation workflows.【F:analysis/controller_factory.py†L1-L124】
- **optimization_suite** – contains PID/FLC optimizers and the RL training pipeline, including a curriculum-aware `RLTrainer` that saves checkpoints and triggers automated validation/reporting once training completes.【F:optimization_suite/rl_trainer.py†L1-L245】
- **analysis** – implements scenario definitions, deterministic execution, metric computation, and report generation to benchmark controllers over curated operating conditions.【F:analysis/scenario_definitions.py†L1-L200】【F:analysis/metrics_engine.py†L1-L200】【F:main_analysis.py†L1-L151】

## Scenario & Evaluation Pipeline
- Scenario definitions combine load profiles, adversarial disturbances, and metadata that drive both training curricula and offline validation runs.【F:analysis/scenario_definitions.py†L87-L200】
- `main_analysis.py` loads parameter sets, runs configured controllers across all scenarios, aggregates metrics, and optionally renders reports via the analysis toolkit.【F:main_analysis.py†L33-L151】
- `run_training.py` exposes a CLI harness that expands scenario suites, builds optimized controllers, and runs deterministic validation matrices (training is intentionally stubbed).【F:run_training.py†L1-L133】

## Metrics and Reporting
- `analysis/metrics_engine.py` computes KPIs from scenario timeseries, including RMSE, spectral analysis, bandwidth, coherence, and risk-focused statistics, ensuring robustness checks across safety envelopes.【F:analysis/metrics_engine.py†L21-L200】
- Reporting destinations (templates, plots, comparison criteria) are configurable via the reporting block in `config/parameters.py`, enabling consistent artifact generation for optimization and evaluation studies.【F:config/parameters.py†L34-L51】

## Automation & Validation
- The RL training pipeline leverages Stable Baselines 3 SAC agents with curriculum promotions tied to metrics-based thresholds; successful phases trigger reward adjustments and scenario switching to progressively harden controllers.【F:optimization_suite/rl_trainer.py†L22-L239】
- Post-training, the trainer invokes `auto_validate_and_report` so every champion model immediately flows through the deterministic validation and reporting stack.【F:optimization_suite/rl_trainer.py†L198-L239】
- CLI tooling provides batch validation with scenario suite expansion and fallbacks, helping ensure repeatable evaluations even when custom scenario files are missing.【F:run_training.py†L42-L133】

## Key Takeaways
- The repository emphasizes a unified configuration source, automated curriculum-based RL training, deterministic evaluation, and comprehensive reporting to benchmark PID, FLC, and RL controllers under diverse grid/plant scenarios.【F:README.md†L9-L72】【F:config/parameters.py†L61-L141】【F:main_analysis.py†L33-L151】

## Requesting Project-Level Changes
- Strategic or cross-cutting changes should be discussed via an issue that references the most recent release context captured in `RELEASE_NOTES.md` and `CHANGELOG.md`, ensuring proposals acknowledge shipped capabilities and outstanding debt.【F:RELEASE_NOTES.md†L1-L160】【F:CHANGELOG.md†L1-L160】
- When a proposal affects external deliverables (validation artefacts, packaged models, or schema contracts), include updates for `RELEASE_MANIFEST.json` and `docs/artifact_contract.md` so traceability remains intact across dependent teams.【F:RELEASE_MANIFEST.json†L1-L200】【F:docs/artifact_contract.md†L1-L3】
- For governance approvals, document impacted configurations (`config/parameters.py`), automation hooks (`run_training.py`, `main_analysis.py`), and UI workflows (`ui/app.py`) within the issue so maintainers can evaluate regression risk and review scope before green-lighting the change.【F:config/parameters.py†L3-L141】【F:run_training.py†L1-L133】【F:main_analysis.py†L1-L151】【F:ui/app.py†L1-L200】
