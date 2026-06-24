# Source Index

Generated: 2026-06-24  
Scope checked: all PDFs under `sparsity papers/` and one-level subfolders, plus `sparsity papers/CHT_papers_Sparsemind/cht_papers.txt`.  
Text extracts: `source_text/`.

## Coverage Summary

I found 25 PDF files under `sparsity papers/` including duplicate copies. By content hash and title grouping, these reduce to the source groups below. The `source_text/` folder contains 24 text extracts because one identical duplicate pair shares the same sanitized extract filename and content group.

The curated file `sparsity papers/CHT_papers_Sparsemind/cht_papers.txt` lists **Pivoting Factorization: A Compact Meta Low-Rank Representation of Sparsity for Efficient Inference in Large Language Models**, but I did not find a matching PDF in the local `sparsity papers/` folder during this pass.

## Core CHT And CHT-Adjacent Papers

| Source group | Local reviewed file | Role in curriculum |
|---|---|---|
| Epitopological Learning and Cannistraci-Hebb Network Shape Intelligence | `sparsity papers/Epitopological_learning.pdf` and duplicate under `CHT_papers_Sparsemind/` | Original CHT framing, ultra-sparse advantage, ESML, CHT loop |
| Brain Network Science Modelling of Sparse Neural Networks Enables Transformers and LLMs to Perform as Fully Connected | `sparsity papers/Brain network science modelling of sparse neural networks enables Transformers and LLMs to perform as fully connected.pdf` and duplicate under `CHT_papers_Sparsemind/` | CHTs, CHTss, scalable Transformer and LLM sparse training |
| Adaptive Cannistraci-Hebb Network Automata | `sparsity papers/Adaptive Cannistraci-Hebb Network Automata Modelling of Complex Networks for Path-based Link Prediction.pdf` and copy under `CHT_papers_Sparsemind/` | Network-science link-prediction foundation |
| Sparse Spectral Training and Inference on Euclidean and Hyperbolic Neural Networks | `sparsity papers/CHT_papers_Sparsemind/Sparse Spectral Training and Inference on Euclidean and Hyperbolic Neural Networks_副本.pdf` | Low-rank/spectral sparse training context |
| Plug-and-Play: An Efficient Post-Training Pruning Method for Large Language Models | `sparsity papers/plug and play/Plug_and_Play_Pruning.pdf` and copy under `CHT_papers_Sparsemind/` | Post-training pruning and N:M pruning context |
| Alignment-Enhanced Integration of Connectivity and Spectral Sparsity in Dynamic Sparse Training of LLM | `sparsity papers/CHT_papers_Sparsemind/Alignment-Enhanced Integration of Connectivity and Spectral Sparsity in Dynamic Sparse Training of LLM_副本.pdf` plus another version at root | CHTsL, sparse plus low-rank alignment, LLM sparse pretraining |
| Cannistraci-Hebb Training with N:M Semi-Structured Sparsity for Pre-Training and Re-Training | `sparsity papers/CHTss with NM  for PreTraining and ReTraining.pdf` and identical duplicate under `CHT_papers_Sparsemind/` | CHTs24, 2:4 sparsity, eDSrT dense-to-sparse retraining |
| Towards NVIDIA 1-4 Semi-Structured 75% Sparsity via Cannistraci-Hebb N-M Dynamic Sparse-to-Sparse Training | `sparsity papers/Towards NVIDIA 1-4 Semi-Structured 75% Sparsity via Cannistraci–Hebb N-M Dynamic Sparse-to-Sparse Training.pdf` and identical duplicate under `CHT_papers_Sparsemind/` | CHTsNM, TANS, 2:4/1:4 direction |
| Edge of Sparse Stability for CHTsNM / TANS | `sparsity papers/EOS/Eos_CHTsNM_Exploration.pdf` | Internal research-plan style source for training dynamics and sparse stability |
| Cannistraci-Hebb Training on Ultra-Sparse Spiking Neural Networks | `sparsity papers/snn/CANNISTRACI-HEBB TRAINING ON ULTRA-SPARSE SPIKING NEURAL NETWORKS.pdf` and copy under `CHT_papers_Sparsemind/` | CH-SNN and temporal plus structural sparsity |
| Constructing Brain-Inspired Sparse Topologies for Energy-Efficient ANN-to-SNN Conversion via CHT | `sparsity papers/snn/Constructing Brain-Inspired Sparse Topologies for Energy-Efficient ANN-toSNN Conversion via Cannistraci-Hebb Training.pdf` | CHT-trained ANN topology inherited by converted SNNs |
| Network Shape Intelligence Outperforms AlphaFold2 Intelligence in Vanilla Protein Interaction Prediction | `sparsity papers/alpha_fold/Network shape intelligence outperforms AlphaFold2 intelligence in vanilla protein interaction prediction..pdf` | Non-neural-network example of network shape intelligence |

## Broader Sparsity And Theory Sources

| Source group | Local reviewed file | Role in curriculum |
|---|---|---|
| Sparser, Faster, Lighter Transformer Language Models | `sparsity papers/sparse engineering techniques/Sparser, Faster, Lighter Transformer Language Models.pdf` | Sparse kernels, packing formats, practical speedup |
| Sub-quadratic Sparse Attention | `sparsity papers/SSA/Sub-quadratic_Sparse_Attention.pdf` | Long-context sparse attention note; local note says official paper/code were not released |
| Weight-Sparse Transformers Have Interpretable Circuits | `sparsity papers/explainability/Weight-sparse transformers have interpretable circuits.pdf` | Sparsity and mechanistic interpretability |
| Dense Neural Networks Are Not Universal Approximators | `sparsity papers/sparsity theory/dense NN not universal approximator.pdf` | Theory motivation for sparse connectivity |
| Task Complexity Shapes Internal Representations and Robustness in Neural Networks | `sparsity papers/sparsity theory/Jankowski_2026_Mach._Learn.__Sci._Technol._7_025045.pdf` | Network-science view of internal representations and robustness |

## Duplicate Handling

Some papers appear both at the root of `sparsity papers/` and inside `sparsity papers/CHT_papers_Sparsemind/`. Where duplicate content hashes matched, the curriculum treats them as one source group. Where versions differed, I used the more publication-ready or curated version for the teaching narrative and noted the duplicate in the table.

## Curriculum Mapping

| Lesson | Main source groups |
|---|---|
| `01-why-sparsity-matters.md` | Full corpus overview, CHT scaling, N:M, SNN, systems sparsity |
| `02-sparsity-is-not-just-compression.md` | Plug-and-Play pruning, CHT, CHTs, CHTss, CHTsL, CHTs24, CHTsNM |
| `03-network-shape-intelligence.md` | Epitopological Learning, CHA, protein-interaction NSI, dense-network theory |
| `04-the-cht-learning-loop.md` | Epitopological Learning, Brain Network Science |
| `05-scaling-cht-to-transformers-and-llms.md` | Brain Network Science, CHTsL, SST, CHTsNM |
| `06-hardware-friendly-nm-sparsity.md` | CHTs24, CHTsNM, Sparser Faster Lighter |
| `07-retraining-distillation-and-recovery.md` | CHTs24/eDSrT, Plug-and-Play, internal CHT distillation guidance |
| `08-snn-edge-and-energy.md` | CH-SNN, ANN-to-SNN conversion |
| `09-evaluation-product-proof.md` | Full corpus, especially CHTs24/CHTsNM and SparseMind product-proof framing |
| `10-source-map-and-reading-plan.md` | Full corpus |
