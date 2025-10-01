# CodebaseRepo_V2 — Full Updated Release
Date: 2025-09-26 02:21:56

## Highlights
- Adds **total_time_unsafe_s** and **core_power_oscillation_index** to metrics engine.
- Keeps **time_outside_freq_limit_s** as the canonical freq safety metric (with alias support).
- Hardened 7D/CRS builders with alias resolution and neutral fallback (no KeyErrors).
- `validate_and_report.py` preflight: regenerates combined metrics if missing and checks schema vs registry.
- Public `apply_style(...)` exported from `analysis/visualization_engine.py` (figure registry compatibility).
- Path bootstrap in `validate_and_report.py` (no PYTHONPATH hacks).
- New schema files: `config/metrics_registry.yaml` & `.json`.

## Run
```bash
python scripts/validate_and_report.py \
  --controllers PID FLC RL \
  --config config/configdefault.yaml \
  --rl-model config/optimized_controllers/RL_Agent_Optimized.zip \
  --scenarios all \
  --out results \
  --deterministic
```
