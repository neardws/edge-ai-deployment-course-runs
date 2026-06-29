# Results Summary

## Model

| Field | Value |
| --- | --- |
| File | `qwen2.5-0.5b-instruct-q4_k_m.gguf` |
| Quantization | Q4_K_M |
| Size | 469 MiB |
| SHA256 | `74a4da8c9fdbcd15bd1f6d01d621410d31c6fc00986f5eb687824e7b93d7a9db` |
| `llama.cpp` commit | `8c146a8` |

## GPU Offload

| `-ngl` | Prompt eval | Prompt eval speed | Eval | Eval speed | Total time |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 0 | 52.50 ms / 17 tokens | 323.82 tokens/s | 1096.93 ms / 95 runs | 86.61 tokens/s | 1175.11 ms |
| 99 | 9.73 ms / 17 tokens | 1746.99 tokens/s | 209.04 ms / 95 runs | 454.45 tokens/s | 234.60 ms |

Student judgment: `-ngl 99` was clearly faster for this model and device.

## Context Size

| `ctx-size` | Prompt eval | Prompt eval speed | Eval | Eval speed | Total time |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 1024 | 12.10 ms / 27 tokens | 2231.96 tokens/s | 209.50 ms / 95 runs | 453.46 tokens/s | 237.70 ms |
| 2048 | 10.67 ms / 27 tokens | 2531.41 tokens/s | 212.54 ms / 95 runs | 446.97 tokens/s | 239.21 ms |
| 4096 | 15.52 ms / 27 tokens | 1740.03 tokens/s | 214.81 ms / 95 runs | 442.24 tokens/s | 246.74 ms |

Student judgment: with this short prompt and small model, `ctx-size` did not produce a strong monotonic speed change. This is not enough to conclude that context size has no memory impact.

## CPU Threads

| Threads | Prompt eval | Prompt eval speed | Eval | Eval speed | Total time |
| ---: | ---: | ---: | ---: | ---: | ---: |
| 2 | 97.94 ms / 20 tokens | 204.20 tokens/s | 361.52 ms / 22 runs | 60.85 tokens/s | 467.20 ms |
| 4 | 50.75 ms / 20 tokens | 394.13 tokens/s | 256.38 ms / 22 runs | 85.81 tokens/s | 314.33 ms |
| 8 | 59.90 ms / 20 tokens | 333.90 tokens/s | 261.40 ms / 22 runs | 84.16 tokens/s | 329.68 ms |

Student judgment: 4 threads was best in this small CPU-path run. More threads did not keep improving.

## llama-bench

| `-ngl` | `pp128` | `tg64` |
| ---: | ---: | ---: |
| 0 | 497.34 +/- 50.76 tokens/s | 85.30 +/- 1.90 tokens/s |
| 99 | 11225.55 +/- 1566.34 tokens/s | 494.28 +/- 2.05 tokens/s |

## Resource Snapshot Note

Before/after `nvidia-smi` snapshots did not show meaningful delta because the runs were very short and the server already had other GPU memory in use. For short runs, continuous sampling or `llama-bench` output is more useful than only before/after snapshots.
