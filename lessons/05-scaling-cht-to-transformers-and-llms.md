# Scaling CHT to Transformers and LLMs

**Learning objective:** Understand how CHT adapts when moving from small networks to large-scale Transformer architectures and language models.

---

## The Scale Problem

It is one thing to demonstrate sparse topology learning on MLPs or CNNs. It is another to make it work for Transformers and large language models.

LLMs create several additional pressures:

- Weight matrices are enormous.
- Training is memory-intensive.
- Attention, MLP blocks, embeddings, and output heads behave differently.
- Sparse operations may not accelerate unless the pattern fits hardware.
- Evaluation must cover broad capability, not just a single loss curve.

The CHT family evolves because these pressures force it to evolve.

---

## The Scaling Breakthrough

The **[Brain Network Science](https://arxiv.org/abs/2501.19107)** paper (2025) is the key bridge from original CHT to scalable CHT. It introduces several critical improvements:

- **BRF initialization** for sparse neural networks — giving models a better starting topology.
- **GPU-friendly approximation** of the CH link predictor — making topology updates tractable at scale.
- **CHTs** — a soft rule for link removal and regrowth that balances exploration and exploitation.
- **CHTss** — gradual density decay that lets the model adapt to sparsity progressively.

The paper reports results across MLPs, machine-translation Transformers, and LLaMA-family language modeling experiments. The lesson is not simply that sparse models always beat dense ones. The lesson is that topology-guided sparse training can survive contact with larger architectures when the method becomes softer, more scalable, and schedule-aware.

---

## Why Transformers Are Different

Transformers contain multiple types of computation:

- Attention projections (Q, K, V, output).
- Feed-forward blocks.
- Residual connections.
- Layer normalization.
- Embeddings.
- Output projection.

Not every component should be sparsified the same way. Most CHT research focuses on linear and attention layers inside Transformer blocks — especially attention and MLP matrices — because those dominate parameters and compute.

The papers often distinguish **sparse-able parameters** from **dense parameters** (such as embeddings or layer norms). This matters for reporting. A model may have 50% sparsity in sparse-able layers but much less total model sparsity once all components are counted.

Always ask: *Where is sparsity applied?*

---

## CHTsL: Connectivity Plus Spectral Sparsity

The **[Alignment-Enhanced CHTsL](https://openreview.net/forum?id=jZplmg7Ad9)** paper (ICLR 2026) combines CHTs with low-rank training, producing a model with two parallel weight branches:

$$W = W_{\text{sparse}} + W_{\text{low-rank}}$$

where $W_{\text{sparse}}$ is a sparse matrix trained with CHT-guided topology evolution, and $W_{\text{low-rank}} = AB^\top$ is a low-rank factorization with $A \in \mathbb{R}^{m \times r}$, $B \in \mathbb{R}^{n \times r}$, and rank $r \ll \min(m, n)$.

**Why two branches?** Connectivity sparsity captures selected direct neuron-to-neuron links — precise, local, interpretable. Low-rank factorization captures broad spectral structure — diffuse, global, parameter-efficient. The hypothesis is that these two forms of efficiency are complementary: the sparse branch handles fine-grained patterns while the low-rank branch models the dominant modes of the weight matrix.

**The cancellation problem.** Naively adding the two branches creates a risk: the sparse branch may learn to "fight" the low-rank branch. If both branches try to model the same signal, their gradients can oppose each other, leading to destructive interference and slower convergence — or worse, a combined model that underperforms either branch alone.

**The alignment loss.** CHTsL introduces an alignment regularization term:

$$\mathcal{L}_{\text{align}} = \| W_{\text{sparse}}^\top W_{\text{low-rank}} \|_F^2$$

This penalizes the sparse and low-rank branches for occupying overlapping representational directions. By encouraging them to be orthogonal in their contributions, the alignment loss ensures the branches complement rather than cancel each other.

The practical result: CHTsL achieves better quality than CHTs alone or low-rank alone at the same parameter budget, especially in LLM pretraining settings where both local connectivity patterns and global spectral structure matter.

---

## CHTsNM: Sparse-to-Sparse Training Under N:M Constraints

The **[CHTsNM](https://www.preprints.org/manuscript/202605.0851)** paper (2026) pushes CHT into N:M semi-structured sparse-to-sparse training, introducing several novel components designed to handle the strict structural constraints of hardware-friendly sparsity.

### The Core Challenge

Under N:M constraints (e.g., 2:4 or 1:4), the sparse pattern is rigid: exactly N weights must be active in every contiguous group of M. This creates a tension with CHT's topology updates — removing and regrowing links must respect the local N:M structure, and optimizer dynamics must adapt to a mask that changes during training.

### CoMpLoRA: Complementary Low-Rank Compensation

When topology refreshes occur and the sparse mask changes abruptly, the model can experience a temporary quality drop — the optimizer's momentum and second-moment estimates (in Adam-style optimizers) were accumulated under the old mask and are now stale.

**CoMpLoRA** (Complementary Low-Rank Adaptation) mitigates this by maintaining a small, continuously updated low-rank branch alongside the sparse weights:

$$W = W_{\text{N:M sparse}} + \alpha \cdot BA^\top$$

Unlike CHTsL where both branches are co-equal, in CoMpLoRA the low-rank branch acts as a *buffer*: after each topology refresh, it absorbs the representational shift, providing a smooth transition while the sparse branch's optimizer state catches up. The low-rank branch is compact ($r$ is very small, typically 4–16) and adds negligible overhead.

### TANS: Topology-Aware Newton-Schulz Optimizer

Standard optimizers like Adam maintain per-parameter state (first and second moment estimates) that assumes parameters persist across training steps. In CHT, parameters *appear and disappear* as the topology evolves — newly regrown weights have no optimizer history, and removed weights leave behind stale state.

**TANS** (Topology-Aware Newton-Schulz) replaces Adam's element-wise preconditioning with a matrix-level second-order update inspired by the Newton-Schulz iteration:

$$\theta_{t+1} = \theta_t - \eta \cdot \text{NS}(G_t)$$

where $G_t$ is the gradient and $\text{NS}(\cdot)$ is a Newton-Schulz-based preconditioner that operates on the full gradient matrix rather than per-parameter. Because it does not maintain per-parameter state, TANS is naturally robust to mask changes — a newly regrown weight is treated identically to a long-existing one, since neither carries optimizer memory.

### Active-Mask Projection and Refresh-Aware Ramping

Two additional mechanisms stabilize training:

- **Active-mask projection:** after each topology update, the optimizer state (for Adam-based alternatives) is projected onto the new active mask. Weights that were removed have their optimizer buffers zeroed; weights that were regrown are initialized with warm-started statistics from structurally similar surviving weights.
- **Refresh-aware ramping:** the learning rate is temporarily reduced after each topology refresh event, then ramped back up. This prevents the model from making large updates based on stale optimizer state immediately after a mask change.

Together, these components enable CHT to train under strict N:M constraints with topology evolution — moving beyond static sparse masks or dense-to-sparse pruning toward true dynamic sparse-to-sparse training.

---

## Key Takeaways

- Scaling CHT means solving more than accuracy — it means addressing computational cost, topology update cost, optimizer behavior, memory, hardware format, and broad capability retention.
- The CHT family evolves because each scale adds a new constraint.
- CHTsL demonstrates that combining efficiency methods requires measuring interactions.

**Previous:** [The CHT Learning Loop](04-the-cht-learning-loop.md)  
**Next:** [Hardware-Friendly N:M Sparsity](06-hardware-friendly-nm-sparsity.md)
