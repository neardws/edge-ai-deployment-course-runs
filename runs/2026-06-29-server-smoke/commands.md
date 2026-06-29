# Commands

## Connect

```bash
ssh <student>@<server>
```

## Prepare Workspace

```bash
mkdir -p ~/edge-ai-lab/{env,models/qwen,src,logs,results,report}
find ~/edge-ai-lab -maxdepth 2 -type d | sort
```

## Environment Snapshot

```bash
uname -a
lsb_release -a || cat /etc/os-release
lscpu
free -h
df -h ~ ~/edge-ai-lab
nvidia-smi --query-gpu=name,driver_version,memory.total,memory.used --format=csv,noheader

python3 --version
cmake --version | head -n 1
git --version
gcc --version | head -n 1
g++ --version | head -n 1
```

## Get llama.cpp

```bash
cd ~/edge-ai-lab/src
git clone --depth 1 https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
git rev-parse --short HEAD | tee ~/edge-ai-lab/results/llama-cpp-commit.txt
```

## Build llama.cpp with CUDA

```bash
cd ~/edge-ai-lab/src/llama.cpp
cmake -B build -DGGML_CUDA=ON 2>&1 | tee ~/edge-ai-lab/logs/cmake-cuda.txt
```

The full default build was very long in this environment, so the smoke test switched to targeted build commands:

```bash
cmake --build build --config Release --target llama-cli llama-bench llama-server -j8 \
  2>&1 | tee ~/edge-ai-lab/logs/build-minimal-targets.txt

cmake --build build --config Release --target llama-completion -j8 \
  2>&1 | tee ~/edge-ai-lab/logs/build-llama-completion.txt
```

## Download First GGUF Model

```bash
cd ~/edge-ai-lab/models/qwen
curl -L -C - --retry 3 --retry-delay 3 \
  -o qwen2.5-0.5b-instruct-q4_k_m.gguf \
  https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF/resolve/main/qwen2.5-0.5b-instruct-q4_k_m.gguf

ls -lh qwen2.5-0.5b-instruct-q4_k_m.gguf
sha256sum qwen2.5-0.5b-instruct-q4_k_m.gguf
```

## Baseline Command That Failed as a Student Path

This command entered interactive chat mode with the current `llama.cpp` build and kept writing prompts until stopped:

```bash
CUDA_VISIBLE_DEVICES=1 ./build/bin/llama-cli \
  -m ~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf \
  -p "用三句话解释端侧模型量化的价值。" \
  -n 128 \
  -ngl 99 \
  --ctx-size 2048 \
  --temp 0.2 \
  --seed 42 \
  --no-conversation
```

Observed warning:

```text
--no-conversation is not supported by llama-cli
please use llama-completion instead
```

## Corrected Non-Interactive Baseline

```bash
cd ~/edge-ai-lab/src/llama.cpp

nvidia-smi --query-gpu=index,name,memory.used,memory.total --format=csv,noheader \
  > ~/edge-ai-lab/results/gpu-before-baseline-public.csv

CUDA_VISIBLE_DEVICES=1 ./build/bin/llama-completion \
  -m ~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf \
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
  2>&1 | tee ~/edge-ai-lab/logs/qwen-baseline-q4-k-m-completion.txt

nvidia-smi --query-gpu=index,name,memory.used,memory.total --format=csv,noheader \
  > ~/edge-ai-lab/results/gpu-after-baseline-public.csv
```

## Small Benchmark

```bash
cd ~/edge-ai-lab/src/llama.cpp

CUDA_VISIBLE_DEVICES=1 ./build/bin/llama-bench \
  -m ~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf \
  -ngl 99 \
  -p 128 \
  -n 128 \
  -r 3 \
  2>&1 | tee ~/edge-ai-lab/logs/qwen-bench-q4-k-m.txt
```
