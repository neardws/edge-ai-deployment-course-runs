# Student Notes

## What Worked

- Three GGUF variants could be downloaded with resumable `curl`.
- `llama-completion -cnv -st` exited cleanly for all three variants.
- `llama-bench` produced compact `pp128` and `tg128` rows.
- `labs/scripts/parse_llama_log.py` successfully extracted timing fields from current `common_perf_print` logs.

## What Was Confusing

1. The course chapter still showed `llama-cli`, but the previous baseline run showed that current `llama-cli --no-conversation` is not the safe non-interactive path.
2. Download time and resumable download behavior matter even for 0.5B models; the chapter should keep the `curl -C - --retry` style.
3. If logs are on a remote server and the course repo is local, the parser command is not immediately runnable unless logs are copied locally or the course repo is cloned on the server.
4. The model printed a warning: `User-specified prompt will pre-start conversation`. It did not break the run, but students may wonder whether their chat template is correct.
5. The generated answers were fast but semantically weak. The lab needs to make quality failure a first-class result.

## Course Feedback

- Replace quantization-loop examples with `llama-completion -cnv -st --perf`.
- Add a concrete Qwen2.5 0.5B Q4/Q5/Q8 download block for smoke tests.
- Mention that parser scripts need access to the log files; remote students may need `scp` or a course repo clone on the server.
- Add one sentence that speed-only ranking is not enough when all variants fail the fixed prompt quality check.
