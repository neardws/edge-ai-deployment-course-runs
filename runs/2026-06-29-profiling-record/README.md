# 2026-06-29 Profiling Record Run

## Goal

Act as a student following the profiling lab after baseline, quantization, inference acceleration, and local service runs.

This run checks:

- copying the profiling template and parser
- parsing existing llama.cpp timing logs
- running one fresh Q4 profiling sample
- recording GPU memory, temperature, and power with `nvidia-smi`
- identifying what the profiling record still cannot prove

No private hosts, addresses, usernames, raw logs, or model weights are stored here.

## Status

Complete.

## Files

- [commands.md](commands.md): sanitized command shapes used in the run
- [results-summary.md](results-summary.md): parsed timing and resource summary
- [student-notes.md](student-notes.md): confusing points and course feedback

## Key Conclusion

The parser and profiling table approach work, but students must parse the stream that actually contains timing output. With `llama-completion`, timing went to stderr in the measured run. Short runs can also make `nvidia-smi` show `0%` GPU utilization even though memory and power changed.
