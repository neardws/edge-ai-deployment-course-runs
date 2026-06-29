# Commands

## Copy Lab Files from macOS

```bash
COPYFILE_DISABLE=1 tar -C <course-repo> -czf - labs/finetuning \
  | ssh <server> 'mkdir -p ~/edge-ai-lab/course && tar -C ~/edge-ai-lab/course -xzf -'
```

Without `COPYFILE_DISABLE=1`, macOS created `._*` AppleDouble files in the remote lab directory.

## Prepare Data

```bash
mkdir -p ~/edge-ai-lab/finetune/{data,outputs,logs,configs,results}
cp ~/edge-ai-lab/course/labs/finetuning/sample_sft_data.jsonl \
  ~/edge-ai-lab/finetune/data/sample_sft_data.jsonl
```

## Check Data

```bash
python - <<'PY'
import json
from pathlib import Path

path = Path.home() / "edge-ai-lab/finetune/data/sample_sft_data.jsonl"
for i, line in enumerate(path.read_text(encoding="utf-8").splitlines(), 1):
    row = json.loads(line)
    assert "messages" in row, f"line {i} missing messages"
    roles = [m["role"] for m in row["messages"]]
    assert roles[0] == "system", f"line {i} first role should be system"
    assert roles[-1] == "assistant", f"line {i} last role should be assistant"
print("ok")
print("samples", i)
PY
```

## Print Chat Template

```bash
CUDA_VISIBLE_DEVICES=1 python ~/edge-ai-lab/course/labs/finetuning/train_lora_smoke.py \
  --model Qwen/Qwen2.5-0.5B-Instruct \
  --data ~/edge-ai-lab/finetune/data/sample_sft_data.jsonl \
  --output ~/edge-ai-lab/finetune/outputs/template-check \
  --print-sample
```

## Run Smoke Test

```bash
CUDA_VISIBLE_DEVICES=1 python ~/edge-ai-lab/course/labs/finetuning/train_lora_smoke.py \
  --model Qwen/Qwen2.5-0.5B-Instruct \
  --data ~/edge-ai-lab/finetune/data/sample_sft_data.jsonl \
  --output ~/edge-ai-lab/finetune/outputs/qwen-lora-smoke \
  --max-steps 5 \
  --max-seq-length 512 \
  2>&1 | tee ~/edge-ai-lab/finetune/logs/qwen-lora-smoke-fixed.log
```

## Compare Base vs Adapter

```bash
CUDA_VISIBLE_DEVICES=1 python compare_base_adapter.py \
  2>&1 | tee ~/edge-ai-lab/finetune/logs/qwen-lora-compare.log
```

The comparison script loaded the base model, generated three fixed prompts, then loaded the saved adapter with PEFT and generated the same prompts again.
