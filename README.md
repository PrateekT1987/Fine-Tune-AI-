# LoRA Fine-Tuning Demo

A minimal, educational Python project demonstrating how to configure and run
LoRA (Low-Rank Adaptation) fine-tuning on a causal language model using
`transformers` + `peft`.

## What is LoRA?

LoRA freezes the base model weights and injects small trainable low-rank
matrices into selected layers (e.g., attention projections). This reduces
trainable parameters by ~99%, enabling fine-tuning on modest hardware.

## Files

| File | Purpose |
|---|---|
| `config.py` | All hyperparameters in one place: base model, LoRA settings (rank `r`, alpha, dropout, target modules), training arguments, and dataset config. |
| `train.py` | Main entry point: loads the model/tokenizer, attaches LoRA adapters via `peft`, prepares the dataset, and runs Hugging Face `Trainer`. |
| `requirements.txt` | Pinned dependency ranges for reproducibility. |
| `README.md` | This file. |

## Usage

```bash
pip install -r requirements.txt
python train.py
```

The trained adapter is saved to `./lora-output` (not the full model — only
the LoRA weights, which are just a few MB).

## Training Data Format

Point `DataConfig.train_file` / `eval_file` at local JSONL files (one JSON
object per line). Sample files live in `edtech-ai/data/training/`:

| Field | Required | Description |
|---|---|---|
| `instruction` | yes | The question/task |
| `output` | yes | The target answer (loss is computed only on these tokens) |
| `input` | no | Extra context (defaults to `""`) |
| `language` | no | `english`, `hinglish`, `marathi_devanagari`, or `roman_marathi`; rendered as a `### Language:` tag so answers follow the requested style |

Prompt tokens are masked with `-100` in the labels so training only teaches
the model *answers*, not questions or filler.

## Key Configuration (see `config.py`)

- `r = 16` — LoRA rank; higher = more capacity but more memory.
- `lora_alpha = 32` — scaling factor; commonly set to 2x rank.
- `target_modules` — attention projections (`q/k/v/o_proj`) plus MLP
  projections (`gate/up/down_proj`), where much task knowledge lives.
- `learning_rate = 2e-4` — LoRA tolerates much higher LR than full fine-tuning.

## Note

This project is for demonstration. No dependencies are installed or code
executed as part of this repo setup.
