# Sparsity Is Not Just Compression

**Learning objective:** Understand the major families of sparsity and where CHT fits among them.

---

## One Word, Many Meanings

"Sparsity" sounds like a single idea, but in practice it refers to several distinct mechanisms. Different papers, engineers, and practitioners may use the same word while pointing to entirely different approaches. This lesson maps the terrain.

---

## Post-Training Pruning

Post-training pruning starts with a trained dense model and removes weights afterward.

The **[Plug-and-Play](https://openreview.net/forum?id=Tr0lPx9woF)** paper (ICLR 2024) is a representative example. It focuses on large language models and proposes improved importance scoring and channel permutation for N:M semi-structured pruning. The core idea is not to train sparse from scratch but to take an existing dense model and make it sparse with minimal quality loss.

This path is attractive because organizations already have trained dense models. They want a way to reduce cost without retraining from zero.

The risk is that pruning can damage hidden capabilities. A model may look fine on perplexity while degrading on Chinese understanding, code generation, mathematical reasoning, factual recall, long-context behavior, or instruction following.

---

## Static Sparse Training

Static sparse training begins with a sparse network and keeps the same sparse mask throughout training.

This reduces parameters from the start, but it asks the initial mask to be good enough. If the wrong connections are missing, the model may never recover. Static sparsity is simple, but it is rarely sufficient as a standalone strategy.

---

## Dynamic Sparse Training

Dynamic sparse training keeps the model sparse but changes which connections exist during training. This is the family CHT belongs to.

Most dynamic sparse training methods follow a prune-and-regrow loop:

1. Train active weights.
2. Remove some weak or low-importance connections.
3. Regrow the same number of new connections.
4. Continue training.

Different methods mainly differ in how they choose which new links to add.

Gradient-based approaches (such as RigL) use gradient signals to score missing links. CHT takes a different approach: it uses network topology and link-prediction rules derived from network science. This distinction matters because computing dense gradients for all absent links can be expensive and may undermine the goal of true sparse training.

---

## CHT: Topology-Guided Dynamic Sparse Training

CHT is a dynamic sparse training method with a specific identity:

- It treats the network as a graph.
- It uses network-science link prediction to guide regrowth.
- It is grounded in Cannistraci-Hebb theory and epitopological learning.
- It aims to learn sparse structure, not just sparse weights.

The key phrase is **network shape**. CHT assumes that the current pattern of connections contains information about which missing connections are likely to become useful. This is why CHT is fundamentally a topology-learning method.

---

## Unstructured Sparsity

Unstructured sparsity means any weight can be zero, with no pattern constraints.

This gives the algorithm maximum freedom to learn whatever sparse pattern seems useful. Many early CHT results operate in this regime.

The problem is hardware. Modern GPUs excel at dense matrix multiplication. If zeros appear in arbitrary positions, the hardware may spend extra effort tracking sparse indices and may not deliver real speedups. Sometimes the sparse model has fewer theoretical FLOPs but runs slower.

This is one of the most important practical lessons: fewer parameters do not automatically mean faster deployment.

---

## Semi-Structured N:M Sparsity

N:M sparsity adds a local constraint. In a group of M consecutive weights, exactly N are nonzero (or exactly N are zero, depending on convention). The common examples are:

- **2:4 sparsity:** two active weights in every group of four, yielding 50% local sparsity.
- **1:4 sparsity:** one active weight in every group of four, yielding 75% local sparsity (a more aggressive future target).

The exact convention must be checked in each paper, but the practical meaning is stable: the sparse pattern is constrained so that hardware can exploit it efficiently.

The **[CHTs24](https://openreview.net/forum?id=oo7G9mheNo)** (CPAL 2026) and **[CHTsNM](https://www.preprints.org/manuscript/202605.0851)** (2026) papers are important because they move CHT toward hardware-friendly sparsity. They ask: can topology-guided sparse training work inside rigid N:M constraints?

---

## Low-Rank and Spectral Sparsity

Low-rank training represents a weight matrix through smaller factors rather than selecting individual connections.

**[Sparse Spectral Training](https://arxiv.org/abs/2405.15481)** (ICML 2025) explores low-rank and spectral approaches. **[CHTsL](https://openreview.net/forum?id=jZplmg7Ad9)** (ICLR 2026) combines dynamic connectivity sparsity with low-rank spectral sparsity and adds an alignment loss to reduce conflict between the two branches.

This is a major insight: structural efficiency is not one trick. Connectivity sparsity, low-rank methods, semi-structured sparsity, and pruning can be combined—but combinations create interaction effects that must be carefully measured.

---

## The CHT Vocabulary

When studying this field, use these precise distinctions:

| Term | Meaning |
|---|---|
| **CHT** | Topology-guided dynamic sparse training (the foundational method). |
| **CHTs** | A soft-rule scalable variant that improves exploration and avoids brittle top-k regrowth. |
| **CHTss** | CHTs plus gradual density decay for more stable sparse training. |
| **CHTsL** | CHTs combined with low-rank spectral sparsity and alignment loss. |
| **CHTs24** | CHT adapted to 2:4 semi-structured sparsity. |
| **eDSrT** | Dense-to-sparse retraining pipeline using epitopological principles. |
| **CHTsNM** | N:M sparse-to-sparse training framework for 2:4 and future 1:4 directions. |
| **TANS** | Topology-aware Newton-Schulz optimizer concept used in the CHTsNM direction. |

---

## Key Takeaways

- Sparsity is a design space, not a single method.
- CHT belongs to dynamic sparse training, but its differentiator is topology-guided regrowth.
- Hardware-friendly sparsity requires constraints. Those constraints make the algorithm harder but the deployment path more realistic.

**Previous:** [Why Sparsity Matters](01-why-sparsity-matters.md)  
**Next:** [Network Shape Intelligence](03-network-shape-intelligence.md)
