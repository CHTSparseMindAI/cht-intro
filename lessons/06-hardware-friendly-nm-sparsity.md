# Hardware-Friendly N:M Sparsity

**Learning objective:** Understand when and how sparse models translate to real computational speedup on modern hardware.

---

## The Sparse Hardware Trap

The most common mistake in sparse AI is assuming that fewer nonzero weights automatically means faster training or inference. That is often false.

Modern GPUs are built around highly optimized dense matrix multiplication. Dense operations are predictable, regular, and heavily tuned. Arbitrary sparse operations can require index lookup, irregular memory access, load imbalance, and custom kernels. The result can be surprising: a sparse model with fewer theoretical FLOPs that runs slower than its dense counterpart.

This is why hardware must be treated as part of the algorithm, not as an afterthought.

---

## Why N:M Exists

N:M sparsity is a compromise between algorithmic freedom and hardware regularity.

Instead of allowing zeros anywhere (unstructured sparsity), N:M enforces a local pattern inside small groups of weights. The most familiar current format is **2:4 semi-structured sparsity** supported on NVIDIA hardware. The future-facing research direction includes **1:4 sparsity** for higher local sparsity.

The engineering logic:

- **Unstructured sparsity** gives algorithms maximum freedom but weak hardware predictability.
- **Dense computation** gives hardware maximum predictability but wastes work.
- **N:M sparsity** gives a constrained sparse pattern that hardware can exploit.

The constraint makes topology learning harder. It also makes deployment more real.

---

## CHTs24: True Sparse Training Under 2:4

The **[CHTs24](https://openreview.net/forum?id=oo7G9mheNo)** paper (CPAL 2026) introduces CHT adapted to 2:4 semi-structured sparsity. It also introduces **eDSrT**, a dense-to-sparse retraining pipeline.

Why this matters:

Many N:M training methods use straight-through estimators and keep dense master weights behind the scenes. That can preserve accuracy, but it weakens the memory and training-efficiency story because the model is not truly sparse during the full backward and update process.

CHTs24 aims for **true sparse training** inside the 2:4 constraint. It does not merely place a sparse mask on top of a dense training process.

The central distinction:

> Sparse forward pass is not the same as sparse training.

---

## Transposable 2:4

One subtle hardware issue is whether sparsity survives matrix transposition.

A row-wise 2:4 pattern may help a forward matrix multiplication, but the backward pass often uses transposed matrices. If the transposed matrix no longer satisfies the required pattern, acceleration may disappear.

That is why the literature discusses stricter or transposable 2:4 structures. The aim is to accelerate both forward and backward paths, not just one convenient operation.

---

## CHTsNM and 1:4 Sparsity

The **[CHTsNM](https://www.preprints.org/manuscript/202605.0851)** paper (2026) pushes toward 1:4 semi-structured sparsity — a high-sparsity direction for future accelerators.

- **2:4** is already meaningful because it has current hardware support.
- **1:4** is more aggressive, keeping fewer active weights per local group. The appeal is larger potential efficiency. The challenge is quality retention, optimizer stability, and limited current software/hardware support.

The paper is careful in its reporting: 2:4 is close to dense on most tasks and can show sparse-over-dense gains on some tasks, while 1:4 approaches dense performance but does not consistently surpass it.

---

## What to Measure

For N:M work, always measure:

- Dense baseline quality.
- Sparse model quality.
- Parameter sparsity in sparse-able layers.
- Total model sparsity.
- Training memory and optimizer-state memory.
- Forward speed and backward speed.
- End-to-end tokens per second.
- Latency and time-to-first-token for inference.
- Energy consumption if available.
- Kernel support and fallback paths.
- Deployment stability.

Do not stop at theoretical FLOPs. The numbers that matter are the ones measured on real hardware.

---

## The Practical Translation

For engineers, the value proposition is not "the model has zeros." It is:

> We can train or convert models into sparse structures that preserve quality while reducing real compute, memory, or energy on target hardware.

That sentence has three proof requirements:

1. Quality preserved.
2. Efficiency measured.
3. Target hardware identified.

---

## Key Takeaways

- N:M sparsity is the bridge from elegant theory to deployable efficiency.
- CHTs24 and CHTsNM matter because they force CHT to obey hardware-friendly structure.
- The goal is not only sparse weights. The goal is sparse weights that run on sparse hardware.

**Previous:** [Scaling CHT to Transformers and LLMs](05-scaling-cht-to-transformers-and-llms.md)  
**Next:** [Retraining, Distillation, and Recovery](07-retraining-distillation-and-recovery.md)
