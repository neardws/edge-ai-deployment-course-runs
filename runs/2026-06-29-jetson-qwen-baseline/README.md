# 2026-06-29 Jetson Qwen Baseline

## Goal

Continue the Jetson lab from the previous build preflight until a real Qwen GGUF baseline runs on the edge device.

This record keeps the student-facing facts and omits private hosts, addresses, usernames, host keys, and full raw logs.

## Status

Complete.

## What Changed Since Preflight

- Reused the existing `build-jetson` directory.
- Continued the interrupted `llama.cpp` target build.
- Finished `llama-cli`, `llama-bench`, and `llama-completion`.
- Copied the Q4_K_M GGUF from the lab server to the Jetson instead of downloading from the Jetson.
- Ran a Qwen Q4 baseline with `tegrastats`.

## Build Summary

| Field | Value |
| --- | --- |
| `llama.cpp` commit | `8c146a8` |
| Build directory | `build-jetson` |
| CUDA architecture | `87` |
| CUDA compiler | CUDA 12.6 |
| Built targets | `llama-cli`, `llama-bench`, `llama-completion` |
| Runtime system info | `CUDA : ARCHS = 870`, `NEON`, `ARM_FMA`, `FP16_VA`, `DOTPROD`, `OPENMP` |

Student note: even targeted builds compile `server-context`, `mtmd`, and many model adapters. This is normal for the current upstream build graph, but it should be explained in the course.

## Model

| Field | Value |
| --- | --- |
| File | `qwen2.5-0.5b-instruct-q4_k_m.gguf` |
| Quantization | Q4_K_M |
| Size | 469 MiB |
| SHA256 | `74a4da8c9fdbcd15bd1f6d01d621410d31c6fc00986f5eb687824e7b93d7a9db` |

## Baseline Command Shape

```bash
./build-jetson/bin/llama-completion \
  -m ~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf \
  -p "用三句话解释 Jetson 上做端侧模型部署需要关注什么。" \
  -n 96 \
  -ngl 99 \
  --ctx-size 1024 \
  --temp 0.2 \
  --seed 42 \
  -cnv \
  -st \
  --no-display-prompt \
  --perf
```

## Baseline Result

| Metric | Value |
| --- | ---: |
| Prompt eval | 57.06 ms / 24 tokens |
| Prompt eval speed | 420.64 tokens/s |
| Eval | 753.86 ms / 75 runs |
| Eval speed | 99.49 tokens/s |
| Total time | 870.24 ms / 99 tokens |

`llama-bench` result:

| Test | Throughput |
| --- | ---: |
| `pp64` | 2231.94 +/- 714.71 tokens/s |
| `tg64` | 115.88 +/- 9.03 tokens/s |

## tegrastats Summary

During the short run:

| Field | Observed |
| --- | --- |
| RAM | about 3202-3384 / 15656 MB |
| GR3D | peaked near 97% |
| GPU temperature | about 57.3-60.8 C |
| CPU/TJ temperature | about 57.9-60.8 C |
| VDD_IN | about 5.3 W idle after run, peak sample about 15.0 W |

## Output Quality

The answer was fluent and relevant to Jetson, but still generic. It mentioned hardware resources and optimization, but did not explicitly discuss memory pressure, thermal/power limits, offline operation, or `tegrastats`.

Student judgment: the run is a valid Jetson baseline, but the report should mark output quality as "partial" rather than "good".

## Course Feedback

1. Add a concrete Jetson baseline result example so students know what fields are expected.
2. Mention that `llama-bench` on Jetson should use smaller `p/n` values when doing a quick smoke test.
3. Explain that target build may still compile `server-context`, `mtmd`, and many model adapters.
4. Recommend transferring prepared GGUF files from a lab server when Jetson internet access is slow or unreliable.
