# Phase 4: Court judgment prediction

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

Binary judgment-outcome classification uses Model 2/4 legal backbones with full fine-tuning, LoRA, LoRMA, and teacher–student knowledge distillation.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Model 2 full fine-tuning](./model2_full_finetune/README.md) | Best dev Macro-F1 0.592369; submission created for 1,517 test cases. |
| [Model 2 LoRA](./model2_lora/README.md) | Best dev Macro-F1 0.537609; submission created. |
| [Model 4 LoRMA](./model4_lorma/README.md) | Best dev Macro-F1 0.544761; submission created. |
| [Model 4 knowledge distillation](./model4_distillation/README.md) | Best student dev Macro-F1 0.586640; submission created. |

## Dataset and evaluation

- **Inputs:** `Phase_4_CJPE/Train/train.json`, `Train/dev.json`, and `Test/test.json`, plus the matching foundation checkpoint and tokenizer.
- **Local evaluation:** Binary development Macro-F1 and test label-prediction export.
- **Best official hidden-test Codabench score:** **.5746** (rank **125**, submission ID `1008152`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Mount Phase 4 train/dev/test files and the matching foundation model. Train the binary classification head with the selected adaptation method, validate the classifier, then export test label JSON and the submission archive.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
