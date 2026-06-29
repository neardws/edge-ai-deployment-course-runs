# Commands

## Start Local API

```bash
cd ~/edge-ai-lab/src/llama.cpp

PORT=18083
MODEL=~/edge-ai-lab/models/qwen/qwen2.5-0.5b-instruct-q4_k_m.gguf

./build/bin/llama-server \
  -m "$MODEL" \
  -ngl 99 \
  --ctx-size 2048 \
  --host 127.0.0.1 \
  --port "$PORT" \
  --parallel 1 \
  > ~/edge-ai-lab/logs/agent/local-agent-server.log 2>&1 &
```

## Ask for Agent Tool Policy

```bash
cat > ~/edge-ai-lab/logs/agent/local-agent-request.json <<'JSON'
{
  "model": "qwen-local",
  "messages": [
    {
      "role": "system",
      "content": "你是一个端侧 Local Agent planner。只输出 JSON，不要 Markdown。schema: {allowed_tools:string[], confirm_required:string[], blocked_tools:string[], fallback:string, reason:string}。"
    },
    {
      "role": "user",
      "content": "任务：总结本地 profiling 日志并生成部署评估报告草稿。可用工具：read_log, parse_metric_csv, write_report_draft, run_shell, send_network_request, delete_file。请按最小权限原则分类。"
    }
  ],
  "temperature": 0,
  "max_tokens": 256
}
JSON

curl -sS "http://127.0.0.1:${PORT}/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d @~/edge-ai-lab/logs/agent/local-agent-request.json \
  -o ~/edge-ai-lab/logs/agent/local-agent-response.json \
  -w "HTTP status: %{http_code}\nelapsed: %{time_total}s\n"
```

## Validate Policy

The validation checked:

- valid JSON
- required keys
- no overlap between `allowed_tools`, `confirm_required`, and `blocked_tools`
- high-risk tools are not allowed

The model output failed the policy validation because the same tools appeared in conflicting sets.

## Generate Final Report Draft

```bash
python3 generate_deployment_report.py
```

The report draft was built from existing timing CSVs, model SHA256, Agent validation output, and prior run summaries. It was saved outside the course repo:

```text
~/edge-ai-lab/report/deployment-evaluation-report.md
```
