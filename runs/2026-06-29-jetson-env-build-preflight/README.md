# 2026-06-29 Jetson Environment and Build Preflight

## Goal

Continue the Jetson chapter as a student after the first login attempt failed.

This run verifies the device environment, captures an idle `tegrastats` sample, and attempts the first `llama.cpp` CUDA build path.

Private hostnames, addresses, usernames, host keys, and full raw logs are omitted.

## Status

Partial.

Environment checks passed. CUDA configure passed after fixing the CUDA path and architecture setting. The target build was stopped because it was still compiling CUDA backend files after several minutes; this is recorded as a course timing and setup issue.

## Environment Summary

| Field | Value |
| --- | --- |
| Device class | NVIDIA Jetson Orin NX Super developer kit class |
| Architecture | aarch64 |
| Jetson Linux / L4T | R36.4.3 |
| OS | Ubuntu 22.04.5 LTS |
| Kernel | 5.15.148-tegra |
| RAM | 15 GiB total, about 11 GiB available at idle |
| Swap | 7.6 GiB |
| Disk | 233 GiB filesystem, about 114 GiB available |
| Power mode | `MAXN_SUPER` |
| TensorRT Python | 10.3.0 |
| Python | 3.10.12 |
| CMake | 3.22.1 |
| Git | 2.34.1 |
| GCC/G++ | 11.4.0 |
| CUDA compiler | Installed at `/usr/local/cuda-12.6/bin/nvcc`, not on default `PATH` |

## Idle tegrastats Sample

Observed idle range over a short sample:

| Field | Observed |
| --- | --- |
| RAM | about 3251 / 15656 MB |
| GR3D | 0% |
| GPU temperature | about 54.3-54.6 C |
| CPU temperature | about 54.8-55.1 C |
| VDD_IN | about 5298 mW |

## Build Findings

Direct `git clone` from the Jetson to GitHub stalled before the source checkout completed. The working path used a lab gateway that already had the same `llama.cpp` source checkout, transferred as a source archive.

The first CUDA configure without an explicit CUDA architecture selected many architectures:

```text
50-virtual;61-virtual;70-virtual;75-virtual;80-virtual;86-real;89-real;90-virtual
```

That made CUDA compilation too slow for a first student lab on the device.

Reconfiguring with Orin architecture only worked:

```bash
export PATH=/usr/local/cuda-12.6/bin:$PATH
cmake -B build-jetson -DGGML_CUDA=ON -DCMAKE_CUDA_ARCHITECTURES=87
```

CMake confirmed:

```text
Using CMAKE_CUDA_ARCHITECTURES=87
Found CUDAToolkit: /usr/local/cuda-12.6/include (found version "12.6.68")
```

The target build then progressed through CPU backend and into CUDA backend compilation, but was stopped at roughly the CUDA backend template-instantiation stage to avoid leaving a long-running compile in this session.

## Course Feedback

1. Add a gateway-login note: some lab devices must be reached by first logging into a gateway, then using gateway-side credentials.
2. Add a CUDA PATH note for Jetson: `nvcc` may exist under `/usr/local/cuda-12.6/bin` even when `nvcc` is not on the default `PATH`.
3. For Orin NX, use `-DCMAKE_CUDA_ARCHITECTURES=87` in the teaching command.
4. Warn that Jetson CUDA builds can take much longer than the server build and may need teacher-prepared source archives or prebuilt binaries.
5. Tell students to check for leftover remote build processes after interrupting SSH commands.
