# Phase 7: Prior-case retrieval

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

The retrieval pipeline produces seven prior-case candidates per query using lexical TF-IDF shortlisting and learned neural relevance scoring. Experiments compare full/partial fine-tuning, distillation, LoRMA, and lexical–neural fusion calibrated on development data.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Model 2 partial fine-tuning](./model2_partial/README.md) | Dev Micro-F1@7 0.301278; 237 test-query outputs validated; selected fusion weight on neural scores 0.0. |
| [Model 2 full fine-tuning](./model2_full_finetune/README.md) | Dev Micro-F1@7 0.301278; 237 test-query outputs validated; selected fusion weight on neural scores 0.0. |
| [Model 4 knowledge distillation](./model4_distillation/README.md) | Dev Micro-F1@7 0.301278; 237 test-query outputs validated; selected fusion weight on neural scores 0.0. |
| [Model 4 LoRMA](./model4_lorma/README.md) | Dev Micro-F1@7 0.301278; 237 test-query outputs validated; selected fusion weight on neural scores 0.0. |

**Fusion limitation:** Development tuning assigned a neural-score weight of `0.0` for every variant. Consequently, the shared `0.301278` Micro-F1@7 is the lexical-shortlist result. The `0.518066` lexical-shortlist oracle is an upper bound computed with development labels, not a test prediction score.

## Dataset and evaluation

- **Inputs:** `Phase_7_PCR` train, dev, and test query/candidate JSON files; matching foundation checkpoint and tokenizer, plus a trained retrieval teacher for distillation.
- **Local evaluation:** Development Micro-F1@7 and top-seven predictions for 237 test queries.
- **Best official hidden-test Codabench score:** **.2806** (rank **107**, submission ID `1037975`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Mount Phase 7 query/candidate files and the matching foundation model/tokenizer; provide a Phase 7 teacher for distillation. Build lexical candidates, train the neural scorer, tune fusion against development data, then generate and validate outputs for 237 test queries.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
