# Hardware-Friendly N:M Sparsity

Core question: when do sparse models become real acceleration?

## The Sparse Hardware Trap

The most common mistake in sparse AI is to assume that fewer nonzero weights automatically means faster training or inference.

That is often false.

Modern GPUs are built around highly optimized dense matrix multiplication. Dense operations are predictable, regular, and heavily tuned. Arbitrary sparse operations can require index lookup, irregular memory access, load imbalance, and custom kernels. The result can be embarrassing: a sparse model with fewer theoretical FLOPs that runs slower than the dense model.

This is why SparseMind must treat hardware as part of the algorithm, not as an afterthought.

## Why N:M Exists

N:M sparsity is a compromise between freedom and hardware regularity.

Instead of allowing zeros anywhere, N:M enforces a local pattern inside small groups of weights. The most familiar current format is 2:4 semi-structured sparsity on NVIDIA hardware. The future-facing research direction includes 1:4 sparsity for higher local sparsity.

The engineering logic is:

- unstructured sparsity gives algorithms maximum freedom but weak hardware predictability;
- dense computation gives hardware maximum predictability but wastes work;
- N:M sparsity gives a constrained sparse pattern that hardware can exploit.

The constraint makes topology learning harder. It also makes deployment more real.

## CHTs24

The CHT with N:M Semi-Structured Sparsity paper introduces CHTs24.

Its key contribution is to combine Cannistraci-Hebb topology learning with 2:4 semi-structured sparsity. It also introduces eDSrT, a dense-to-sparse retraining pipeline.

Why this matters:

Many N:M training methods use straight-through estimators and keep dense master weights. That can preserve accuracy, but it weakens the memory and training-efficiency story because the model is not truly sparse during the full backward/update process.

CHTs24 aims for true sparse training inside the 2:4 constraint. It does not merely put a sparse mask on a dense training process.

For a new hire, the central distinction is:

Sparse forward pass is not the same as sparse training.

## Transposable 2:4

One subtle hardware issue is whether sparsity survives transposition.

A row-wise 2:4 pattern may help a forward matrix multiplication, but the backward pass often uses transposed matrices. If the transposed matrix no longer satisfies the required pattern, acceleration may disappear.

That is why the papers discuss stricter or transposable 2:4 structures. The aim is to accelerate both forward and backward paths, not just one convenient operation.

This is a good example of how paper-level sparsity must be translated into systems-level constraints.

## CHTsNM And 1:4

The CHTsNM paper pushes toward 1:4 semi-structured sparsity, framed as a high-sparsity direction for future accelerators.

2:4 is already meaningful because it has current hardware support. 1:4 is more aggressive because it keeps fewer active weights per local group. The appeal is larger potential efficiency. The challenge is quality retention, optimizer stability, and limited current software/hardware support.

The local CHTsNM paper is careful: it reports that 2:4 is close to dense on most tasks and can show sparse-over-dense gains on some tasks, while 1:4 approaches dense performance but does not consistently surpass it. That caution should be preserved in product language.

## Sparser, Faster, Lighter

The Sparser, Faster, Lighter Transformer paper is not a CHT paper, but it is useful for hardware thinking.

It shows that sparse acceleration often needs custom packing formats and kernels. It focuses on unstructured feed-forward activation sparsity and reports that dedicated GPU kernels can turn high sparsity into throughput, memory, and energy benefits.

The lesson for SparseMind is not "copy this method." The lesson is:

Sparse algorithm wins become product wins only when the execution path is designed for sparsity.

## What To Measure

For N:M work, always measure:

- dense baseline quality;
- sparse model quality;
- parameter sparsity in sparse-able layers;
- total model sparsity;
- training memory;
- optimizer-state memory;
- forward speed;
- backward speed;
- end-to-end tokens per second;
- latency and TTFT for inference;
- energy if available;
- kernel support and fallback paths;
- deployment stability.

Do not stop at theoretical FLOPs.

## A Simple Product Translation

For customers, the value is not "we have zeros."

The value is:

We can train or convert models into sparse structures that preserve quality while reducing real compute, memory, or energy on target hardware.

That sentence has three proof requirements:

1. Quality preserved.
2. Efficiency measured.
3. Target hardware identified.

## What To Remember

N:M sparsity is the bridge from elegant sparsity to deployable sparsity.

CHTs24 and CHTsNM matter because they force CHT to obey hardware-friendly structure.

The goal is not only sparse weights. The goal is sparse weights that run.

Previous: [05-scaling-cht-to-transformers-and-llms.md](05-scaling-cht-to-transformers-and-llms.md)  
Next: [07-retraining-distillation-and-recovery.md](07-retraining-distillation-and-recovery.md)

