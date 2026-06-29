# 2026-06-29 Server Smoke Run

## Goal

Act as a first-time student and follow the course's shortest path:

1. Read `Start Here`.
2. Prepare `~/edge-ai-lab`.
3. Capture an Ubuntu/NVIDIA environment snapshot.
4. Build `llama.cpp` with CUDA.
5. Prepare one Qwen GGUF file.
6. Attempt a first baseline run.

This run uses a server GPU environment instead of a physical edge device. That is acceptable for the course's 40-hour path, but it does not cover Jetson power, temperature, or shared-memory behavior.

## Status

Complete for the first smoke test.

## Result Summary

| Item | Result |
| --- | --- |
| Environment setup | Passed |
| `llama.cpp` CUDA configure | Passed |
| Full default build | Stopped manually because it pulled in a long, noisy build path |
| Targeted build | Passed for `llama-cli`, `llama-bench`, `llama-server`, and then `llama-completion` |
| Qwen GGUF download | Passed |
| First `llama-cli` baseline command | Failed as a student-facing command because it entered interactive chat mode |
| Corrected non-interactive run | Passed with `llama-completion` |
| Small benchmark | Passed with `llama-bench` |

## Baseline Snapshot

| Metric | Value |
| --- | --- |
| Model file | `qwen2.5-0.5b-instruct-q4_k_m.gguf` |
| Quantization | Q4_K_M |
| `llama.cpp` commit | `8c146a8` |
| Prompt processing | 1885.86 tokens/s for 19 tokens |
| Generation | 433.42 tokens/s for 46 runs |
| `llama-bench` prompt throughput | 21932.31 +/- 4643.15 tokens/s |
| `llama-bench` generation throughput | 457.48 +/- 34.58 tokens/s |

The generated answer was not good enough pedagogically: it described generic business value instead of explaining edge model quantization. The course should treat this as part of the first lab, not just as a performance run.

## Early Student Findings

| Finding | Why it matters for students |
| --- | --- |
| The course says to prepare a Qwen GGUF file, but the baseline chapter does not give a concrete first download command. | A first-time student may stop before the first model run. |
| The environment chapter is clear enough to create `~/edge-ai-lab` and capture system state. | This part worked as written. |
| A default full `llama.cpp` build is very long and currently triggers UI/frontend build steps too. | Students may think something is wrong unless the course recommends targeted build commands first. |
| The first `llama-cli` non-interactive command was not compatible with the current tool behavior. | It entered interactive chat mode and produced a huge log until stopped. |
| The corrected `llama-completion` run exits cleanly, but still needs clearer chat-template guidance. | The model response was generic and off-topic, so students need quality checks in addition to speed metrics. |
| `-ngl`, `ctx-size`, `prompt eval`, and `eval time` still need to be tied to exact report fields during the first baseline. | Running is not enough; students need to know what to copy into the report. |

## Files

- [commands.md](commands.md): commands used in this run
- [environment-summary.md](environment-summary.md): sanitized server environment snapshot
- [model-summary.md](model-summary.md): public model metadata, no weights
- [baseline-summary.md](baseline-summary.md): baseline result, benchmark, and failure notes
- [student-notes.md](student-notes.md): confusion points and course improvement ideas
