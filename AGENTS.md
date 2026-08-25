# Project Instructions

## Project
LoRA fine-tuning of causal LMs (Hugging Face transformers + peft). Two-file core:
`train.py` is the only entry point; `config.py` holds all hyperparameters, model name,
and data paths. Adapter output goes to `./lora-output`.

`package.json` exists only to pin the OpenCode CLI. This is not a JavaScript project;
there is no Node build/test tooling.

## Environment
- Always use the project venv: `.venv\Scripts\python.exe ...` (Python 3.14).
- Dependencies are already installed (torch 2.11, transformers 5.5, peft 0.20,
  datasets 4.3, accelerate 1.14). Ask before installing anything.
- The base model (`meta-llama/Llama-3.2-1B-Instruct`) is gated on Hugging Face Hub:
  running `train.py` requires an HF token plus accepted license for that repo.
- No test/lint/typecheck config exists. Verify changes by executing code directly
  (e.g. `.venv\Scripts\python.exe -c "from config import *"`), not by inventing commands.

## Known quirk: do not "fix" the dataset loader
`load_jsonl` in `train.py` deliberately returns a plain list instead of a
`datasets.Dataset`. Dataset fingerprinting crashes under this environment's
Python 3.14 + dill combination. Keep the plain list.

## Training data format (JSONL)
Per-line JSON object; required fields `instruction` and `output`; optional `input`
and `language` (`english` | `hinglish` | `marathi_devanagari` | `roman_marathi`).
Prompt tokens are masked with `-100`, so loss is computed on answers only.
Files: `edtech-ai/data/training/train.jsonl`, `validation.jsonl`.

## Git
- Repo currently has zero commits on `master` and **no `.gitignore`**. Before any
  commit, create one covering `.venv/`, `node_modules/`, `__pycache__/`,
  `edtech-ai/data/raw/` (~432 MB parquet), `__pycache__` artifacts. Never `git add -A`.
- Do not modify Git history or push without asking.

## Repo layout notes
- `edtech-ai/` is mostly empty placeholders (`model/`, `rag/`, `server/`, `tests/`);
  only `data/` has real content. Raw source data: IndicVoices Marathi parquet under
  `edtech-ai/data/raw/IndicVoices/`.
- `evaluation_report.txt` is output from a previous run on another machine
  (its `/workspace/...` paths don't exist here); treat as historical context only.

## Safety
- Do not delete files without asking.
- Do not install packages without asking.
- Do not expose API keys, passwords, tokens, or secrets.
- Never read `.env` files unless explicitly requested (none exists at present).
