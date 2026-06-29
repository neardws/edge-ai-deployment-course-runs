# Student Notes

## What Worked

- The `Start Here` path makes it clear that the first target is environment plus baseline, not the whole course.
- `~/edge-ai-lab/{env,models/qwen,src,logs,results,report}` is easy to create and understand.
- The environment chapter gives enough commands to capture OS, CPU, memory, disk, GPU, and toolchain.
- Targeted `llama.cpp` builds worked well once the exact targets were named.
- `llama-bench` produced a compact table that is easier to report than a full generation log.

## What Was Confusing

1. The baseline chapter says to prepare a Qwen GGUF file but does not give a concrete first download command.
2. It is unclear where a remote student should place the course repository if they want to run course helper scripts.
3. The first `llama.cpp` CUDA build is noisy and long; it also builds UI assets. A beginner may think the build is off track.
4. The current `llama-cli --no-conversation` path is dangerous for beginners because it enters interactive chat mode and can create a huge log.
5. After a generation run, the student still needs help mapping `prompt eval time`, `eval time`, and `tokens per second` to report fields.
6. The first output can be fast but semantically weak. The lab needs a simple quality checklist, not only speed numbers.
7. The course mentions watching GPU memory, but new SSH users may need a one-terminal alternative before learning `tmux` or multiple sessions.

## Suggested Course Fixes

- Add one concrete Qwen GGUF download example for the first baseline.
- Recommend targeted `llama.cpp` build commands first, then mention that a full build can be long and may include UI/frontend output.
- Replace or revise the `llama-cli --no-conversation` baseline path. With this tested build, `llama-completion -cnv -st` was the safer non-interactive command.
- Add a one-terminal baseline command that captures `nvidia-smi` before and after the run.
- Add a short "which lines to copy into the report" snippet immediately after the first baseline command.
- Add a short chat-template and answer-quality note for Qwen Instruct models.
