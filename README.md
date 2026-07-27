# AgentK — Anonymous Submission for TMLR Review

Artifacts supporting the anonymous TMLR submission:

**The Illusion of Memory Variance: Why Simple Averages Beat Analytical Memory Predictions for AI Agents**

The reviewer-facing anonymized URL is the anonymous.4open.science mirror of this repository (see the OpenReview submission for the exact `/r/<slug>/` URL).

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

Each archive contains its own `LICENSE` (Apache-2.0 for our contributions) and `LICENSE-KernelBench` (MIT, third-party). See `THIRD_PARTY_LICENSES.md` inside each archive for the full third-party attribution.

## Checksums

```
agentk_code.zip                          sha256:<compute with: sha256sum agentk_code.zip>
dataset_release_v2_derived.tar.zst       sha256:872ce39fdbf539589dd92b02b6bde9e246d63f88585a040aa853ba01c41b4746
dataset_release_v2_raw_traces.tar.zst    sha256:226ebced2fbd55d28d786e5247ad01d4d92f4de2e3e826d17011dabaeaae1a
```

## Extract

```
unzip agentk_code.zip
tar --zstd -xf data/dataset_release_v2_derived.tar.zst
tar --zstd -xf data/dataset_release_v2_raw_traces.tar.zst
```

## Reproduce paper tables and figures (~5 minutes on any CPU)

From inside the extracted `agentk_code` directory, after extracting `dataset_release_v2_derived.tar.zst` alongside:

```
python3 -m pip install -r requirements.txt
make evaluate  CONFIG=configs/tracing_v2.yaml SKIP=B4
make figures   CONFIG=configs/tracing_v2.yaml SKIP=B4
make analyses  CONFIG=configs/tracing_v2.yaml
```

Expected: `paper/results_v2/tables/primary_results.tex` (20 rows = 4 agents × 5 methods; B4 omitted per §Experiments), plus figures under `paper/figures_v2/`.

## Reproduce raw tracing from scratch (~57 GPU-h on 1× H100 80 GB)

```
bash scripts/01_download_models.sh
make trace-batch-v2 CONFIG=configs/tracing_v2.yaml
make build-trace-index build-correctness-report build-labels
```

## Third-party components

- **KernelBench** (MIT) at pinned commit `423217d9fda91e0c2d67e4a43bf62f96f6d104f1`.
- **LangGraph** (MIT), pip-installed via `langgraph==1.2.9`; not vendored.
- **LLM backbones**: Qwen2.5-Coder-7B/14B (Apache-2.0), Phi-4-mini (MIT), Mistral-7B-Instruct-v0.3 (Apache-2.0). Attribution, upstream URLs, and SHA256 pins in `configs/model_manifest.yaml` (inside `agentk_code.zip`).

## License

Our contributions are released under the Apache License, Version 2.0 (see `LICENSE`). Third-party components retain their upstream licenses.

## Anonymity notice

This is a double-blind submission to TMLR. Reviewers are asked not to attempt to identify the authors. `AgentK` is a review-only pseudonym; the framework will be released under its real name upon acceptance.

## Contact during review

Correspondence should go through the OpenReview discussion for this submission. Do not open GitHub issues against this repository during the review period — issue authorship would break anonymity for both parties.
