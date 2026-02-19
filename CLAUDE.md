# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

Interactive Python generator that produces self-contained bash scripts for setting up custom Jupyter kernels on [CoCalc](https://cocalc.com) using `uv`.

## Architecture

- **generate-setup.py** — Interactive Python script (the generator). Prompts for environment name, display name, Python version, and packages. Outputs a `<name>-setup.sh` bash script with the executable bit set.
- **Generated `*-setup.sh` files** — Self-contained bash scripts produced by the generator. Portable: can be copied to any CoCalc project. Idempotent: if the venv already exists, it updates in-place rather than recreating.

## Running

```bash
python3 generate-setup.py     # interactive — prompts for config, writes a *-setup.sh
bash <name>-setup.sh          # run the generated script on CoCalc
```

The generated scripts depend on CoCalc's environment (`/ext/venvs/cocalc/bin/activate`, `uv`). The generator itself runs anywhere with Python 3.10+.

## Key Conventions

- Generator is pure Python with no third-party dependencies (stdlib only).
- Generated scripts use `set -euo pipefail` and must fail fast on errors.
- Generated scripts deactivate CoCalc's conda environment before creating the uv venv to avoid path conflicts.
- Environment names are validated as lowercase-dash format (`[a-z][a-z0-9]*(-[a-z0-9]+)*`).
- `ipykernel` is always included in the generated package list even if the user omits it.
