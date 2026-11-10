# Model 2 hierarchical-context full fine-tuning

[All phases](../../README.md) · [Phase 3 experiments](../README.md) · [Notebook](notebook.ipynb)

## Architecture and methodology

The Model 2 full-fine-tuning experiment used previous-sentence context, positional/document features, mean-vs-last pooling probes and a layer-wise learning-rate decay setup.

## Implementation

- **Source notebook:** [`notebook.ipynb`](notebook.ipynb).
- **Execution pipeline:** Model/backbone and adaptation setup, input processing, task head, training or checkpoint restoration, development evaluation, and prediction export, as implemented in the notebook.
- **Model assets:** The implementations use the custom Phase 1 backbone/checkpoint and corresponding tokenizer, rather than replacing the backbone with an unrelated pretrained model.

## Experimental configuration

The following values are literal assignments recorded in the source notebook. Their presence does not imply that every execution path uses the same settings; inspect the active configuration cells before reproducing the run.

- `SMOKE_TEST = bool(int(os.environ.get('RR_SMOKE_TEST', '0')))`
- `SEED = 42`
- `VOCAB_SIZE = 32000 if not SMOKE_TEST else 512`
- `N_EMBD = 512 if not SMOKE_TEST else 64`
- `N_LAYER = 15 if not SMOKE_TEST else 2`
- `N_HEAD = 8 if not SMOKE_TEST else 4`
- `N_KV_HEAD = 4 if not SMOKE_TEST else 2`
- `MODEL_CONTEXT_LENGTH = 2048 if not SMOKE_TEST else 128`
- `MAX_LENGTH = 320 if not SMOKE_TEST else 48`
- `MAX_PREV_TOKENS = 112 if not SMOKE_TEST else 16`
- `BATCH_SIZE = 4`

## Recorded results

13-label dev Macro-F1 0.253523; saved notebook shows smoke test disabled, complete training, and test submission generated.

The [Phase 3 guide](../README.md) reports the **official phase-level hidden-test Codabench score** separately. A specific Codabench submission ID is not assigned to this notebook without a verified variant-to-submission mapping.

### Experiment mode

The recorded full experiment used `RR_SMOKE_TEST=0` (`smoke: False` in saved output). The optional synthetic-data smoke-test path is not the source of the reported development score.

## Execution instructions

1. Install the [repository dependencies](../../../requirements.txt) and open [`notebook.ipynb`](notebook.ipynb) in a GPU-enabled Kaggle or compatible Jupyter runtime.
2. Attach the required inputs and model assets: Phase 3 rhetorical-role train/dev/test JSON, the matching Model 2 foundation checkpoint, and the corresponding tokenizer.
3. Set the dataset, checkpoint, tokenizer, and output paths according to the active notebook configuration. Where used, configure `ASSET_DIR` and the Kaggle `HF` secret for authorized data access; non-Kaggle runtimes may require a different secret loader.
4. Mount Phase 3 train/dev/test JSON and the configured foundation model. Execute the experiment cells, inspect 13-class Macro-F1, and export test role sequences.
5. Inspect development metrics and any exported `prediction.json` or `submission.zip` in the configured output directory.

Notebooks preserve recorded execution outputs, but rerunning the experiments requires the proper datasets, checkpoints, tokenizer files, and GPU runtime. Dataset files, model weights, checkpoints, generated predictions, and submission ZIPs are not included in the public repository.
