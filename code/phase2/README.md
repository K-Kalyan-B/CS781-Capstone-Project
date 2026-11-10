# Phase 2: Legal named-entity recognition

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

This phase detects 12 legal entity types. Experiments compare character-offset span extraction with BIO/BIOES token tagging, full fine-tuning, LoRA, LoRMA+, and teacher–student distillation. The final outputs are character-offset entity spans.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Character-span extraction on Model 2](./character_span/README.md) | Strict Fold 2 Macro-F1 0.233933; character-boundary diagnostics and test export executed. |
| [Model 2 full fine-tuning with BIO tags](./full_finetune_bio/README.md) | Strict Fold 2 Macro-F1 0.077863; prediction export recorded. |
| [Model 2 LoRA with BIO tags](./lora_bio/README.md) | Best strict Fold 2 Macro-F1 0.1283; Fold 2 and Fold 3 prediction files and submission path recorded. |
| [Model 2 LoRA with BIOES tags](./lora_bioes/README.md) | Strict Fold 2 Macro-F1 0.161582; submission ZIP and official local evaluator output recorded. |
| [Model 2 LoRMA+ with BIOES tags](./lorma_bioes/README.md) | Strict Fold 2 Macro-F1 0.131497; submission ZIP and official local evaluator output recorded. |
| [Model 2 teacher–student NER distillation](./knowledge_distillation/README.md) | Strict Fold 2 Macro-F1 0.115899; student predictions and submission ZIP recorded. |

## Dataset and evaluation

- **Inputs:** `Phase_2_LNER/Train/fold1-text.json`, `fold1-gold.json`, `fold2-text.json`, `fold2-gold.json`, `labels.txt`, and `Test/fold3-text.json`; matching Model 2 checkpoint and byte-level BPE tokenizer.
- **Local evaluation:** Strict character-offset entity-level Macro-F1 on Fold 2; export of Fold 3 test spans.
- **Best official hidden-test Codabench score:** **.1915** (rank **128**, submission ID `974914`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Mount the matching Model 2 backbone, tokenizer, and NER folds. Execute the task-specific training/evaluation cells, validate strict Fold 2 character offsets, and export Fold 3 predictions. In the LoRA/BIO notebooks, earlier Phase 1 setup cells primarily restore assets; NER training appears in later cells.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
