# Commands

## Shared Setup

```bash
cd ~/edge-ai-lab/src/llama.cpp
model=~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf
mkdir -p ~/edge-ai-lab/results/inference-acceleration
```

## GPU Offload

```bash
for ngl in 0 99; do
  ./build/bin/llama-completion \
    -m "$model" \
    -p "解释端侧模型推理加速的主要手段。" \
    -n 96 \
    --ctx-size 2048 \
    -ngl "$ngl" \
    --temp 0.2 \
    --seed 42 \
    -cnv \
    -st \
    --no-display-prompt \
    --perf \
    2>&1 | tee ~/edge-ai-lab/logs/inferacc-ngl-${ngl}.log
done
```

## Context Size

```bash
for ctx in 1024 2048 4096; do
  ./build/bin/llama-completion \
    -m "$model" \
    -p "请用项目复盘方式解释 KV Cache 对端侧部署的影响，并列出三个风险。" \
    -n 96 \
    --ctx-size "$ctx" \
    -ngl 99 \
    --temp 0.2 \
    --seed 42 \
    -cnv \
    -st \
    --no-display-prompt \
    --perf \
    2>&1 | tee ~/edge-ai-lab/logs/inferacc-ctx-${ctx}.log
done
```

## CPU Threads

```bash
for threads in 2 4 8; do
  ./build/bin/llama-completion \
    -m "$model" \
    -p "用三句话解释 CPU fallback 为什么会影响推理速度。" \
    -n 64 \
    --ctx-size 1024 \
    -ngl 0 \
    -t "$threads" \
    --temp 0.2 \
    --seed 42 \
    -cnv \
    -st \
    --no-display-prompt \
    --perf \
    2>&1 | tee ~/edge-ai-lab/logs/inferacc-cpu-t${threads}.log
done
```

## llama-bench

```bash
for ngl in 0 99; do
  ./build/bin/llama-bench \
    -m "$model" \
    -p 128 \
    -n 64 \
    -ngl "$ngl" \
    -r 3 \
    2>&1 | tee ~/edge-ai-lab/logs/inferacc-bench-ngl-${ngl}.log
done
```
