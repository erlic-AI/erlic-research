# Known limitations and engineering roadmap

This file separates implemented code from future optimization work.

## Implemented in the current prototype

- Transformer/MoE model definition and analytical parameter count.
- Tokenizer training pipeline.
- Tokenized shard loading with deterministic window order.
- Pretraining loop with DDP, gradient accumulation, gradient checkpointing and optimizer checkpointing.
- SFT and RLVR/GRPO prototype loops.
- Atomic checkpoint helper and JSONL training logs.
- CPU synthetic test environment.
- CUDA smoke test using a small test architecture.
- Main-model BF16/memory preflight.

## Important limitations

1. **DDP is replicated, not sharded.** Each rank stores the full model. Two GPUs do not combine their VRAM. FSDP/ZeRO/tensor parallelism is not implemented in this repository.
2. **BF16 main-training path.** Current main-model training expects a BF16-capable CUDA GPU. T4 is not suitable for this path as currently written.
3. **MoE expert dispatch is Python-loop based.** The current routing implementation sorts token assignments and calls experts in a Python loop. It is a correctness-oriented prototype, not a fused high-throughput MoE kernel. `sayac.tolist()` also causes a device-to-host synchronization.
4. **No production inference server.** The repository does not yet include a hardened serving stack, batching scheduler, quantized deployment profile or autoscaling web service.
5. **Sandbox is not hardened.** `egitim/sandbox.py` is not a security boundary for arbitrary hostile code.
6. **Dataset licensing is not pre-approved.** Upstream sources need per-dataset and, where applicable, per-file review.
7. **Deduplication is not persistent across interrupted data-preparation runs.** The current source pipeline keeps its exact-document hash set in memory for one process. SHA-256 is used, but a persistent transactional dedup index and crash-safe shard/hash commit protocol remain to be implemented.
8. **Benchmark coverage is incomplete.** Do not claim SWE-Bench, Terminal-Bench or broad Turkish reasoning scores until those harnesses and results exist.
9. **No completed large-scale pretraining claim.** The model size in `config.py` is a parameter-count calculation, not proof of a trained model.

## Recommended engineering order

1. Complete CPU tests and a small CUDA test.
2. Run a main-model smoke on one suitable BF16 GPU; measure memory and throughput.
3. If the desired GPU class is too small, implement and test FSDP/ZeRO before requesting a long allocation.
4. Profile the MoE dispatch and replace Python-loop expert calls with a measured grouped/fused implementation.
5. Add robust data manifests, deduplication reports and license checks.
6. Add benchmark adapters with reproducible task versions.
7. Build inference/serving only after a usable checkpoint exists.
8. Build a public demo and website after model and evaluation artifacts are ready.

## Python vs Rust

The Hugging Face `tokenizers` package already uses a Rust implementation for core tokenization. Rewriting the training pipeline in Rust is not the first optimization. First profile actual bottlenecks (data loading, expert dispatch, communication, optimizer and checkpoint I/O), then optimize the measured hot paths. Keep Python for orchestration unless profiling demonstrates a specific reason to move a component.
