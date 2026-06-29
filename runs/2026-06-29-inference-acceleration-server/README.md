# 2026-06-29 Inference Acceleration Server Run

## Goal

Act as a student following the inference acceleration lab on the server GPU path.

This run tests:

- `-ngl 0` vs `-ngl 99`
- `ctx-size` 1024 / 2048 / 4096
- CPU threads 2 / 4 / 8 on the CPU path
- `llama-bench` with `-ngl 0` and `-ngl 99`

No model weights or raw logs are stored in this repository.

## Status

Complete.

## Files

- [commands.md](commands.md): commands used in the run
- [results-summary.md](results-summary.md): measured timing and benchmark results
- [student-notes.md](student-notes.md): confusing points and course feedback
