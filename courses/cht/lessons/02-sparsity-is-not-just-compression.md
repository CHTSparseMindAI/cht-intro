# Sparsity Is Not Just Compression

Read time: 9 minutes  
Core question: what kind of sparsity are we talking about?

## One Word, Many Meanings

"Sparsity" sounds like one idea, but in practice it means several different things. New hires can get confused because papers, engineers, and customers may all use the same word while pointing to different mechanisms.

Here is the useful map.

## Post-Training Pruning

Post-training pruning starts with a trained dense model and removes weights afterward.

The Plug-and-Play pruning paper in the local source folder is a good example. It focuses on large language models and proposes better importance scoring and channel permutation for N:M semi-structured pruning. The core idea is not to train sparse from scratch. It is to take an existing dense model and make it sparse with minimal quality loss.

This path is attractive because companies already have dense models. They want a way to reduce cost without retraining from zero.

The risk is that pruning can damage hidden capabilities. A model may look fine on perplexity while drifting on Chinese understanding, code, math, factual recall, long-context behavior, or instruction following.

## Static Sparse Training

Static sparse training begins with a sparse network and keeps the same sparse mask during training.

This reduces parameters, but it asks the initial mask to be good enough. If the wrong connections are missing, the model may never recover.

Static sparsity is simple. It is rarely the full answer for CHT.

## Dynamic Sparse Training

Dynamic sparse training keeps the model sparse but changes which connections exist during training.

This is closer to the CHT world. The model is not only learning weights. It is also changing topology.

Most dynamic sparse training methods follow a prune-and-regrow loop:

1. train active weights;
2. remove some weak or low-importance connections;
3. regrow the same number of new connections;
4. continue training.

Different methods mainly differ in how they choose the new links.

RigL-style approaches use gradient signals for missing links. CHT-style approaches use network topology and link prediction rules. This distinction matters because dense gradient search can be expensive and may undermine the goal of true sparse training.

## CHT: Topology-Guided Dynamic Sparse Training

CHT is a dynamic sparse training method, but its identity is more specific:

- it treats the network as a graph;
- it uses network-science link prediction to guide regrowth;
- it is inspired by Cannistraci-Hebb theory and epitopological learning;
- it aims to learn sparse structure, not only sparse weights.

The important phrase is **network shape**. CHT assumes that the current pattern of connections contains information about which missing connections are likely to become useful.

That is why CHT is a topology-learning method.

## Unstructured Sparsity

Unstructured sparsity means any weight can be zero.

This gives the algorithm freedom. It can learn whatever sparse pattern seems useful. Many early CHT results live here.

The problem is hardware. Modern GPUs are extremely good at dense matrix multiplication. If zeros appear in arbitrary places, the hardware may spend extra effort tracking sparse indices and may not deliver real speedups. Sometimes the sparse model has fewer theoretical FLOPs but runs slower.

This is one of the most important product lessons: fewer parameters do not automatically mean faster deployment.

## Semi-Structured N:M Sparsity

N:M sparsity adds a local rule. In a group of M weights, only N are active or only N are zero depending on notation. In SparseMind's current hardware discussion, the common examples are:

- 2:4 sparsity, often meaning two active weights in every four or two zeros in every four depending on the paper's convention, with 50% local sparsity;
- 1:4 sparsity, with one active weight in every four and 75% local sparsity in the high-sparsity target framing.

The exact convention must be checked in each paper, but the business meaning is stable: the sparse pattern is constrained so hardware can exploit it.

The CHTs24 and CHTsNM papers are important because they move CHT toward hardware-friendly sparsity. They ask: can topology-guided sparse training work inside rigid N:M constraints?

## Low-Rank And Spectral Sparsity

Low-rank training is another efficiency family. Instead of keeping only selected connections, it represents a matrix through smaller factors.

Sparse Spectral Training and CHTsL are relevant here. CHTsL combines dynamic connectivity sparsity with low-rank spectral sparsity and adds alignment to reduce conflict between branches.

This is a major lesson for new hires: structural efficiency is not one trick. Connectivity sparsity, low-rank methods, semi-structured sparsity, and pruning can be combined, but combinations create interaction problems that must be measured.

## Activation Sparsity And Sparse Attention

Some sparsity happens in activations rather than weights. The Sparser, Faster, Lighter Transformer paper focuses on feed-forward activation sparsity and kernels. The Sub-quadratic Sparse Attention note focuses on sparse attention for long context, although the local note says the official paper and code were not released at the time of that note.

These are not CHT papers, but they help new hires understand the wider sparse AI world.

## The SparseMind Vocabulary

When talking internally, use these distinctions:

- **CHT:** topology-guided dynamic sparse training.
- **CHTs:** a soft-rule scalable variant that improves exploration and avoids brittle hard top-k regrowth.
- **CHTss:** CHTs plus gradual density decay for stronger sparse training.
- **CHTsL:** CHTs combined with low-rank spectral sparsity and alignment loss.
- **CHTs24:** CHT adapted to 2:4 semi-structured sparsity.
- **eDSrT:** dense-to-sparse retraining pipeline using epitopological principles.
- **CHTsNM:** N:M sparse-to-sparse training framework for 2:4 and future 1:4 directions.
- **TANS:** topology-aware Newton-Schulz optimizer concept used in the CHTsNM direction.

## What To Remember

Sparsity is a design space, not a single method.

CHT belongs to dynamic sparse training, but its differentiator is topology-guided regrowth.

Hardware-friendly sparsity usually requires constraints. Those constraints make the algorithm harder but the product path more realistic.

## New-Hire Exercise

Make a two-column table:

- left column: pruning, static sparsity, dynamic sparsity, CHT, N:M sparsity, low-rank training;
- right column: when you would use it and what can go wrong.

Previous: [01-why-sparsity-matters.md](01-why-sparsity-matters.md)  
Next: [03-network-shape-intelligence.md](03-network-shape-intelligence.md)

