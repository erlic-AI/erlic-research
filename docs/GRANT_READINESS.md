# Grant Readiness Checklist

## Before applying

- [ ] Public GitHub organization profile and one clean primary repository.
- [ ] Hugging Face organization profile with accurate description.
- [ ] Technical README with architecture values sourced from `config.py`.
- [ ] Reproducible CPU test output.
- [ ] Kaggle GPU smoke output (small test architecture; not presented as main-model training).
- [ ] Main-model hardware preflight output from an appropriate BF16 GPU.
- [ ] A short main-model smoke run with logs and checkpoint restore evidence.
- [ ] Dataset manifest and source-license review.
- [ ] Compute budget based on measured tokens/sec and peak VRAM.
- [ ] Clear distinction between completed work and planned work.
- [ ] Named project contact and institutional affiliation only if true.

## Do not claim yet

- 20B synthetic tokens generated.
- 3.9B model pretrained or benchmark-competitive.
- SWE-Bench / Terminal-Bench verified.
- Secure Docker sandbox deployed.
- FSDP/ZeRO/tensor parallelism implemented.
- Apache-2.0 licensing for upstream data without per-source verification.

## Evidence to attach

1. GitHub repository and commit hash.
2. CPU test log.
3. Kaggle GPU smoke log.
4. Main-model smoke log from suitable hardware.
5. Compute/memory report.
6. Research overview and experiment plan.
7. Data manifest and license review.
