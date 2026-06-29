# Commands

## Check Access Through Gateway

```bash
ssh <gateway> 'ssh <jetson-user>@<jetson-host> "
  whoami
  hostname
  cat /etc/os-release | sed -n \"1,8p\"
  cat /etc/nv_tegra_release
  free -h
  df -h / /home
"'
```

## Build `llama-server`

The existing Jetson build already had `llama-cli`, `llama-completion`, and `llama-bench`, but not `llama-server`.

```bash
cd ~/edge-ai-lab/src/llama.cpp
cmake --build build-jetson --target llama-server -j 4 \
  2>&1 | tee ~/edge-ai-lab/logs/jetson-build-llama-server.txt
```

Student note: in this upstream version, building `llama-server` also built Web UI assets with `npm install` and `vite build`. This is much slower than building the CLI target.

## Start Local Service and Smoke Test

```bash
cd ~/edge-ai-lab/src/llama.cpp

PORT=18081
MODEL=~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf

ss -ltnp | grep -E ":${PORT}\b" || true

tegrastats --interval 1000 > ~/edge-ai-lab/logs/jetson-local-service-tegrastats.txt 2>&1 &
STATS_PID=$!

./build-jetson/bin/llama-server \
  -m "$MODEL" \
  -ngl 99 \
  --ctx-size 1024 \
  --host 127.0.0.1 \
  --port "$PORT" \
  --parallel 1 \
  > ~/edge-ai-lab/logs/jetson-local-service-server.log 2>&1 &
SERVER_PID=$!

for i in $(seq 1 90); do
  curl -fsS "http://127.0.0.1:${PORT}/health" >/dev/null 2>&1 && break
  curl -fsS "http://127.0.0.1:${PORT}/v1/models" >/dev/null 2>&1 && break
  sleep 1
done

cat > ~/edge-ai-lab/logs/jetson-local-service-request.json <<'JSON'
{"model":"qwen-local","messages":[{"role":"user","content":"用三句话解释端侧模型量化。"}],"temperature":0.2,"max_tokens":96}
JSON

curl -sS "http://127.0.0.1:${PORT}/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d @~/edge-ai-lab/logs/jetson-local-service-request.json \
  -o ~/edge-ai-lab/logs/jetson-local-service-response.json \
  -w "HTTP status: %{http_code}\nelapsed: %{time_total}s\n" \
  | tee ~/edge-ai-lab/logs/jetson-local-service-curl-meta.txt

kill "$SERVER_PID"
kill "$STATS_PID"
```

In a real lab script, wrap cleanup in a shell trap so the server is stopped if the smoke test fails.
