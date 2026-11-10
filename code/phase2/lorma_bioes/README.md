# Model 2 LoRMA+ with BIOES tags

[All phases](../../README.md) · [Phase 2 experiments](../README.md) · [Notebook](notebook.ipynb)

## Architecture and methodology

The experiment evaluated multiplicative low-rank adaptation of the decoder with BIOES tagging, then selected the best checkpoint by strict span Macro-F1.

## Implementation

- **Source notebook:** [`notebook.ipynb`](notebook.ipynb).
- **Execution pipeline:** Model/backbone and adaptation setup, input processing, task head, training or checkpoint restoration, development evaluation, and prediction export, as implemented in the notebook.
- **Model assets:** The implementations use the custom Phase 1 backbone/checkpoint and corresponding tokenizer, rather than replacing the backbone with an unrelated pretrained model.

## Experimental configuration

The following values are literal assignments recorded in the source notebook. Their presence does not imply that every execution path uses the same settings; inspect the active configuration cells before reproducing the run.

- `TRAIN_TOKENS = 150_000_000`
- `VAL_TOKENS = 4_000_000`
- `EPOCHS = 1`
- `SEED = 42`
- `MODEL_CONTEXT_LENGTH = 2048`
- `N_EMBD = 512`
- `N_LAYER = 15`
- `N_HEAD = 8`
- `N_KV_HEAD = 4`
- `VOCAB_SIZE = 32000`
- `BATCH_SIZE = 2`

Some retained settings belong to the Phase 1 model-loading/pretraining setup; task-specific training and evaluation controls appear in later notebook cells.

## Recorded results

Strict Fold 2 Macro-F1 0.131497; submission ZIP and official local evaluator output recorded.

The [Phase 2 guide](../README.md) reports the **official phase-level hidden-test Codabench score** separately. A specific Codabench submission ID is not assigned to this notebook without a verified variant-to-submission mapping.

## Execution instructions

1. Install the [repository dependencies](../../../requirements.txt) and open [`notebook.ipynb`](notebook.ipynb) in a GPU-enabled Kaggle or compatible Jupyter runtime.
2. Attach the required inputs and model assets: Phase 2 NER train/dev/test folds, the matching Model 2 foundation checkpoint, and the corresponding tokenizer.
3. Set the dataset, checkpoint, tokenizer, and output paths according to the active notebook configuration. Where used, configure `ASSET_DIR` and the Kaggle `HF` secret for authorized data access; non-Kaggle runtimes may require a different secret loader.
4. Mount the matching Model 2 backbone, tokenizer, and NER folds. Execute the task-specific training/evaluation cells, validate strict Fold 2 character offsets, and export Fold 3 predictions. In the LoRA/BIO notebooks, earlier Phase 1 setup cells primarily restore assets; NER training appears in later cells.
5. Inspect development metrics and any exported `prediction.json` or `submission.zip` in the configured output directory.

Notebooks preserve recorded execution outputs, but rerunning the experiments requires the proper datasets, checkpoints, tokenizer files, and GPU runtime. Dataset files, model weights, checkpoints, generated predictions, and submission ZIPs are not included in the public repository.
