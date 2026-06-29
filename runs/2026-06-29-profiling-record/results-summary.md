# Results Summary

## Parsed Existing Timing Logs

| Log | load ms | prompt eval ms | prompt tok/s | eval ms | eval tok/s | total ms |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Q4 completion | 454.91 | 14.44 | 1315.61 | 106.68 | 431.20 | 130.51 |
| Q5 completion | 449.64 | 9.72 | 1955.34 | 99.00 | 414.14 | 116.57 |
| Q8 completion | 554.41 | 5.95 | 3191.13 | 171.93 | 453.68 | 191.04 |
| `-ngl 0` | 417.12 | 52.50 | 323.82 | 1096.93 | 86.61 | 1175.11 |
| `-ngl 99` | 430.18 | 9.73 | 1746.99 | 209.04 | 454.45 | 234.60 |
| `ctx 1024` | 439.98 | 12.10 | 2231.96 | 209.50 | 453.46 | 237.70 |
| `ctx 4096` | 435.80 | 15.52 | 1740.03 | 214.81 | 442.24 | 246.74 |

## Fresh Q4 Profiling Sample

Parsed from stderr/time log:

| Field | Value |
| --- | ---: |
| load time | 378.23 ms |
| prompt eval | 9.73 ms |
| prompt eval speed | 1951.72 tokens/s |
| eval | 134.31 ms |
| eval speed | 655.20 tokens/s |
| total | 158.42 ms |

GPU monitor summary from 0.2-second sampling:

| Field | Observed |
| --- | ---: |
| samples | 5 |
| memory used max | 1111 MiB |
| GPU utilization max | 0% |
| temperature max | 35 C |
| power draw max | 55.72 W |

The utilization value is not trusted as a peak because the run was too short. Memory and power changed, so the GPU path was active, but a longer run or `llama-bench` is better for utilization profiling.

## Output Quality

The model answered in three numbered points and stayed on the general idea of monitoring performance and resources. It was usable as a profiling explanation, but generic. It did not mention tokens/s, KV cache, or edge-device thermal limits.

## Judgment

This profiling record is good enough to fill a report table and teach students how to trace metrics back to logs. It is not enough for a rigorous performance claim because the fresh run is short and only sampled one prompt.
