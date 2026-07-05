#!/usr/bin/env bash
set -euo pipefail

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
cd "$SCRIPT_DIR"

ENV_NAME="${ENV_NAME:-assignment1_ai}"

if ! command -v conda >/dev/null 2>&1; then
  _user="${USER:-${USERNAME:-}}"
  for conda_sh in \
    "${CONDA_PREFIX:-}/etc/profile.d/conda.sh" \
    "$HOME/miniconda3/etc/profile.d/conda.sh" \
    "$HOME/anaconda3/etc/profile.d/conda.sh" \
    "/c/Users/${_user}/miniconda3/etc/profile.d/conda.sh"; do
    if [[ -f "$conda_sh" ]]; then
      # shellcheck disable=SC1090
      source "$conda_sh"
      break
    fi
  done
fi

if ! command -v conda >/dev/null 2>&1; then
  echo "Error: conda not found. Install Miniconda or Anaconda first." >&2
  exit 1
fi

if conda env list | awk '{print $1}' | grep -qx "$ENV_NAME"; then
  echo "Updating existing environment: $ENV_NAME"
  conda env update -f environment.yml -n "$ENV_NAME" --prune
else
  echo "Creating environment: $ENV_NAME"
  conda env create -f environment.yml
fi

echo
echo "Environment ready. Activate with:"
echo "  conda activate $ENV_NAME"
echo
echo "Run the scripts:"
echo "  bash hello.sh"
echo "  python hello.py"
echo "  Rscript hello.R"
