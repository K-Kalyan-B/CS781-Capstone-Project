# Model 4 LoRMA

[All phases](../../README.md) · [Phase 5 experiments](../README.md) · [Notebook](notebook.ipynb)

## Architecture and methodology

The experiment applied multiplicative low-rank adaptation to Model 4 for multi-label statute prediction and tuned thresholds against the development data.

## Implementation

- **Source notebook:** [`notebook.ipynb`](notebook.ipynb).
- **Execution pipeline:** Model/backbone and adaptation setup, input processing, task head, training or checkpoint restoration, development evaluation, and prediction export, as implemented in the notebook.
- **Model assets:** The implementations use the custom Phase 1 backbone/checkpoint and corresponding tokenizer, rather than replacing the backbone with an unrelated pretrained model.

## Experimental configuration

The following values are literal assignments recorded in the source notebook. Their presence does not imply that every execution path uses the same settings; inspect the active configuration cells before reproducing the run.

- `SEED = 42`
- `MODEL_ID = 4`
- `METHOD = 'lorma'`
- `VOCAB_SIZE = 32000`
- `N_EMBD = 512 if MODEL_ID == 2 else 560`
- `N_LAYER = 15`
- `N_HEAD = 8`
- `N_KV_HEAD = 4`
- `BATCH_SIZE = 2`
- `GRAD_ACCUM = 4`
- `EPOCHS = 3`

## Recorded results

Best dev Macro-F1 0.153031; test submission created.

The [Phase 5 guide](../README.md) reports the **official phase-level hidden-test Codabench score** separately. A specific Codabench submission ID is not assigned to this notebook without a verified variant-to-submission mapping.

## Execution instructions

1. Install the [repository dependencies](../../../requirements.txt) and open [`notebook.ipynb`](notebook.ipynb) in a GPU-enabled Kaggle or compatible Jupyter runtime.
2. Attach the required inputs and model assets: Phase 5 statute-identification train/dev/test JSON, the matching Model 4 foundation checkpoint, and the corresponding tokenizer.
3. Set the dataset, checkpoint, tokenizer, and output paths according to the active notebook configuration. Where used, configure `ASSET_DIR` and the Kaggle `HF` secret for authorized data access; non-Kaggle runtimes may require a different secret loader.
4. Mount Phase 5 train/dev/test files and the correct model assets. Train the multi-label head, select thresholds using development data, and export predicted statute identifiers.
5. Inspect development metrics and any exported `prediction.json` or `submission.zip` in the configured output directory.

Notebooks preserve recorded execution outputs, but rerunning the experiments requires the proper datasets, checkpoints, tokenizer files, and GPU runtime. Dataset files, model weights, checkpoints, generated predictions, and submission ZIPs are not included in the public repository.
