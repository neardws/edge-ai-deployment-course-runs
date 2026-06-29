# Commands

## Copy Template and Parser

```bash
COPYFILE_DISABLE=1 tar -C <course-repo> -czf - \
  labs/scripts/parse_llama_log.py \
  labs/templates/profiling-results.md \
  | ssh <server> 'mkdir -p ~/edge-ai-lab/course && tar -C ~/edge-ai-lab/course -xzf -'
```

## Parse Existing Logs

```bash
mkdir -p ~/edge-ai-lab/results/profiling ~/edge-ai-lab/logs/profiling
cp ~/edge-ai-lab/course/labs/templates/profiling-results.md \
  ~/edge-ai-lab/results/profiling/profiling-results.md

rm -f ~/edge-ai-lab/results/profiling/llama-timing-summary.csv

for log in \
  ~/edge-ai-lab/logs/qwen2.5-0.5b-instruct-q4_k_m-completion.log \
  ~/edge-ai-lab/logs/qwen2.5-0.5b-instruct-q5_k_m-completion.log \
  ~/edge-ai-lab/logs/qwen2.5-0.5b-instruct-q8_0-completion.log \
  ~/edge-ai-lab/logs/inferacc-ngl-0.log \
  ~/edge-ai-lab/logs/inferacc-ngl-99.log \
  ~/edge-ai-lab/logs/inferacc-ctx-1024.log \
  ~/edge-ai-lab/logs/inferacc-ctx-4096.log
do
  python3 ~/edge-ai-lab/course/labs/scripts/parse_llama_log.py "$log" \
    --append ~/edge-ai-lab/results/profiling/llama-timing-summary.csv
done
```

## Run One Fresh Profiling Sample

```bash
cd ~/edge-ai-lab/src/llama.cpp

model=~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf
gpu_loop=~/edge-ai-lab/logs/profiling/qwen-q4-ngl99-gpu-loop.csv
stderr_log=~/edge-ai-lab/logs/profiling/qwen-q4-ngl99-time.txt
stdout_log=~/edge-ai-lab/logs/profiling/qwen-q4-ngl99-profile.log

(
  echo "timestamp,index,memory.used,memory.total,utilization.gpu,temperature.gpu,power.draw"
  while true; do
    nvidia-smi --id=1 \
      --query-gpu=timestamp,index,memory.used,memory.total,utilization.gpu,temperature.gpu,power.draw \
      --format=csv,noheader,nounits
    sleep 0.2
  done
) > "$gpu_loop" &
loop_pid=$!

CUDA_VISIBLE_DEVICES=1 /usr/bin/time -v ./build/bin/llama-completion \
  -m "$model" \
  -p "用三句话解释 profiling 对端侧部署的价值。" \
  -n 512 \
  -ngl 99 \
  --ctx-size 2048 \
  --temp 0.2 \
  --seed 42 \
  -cnv \
  -st \
  --no-display-prompt \
  --perf \
  > "$stdout_log" 2> "$stderr_log"

kill "$loop_pid"

python3 ~/edge-ai-lab/course/labs/scripts/parse_llama_log.py "$stderr_log"
```

Student note: parsing `stdout_log` returned blank timing fields because the timing lines were in `stderr_log`.
