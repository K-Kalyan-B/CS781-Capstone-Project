# Model 2 full fine-tuning

[All phases](../../README.md) · [Phase 7 experiments](../README.md) · [Notebook](notebook.ipynb)

## Architecture and methodology

The experiment fine-tuned the full Model 2 encoder representation for legal query–candidate relevance, then evaluated candidate reranking.

## Implementation

- **Source notebook:** [`notebook.ipynb`](notebook.ipynb).
- **Execution pipeline:** Model/backbone and adaptation setup, input processing, task head, training or checkpoint restoration, development evaluation, and prediction export, as implemented in the notebook.
- **Model assets:** The implementations use the custom Phase 1 backbone/checkpoint and corresponding tokenizer, rather than replacing the backbone with an unrelated pretrained model.

## Experimental configuration

The following values are literal assignments recorded in the source notebook. Their presence does not imply that every execution path uses the same settings; inspect the active configuration cells before reproducing the run.

- `EXPERIMENT = "full"`
- `TRAIN_BATCH_SIZE = 2`

## Recorded results

Dev Micro-F1@7 0.301278; 237 test-query outputs validated; selected fusion weight on neural scores 0.0.

The [Phase 7 guide](../README.md) reports the **official phase-level hidden-test Codabench score** separately. A specific Codabench submission ID is not assigned to this notebook without a verified variant-to-submission mapping.

### Fusion interpretation

Development calibration selected a neural-score fusion weight of `0.0`. The reported `0.301278` Micro-F1@7 therefore represents lexical retrieval, not a measured gain from this neural variant.

## Execution instructions

1. Install the [repository dependencies](../../../requirements.txt) and open [`notebook.ipynb`](notebook.ipynb) in a GPU-enabled Kaggle or compatible Jupyter runtime.
2. Attach the required inputs and model assets: Phase 7 query/candidate train/dev/test JSON, the matching Model 2 foundation checkpoint, and the corresponding tokenizer.
3. Set the dataset, checkpoint, tokenizer, and output paths according to the active notebook configuration. Where used, configure `ASSET_DIR` and the Kaggle `HF` secret for authorized data access; non-Kaggle runtimes may require a different secret loader.
4. Mount Phase 7 query/candidate files and the matching foundation model/tokenizer. Build lexical candidates, train the neural scorer, tune fusion against development data, then generate and validate outputs for 237 test queries.
5. Inspect development metrics and any exported `prediction.json` or `submission.zip` in the configured output directory.

Notebooks preserve recorded execution outputs, but rerunning the experiments requires the proper datasets, checkpoints, tokenizer files, and GPU runtime. Dataset files, model weights, checkpoints, generated predictions, and submission ZIPs are not included in the public repository.
