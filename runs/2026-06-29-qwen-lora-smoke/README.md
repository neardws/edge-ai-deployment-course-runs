# 2026-06-29 Qwen LoRA Smoke Run

## Goal

Act as a student following the Qwen LoRA fine-tuning lab on a server GPU.

This run checks:

- sample JSONL validation
- Qwen chat template rendering
- 5-step LoRA SFT smoke test
- adapter save path
- base vs adapter output comparison

No model weights, adapters, private hosts, private addresses, or raw logs are stored here.

## Status

Complete.

## Files

- [commands.md](commands.md): sanitized command shapes used in the run
- [results-summary.md](results-summary.md): environment, training, and comparison summary
- [student-notes.md](student-notes.md): confusing points and course feedback

## Key Conclusion

The training loop and adapter saving worked after updating the course script for the current TRL API. The 5-step adapter did not improve task quality, so this run should be treated as a pipeline smoke test, not evidence that fine-tuning is useful for the target deployment task.
