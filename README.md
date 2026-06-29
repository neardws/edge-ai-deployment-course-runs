# Edge AI Deployment Course Runs

This repository stores public, student-facing run records for the Edge AI deployment course.

It is not a model-weight repository. Large files, private machine details, tokens, SSH details, and raw sensitive logs are intentionally excluded.

## Runs

| Run | Scope | Status |
| --- | --- | --- |
| [2026-06-29 server smoke run](runs/2026-06-29-server-smoke/README.md) | Start Here, Ubuntu/NVIDIA environment check, llama.cpp CUDA build, first Qwen GGUF baseline and benchmark | Complete |

## Public Archive Rules

Publish:

- commands that a student can rerun
- sanitized environment summaries
- model filename, size, SHA256, source, and license notes
- short log excerpts that explain what happened
- student-perspective confusion points and fixes

Do not publish:

- GGUF model weights
- SSH usernames, private IPs, tokens, keys, or private download links
- full third-party source trees or build directories
- other users' process details
- huge raw profiling logs unless trimmed and sanitized
