# Student Notes

## What Was Confusing

1. The profiling page says to save logs with `tee`, but when stdout and stderr were split, `llama-completion` timing appeared in stderr. Parsing stdout produced blank fields.
2. `nvidia-smi` 0.2-second polling still missed GPU utilization on a very short run. The memory and power changed, but utilization stayed at `0%`.
3. The parsed table is useful, but it mixes logs from earlier runs with the new profiling run. Students should label which rows are newly measured and which rows are extracted from previous experiments.
4. The output quality was generic. Profiling should not only record speed; it should also say whether the prompt answer is good enough.

## Course Feedback

1. Add a warning: parse the combined log from `2>&1 | tee`, or parse stderr if stdout/stderr were separated.
2. Add a warning: short LLM runs may be too brief for reliable `nvidia-smi` utilization sampling.
3. Recommend `llama-bench` or a longer generation when the goal is GPU utilization rather than just timing extraction.
4. Keep the profiling table evidence-based: every row needs a log path and a note about whether it came from a previous experiment or a fresh run.
