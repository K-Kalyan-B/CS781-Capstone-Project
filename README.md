# CS781 Capstone: In-House Legal Intelligence System

**Kalyan Babu Kakarapathi** · MS(R), Computer Science and Engineering, IIT Kanpur  
Roll number: `261110402` · IITK email: `khlvn26@iitk.ac.in` · Codabench: `k_261110402`

## Overview

This repository documents a seven-phase legal-language processing pipeline: legal language-model pretraining, named-entity recognition, rhetorical-role classification, court judgment prediction, statute identification, abstractive summarization, and prior-case retrieval. The foundation-model study compares four transformer designs. Downstream experiments evaluate full fine-tuning, low-rank adaptation, bottleneck adapters, and teacher–student knowledge distillation, as applicable to each task.

The repository contains **32 distinct model/method variants**, plus an executed grouped-inference optimization of Phase 1 Model 2: **33 notebooks across seven phases**. Each experiment has a linked notebook and variant README. Phase-level Codabench results are reported separately from local notebook metrics. The [final report](report/CS781-CP-Kalyan-Kakarapathi-261110402-report.pdf) contains the full analysis and LLM usage declaration; AI assistance was used for implementation and report drafting as disclosed there.

## Project architecture

### Foundation models (Phase 1)

| Model | Architecture | Parameters |
|---|---|---:|
| [Model 1](code/phase1/model1_decoder/README.md) | Decoder-only causal transformer using a custom BPE tokenizer | 59,653,632 |
| [Model 2](code/phase1/model2_gqa_59m/README.md) | 15-layer, 512-hidden-state causal decoder; 8 query heads, 4 key/value heads, grouped-query attention (GQA), rotary position embeddings (RoPE), QK normalization, RMSNorm, and sequential SiLU feed-forward layers | 59,665,152 |
| [Model 3](code/phase1/model3_encoder_decoder/README.md) | Encoder–decoder transformer | 62,007,296 |
| [Model 4](code/phase1/model4_window_sink/README.md) | Causal decoder with sliding-window/sink-token attention | 53,217,360 |

Model 2 also has a [grouped-inference implementation](code/phase1/model2_gqa_59m/grouped_inference.ipynb) that reduces zero-padding by grouping prompts by input length. This implementation reuses Model 2 weights and is not an additional architecture.

### Downstream processing

The downstream phases use the trained legal foundation models and task-specific heads or adaptation methods:

- **Phase 2 — Legal NER:** Character-span prediction and BIO/BIOES sequence tagging with full fine-tuning, LoRA, LoRMA+, and knowledge distillation.
- **Phase 3 — Rhetorical roles:** Thirteen-label sentence classification using sentence/document context, fine-tuning and adaptation, teacher–student training, and sequence-level decoding.
- **Phase 4 — Judgment prediction:** Binary outcome classification with Model 2/4 backbones and alternative training methods.
- **Phase 5 — Statute identification:** Multi-label classification with calibrated decision thresholds.
- **Phase 6 — Legal summarization:** Causal-decoder abstractive summary generation with LoRA, Pfeiffer adapters, distillation, and LoRMA.
- **Phase 7 — Prior-case retrieval:** TF-IDF candidate shortlisting combined with neural relevance scoring and development-calibrated fusion.

## Methodology and experiments

Each phase uses task-specific training/development data and exports test predictions for Codabench. Variant notebooks retain implementation code, configuration statements, recorded execution outputs, and local diagnostics. Local development scores and Codabench hidden-test results measure different evaluations and must not be conflated.

| Phase | Task | Variants | Best official Codabench score | Rank | Submission ID |
|---|---|---:|---:|---:|---|
| [1](code/phase1/README.md) | Legal language-model pretraining | 4 | 0.8090 | 132 | `963992` |
| [2](code/phase2/README.md) | Legal named-entity recognition | 6 | 0.1915 | 128 | `974914` |
| [3](code/phase3/README.md) | Rhetorical-role classification | 6 | 0.3312 | 129 | `993925` |
| [4](code/phase4/README.md) | Court judgment prediction | 4 | 0.5746 | 125 | `1008152` |
| [5](code/phase5/README.md) | Legal statute identification | 4 | 0.2044 | 131 | `1013449` |
| [6](code/phase6/README.md) | Abstractive legal summarization | 4 | 0.8025 | 125 | `1023369` |
| [7](code/phase7/README.md) | Prior-case retrieval | 4 | 0.2806 | 107 | `1037975` |

The phase guides contain **all 32 model/method variants**, local results, and direct links to the corresponding notebooks:

| Phase | Implemented variants |
|---|---|
| [Phase 1](code/phase1/README.md) | Model 1 decoder; Model 2 GQA/RoPE decoder; Model 3 encoder–decoder; Model 4 sliding-window/sink attention; Model 2 grouped inference (additional notebook) |
| [Phase 2](code/phase2/README.md) | Character-span; full fine-tuning/BIO; LoRA/BIO; LoRA/BIOES; LoRMA+/BIOES; knowledge distillation |
| [Phase 3](code/phase3/README.md) | Model 2 full fine-tuning; Model 2 hierarchical full fine-tuning; Model 2 LoRA; Model 2 distillation; Model 4 full fine-tuning; Model 4 LoRMA+ |
| [Phase 4](code/phase4/README.md) | Model 2 full fine-tuning; Model 2 LoRA; Model 4 LoRMA; Model 4 distillation |
| [Phase 5](code/phase5/README.md) | Model 2 full fine-tuning; Model 2 AdaLoRA; Model 4 LoRMA; Model 4 distillation |
| [Phase 6](code/phase6/README.md) | Model 2 LoRA; Model 2 Pfeiffer adapters; Model 2 distillation; Model 4 LoRMA |
| [Phase 7](code/phase7/README.md) | Model 2 partial fine-tuning; Model 2 full fine-tuning; Model 4 distillation; Model 4 LoRMA |

**Evaluation caveats:** Phase 6 ROUGE-L/BERTScore diagnostics use different local subsets/settings and are not a controlled cross-variant leaderboard. In Phase 7, development tuning assigned **zero weight** to neural scores for all four variants; the shared **0.301278 Micro-F1@7** therefore reflects the lexical shortlist, not a demonstrated neural improvement.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── code/
│   ├── README.md
│   ├── phase1/
│   │   ├── README.md
│   │   └── <model_variant>/
│   │       ├── README.md
│   │       └── notebook.ipynb
│   ├── phase2/
│   ├── phase3/
│   ├── phase4/
│   ├── phase5/
│   ├── phase6/
│   └── phase7/
│       ├── README.md
│       └── <method_variant>/
│           ├── README.md
│           └── notebook.ipynb
└── report/
    └── CS781-CP-Kalyan-Kakarapathi-261110402-report.pdf
```

Every phase contains a phase-level `README.md` and a directory for each variant. The Phase 1 Model 2 directory also contains `grouped_inference.ipynb`.

## Requirements and environment

- Python **3.10+**, compatible PyTorch/CUDA GPU environment, and [packages listed in `requirements.txt`](requirements.txt).
- Most experiments were executed in Kaggle GPU notebooks. Kaggle supplies `kaggle_secrets` and typically provides PyTorch/CUDA; other runtimes require path and secret-loader adjustments.
- Authorized access to the [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone), including phase-specific train/dev/test files.
- Matching Phase 1 **Model 2 or Model 4 checkpoints** and byte-level BPE tokenizer files for downstream experiments; teacher–student variants also require task-trained teacher checkpoints.

Training datasets, model weights, checkpoints, authentication tokens, generated predictions, and submission archives are **not committed** to this public repository.

## Installation and execution

Clone the repository and install dependencies:

```bash
git clone https://github.com/K-Kalyan-B/CS781-Capstone-Project.git
cd CS781-Capstone-Project
python -m pip install -r requirements.txt
jupyter lab
```

1. Open the [phase index](code/README.md), then select the required phase and variant README.
2. Obtain the authorized dataset and attach the phase-specific input files to the notebook runtime. Kaggle paths and mounts vary by environment.
3. Mount the appropriate pretrained foundation checkpoint and tokenizer. For distillation, provide the matching task-trained teacher checkpoint.
4. Review each notebook's configuration and asset-path cells (`ASSET_DIR`, input/output locations, and checkpoint paths where defined). Configure the Kaggle `HF` secret for authorized Hugging Face access, or adapt the secret loader outside Kaggle.
5. Run the relevant setup, training/checkpoint-loading, validation, and prediction-export cells following the variant-specific instructions. Inspect local metrics and the output `prediction.json` or `submission.zip` where generated.

**Reproducibility scope:** Notebooks contain saved execution evidence, but model training is not rerun as part of repository packaging. Some notebooks contain historical setup/error cells and may require selecting the active experiment cells, mounted assets, and a compatible GPU environment; a clean top-to-bottom run is not guaranteed by the saved outputs.

## Results and report

- The [phase index](code/README.md) links to per-phase comparisons and every notebook.
- The [final report](report/CS781-CP-Kalyan-Kakarapathi-261110402-report.pdf) presents the methodology, experiment results, analysis, and required LLM usage declaration.
- Best official Codabench scores are reported **per phase**; the phase-level submission IDs are not attributed to particular variant notebooks without an independently established mapping.

## Public repository hygiene

The [`.gitignore`](.gitignore) excludes checkpoint formats, model weights, raw datasets, environment files, intermediate outputs, prediction files, and submission ZIP archives. Do not commit private Hugging Face tokens or course-restricted data.
