# Baseline Summary

## Non-Interactive Run

The current `llama-cli --no-conversation` command was not a safe first baseline command. It printed:

```text
--no-conversation is not supported by llama-cli
please use llama-completion instead
```

It then entered interactive chat mode and had to be stopped manually. This created a large log before cleanup.

The corrected command used `llama-completion -cnv -st --perf`, which exited cleanly.

## Generated Answer

Prompt:

```text
用三句话解释端侧模型量化的价值。
```

Observed answer:

```text
端侧模型量化的价值在于它能够提供实时的、准确的、可追踪的业务数据，帮助组织更好地理解业务流程、优化运营效率、提升客户体验，从而实现业务的持续增长和创新。
```

Student judgment: this answer is not acceptable for the lab goal. It is generic and does not explain model size, memory pressure, latency, bandwidth, offline operation, or edge-device constraints. The course should ask students to record both performance and answer quality.

## Timing Excerpt

```text
prompt eval time = 10.08 ms / 19 tokens (1885.86 tokens per second)
eval time        = 106.13 ms / 46 runs   (433.42 tokens per second)
total time       = 124.79 ms / 65 tokens
```

## llama-bench Result

```text
| model                  | size       | params   | backend | ngl | test  | t/s                  |
| ---------------------- | ---------: | -------: | ------- | --: | ----: | -------------------: |
| qwen2 1B Q4_K - Medium | 462.96 MiB | 630.17 M | CUDA    |  99 | pp128 | 21932.31 +/- 4643.15 |
| qwen2 1B Q4_K - Medium | 462.96 MiB | 630.17 M | CUDA    |  99 | tg128 | 457.48 +/- 34.58     |
```

## Course Feedback

1. Give students a concrete first GGUF download command.
2. Use targeted build commands for the first lab.
3. Replace the `llama-cli --no-conversation` baseline with a tested non-interactive command.
4. Show exactly which timing lines belong in the report.
5. Add an answer-quality checkpoint before treating the baseline as complete.
