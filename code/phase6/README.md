# Phase 6: Abstractive legal summarization

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

Abstractive summarization generates legal judgment summaries using Model 2/4 causal decoders. Experiments use LoRA, Pfeiffer bottleneck adapters, distillation, and LoRMA.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Model 2 LoRA summarization](./model2_lora/README.md) | BERTScore F1 0.806030 and ROUGE-L 0.124777 in one recorded local diagnostic; separate 30-example ROUGE-L 0.116545; 100 summaries exported. |
| [Model 2 Pfeiffer adapters](./model2_pfeiffer/README.md) | 30-example local ROUGE-L F1 0.113601; 100 test summaries exported. |
| [Model 2 knowledge distillation](./model2_distillation/README.md) | 30-example local ROUGE-L F1 0.112797; 100 test summaries exported. |
| [Model 4 LoRMA](./model4_lorma/README.md) | 30-example local ROUGE-L F1 0.169086; 100 test summaries exported. |

**Evaluation limitation:** ROUGE-L/BERTScore values shown below were recorded on different local subsets/settings. They are diagnostic results, not a controlled cross-variant ranking.

## Dataset and evaluation

- **Inputs:** `Phase_6_Summ/Train/train.json` and `Test/test.json`, the matching foundation checkpoint/tokenizer, and a task-trained teacher for distillation.
- **Local evaluation:** Local ROUGE-L and selected BERTScore diagnostics; hidden-test Codabench results reported separately.
- **Best official hidden-test Codabench score:** **.8025** (rank **125**, submission ID `1023369`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Mount Phase 6 training/test JSON and Model 2/4 assets. Run the summarization adapter or student training cells, evaluate the diagnostic subset as configured, and generate 100 test summaries. Local diagnostic subsets and settings differ across notebooks.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
