# Student Notes

## What Worked

- `llama-completion -cnv -st --perf` worked cleanly for the acceleration lab.
- `-ngl 99` gave a large speedup over `-ngl 0`.
- `llama-bench` gave the clearest comparison for `-ngl 0` vs `-ngl 99`.

## What Was Confusing

1. The course still used `llama-cli` examples in this chapter, but the earlier student run already showed that `llama-completion` is safer for non-interactive labs.
2. `-ngl 0` logs still show the CUDA backend in `system_info`, because the binary has CUDA support. Students should not infer GPU offload only from that line.
3. Before/after `nvidia-smi` snapshots can miss very short inference runs.
4. The `ctx-size` experiment with a short prompt did not strongly expose KV Cache effects.
5. CPU thread scaling was not monotonic; 8 threads was slightly worse than 4 in this run.

## Course Feedback

- Update the acceleration lab commands to use `llama-completion`.
- Add a note that `-ngl 0` means no layer offload even if CUDA appears in `system_info`.
- For resource monitoring, recommend continuous sampling for short runs.
- Tell students not to overinterpret `ctx-size` results from short prompts.
- State explicitly that more CPU threads are not always better.
