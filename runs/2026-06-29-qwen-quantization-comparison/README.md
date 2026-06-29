# 2026-06-29 Qwen Quantization Comparison

## Goal

Act as a student following the Qwen GGUF quantization comparison lab.

This run compares three GGUF variants of the same public Qwen2.5 0.5B Instruct model on the same server GPU setup used in the first smoke run.

No model weights or raw logs are stored in this repository.

## Status

Complete.

## Files

- [commands.md](commands.md): commands used in this run
- [results-summary.md](results-summary.md): public model metadata and measured results
- [student-notes.md](student-notes.md): student-facing issues and course feedback

## Main Finding

The speed differences were small for token generation, while answer quality was weak across Q4/Q5/Q8 for the fixed prompt. This chapter should not let students choose a quantization format by speed alone.
