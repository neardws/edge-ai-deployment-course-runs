# 2026-06-29 Jetson Local Service

## Goal

Act as a student trying to move the Jetson Qwen baseline from CLI inference into a local OpenAI-compatible service.

This record keeps the public, repeatable facts and omits private usernames, private addresses, SSH keys, and raw logs.

## Status

Complete.

## Access Path

The Jetson was reachable only through a lab gateway.

The direct `ProxyJump` shape failed because the final Jetson account did not accept the local machine's public key. The nested login shape worked because the gateway had the needed access material.

Public command shape:

```bash
ssh <gateway> 'ssh <jetson-user>@<jetson-host> "hostname; whoami"'
```

Student note: a course handout should distinguish `ProxyJump` from "SSH into the gateway, then SSH into Jetson". They use different authentication material and fail differently.

## Device Snapshot

| Field | Observed |
| --- | --- |
| OS | Ubuntu 22.04.5 LTS |
| Jetson Linux | R36.4.3 |
| Kernel | 5.15.148-tegra |
| GPU | Orin (`nvgpu`) |
| CUDA reported by `nvidia-smi` | 12.6 |
| RAM | about 15 GiB |
| Free root storage | about 113 GiB |
| Power mode | MAXN_SUPER |

## Model

| Field | Value |
| --- | --- |
| File | `qwen2.5-0.5b-instruct-q4_k_m.gguf` |
| Quantization | Q4_K_M |
| Size class | about 469 MiB |

## Files

- [commands.md](commands.md): sanitized commands used in the run
- [results-summary.md](results-summary.md): service, timing, and resource summary
- [student-notes.md](student-notes.md): confusing points and course feedback
