# Results Summary

## Model Files

| File | Quantization | Size on disk | SHA256 |
| --- | --- | ---: | --- |
| `qwen2.5-0.5b-instruct-q4_k_m.gguf` | Q4_K_M | 469 MiB | `74a4da8c9fdbcd15bd1f6d01d621410d31c6fc00986f5eb687824e7b93d7a9db` |
| `qwen2.5-0.5b-instruct-q5_k_m.gguf` | Q5_K_M | 498 MiB | `041474553fcabfc2a2d67903f9d2c2e50bd92528e670da4f33b5d0ce6e59fd55` |
| `qwen2.5-0.5b-instruct-q8_0.gguf` | Q8_0 | 645 MiB | `ca59ca7f13d0e15a8cfa77bd17e65d24f6844b554a7b6c12e07a5f89ff76844e` |

## Completion Timing

| Quantization | Load time | Prompt eval | Prompt eval speed | Eval time | Eval speed | Total time |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Q4_K_M | 454.91 ms | 14.44 ms | 1315.61 tokens/s | 106.68 ms | 431.20 tokens/s | 130.51 ms |
| Q5_K_M | 449.64 ms | 9.72 ms | 1955.34 tokens/s | 99.00 ms | 414.14 tokens/s | 116.57 ms |
| Q8_0 | 554.41 ms | 5.95 ms | 3191.13 tokens/s | 171.93 ms | 453.68 tokens/s | 191.04 ms |

## llama-bench

| Quantization | Prompt processing `pp128` | Token generation `tg128` |
| --- | ---: | ---: |
| Q4_K_M | 22021.68 +/- 4745.41 tokens/s | 478.37 +/- 17.26 tokens/s |
| Q5_K_M | 21247.92 +/- 4592.61 tokens/s | 465.50 +/- 28.51 tokens/s |
| Q8_0 | 16013.77 +/- 965.26 tokens/s | 442.37 +/- 21.79 tokens/s |

## Quality Notes

All three outputs were fluent Chinese, but all three answered with generic business-value language rather than the concrete deployment value of quantization.

Expected concepts that were mostly missing:

- smaller model files
- lower RAM or VRAM pressure
- lower bandwidth pressure
- lower latency on constrained devices
- offline or on-device deployment feasibility

Student judgment: Q4 had the best small-run benchmark speed and smallest file, but this run alone is not enough to recommend Q4 as a deployment default because the fixed prompt quality was weak across all variants.
