# SNN, Edge, and Energy

**Learning objective:** Understand how structural sparsity from CHT complements temporal sparsity in spiking neural networks for edge deployment.

---

## Two Kinds of Sparsity

Spiking neural networks (SNNs) are attractive because they introduce **temporal sparsity**: a spiking neuron is active only when it fires. In principle, this can dramatically reduce energy consumption on neuromorphic hardware.

CHT introduces **structural sparsity**: it changes which connections exist in the first place.

The natural question:

> What happens when temporal sparsity and structural sparsity work together?

---

## CH-SNN

The **[Cannistraci-Hebb Training on Ultra-Sparse Spiking Neural Networks](https://arxiv.org/abs/2511.05581)** paper (ICLR 2026) introduces CH-SNN. It combines:

- Sparse spike-correlated topological initialization.
- Sparse spike weight initialization.
- Hybrid link removal.
- CH3-L3-inspired regrowth for potential synaptic links.

The reported results are strong: very high structural sparsity, comparable or improved accuracy in tested settings, fewer synaptic operations, and lower energy consumption on edge neuromorphic deployment experiments.

The practical interpretation: CHT can be part of an edge-efficiency strategy because it learns sparse structure that complements the natural event-driven sparsity of SNNs.

---

## ANN-to-SNN Conversion

Directly training SNNs can be difficult because spiking neurons are non-differentiable. A common alternative is ANN-to-SNN conversion:

1. Train an artificial neural network (ANN).
2. Convert it into a spiking neural network (SNN).
3. Preserve as much accuracy as possible while gaining energy advantages.

The **[Conversion of Sparse ANN to Sparse SNN](https://openreview.net/forum?id=lZrZgZ9wIu)** paper (ICLR 2026) argues that most conversion work starts from dense ANNs. It proposes using CHT-trained sparse ANNs so that the converted SNN inherits brain-like sparse topology from the start.

This gives CHT an additional role: shaping the source ANN before conversion so the SNN does not inherit unnecessary dense structure.

---

## Why Topology Matters Beyond Branding

The SNN literature discusses properties such as small-worldness, meta-depth, and brain-like sparse topology. Do not treat these as decorative language. They describe measurable graph structure. If the sparse topology has meaningful organization, it may support performance while reducing connections.

However, "brain-inspired" does not automatically mean better. Every claim must be tied to measured results: accuracy, energy, operations, latency, and robustness on target hardware.

---

## Edge Deployment Considerations

Edge intelligence faces constraints that data-center deployments do not always feel:

- Limited memory and power budgets.
- Limited heat dissipation.
- Real-time latency requirements.
- Device-specific hardware characteristics.
- Reliability in the field.

SNN and sparse topology methods can matter here because energy and operations are not academic metrics — they determine whether deployment is possible at all.

CHT's edge applications span domains including robotics, sensors, embedded devices, scientific instruments, medical edge analysis, and always-on low-power inference. But the proof must be device-specific. A theoretical operation reduction is not the same as measured energy savings on the target platform.

---

## Relation to LLM Work

SNN work may appear separate from LLM sparse training, but they share a root idea:

> Useful intelligence may not require dense all-to-all computation.

For LLMs, the pressure is data-center cost, memory, and throughput. For SNNs and edge models, the pressure is energy, latency, and deployability. CHT is relevant to both because it learns sparse connection structure — a principle that applies across scales.

---

## Key Takeaways

- SNNs bring temporal sparsity. CHT brings structural sparsity.
- Together, they support an energy-efficiency path for edge and neuromorphic deployment.
- The story is not "brain-inspired." It is a measured accuracy-energy trade-off validated on real hardware.

**Previous:** [Retraining, Distillation, and Recovery](07-retraining-distillation-and-recovery.md)  
**Next:** [Evaluation and Engineering Proof](09-evaluation-product-proof.md)
