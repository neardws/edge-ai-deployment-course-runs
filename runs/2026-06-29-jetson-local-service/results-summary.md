# Results Summary

## Build Result

`llama-server` was built successfully in the existing `build-jetson` directory.

Important observation: the target triggered UI asset generation:

- `npm install`
- PWA asset generation
- `vite build`
- C++ server implementation build

This makes the service target noticeably slower to build than the CLI target. It should be done before a live classroom demo when possible.

## Service Result

| Field | Value |
| --- | --- |
| Bind address | `127.0.0.1` |
| Port | `18081` |
| Endpoint | `/v1/chat/completions` |
| HTTP status | `200` |
| curl elapsed | `0.814655 s` |
| Python stdlib client elapsed | `0.63 s` |
| Service cleanup | server stopped; port released |

## Server Timing Excerpt

First curl request:

| Metric | Value |
| --- | ---: |
| Prompt eval | 101.08 ms / 38 tokens |
| Prompt eval speed | 375.92 tokens/s |
| Eval | 700.79 ms / 61 tokens |
| Eval speed | 87.04 tokens/s |
| Total | 801.88 ms / 99 tokens |

Second Python request:

| Metric | Value |
| --- | ---: |
| Prompt eval | 11.71 ms / 1 token |
| Eval | 602.13 ms / 64 tokens |
| Eval speed | 106.29 tokens/s |
| Total | 613.84 ms / 65 tokens |

The second request reused context and was faster. A final report should not mix this with the first-request result without explaining the difference.

## tegrastats Summary

Short smoke-test sample:

| Field | Observed |
| --- | --- |
| RAM | about 3253-3302 / 15656 MB |
| GR3D | peaked near 94% |
| GPU temperature | about 57-59 C |
| VDD_IN | about 6.1-13.1 W in the captured samples |

## Output Quality

The API returned JSON successfully, but the model answer was conceptually wrong and truncated. It described "quantizing task execution time" rather than model weights/activations and deployment tradeoffs.

Student judgment: API service passed, content quality failed for this prompt. The report should separate service availability from answer quality.
