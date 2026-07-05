# Assignment 1

A minimal **Python + R + Bash** project that prints `Hello World` from each language.  This includes a reproducible Conda environment with pinned package versions.

The goal is three hello-world scripts plus environment scaffolding (`environment.yml`, `requirements.txt`, `setup_env.sh`) that anyone can recreate on a fresh machine.

---

## What this project does

Each script is a single-file entry point with **no third-party library dependencies**:


| Script     | Language | Command           | Output        |
| ---------- | -------- | ----------------- | ------------- |
| `hello.sh` | Bash     | `bash hello.sh`   | `Hello World` |
| `hello.py` | Python   | `python hello.py` | `Hello World` |
| `hello.R`  | R        | `Rscript hello.R` | `Hello World` |


Python and R are installed together inside one Conda environment named `assignment1_ai`. Bash comes from your system shell (Git Bash, WSL, macOS Terminal, or Linux shell).

---



## Project structure

```
Assignment1_AI_Version/
├── hello.sh           # Bash Hello World
├── hello.py           # Python Hello World
├── hello.R            # R Hello World
├── environment.yml    # Conda environment definition (pinned versions)
├── requirements.txt   # Pip dependencies (empty for this demo)
├── setup_env.sh       # Bash script that creates or updates the Conda env
└── README.md          # This file
```



### File reference

`hello.sh` — Uses `echo` to print a line to stdout. Must be run with Bash (not PowerShell).

`hello.py` — Standard Python 3 print statement. Runs with the Python interpreter from the Conda env.

`hello.R` — Uses R's `cat()` to print a line with a trailing newline. Runs via `Rscript`.

`environment.yml` — Declares the Conda environment name, channels, and pinned packages. Conda reads this file to install Python, R, and pip at specific versions.

`requirements.txt` — Listed in `environment.yml` under the `pip:` section so future Python packages can be added without editing the YAML. Currently contains only a comment; no packages are required for the hello scripts.

`setup_env.sh` — Idempotent setup script. On first run it creates the env; on later runs it updates the existing env with `--prune`. It also attempts to locate and initialize Conda on Windows Git Bash when `conda` is not already on `PATH`.

---



## Prerequisites

Install these **before** running setup:


| Requirement                                                                                        | Why you need it                                         |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| [Miniconda](https://docs.anaconda.com/miniconda/) or [Anaconda](https://www.anaconda.com/download) | Installs Python, R, and pip in one isolated environment |
| **Bash**                                                                                           | Required for `setup_env.sh` and `hello.sh`              |
| **Git Bash** or **WSL** (Windows only)                                                             | PowerShell cannot run `.sh` files natively              |




### Verify Conda

Open a terminal where Conda is available (Anaconda Prompt, or a shell where you have run `conda init`):

```bash
conda --version
```

You should see something like `conda 26.x.x`.

On Windows, Conda may work in PowerShell but **not** in Git Bash until `setup_env.sh` sources it (the script handles this automatically).

---



## Quick start



### Option A — Recommended (Git Bash / macOS / Linux)

```bash
cd Assignment1_AI_Version
bash setup_env.sh
conda activate assignment1_ai
bash hello.sh
python hello.py
Rscript hello.R
```

Each command should print:

```
Hello World
```



### Option B — Windows PowerShell (Conda only, no Bash setup script)

If you do not have Git Bash, create the environment directly:

```powershell
cd Assignment1_AI_Version
conda env create -f environment.yml
conda activate assignment1_ai
python hello.py
Rscript hello.R
```

Run `hello.sh` separately from Git Bash or WSL:

```bash
bash hello.sh
```

---



## Detailed setup walkthrough



### Step 1 — Clone or copy the project

Place the `Assignment1_AI_Version` folder somewhere on your machine, then change into it:

```bash
cd /path/to/Assignment1_AI_Version
```

On Windows with Git Bash:

```bash
cd /c/Users/yourname/Assignment1_AI_Version
```



### Step 2 — Run the setup script

```bash
bash setup_env.sh
```

**What happens internally:**

1. The script changes to its own directory (so paths to `environment.yml` and `requirements.txt` resolve correctly).
2. If `conda` is not on `PATH`, it searches common install locations and sources `conda.sh`.
3. If env `assignment1_ai` does not exist → runs `conda env create -f environment.yml`.
4. If the env already exists → runs `conda env update -f environment.yml -n assignment1_ai --prune`.
5. Prints activation and run instructions.

First-time setup downloads packages from **conda-forge** and may take several minutes.

### Step 3 — Activate the environment

```bash
conda activate assignment1_ai
```

Your prompt should show `(assignment1_ai)`. Python and R commands now use the versions inside this env, not system-wide installs.

To leave the env:

```bash
conda deactivate
```



### Step 4 — Run the hello scripts

With the env activated:

```bash
bash hello.sh
python hello.py
Rscript hello.R
```

You only **need** the env activated for `hello.py` and `hello.R`. `hello.sh` uses Bash only and does not depend on Conda, but running all three with the env active is the usual workflow.

---



## Pinned versions

Versions are fixed in `environment.yml` for reproducibility:


| Package | Version | Role                                           |
| ------- | ------- | ---------------------------------------------- |
| Python  | 3.12.11 | Runs `hello.py`                                |
| R       | 4.4.3   | Runs `hello.R` via `Rscript`                   |
| pip     | 25.1    | Installs packages listed in `requirements.txt` |


Conda channel: **conda-forge** (no `defaults`-only packages required for this minimal project).

---



## Customizing the environment name

By default the env is named `assignment1_ai`. Override it for a one-off setup:

```bash
ENV_NAME=my_custom_env bash setup_env.sh
conda activate my_custom_env
```

If you change the name permanently, edit the `name:` field at the top of `environment.yml` to match.

---



## Updating or rebuilding the environment



### Update after editing `environment.yml` or `requirements.txt`

```bash
bash setup_env.sh
```

The script detects the existing env and runs `conda env update ... --prune`, removing packages no longer listed in the YAML.

### Start completely fresh

```bash
conda env remove -n assignment1_ai
bash setup_env.sh
```



### Manual Conda commands (without the script)

Create:

```bash
conda env create -f environment.yml
```

Update:

```bash
conda env update -f environment.yml -n assignment1_ai --prune
```

Remove:

```bash
conda env remove -n assignment1_ai
```

---



## Troubleshooting



### `Error: conda not found`

**Cause:** Conda is not installed, or your shell has not been initialized for Conda.

**Fix:**

1. Install [Miniconda](https://docs.anaconda.com/miniconda/) if missing.
2. Restart the terminal, or run `conda init bash` (Git Bash) / `conda init powershell` (PowerShell), then open a new session.
3. On Windows Git Bash, `setup_env.sh` tries to source `~/miniconda3/etc/profile.d/conda.sh` automatically. If Miniconda is installed elsewhere, activate Conda manually first, then run the script:
  ```bash
   source /c/Users/yourname/miniconda3/etc/profile.d/conda.sh
   bash setup_env.sh
  ```



### `bash: command not found` (Windows)

**Cause:** PowerShell does not provide Bash.

**Fix:** Install [Git for Windows](https://git-scm.com/download/win) and use **Git Bash**, or use WSL. Alternatively, use the PowerShell Conda commands in [Option B](#option-b--windows-powershell-conda-only-no-bash-setup-script) above.

### `Rscript: command not found`

**Cause:** The Conda env is not activated, so the shell is using a PATH without R.

**Fix:**

```bash
conda activate assignment1_ai
Rscript hello.R
```



### `python` runs the wrong version

**Cause:** System Python is on `PATH` ahead of the Conda env.

**Fix:** Activate the env and confirm:

```bash
conda activate assignment1_ai
which python    # Git Bash / macOS / Linux
where python    # Windows cmd/PowerShell
python --version   # should report 3.12.x
```



### Environment solve or download failures

**Cause:** Network issues, conda-forge outage, or conflicting local Conda config.

**Fix:**

```bash
conda clean --all
bash setup_env.sh
```

If problems persist, try creating from a clean terminal with no other envs activated.

### Permission or line-ending errors on `setup_env.sh`

**Cause:** File saved with Windows CRLF line endings on some Unix systems.

**Fix (Linux/macOS/WSL):**

```bash
sed -i 's/\r$//' setup_env.sh
chmod +x setup_env.sh
bash setup_env.sh
```

---



## Assignment context (Part B)

This folder satisfies **Part B** of Assignment 1:

- AI-generated scaffolding for a minimal multi-language Hello World project
- Pinned versions in `environment.yml`
- `setup_env.sh` tested on Windows (Git Bash + Miniconda); iterated to fix Conda PATH and strict-mode shell issues
- Intended to live on an `ai` branch or sibling folder inside the `bash_tutorial` repo when committed

For **Part C**, copy highlights from your AI chat session into `REFLECTION.md` (what broke, what you asked the AI to fix, what you learned).

---



## Expected successful session

A complete successful run looks like this:

```bash
$ bash setup_env.sh
Creating environment: assignment1_ai
...
Environment ready. Activate with:
  conda activate assignment1_ai

$ conda activate assignment1_ai
(assignment1_ai) $ bash hello.sh
Hello World
(assignment1_ai) $ python hello.py
Hello World
(assignment1_ai) $ Rscript hello.R
Hello World
```

---



## License

Course assignment demo — no license specified. Use and modify freely for learning purposes.