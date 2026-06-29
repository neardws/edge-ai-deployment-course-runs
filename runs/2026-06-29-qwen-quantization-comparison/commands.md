# Commands

## Download Missing GGUF Variants

```bash
cd ~/edge-ai-lab/models/qwen

base=https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF/resolve/main

for f in qwen2.5-0.5b-instruct-q5_k_m.gguf qwen2.5-0.5b-instruct-q8_0.gguf; do
  curl -L -C - --retry 3 --retry-delay 3 -o "$f" "$base/$f"
  ls -lh "$f"
  sha256sum "$f"
done
```

## Completion Runs

```bash
cd ~/edge-ai-lab/src/llama.cpp
mkdir -p ~/edge-ai-lab/results/quantization

for model in \
  qwen2.5-0.5b-instruct-q4_k_m.gguf \
  qwen2.5-0.5b-instruct-q5_k_m.gguf \
  qwen2.5-0.5b-instruct-q8_0.gguf
do
  stem=${model%.gguf}

  nvidia-smi --query-gpu=index,name,memory.used,memory.total --format=csv,noheader \
    > ~/edge-ai-lab/results/quantization/${stem}-gpu-before.csv

  CUDA_VISIBLE_DEVICES=1 ./build/bin/llama-completion \
    -m ~/edge-ai-lab/models/qwen/$model \
    -p "用三句话解释端侧模型量化的价值。" \
    -n 128 \
    -ngl 99 \
    --ctx-size 2048 \
    --temp 0.2 \
    --seed 42 \
    -cnv \
    -st \
    --no-display-prompt \
    --perf \
    2>&1 | tee ~/edge-ai-lab/logs/${stem}-completion.log

  nvidia-smi --query-gpu=index,name,memory.used,memory.total --format=csv,noheader \
    > ~/edge-ai-lab/results/quantization/${stem}-gpu-after.csv
done
```

## llama-bench Runs

```bash
cd ~/edge-ai-lab/src/llama.cpp

for model in \
  qwen2.5-0.5b-instruct-q4_k_m.gguf \
  qwen2.5-0.5b-instruct-q5_k_m.gguf \
  qwen2.5-0.5b-instruct-q8_0.gguf
do
  stem=${model%.gguf}

  CUDA_VISIBLE_DEVICES=1 ./build/bin/llama-bench \
    -m ~/edge-ai-lab/models/qwen/$model \
    -ngl 99 \
    -p 128 \
    -n 128 \
    -r 3 \
    2>&1 | tee ~/edge-ai-lab/logs/${stem}-bench.log
done
```

## Parse Timing Fields

The course parser worked on the saved completion logs:

```bash
python3 labs/scripts/parse_llama_log.py --self-test
python3 labs/scripts/parse_llama_log.py qwen2.5-0.5b-instruct-q4_k_m-completion.log --append quant_compare.csv
```

If the logs live on a remote server and the course repo lives locally, copy the logs locally first or clone the course repo on the server.
