# The CHT Learning Loop

Core question: what actually happens during CHT training?

## The Loop In One Paragraph

CHT trains a sparse neural network by repeatedly updating active weights, removing selected links, and regrowing new links using a Cannistraci-Hebb link-prediction rule. The network stays sparse, but its sparse topology evolves. Over time, the model searches for a better sparse structure while ordinary gradient learning tunes the active weights.

If you remember only one diagram, remember this:

```text
initialize sparse topology
        |
train active weights
        |
remove selected links
        |
score missing links using CH rule
        |
regrow promising links
        |
repeat
```

## Step 1: Initialize Sparse Topology

The model begins with a sparse set of active connections.

In early sparse training methods, initialization is often random or based on simple heuristics. CHT-related papers put more attention on initialization because starting topology affects what the model can discover.

The Brain Network Science paper introduces bipartite receptive field (BRF) initialization for sparse artificial neural networks. The point is to give the sparse model a better starting structure than naive random connectivity.

For new hires, the key idea is:

Sparse training does not begin from nothing. The initial graph gives the model its first search space.

## Step 2: Train Active Weights

Once the sparse mask exists, the model trains the active weights with normal gradient-based optimization.

This is important: CHT is not replacing all of deep learning. It is adding topology evolution on top of weight learning.

The active connections carry values. Those values are updated by backpropagation. The inactive connections are absent from the computation, depending on implementation.

In true sparse training, we do not secretly keep and update a dense master weight matrix for all missing links. That distinction becomes central in the N:M lessons.

## Step 3: Remove Links

At scheduled intervals, the method removes some existing connections.

Removal can be based on weight magnitude, activation, topology, or hybrid criteria depending on the variant and architecture. The goal is not just to delete weak weights. The goal is to free sparse budget for better future links.

Think of removal as exploration pressure. If the topology never changes, the model may be trapped by its starting mask.

## Step 4: Regrow Links

Regrowth is where CHT becomes CHT.

Instead of randomly selecting missing links or computing dense gradients for all absent links, CHT scores candidate links using Cannistraci-Hebb network-science logic.

The original CHT uses path-based CH link prediction. Later variants adjust the scoring and sampling to improve scalability and avoid brittle choices.

The regrowth step asks:

Which missing connection is structurally promising given the current graph?

This is the topology-learning heart of the method.

## Step 5: Repeat Under A Sparse Budget

The model repeats the loop many times.

The sparse budget can remain fixed, or density can decay over training. In CHTss, gradual density decay is used so the model does not begin training under the final harsh sparsity condition immediately. This is intuitive: early training is noisy, and the network may need more structure before it can confidently become sparse.

The mature CHT family is therefore not a single fixed recipe. It is a training style with variants for different constraints:

- ultra-sparse MLP, CNN, and Transformer settings;
- LLM pretraining and language modeling;
- low-rank plus sparse combinations;
- 2:4 semi-structured sparse training;
- dense-to-sparse retraining;
- SNN and edge settings.

## CHT, CHTs, And CHTss

The original CHT result is the foundation.

CHTs addresses two practical problems:

- hard top-k regrowth can get stuck in local topology choices;
- path-based link prediction can be expensive at larger scale.

CHTs uses softer sampling and more scalable approximations so the network can explore better.

CHTss adds a gradual density decay schedule. Instead of forcing the model to live at the final sparsity from the first step, CHTss lets the model become sparse over time.

In plain English:

- CHT shows topology learning works.
- CHTs makes it softer and more scalable.
- CHTss makes the sparse training schedule more forgiving.

## What Can Go Wrong

CHT can fail or underperform if:

- the initial topology is poor;
- the removal rule deletes useful structure too aggressively;
- regrowth becomes too deterministic and gets stuck;
- topology updates are too expensive;
- the sparse pattern is not hardware-friendly;
- evaluation only checks one metric and misses capability drift;
- the model uses fewer parameters but does not run faster.

These are not reasons to reject CHT. They are reasons to engineer it carefully.

## What To Remember

CHT is a loop, not a magic mask.

Weights learn through gradients. Topology learns through remove-and-regrow updates guided by network science.

Good CHT engineering is about the interaction between initialization, removal, regrowth, sparsity schedule, optimizer, and evaluation.

Previous: [03-network-shape-intelligence.md](03-network-shape-intelligence.md)  
Next: [05-scaling-cht-to-transformers-and-llms.md](05-scaling-cht-to-transformers-and-llms.md)

