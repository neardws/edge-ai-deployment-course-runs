# Results Summary

## Environment

| Field | Observed |
| --- | --- |
| GPU path | one free server GPU selected with `CUDA_VISIBLE_DEVICES=1` |
| Working CUDA stack | PyTorch `2.9.1+cu128`, CUDA available |
| Incompatible stack found first | PyTorch `2.11.0+cu130` with a CUDA 12.8 driver, CUDA unavailable |
| Transformers | `4.57.6` |
| TRL | `1.7.0` |
| PEFT | `0.19.1` |
| Dataset size | 5 samples |

## Data and Template Checks

JSONL check:

```text
ok
samples 5
```

Template rendering showed Qwen chat markers:

```text
<|im_start|>system
...
<|im_start|>user
...
<|im_start|>assistant
...
```

## First Failure

The original course script failed with current TRL:

```text
TypeError: SFTTrainer.__init__() got an unexpected keyword argument 'dataset_text_field'
```

Fix applied in the course repo: use `SFTConfig(dataset_text_field="text", max_length=...)` and pass `processing_class=tokenizer` to `SFTTrainer`.

## Training Result

The fixed script completed 5 training steps and saved the adapter.

| Metric | Value |
| --- | ---: |
| Steps | 5 |
| Train runtime | 1.9852 s |
| Train samples/s | 2.519 |
| Train steps/s | 2.519 |
| Final train loss summary | 5.3668 |
| Peak allocated GPU memory during compare | about 1904.7 MiB |

Saved adapter files included:

- `adapter_config.json`
- `adapter_model.safetensors`
- tokenizer files
- `training_args.bin`

## Base vs Adapter Comparison

Prompt: `什么情况下不应该先做模型微调？`

| Model | Result |
| --- | --- |
| Base | Answered generic training prerequisites and missed the "do not fine-tune first" point. |
| Adapter | Listed generic training failure conditions, still missed the course-specific answer. |

Prompt: `请输出 JSON，总结 Q4 量化的两个风险。`

| Model | Result |
| --- | --- |
| Base | Returned JSON, but risks were about data leakage and system failure rather than Q4 quantization. |
| Adapter | Returned JSON, but interpreted Q4 as "fourth quarter" and discussed business data quality. |

Prompt: `请用表格比较 LoRA 和 QLoRA。`

| Model | Result |
| --- | --- |
| Base | Produced a table, but incorrectly described LoRA/QLoRA as L1/L2 regularization. |
| Adapter | Produced a table, but drifted into communication-signal concepts. |

## Judgment

The adapter proves the training pipeline works. It does not prove fine-tuning improves the target task.

This run should stop at "pipeline smoke test passed; data and evaluation need work" rather than moving directly to merge, GGUF conversion, or Jetson deployment.
