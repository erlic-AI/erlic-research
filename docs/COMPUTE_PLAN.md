# Compute Plan and Resource Request

## Current model size

`proto_ayar()` analytical count:
- Total: 3.916B parameters.
- Active: 1.217B parameters.
- Context: 2,048 tokens.
- Current multi-GPU implementation: DistributedDataParallel (replicated model, not sharded).

## Why the first request is a pilot

The initial request should fund a reproducible hardware and training validation run, not promise a 20B-token training campaign before throughput and memory have been measured.

The pilot should establish:
1. The main model can be instantiated on the proposed accelerator.
2. Forward/backward and optimizer updates are finite.
3. Checkpoint save/load works.
4. Resume behavior is verified.
5. Tokens/second, peak VRAM, step time and storage throughput are measured.
6. A small, documented dataset slice can be loaded reproducibly.

## Memory warning

The rough lower-bound estimate for BF16 weights + BF16 gradients + 8-bit optimizer state is approximately 6 bytes per parameter, before activations and runtime overhead. For 3.916B parameters, this is already roughly 21.9 GiB per DDP rank, plus working memory.

Two 16 GiB T4 cards do **not** become one 32 GiB device. Current DDP replicates the entire model on each GPU. T4 also does not provide native BF16 support. The current main-model BF16 training path therefore must not be launched on Kaggle 2×T4.

Kaggle T4 is useful for:
- CPU pipeline tests.
- CUDA/NCCL tests using the small test architecture.
- Verifying that the expected number of GPUs is visible.
- Testing packaging, data loading and logging.

A true 3.916B main-model smoke run needs BF16-capable hardware with sufficient per-GPU VRAM, or a separately implemented and validated sharded training stack (FSDP/ZeRO/tensor parallelism).

## Requested resource profile (initial discussion, not a guaranteed allocation)

Ask for a short pilot allocation first. State the accelerator model, VRAM, number of GPUs, duration and storage only after a measured memory/throughput test. Do not request a fixed 60–90 day TPU allocation or 20B-token target as if it were guaranteed.

## Reporting template

For every run, record:
- Provider / accelerator / GPU count.
- GPU memory per device.
- CUDA, PyTorch, TorchAO and bitsandbytes versions.
- Git commit hash.
- Dataset manifest and tokenizer revision.
- Micro-batch, sequence length, gradient accumulation.
- Steps, tokens processed, wall time and tokens/sec.
- Peak allocated/reserved VRAM.
- Loss and gradient norm.
- Checkpoint size, save time and restore result.
