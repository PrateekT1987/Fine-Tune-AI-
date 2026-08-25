Fine-tune sarvam-ai/sarvam-2b-v0.5 using 4-bit QLoRA.

Dataset:

/workspace/data/training/train.jsonl

Start with a very small test run.

Use:

LoRA rank: 16
LoRA alpha: 16
LoRA dropout: 0
gradient checkpointing: enabled
4-bit quantization

Use supervised fine-tuning.

Train for only 100 steps.

Save the adapter to:

/workspace/model/edtech-sarvam-2b-lora

Do not merge the model.

Do not convert to GGUF yet.

After training, run 10 evaluation questions automatically.

Compare the results against the base model.

Stop if there is an obvious training error.