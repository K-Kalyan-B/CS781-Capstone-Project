# CS781 Capstone: Phase and Experiment Index

The codebase contains four Phase 1 architectures, 28 downstream model/method variants, and an additional optimized Model 2 inference notebook (**33 executed notebooks**). Each phase guide documents its task, architecture choices, input files, evaluation metrics, and execution workflow. Each variant directory contains an implementation notebook and a corresponding technical README.

| Phase | Task | Model/method variants |
|---|---|---:|
| [Phase 1](phase1/README.md) | Legal language-model pretraining | 4 |
| [Phase 2](phase2/README.md) | Legal named-entity recognition | 6 |
| [Phase 3](phase3/README.md) | Rhetorical-role classification | 6 |
| [Phase 4](phase4/README.md) | Court judgment prediction | 4 |
| [Phase 5](phase5/README.md) | Legal statute identification | 4 |
| [Phase 6](phase6/README.md) | Abstractive legal summarization | 4 |
| [Phase 7](phase7/README.md) | Prior-case retrieval | 4 |

The additional [Phase 1 grouped-inference notebook](phase1/model2_gqa_59m/grouped_inference.ipynb) uses the existing Model 2 architecture; it is not counted as a fifth model. See the [repository README](../README.md) for dependencies, environment setup, dataset access, checkpoint prerequisites, installation commands, and the distinction between local metrics and Codabench hidden-test scores.
