# Model 2 teacher–student knowledge distillation

[All phases](../../README.md) · [Phase 3 experiments](../README.md) · [Notebook](notebook.ipynb)

## Architecture and methodology

The implementation loaded a trained Phase 3 teacher with matching tokenizer/classification labels and trained a student classifier using supervised and teacher signals.

## Implementation

- **Source notebook:** [`notebook.ipynb`](notebook.ipynb).
- **Execution pipeline:** Model/backbone and adaptation setup, input processing, task head, training or checkpoint restoration, development evaluation, and prediction export, as implemented in the notebook.
- **Model assets:** The implementations use the custom Phase 1 backbone/checkpoint and corresponding tokenizer, rather than replacing the backbone with an unrelated pretrained model.

## Experimental configuration

The following values are literal assignments recorded in the source notebook. Their presence does not imply that every execution path uses the same settings; inspect the active configuration cells before reproducing the run.

- `SEED = 42`
- `POOLING = 'target_mean_and_last_average'`
- `VOCAB_SIZE = 32000`
- `N_LAYER = 15`
- `N_HEAD = 8`
- `N_KV_HEAD = 4`
- `MODEL_CONTEXT_LENGTH = 2048`
- `MAX_LENGTH = 256`
- `BATCH_SIZE = 4`
- `EPOCHS = 8`
- `GRAD_ACCUM_STEPS = 4`

## Recorded results

Student dev 13-class Macro-F1 0.287443; submission ZIP created.

The [Phase 3 guide](../README.md) reports the **official phase-level hidden-test Codabench score** separately. A specific Codabench submission ID is not assigned to this notebook without a verified variant-to-submission mapping.

## Execution instructions

1. Install the [repository dependencies](../../../requirements.txt) and open [`notebook.ipynb`](notebook.ipynb) in a GPU-enabled Kaggle or compatible Jupyter runtime.
2. Attach the required inputs and model assets: Phase 3 rhetorical-role train/dev/test JSON, the matching Model 2 foundation checkpoint, and the corresponding tokenizer. A task-trained teacher checkpoint is also required for distillation.
3. Set the dataset, checkpoint, tokenizer, and output paths according to the active notebook configuration. Where used, configure `ASSET_DIR` and the Kaggle `HF` secret for authorized data access; non-Kaggle runtimes may require a different secret loader.
4. Mount Phase 3 train/dev/test JSON and the configured foundation model. Execute the experiment cells, inspect 13-class Macro-F1, and export test role sequences.
5. Inspect development metrics and any exported `prediction.json` or `submission.zip` in the configured output directory.

Notebooks preserve recorded execution outputs, but rerunning the experiments requires the proper datasets, checkpoints, tokenizer files, and GPU runtime. Dataset files, model weights, checkpoints, generated predictions, and submission ZIPs are not included in the public repository.
