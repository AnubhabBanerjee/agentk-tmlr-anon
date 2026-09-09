# AgentK — Anonymous Submission for TAI Review

Artifacts supporting the anonymous TAI submission:

**Anatomy of a Quantized Agent: VRAM Stability and Forecasting in Code-Synthesis Agentic Workloads**

The reviewer-facing anonymized URL is the anonymous.4open.science mirror of this repository (see the TAI submission for the exact `/r/<slug>/` URL).

## Contents

```
.
├── LICENSE                                        # Apache-2.0 for our contributions
├── README.md                                      # this file
├── agentk_code.zip                                # anonymized source bundle (code, configs, paper source)
├── Licenses                                       # 3rd party licenses
└── data/
    ├── dataset_release_v2_derived.tar.zst         # 1.2 MB — derived artifacts
    └── dataset_release_v2_raw_traces.tar.zst      # 4.7 MB — raw traces + CUDA sidecars
```

Each archive contains its own `LICENSE` (Apache-2.0 for our contributions) and `LICENSE-KernelBench` (MIT, third-party). See `Licenses/THIRD_PARTY_LICENSES.md` inside each archive for the full third-party attribution.

## Dataset at a glance

| Quantity | Value |
|---|---|
| Traced runs | 1,920 |
| Agent backbones | 4 (all Q4\_K\_M) |
| Runs per backbone | Phi-4-mini 300; Qwen2.5-Coder-7B, Qwen2.5-Coder-14B, Mistral-7B-Instruct-v0.3 540 each |
| Prompt suite | 300 (100 KernelBench, 100 clean-synthetic, 100 retry-provoking-synthetic) |
| Prompt split | stratified 70/15/15, seed 20260715 → 210 / 45 / 45 prompts |
| Trace-row split | 1,416 train / 234 val / 270 test |
| Hardware | 1× NVIDIA H100 80 GB, no MIG |

Per-artifact row counts, byte sizes, and SHA-256 prefixes are itemised in `data/dataset_release_manifest_v2.md`.

## Checksums

```
agentk_code.zip                          sha256:2bdad588864cc7a913d618971c8e94088c6c4e59f2d05da6f62827096f3bdc8b
dataset_release_v2_derived.tar.zst       sha256:872ce39fdbf539589dd92b02b6bde9e246d63f88585a040aa853ba01c41b4746
dataset_release_v2_raw_traces.tar.zst    sha256:226ebced2fbd55d28d786e5247ad01d4d4d92f4de2e3e826d17011dabaeaae1a
```

Verify with `sha256sum <file>`.

## Extract

`agentk_code.zip` unpacks to a directory named `agentk_anonymous_bundle/`:

```
unzip agentk_code.zip
```

The code bundle already ships the derived data it needs under `agentk_anonymous_bundle/data/` (`traces_v2/index.parquet`, `labels_v2/`, `prompts_v2/`, `correctness_v2/`), so **no archive extraction is required to reproduce the paper tables and figures**.

The two archives are supplied for independent inspection and for the raw traces:

```
# Standalone derived-dataset release; unpacks to ./dataset_release_v2/
tar --zstd -xf data/dataset_release_v2_derived.tar.zst

# Raw per-run traces + CUDA sidecars; unpacks to ./data/ plus top-level licence files.
# Run this from inside agentk_anonymous_bundle/ to merge the raw traces into the bundle.
# It rewrites LICENSE, LICENSE-KernelBench, THIRD_PARTY_LICENSES.md, and
# data/traces_v2/index.parquet with byte-identical copies, so the merge is lossless.
tar --zstd -xf ../data/dataset_release_v2_raw_traces.tar.zst
```

## Reproduce paper tables and figures (~5 minutes on any CPU)

```
cd agentk_anonymous_bundle
python3 -m pip install -r requirements.txt
make evaluate  CONFIG=configs/tracing_v2.yaml SKIP=B4
make figures   CONFIG=configs/tracing_v2.yaml SKIP=B4
make analyses  CONFIG=configs/tracing_v2.yaml
```

Expected: `paper/results_v2/tables/primary_results.tex` with 20 data rows (4 agents × 5 methods — B0, B1, B2, B3, B5; B4 omitted per §Experiments), plus figures under `paper/figures_v2/`.

## Reproduce raw tracing from scratch (~57 GPU-h on 1× H100 80 GB)

```
cd agentk_anonymous_bundle
bash scripts/01_download_models.sh
make trace-batch-v2 CONFIG=configs/tracing_v2.yaml
make build-trace-index build-correctness-report build-labels
```

`trace-batch-v2` runs the 1,200 base-configuration traces (4 agents × 300 prompts) followed by the 720 sweep traces (3 agents × 40 prompts × 2 context sizes × 3 KV-cache dtypes; Phi-4-mini is excluded from the sweep), totalling 1,920. The ~57 GPU-h figure is a projected wall-clock estimate, not a measured total.

## Third-party components

- **KernelBench** (MIT) at pinned commit `423217d9fda91e0c2d67e4a43bf62f96f6d104f1`.
- **LangGraph** (MIT), pip-installed via `langgraph==1.2.9`; not vendored.
- **LLM backbones**: Qwen2.5-Coder-7B/14B (Apache-2.0), Phi-4-mini (MIT), Mistral-7B-Instruct-v0.3 (Apache-2.0). Attribution, upstream URLs, and SHA256 pins in `configs/model_manifest.yaml` (inside `agentk_code.zip`).

## License

Our contributions are released under the Apache License, Version 2.0 (see `LICENSE`). Third-party components retain their upstream licenses.

## Anonymity notice

This is a double-blind submission to TAI. Reviewers are asked not to attempt to identify the authors. `AgentK` is a review-only pseudonym; the framework will be released under its real name upon acceptance.

## Contact during review

Correspondence should go through the OpenReview discussion for this submission. Do not open GitHub issues against this repository during the review period — issue authorship would break anonymity for both parties.
