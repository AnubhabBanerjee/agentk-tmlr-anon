# Third-party licenses — `agentk-vram-v2` release bundles

This repository's **own** dataset and code artifacts are released under **Apache-2.0**
(see repository `LICENSE` when published). The items below are **third-party** components
referenced or redistributed inside the derived/raw dataset bundles.

## KernelBench (prompt text and problem IDs)

- **Upstream:** [ScalingIntelligence/KernelBench](https://github.com/ScalingIntelligence/KernelBench)
- **Pinned commit:** `423217d9fda91e0c2d67e4a43bf62f96f6d104f1` (`configs/kernelbench_version.txt`)
- **What we redistribute:** 100 KernelBench-derived prompt definitions as text in
  `data/prompts_v2/all.jsonl` (derived bundle); trace rows and CUDA sidecars reference
  KernelBench prompt IDs (`kbench-l*-*`) but do **not** ship upstream KernelBench source.
- **License:** MIT — verbatim copy in `LICENSE-KernelBench` at each bundle root.

## Agent LLM backbones (generated trace text)

Per-run `.jsonl.zst` traces contain **model-generated** text (reasoning, code, tool outputs).
Source models and upstream licenses (from `configs/model_manifest.yaml`):

| Model | Upstream repo | License |
|-------|---------------|---------|
| Qwen2.5-Coder-7B-Instruct | `Qwen/Qwen2.5-Coder-7B-Instruct` | Apache-2.0 |
| Qwen2.5-Coder-14B-Instruct | `Qwen/Qwen2.5-Coder-14B-Instruct` | Apache-2.0 |
| Phi-4-mini-Instruct | `microsoft/Phi-4-mini-instruct` | MIT |
| Mistral-7B-Instruct-v0.3 | `mistralai/Mistral-7B-Instruct-v0.3` | Apache-2.0 |

GGUF quantizations were served via `llama.cpp` from locally cached Q4_K_M weights;
we redistribute **outputs only**, not the weight files.

## LangGraph

- **Dependency:** `langgraph==1.2.9` (imported via `pip install`; **not** vendored in bundles).
- **License:** MIT (see PyPI / upstream repository).
