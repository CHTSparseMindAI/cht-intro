# Source Map And Reading Plan

Read time: 12 minutes  
Core question: what should a new hire read, and in what order?

## The Reading Strategy

Do not read the papers in file-system order. Read them in concept order.

The goal is to build a mental stack:

1. Why sparsity can be more than compression.
2. How network-shape intelligence supports link prediction.
3. How CHT turns link prediction into sparse training.
4. How CHT scales to Transformers and LLMs.
5. How CHT becomes hardware-aware through N:M sparsity.
6. How recovery, distillation, and evaluation turn papers into products.
7. How adjacent sparsity work informs engineering decisions.

## Phase 1: Foundation

Start with:

1. **Epitopological Learning and Cannistraci-Hebb Network Shape Intelligence**  
   Learn the original CHT framing, ultra-sparse advantage challenge, ESML, CHT, percolation, and topology-guided regrowth.

2. **Adaptive Cannistraci-Hebb Network Automata**  
   Learn the broader network-science link-prediction foundation. This helps you understand why CHT uses topology rather than only gradients.

3. **Network Shape Intelligence Outperforms AlphaFold2 Intelligence**  
   Read this as a conceptual bridge, not as a CHT implementation paper. It shows how network shape intelligence can matter outside neural network training.

Output after Phase 1:

Write a one-page note explaining why CHT is a link-prediction method applied to neural network topology.

## Phase 2: CHT For Neural Networks And LLMs

Then read:

4. **Brain Network Science Modelling of Sparse Neural Networks Enables Transformers and LLMs to Perform as Fully Connected**  
   This is the main scaling paper. Focus on BRF, CHTs, CHTss, soft regrowth, gradual density decay, and LLaMA experiments.

5. **Alignment-Enhanced Integration of Connectivity and Spectral Sparsity in Dynamic Sparse Training of LLM**  
   Learn CHTsL and the cancellation problem between sparse and low-rank branches.

6. **Sparse Spectral Training and Inference on Euclidean and Hyperbolic Neural Networks**  
   Read this to understand the low-rank and spectral side that CHTsL builds on.

Output after Phase 2:

Draw a table comparing CHT, CHTs, CHTss, and CHTsL.

## Phase 3: Hardware-Aware Sparse Training

Then read:

7. **Cannistraci-Hebb Training with N:M Semi-Structured Sparsity for Pre-Training and Re-Training**  
   Focus on CHTs24, true sparse training, 2:4 constraints, eDSrT, and why STE-based methods can still carry dense training costs.

8. **Towards NVIDIA 1-4 Semi-Structured 75% Sparsity via Cannistraci-Hebb N-M Dynamic Sparse-to-Sparse Training**  
   Focus on CHTsNM, TANS, 2:4 versus 1:4, and the caution that 1:4 is promising but not yet consistently better than dense.

9. **Edge of Sparse Stability for CHTsNM / TANS**  
   This is an internal-style research proposal. Read it after CHTsNM, not before. It helps you think about optimizer dynamics and topology refresh events.

Output after Phase 3:

Write a short answer to: "Why does hardware-aware sparsity reduce algorithmic freedom, and why is that worth it?"

## Phase 4: Recovery And Pruning

Then read:

10. **Plug-and-Play: An Efficient Post-Training Pruning Method for Large Language Models**  
    Learn post-training pruning, Relative Importance and Activation, channel permutation, and N:M pruning as a dense-to-sparse route.

11. **CHTss with N:M / eDSrT sections from the CHTs24 paper again**  
    Read the retraining parts twice. They are central for customer-facing model conversion.

Output after Phase 4:

Design a dense-to-2:4 sparse recovery experiment for a small LLM.

## Phase 5: Edge, SNN, And Energy

Then read:

12. **Cannistraci-Hebb Training on Ultra-Sparse Spiking Neural Networks**  
    Learn CH-SNN, temporal plus structural sparsity, synaptic operation reduction, and neuromorphic deployment.

13. **Constructing Brain-Inspired Sparse Topologies for Energy-Efficient ANN-to-SNN Conversion via CHT**  
    Learn how CHT-trained sparse ANNs can become better sources for SNN conversion.

Output after Phase 5:

Write a one-page memo on when SparseMind should talk about SNNs and when it should stay focused on LLMs.

## Phase 6: Broader Sparsity Context

Finally read:

14. **Sparser, Faster, Lighter Transformer Language Models**  
    Understand custom sparse kernels, packing formats, and why practical speedup needs systems work.

15. **Weight-Sparse Transformers Have Interpretable Circuits**  
    Understand sparsity as an interpretability tool and the capability-interpretability trade-off.

16. **Dense Neural Networks Are Not Universal Approximators**  
    Understand the theoretical motivation that dense connectivity can have limits under certain assumptions.

17. **Task Complexity Shapes Internal Representations and Robustness in Neural Networks**  
    Understand neural layers as signed weighted bipartite graphs and how topology relates to robustness and task difficulty.

18. **Sub-quadratic Sparse Attention**  
    Treat this as a brief market/technology note, not as a verified paper. The local note says official paper and code were not released at the time.

Output after Phase 6:

Create a one-page "sparse ecosystem map" showing where CHT is central and where other sparse methods are adjacent.

## A Two-Week Onboarding Schedule

Day 1: Lessons 0-2 from this course.  
Day 2: Epitopological Learning paper.  
Day 3: CHA and network-shape intelligence papers.  
Day 4: Lesson 4 plus Brain Network Science paper.  
Day 5: CHTs, CHTss, and CHTsL comparison note.  
Day 6: N:M lesson plus CHTs24 paper.  
Day 7: CHTsNM and TANS notes.  
Day 8: Retraining and distillation lesson.  
Day 9: Plug-and-Play pruning and eDSrT recovery plan.  
Day 10: SNN and ANN-to-SNN papers.  
Day 11: Systems sparsity papers and kernel lessons.  
Day 12: Evaluation and product proof lesson.  
Day 13: New-hire presentation draft.  
Day 14: Team review and first assigned experiment or product memo.

## Final New-Hire Presentation

At the end of onboarding, each new hire should present:

- what CHT is;
- what problem it solves;
- how it differs from pruning;
- how it differs from ordinary dynamic sparse training;
- how it connects to N:M hardware;
- how dense-to-sparse recovery should be evaluated;
- one claim SparseMind can safely make;
- one claim SparseMind should not make without more evidence.

Previous: [09-evaluation-product-proof.md](09-evaluation-product-proof.md)  
Next: [source_index.md](../sources/source_index.md)

