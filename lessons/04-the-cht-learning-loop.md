# The CHT Learning Loop

**Learning objective:** Understand the four-step cycle at the heart of Cannistraci-Hebb Training.

---

## The Loop in One Paragraph

CHT trains a sparse neural network by repeatedly: (1) updating active weights with gradient-based optimization, (2) removing selected connections, (3) scoring candidate missing links using a Cannistraci-Hebb link-prediction rule, and (4) regrowing the most promising candidates. The network stays sparse throughout, but its topology evolves. Over time, the model searches for a better sparse structure while ordinary gradient learning tunes the active weights.

```
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

---

## Step 1: Initialize Sparse Topology

The model begins with a sparse set of active connections. The choice of initial topology matters — a poor starting mask can trap the model in suboptimal structure for the entire training run.

**Random sparse initialization** is the simplest baseline: for each layer, randomly select a fixed fraction of connections to be active. While simple, it gives the model no structural guidance.

**Erdős–Rényi (ER) initialization** scales the per-layer sparsity based on layer size. For a layer with $n_{\text{in}}$ inputs and $n_{\text{out}}$ outputs, the connection probability is proportional to $\frac{n_{\text{in}} + n_{\text{out}}}{n_{\text{in}} \cdot n_{\text{out}}}$. This allocates more connections to smaller layers and fewer to larger ones, reflecting the intuition that smaller layers need higher relative connectivity to pass information.

**Bipartite Receptive Field (BRF) initialization**, introduced in the **[Brain Network Science](https://arxiv.org/abs/2501.19107)** paper (2025), goes further. Inspired by the organization of biological neural circuits, BRF arranges neurons into overlapping receptive fields within a bipartite graph structure. Each output neuron connects to a localized, structured subset of input neurons rather than a random subset. Empirically, BRF provides a better starting topology than both random and ER initialization, especially at high sparsity levels.

The key insight: sparse training does not begin from nothing. The initial graph defines the model's first search space. A well-chosen initialization gives CHT a head start — the topology updates refine an already-reasonable structure rather than rescuing a chaotic one.

---

## Step 2: Train Active Weights

Once the sparse mask exists, the model trains the active weights using normal gradient-based optimization.

This is important: CHT is not replacing all of deep learning. It is adding topology evolution *on top of* weight learning. The active connections carry values updated by backpropagation. The inactive connections are absent from the computation.

In true sparse training, there is no hidden dense master weight matrix tracking all missing links. This distinction becomes critical when we discuss N:M hardware-aware sparsity in later lessons.

---

## Step 3: Remove Links

At scheduled intervals, the method removes some existing connections.

Removal can be based on weight magnitude, activation patterns, topology criteria, or hybrid strategies depending on the variant and architecture. The goal is not merely to delete weak weights—it is to free sparse budget for better future links.

Think of removal as exploration pressure. If the topology never changes, the model may be trapped by its starting mask.

---

## Step 4: Regrow Links — The CHT Distinction

Regrowth is where CHT becomes CHT.

Instead of randomly selecting missing links or computing dense gradients for all absent connections, CHT scores candidate links using Cannistraci-Hebb network-science logic. The original CHT uses path-based CH link prediction. Later variants adjust the scoring and sampling to improve scalability and avoid brittle choices.

**The Cannistraci-Hebb link prediction formula** scores a candidate connection between two unlinked nodes $i$ and $j$ based on their shared neighborhood structure:

$$\text{CH}(i, j) = \sum_{k \in \Gamma(i) \cap \Gamma(j)} \frac{1 + \text{CN}(i, k)}{1 + \text{CN}(j, k)}$$

where $\Gamma(i)$ is the set of neighbors of node $i$, and $\text{CN}(i, k)$ is the number of common neighbors between $i$ and $k$.

**Intuition:** The formula asks: *how strongly are $i$ and $j$ embedded in the same local community?* It does not simply count their shared neighbors (that would be standard Common Neighbors). Instead, it weights each shared neighbor $k$ by how asymmetrically $k$ is connected to $i$ versus $j$. If $k$ shares many neighbors with $i$ but few with $j$, then $k$ is a stronger signal that $i$ and $j$ belong in the same local cluster — and the missing link between them is structurally promising.

In the neural network context:

- Nodes are neurons.
- Existing connections form the current sparse graph.
- Missing connections are scored by CH link prediction.
- The highest-scoring candidates are regrown, guided purely by topology — not by gradient information.

The regrowth step asks a specific question:

> Which missing connection is structurally promising given the current graph?

This is the topology-learning heart of the method.

---

## Step 5: Repeat Under a Sparse Budget

The model repeats the loop many times.

The sparse budget can remain fixed, or density can decay over training. In **CHTss**, gradual density decay is used so the model does not begin training under the final harsh sparsity condition immediately. Early training is noisy, and the network may need more structure before it can confidently become sparse.

---

## CHT, CHTs, and CHTss — The Evolution

### CHT (Original)

The original CHT uses:
- **Hard top-k** for both removal and regrowth: exactly the $k$ weakest links are removed, and exactly the $k$ highest-scoring candidates are regrown, with no randomness.
- **Path-based CH link prediction** that considers not just direct neighbors but also 2-hop and 3-hop paths between candidate nodes. This is powerful but computationally expensive — the time complexity is $O(N d^3)$ where $N$ is network size and $d$ is average degree.

These choices work well for small networks but create two problems at scale:
1. Hard top-k can get stuck: the same links may be repeatedly removed and regrown because there is no exploration randomness.
2. Path-based scoring becomes prohibitively expensive for large layers.

### CHTs (Soft)

CHTs addresses both problems:

- **Soft sampling:** instead of taking the exact top-k, CHTs samples links for removal and regrowth from probability distributions derived from their scores. A link with a slightly lower CH score still has a chance of being regrown. This introduces controlled randomness that prevents the topology from cycling through the same few configurations.
- **Matrix-based approximation:** the CH link predictor is reformulated as matrix operations, reducing the time complexity from $O(N d^3)$ to $O(N^3)$. This is still cubic, but it is GPU-friendly and scales to layers with thousands of neurons.

The trade-off: CHTs sacrifices the guarantee of always picking the absolute highest-scoring links in exchange for better exploration and scalability. In practice, the exploration benefit outweighs the precision loss.

### CHTss (Soft + Schedule)

CHTss adds a **gradual density decay schedule** on top of CHTs.

Instead of training at the target sparsity (e.g., 99%) from the first epoch, CHTss starts at a higher density (e.g., 50% connectivity) and gradually reduces it over training. The density follows a schedule — often sigmoidal or cosine — that reaches the target sparsity only in the final phase of training.

Why this helps:
- Early training is noisy; the model benefits from extra connectivity while learning basic features.
- Gradual decay lets the model *choose* which connections to keep rather than having them forcibly removed upfront.
- By the time sparsity becomes extreme, the remaining topology is already well-structured.

In summary:
- **CHT:** hard top-k + path-based scoring. Shows topology learning works, but doesn't scale.
- **CHTs:** soft sampling + matrix approximation. Scales to larger networks with better exploration.
- **CHTss:** CHTs + gradual density decay. The most forgiving schedule — the model eases into sparsity rather than being thrown into it.

---

## What Can Go Wrong

CHT can underperform if:

- The initial topology is poor.
- The removal rule deletes useful structure too aggressively.
- Regrowth becomes too deterministic and gets stuck.
- Topology updates are too expensive computationally.
- The sparse pattern is not hardware-friendly.

---

## Key Takeaways

- CHT is a loop, not a magic mask.
- Weights learn through gradients. Topology learns through remove-and-regrow updates guided by network science.
- Good CHT engineering is about the interaction between initialization, removal, regrowth, sparsity schedule, optimizer, and evaluation.

**Previous:** [Network Shape Intelligence](03-network-shape-intelligence.md)  
**Next:** [Scaling CHT to Transformers and LLMs](05-scaling-cht-to-transformers-and-llms.md)
