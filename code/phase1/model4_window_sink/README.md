# Model 4: sliding-window/sink-attention decoder

[All phases](../../README.md) · [Phase 1 experiments](../README.md) · [Notebook](notebook.ipynb)

## Architecture and methodology

The implementation used a causal decoder with sliding-window attention, sink tokens and a different block/FFN design, restored its trained checkpoint, and generated test predictions.

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
- `N_EMBD = 560`
- `N_LAYER = 15`
- `N_HEAD = 8`
- `N_KV_HEAD = 4`
- `VOCAB_SIZE = 32000`
- `BATCH_SIZE = 2`

## Recorded results

53,217,360 foundation-model parameters; 1,000 test generations and a submission ZIP recorded.

The [Phase 1 guide](../README.md) reports the **official phase-level hidden-test Codabench score** separately. A specific Codabench submission ID is not assigned to this notebook without a verified variant-to-submission mapping.

## Execution instructions

1. Install the [repository dependencies](../../../requirements.txt) and open [`notebook.ipynb`](notebook.ipynb) in a GPU-enabled Kaggle or compatible Jupyter runtime.
2. Attach the required inputs and model assets: Phase 1 legal pretraining train/test data, the appropriate tokenizer or tokenizer-training files, and a matching model checkpoint when restoring a trained run.
3. Set the dataset, checkpoint, tokenizer, and output paths according to the active notebook configuration. Where used, configure `ASSET_DIR` and the Kaggle `HF` secret for authorized data access; non-Kaggle runtimes may require a different secret loader.
4. Attach the Phase 1 train/test data and the appropriate tokenizer; train the chosen architecture or restore its matching checkpoint. Run generation and export cells to create test predictions. For the grouped-inference experiment, restore the trained Model 2 weights and tokenizer before running `model2_gqa_59m/grouped_inference.ipynb`.
5. Inspect development metrics and any exported `prediction.json` or `submission.zip` in the configured output directory.

Notebooks preserve recorded execution outputs, but rerunning the experiments requires the proper datasets, checkpoints, tokenizer files, and GPU runtime. Dataset files, model weights, checkpoints, generated predictions, and submission ZIPs are not included in the public repository.
