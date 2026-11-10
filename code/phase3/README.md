# Phase 3: Rhetorical-role classification

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

This phase predicts one of 13 rhetorical roles for legal-judgment sentences. Experiments combine sentence context and document-position information with Model 2/4 representations, different adaptation methods, distillation, and sequence decoding.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Model 2 full fine-tuning with calibrated decoding](./model2_full_finetune/README.md) | Best raw 13-class dev Macro-F1 0.250149; decoded dev result 0.2770; test predictions exported. |
| [Model 2 hierarchical-context full fine-tuning](./model2_full_finetune_hierarchical/README.md) | 13-label dev Macro-F1 0.253523; saved notebook shows smoke test disabled, complete training, and test submission generated. |
| [Model 2 LoRA](./model2_lora/README.md) | Best recorded epoch dev Macro-F1 0.270535; prediction JSON exported. |
| [Model 2 teacher–student knowledge distillation](./model2_distillation/README.md) | Student dev 13-class Macro-F1 0.287443; submission ZIP created. |
| [Model 4 full fine-tuning with sequence decoding](./model4_full_finetune/README.md) | Raw dev Macro-F1 0.299630; postprocessed dev Macro-F1 0.351803; test predictions generated. |
| [Model 4 LoRMA+](./model4_lorma/README.md) | Best training-epoch dev Macro-F1 0.2583; submission ZIP recorded. |

**Distinct full-fine-tuning configurations:** The two Model 2 full-fine-tuning notebooks compare class-balanced training and calibrated decoding against a hierarchical context/mean-pooling setup with layer-wise learning-rate decay. Both produced saved development outputs and test predictions.

## Dataset and evaluation

- **Inputs:** `Phase_3_RR/Train/CL_train.json`, `IT_train.json`, `CL_dev.json`, `IT_dev.json`, and test splits; matching Model 2/4 backbone, tokenizer, and a task-trained teacher checkpoint for distillation.
- **Local evaluation:** 13-class development Macro-F1, with raw and decoded results distinguished where applicable.
- **Best official hidden-test Codabench score:** **.3312** (rank **129**, submission ID `993925`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Mount Phase 3 train/dev/test JSON, the correct Model 2/4 checkpoint and tokenizer, and a matching Phase 3 teacher checkpoint for distillation. Execute the experiment cells, inspect 13-class Macro-F1, and export test role sequences.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
