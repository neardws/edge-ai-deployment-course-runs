# Edge AI Deployment Course Runs

This repository stores public, student-facing run records for the Edge AI deployment course.

It is not a model-weight repository. Large files, private machine details, tokens, SSH details, and raw sensitive logs are intentionally excluded.

## Runs

| Run | Scope | Status |
| --- | --- | --- |
| [2026-06-29 server smoke run](runs/2026-06-29-server-smoke/README.md) | Start Here, Ubuntu/NVIDIA environment check, llama.cpp CUDA build, first Qwen GGUF baseline and benchmark | Complete |
| [2026-06-29 Jetson login check](runs/2026-06-29-jetson-login-check/README.md) | Jetson SSH preflight before environment inspection | Blocked at authentication |
| [2026-06-29 Qwen quantization comparison](runs/2026-06-29-qwen-quantization-comparison/README.md) | Q4/Q5/Q8 GGUF comparison with llama.cpp on server GPU | Complete |
| [2026-06-29 Jetson environment and build preflight](runs/2026-06-29-jetson-env-build-preflight/README.md) | Jetson environment snapshot, tegrastats idle sample, llama.cpp CUDA configure/build preflight | Partial |
| [2026-06-29 Jetson Qwen baseline](runs/2026-06-29-jetson-qwen-baseline/README.md) | Completed Jetson CUDA build and Qwen Q4 baseline with tegrastats | Complete |
| [2026-06-29 inference acceleration server run](runs/2026-06-29-inference-acceleration-server/README.md) | Server-side `-ngl`, `ctx-size`, CPU threads, and `llama-bench` comparison | Complete |
| [2026-06-29 Jetson local service](runs/2026-06-29-jetson-local-service/README.md) | Jetson gateway access, `llama-server` build, local OpenAI-compatible API smoke test, and tegrastats | Complete |

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
