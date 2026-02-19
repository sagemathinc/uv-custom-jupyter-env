# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

Interactive Python generator that produces self-contained bash scripts (and starter notebooks) for setting up custom Jupyter kernels on [CoCalc](https://cocalc.com) using `uv`.

## Architecture

- **generate-setup.py** — Interactive Python script (the generator). Prompts for environment name, display name, Python version, and packages. Outputs two files:
  - `<name>-setup.sh` — Self-contained bash setup script (executable bit set). Portable: can be copied to any CoCalc project. Idempotent: if the venv already exists, it updates in-place rather than recreating.
  - `<name>-test.ipynb` — Starter Jupyter notebook with the kernel pre-selected, containing cells to verify `sys.version` and `pip list`.

## Running

```bash
python3 generate-setup.py     # interactive — prompts for config, writes *-setup.sh and *-test.ipynb
bash <name>-setup.sh          # run the generated script on CoCalc
open <name>-test.ipynb         # open the test notebook (CoCalc's open command)
```

The generated scripts depend on CoCalc's environment (`/ext/venvs/cocalc/bin/activate`, `uv`). The generator itself runs anywhere with Python 3.10+ (stdlib only, no third-party dependencies).

## Key Conventions

- Generated scripts use `set -euo pipefail` and must fail fast on errors.
- Generated scripts deactivate CoCalc's conda environment before creating the uv venv to avoid path conflicts.
- Environment names are validated as lowercase-dash format (`[a-z][a-z0-9]*(-[a-z0-9]+)*`).
- `ipykernel` is always included in the generated package list even if the user omits it.
- Generated scripts patch the kernel spec (`kernel.json`) with CoCalc-specific metadata (priority, description, URL).
