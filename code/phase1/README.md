# Phase 1: Legal language-model pretraining

[Project overview](../../README.md) · [All phases](../README.md)

## Task and methodology

The pretraining phase compares four transformer foundation-model architectures on the legal corpus. The notebooks implement byte-level BPE tokenization, causal or encoder–decoder sequence objectives, training/evaluation, and Phase 1 test prediction export.

## Architecture and experiment variants

| Implemented variant | Local execution result |
|---|---|
| [Model 1: decoder-only transformer](./model1_decoder/README.md) | 59,653,632 trainable parameters; completed training with recorded validation CE 6.0278; a 1,000-example submission was generated. |
| [Model 2: GQA/RoPE/RMSNorm decoder](./model2_gqa_59m/README.md) | 59,665,152 trainable parameters; train CE 4.6083 and validation CE 5.1199 in a completed epoch; test submissions generated in both inference notebooks. |
| [Model 3: encoder–decoder transformer](./model3_encoder_decoder/README.md) | 62,007,296 trainable parameters; notebook records training completion and a 1,000-example test submission. |
| [Model 4: sliding-window/sink-attention decoder](./model4_window_sink/README.md) | 53,217,360 foundation-model parameters; 1,000 test generations and a submission ZIP recorded. |

**Model 2 inference note:** The additional [`grouped_inference.ipynb`](model2_gqa_59m/grouped_inference.ipynb) groups 1,000 prompts by length to reduce padding and uses the existing Model 2 checkpoint. It is not a separate fifth architecture.

## Dataset and evaluation

- **Inputs:** `Phase_1_Pretraining` corpus, model-specific tokenizer or tokenizer-training files, and Phase 1 test input.
- **Local evaluation:** Training/validation cross-entropy (CE) and the production of Phase 1 test submissions.
- **Best official hidden-test Codabench score:** **.8090** (rank **132**, submission ID `963992`). This is a **phase-level** result and is not automatically attributable to any single variant's local metrics.

## Reproduction

Install dependencies from the repository root and start Jupyter Lab:

```bash
python -m pip install -r requirements.txt
jupyter lab
```

1. Open a variant README from the comparison table and follow its link to `notebook.ipynb`.
2. Use a compatible GPU runtime and obtain the authorized [CS781 Capstone dataset](https://huggingface.co/datasets/Exploration-Lab/CS-781-Capstone).
3. Attach any required pretrained foundation checkpoint, matching tokenizer, and task-trained teacher model as documented in the variant README.
4. Attach the Phase 1 train/test data and the appropriate tokenizer; train the chosen architecture or restore its matching checkpoint. Run generation and export cells to create test predictions. For the grouped-inference experiment, restore the trained Model 2 weights and tokenizer before running `model2_gqa_59m/grouped_inference.ipynb`.
5. Inspect the saved local metrics and generated prediction JSON or submission ZIP under the notebook's configured output path.

Model weights, raw data, checkpoints, and generated submission archives are excluded from this public repository. Source notebooks preserve the recorded execution outputs; fresh runs depend on the correct assets and runtime configuration.
