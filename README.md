# uv-custom-jupyter-env

Generate self-contained setup scripts for **custom Jupyter kernels** on [CoCalc](https://cocalc.com) using [uv](https://docs.astral.sh/uv/).

## Why?

CoCalc provides a rich default software environment, but sometimes you need:

- A **specific Python version** (e.g. 3.11, 3.12)
- Packages at **pinned versions** for reproducibility
- Packages that are **not pre-installed** on CoCalc
- An **isolated environment** that won't conflict with CoCalc's defaults

This tool gives instructors and users a proven way to generate a setup script
that creates a custom Jupyter kernel — and that script can be **copied to any
CoCalc project** and run there.

## Quick start

1. Open a **Linux terminal** in your CoCalc project.
2. Run the generator:
   ```bash
   python3 generate-setup.py
   ```
3. Answer the prompts:
   - **Environment name** — lowercase with dashes, e.g. `my-python-env`
   - **Display name** — what appears in the Jupyter kernel picker, e.g. `My Python Env`
   - **Python version** — e.g. `3.11`
   - **Packages** — enter one per line, press Enter on an empty line when done
4. The generator writes two files:
   - `my-python-env-setup.sh` — the setup script
   - `my-python-env-test.ipynb` — a starter notebook with the kernel pre-selected
5. Run the generated script:
   ```bash
   bash my-python-env-setup.sh
   ```
6. Open `my-python-env-test.ipynb` to verify everything works.

## Sharing the generated script

The generated `*-setup.sh` script is **completely self-contained**. You can:

- Copy both files to another CoCalc project and run the setup there
- Send them to students so they can set up the same environment
- Check them into a course repository

Anyone with a CoCalc project can run it — no need to clone this repository.

## Updating an existing environment

The generated script is safe to re-run. If the environment already exists, it
**updates it in-place** rather than recreating it from scratch:

- Existing venv is preserved (no re-download of Python)
- Packages are added or upgraded as needed
- The Jupyter kernel registration is refreshed

To update packages, edit the `PACKAGES` array in the generated script and
re-run it.

## Example session

```
$ python3 generate-setup.py

============================================================
  CoCalc Custom Jupyter Kernel — Setup Script Generator
============================================================

This will generate a self-contained bash script that sets up
a custom Jupyter kernel in any CoCalc project using uv.

Environment name (lowercase, dashes ok) [my-course-env]: my-python-env
Display name (shown in the Jupyter kernel picker) [My Python Env]: My Python Env
Python version [3.11]: 3.14

Enter the packages to install, one per line.
You can use pip version syntax (e.g. numpy, pandas==3.0, 'numpy>=2.3,<2.4').
For GitHub repos: git+https://github.com/user/repo.git
Press Enter on an empty line when done.

  package [1]: pandas==3.0
  package [2]: seaborn==0.13.2
  package [3]: numpy>=2.3,<2.4
  package [4]:

Write 'my-python-env-setup.sh' and 'my-python-env-test.ipynb' to the current directory? [yes]: yes

Wrote: /home/user/my-python-env-setup.sh
Wrote: /home/user/my-python-env-test.ipynb

Next steps:
  1. Run the setup:        bash my-python-env-setup.sh
  2. Open the notebook:    my-python-env-test.ipynb
  3. Or copy both files to another CoCalc project.
  4. To update packages later, edit the PACKAGES array in
     my-python-env-setup.sh and re-run it.
```

## How the generated script works

1. **Deactivates CoCalc's conda environment** to avoid path conflicts.
2. **Creates a virtual environment** with `uv venv` (skipped if it already exists).
3. **Installs packages** with `uv pip install`.
4. **Registers a Jupyter kernel** via `ipykernel install --user`, making it visible in CoCalc's notebook interface.

## Package version syntax

The package list supports standard pip version specifiers. You can also
install packages directly from GitHub repositories using the `git+https://`
syntax — useful for unreleased versions or forks:

```
numpy                  # latest version
pandas==3.0            # exact version
seaborn==0.13.2        # exact version
numpy>=2.3,<2.4        # version range (2.3.x)
matplotlib<3.10        # upper bound
git+https://github.com/user/repo.git            # latest commit from default branch
git+https://github.com/user/repo.git@v1.2.3     # specific tag
git+https://github.com/user/repo.git@main       # specific branch
```

## Requirements

- A [CoCalc](https://cocalc.com) project (where `uv` and Python 3 are pre-installed)

## License

Apache 2.0 — see [LICENSE](LICENSE).
