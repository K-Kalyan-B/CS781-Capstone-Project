# Phase 5: Legal statute identification

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

Multi-label statute classification uses sigmoid outputs and development-calibrated thresholds to generate statute-ID lists. Variants compare full fine-tuning, AdaLoRA, LoRMA, and knowledge distillation.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Model 2 full fine-tuning](./model2_full_finetune/README.md) | Best dev Macro-F1 0.196579; test submission created. |
| [Model 2 AdaLoRA](./model2_adalora/README.md) | Best dev Macro-F1 0.086660; test submission created. |
| [Model 4 LoRMA](./model4_lorma/README.md) | Best dev Macro-F1 0.153031; test submission created. |
| [Model 4 knowledge distillation](./model4_distillation/README.md) | Best student dev Macro-F1 0.217616; test submission created. |

## Dataset and evaluation

- **Inputs:** `Phase_5_LSI/Train/train.json`, `Train/dev.json`, and `Test/test.json`, plus the matching foundation checkpoint and tokenizer and a task-trained teacher for distillation.
- **Local evaluation:** Multi-label development Macro-F1 and statute-ID prediction export.
- **Best official hidden-test Codabench score:** **.2044** (rank **131**, submission ID `1013449`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Mount Phase 5 train/dev/test files and the correct model assets. Train the multi-label head, select thresholds using development data, and export predicted statute identifiers.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
