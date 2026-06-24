# Scaling CHT To Transformers And LLMs

Read time: 11 minutes  
Core question: what changes when CHT moves from smaller networks to Transformers and LLMs?

## The Scale Problem

It is one thing to show sparse topology learning on MLPs or CNNs. It is another thing to make it work for Transformers and LLMs.

LLMs create several extra pressures:

- weight matrices are huge;
- training is memory-intensive;
- small quality regressions matter;
- attention, MLP blocks, embeddings, and output heads behave differently;
- dense baselines are already strong;
- sparse operations may not accelerate unless the pattern fits hardware;
- evaluation must include broad capability, not only one loss curve.

The CHT family evolves because these pressures force it to evolve.

## The NeurIPS CHT Scaling Result

The Brain Network Science paper is the key bridge from original CHT to scalable CHT.

It introduces several improvements:

- BRF initialization for sparse neural networks;
- GPU-friendly approximation of the CH link predictor;
- CHTs, a soft rule for link removal and regrowth;
- CHTss, which adds gradual density decay.

The paper reports strong results across MLPs, machine-translation Transformers, and LLaMA-family language modeling experiments. The lesson is not simply that sparse always beats dense. The lesson is that topology-guided sparse training can survive contact with larger architectures when the method becomes softer, more scalable, and schedule-aware.

## Why Transformers Are Different

Transformers contain multiple types of computation:

- attention projections;
- feed-forward blocks;
- residual connections;
- normalization;
- embeddings;
- output projection.

Not every component should be sparsified the same way. Many SparseMind discussions focus on linear layers inside Transformer blocks, especially attention and MLP matrices, because those dominate parameters and compute.

The CHT papers often distinguish sparse-able parameters from dense parameters. This matters for reporting. A model may have 50% sparsity in sparse-able layers but less total model sparsity once embeddings, layer norms, or other dense components remain.

New hires should always ask:

Where is sparsity applied?

## CHTsL: Connectivity Plus Spectral Sparsity

The Alignment-Enhanced Integration paper introduces CHTsL, which combines CHTs with low-rank training.

The motivation is straightforward: connectivity sparsity and low-rank factorization are two different forms of structural efficiency. A sparse branch can capture selected direct connections. A low-rank branch can capture broad spectral structure.

But naive combination can create cancellation: the sparse branch and low-rank branch may disagree and reduce each other's usefulness. The paper introduces an alignment loss to make the branches cooperate.

This is a general lesson for SparseMind engineering:

Combining efficiency methods is powerful, but interaction effects are real.

Do not assume CHT plus another method automatically improves results. Measure branch conflict, convergence, memory, speed, and quality.

## CHTsNM And TANS

The CHTsNM direction pushes CHT toward N:M semi-structured sparse-to-sparse training. It introduces a topology-aware Newton-Schulz optimizer idea, TANS, plus mechanisms such as active-mask projection, active-support RMS matching, refresh-aware ramping, contextual low-rank compensation, and motif pattern revisitation.

You do not need to master every component on first read. The high-level idea is:

When topology changes under strict N:M constraints, the optimizer must respect the active sparse support.

That means ordinary dense optimizer assumptions may not hold cleanly. Sparse topology refreshes can disturb training dynamics. TANS is one attempt to make optimizer behavior aware of active sparse structure.

The Edge of Sparse Stability proposal extends this by asking whether training-dynamics phenomena such as Edge of Stability can help explain and control CHTsNM behavior.

## Why We Talk About 60M, 130M, 1B, And Beyond

The local papers include LLaMA-60M, LLaMA-130M, and larger-direction discussions. These small-to-mid LLMs are not the final product scale, but they are useful stepping stones.

They let the team test:

- whether CHT variants train stably;
- whether sparse quality tracks dense quality;
- whether topology updates create convergence penalties;
- whether N:M constraints are too restrictive;
- whether training recipes scale before spending massive compute.

A new hire should not dismiss 60M or 130M experiments as too small. They are diagnostic instruments. But they also should not overgeneralize them to frontier-scale LLMs without further evidence.

## The Safe Scaling Claim

A safe internal claim is:

The CHT research line has evidence that topology-guided dynamic sparse training can scale from smaller models to Transformer and LLM-style settings, with several variants designed to address exploration, density schedules, low-rank interaction, and hardware-aware N:M constraints.

An unsafe claim is:

CHT is already guaranteed to beat dense LLMs at industrial scale.

SparseMind should pursue the first claim until experiments prove the second in a specific setting.

## What To Remember

Scaling CHT means solving more than accuracy. It means solving computational cost, topology update cost, optimizer behavior, memory, hardware format, and broad capability retention.

The CHT family evolves because each scale adds a new constraint.

## New-Hire Exercise

Pick one CHT variant: CHTs, CHTss, CHTsL, CHTs24, or CHTsNM.

Write:

- what problem it solves;
- what new risk it introduces;
- what metric would prove it works.

Previous: [04-the-cht-learning-loop.md](04-the-cht-learning-loop.md)  
Next: [06-hardware-friendly-nm-sparsity.md](06-hardware-friendly-nm-sparsity.md)

