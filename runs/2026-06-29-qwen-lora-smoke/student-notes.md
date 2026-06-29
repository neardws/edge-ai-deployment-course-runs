# Student Notes

## What Was Confusing

1. The docs suggest creating a venv and installing generic training packages, but the first available Python environment had PyTorch built for CUDA 13.0 while the driver exposed CUDA 12.8. `torch.cuda.is_available()` was false even though `nvidia-smi` showed a GPU.
2. Copying lab files from macOS with plain `tar` produced `._*` files on the server. It did not break training, but it is noisy and confusing.
3. The first training command was silent while Hugging Face model files were downloading. A student may think the script is stuck.
4. Current TRL no longer accepts `dataset_text_field` and `max_seq_length` directly in `SFTTrainer(...)`.
5. Training success and adapter saving did not imply output quality improved.

## Course Feedback

1. Add an environment check that prints `torch.__version__`, `torch.version.cuda`, driver CUDA from `nvidia-smi`, and `torch.cuda.is_available()`.
2. Tell students to prefer an existing CUDA-compatible environment over blindly creating a new venv if the server already has one.
3. Mention `COPYFILE_DISABLE=1` for macOS tar copies, or recommend `rsync --exclude '._*'`.
4. Update the course script for TRL `1.7.0`.
5. Make the lab conclusion explicit: 5-step LoRA is only a pipeline check. A poor base-vs-adapter comparison should stop the fine-tuning route until the dataset and evaluation prompts improve.
