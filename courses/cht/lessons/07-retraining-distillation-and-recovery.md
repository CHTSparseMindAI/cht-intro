# Retraining, Distillation, And Recovery

Read time: 11 minutes  
Core question: how do we turn dense models into useful sparse models?

## Why Retraining Matters

Training sparse models from scratch is scientifically clean, but the world is full of dense models that already exist.

A customer may ask:

Can you take my trained dense model and make it cheaper?

For SparseMind, that means dense-to-sparse conversion and recovery are product-critical. The relevant question is not only whether CHT can pretrain a sparse model. It is also whether CHT can help transform an existing dense model into a hardware-friendly sparse model without losing the capabilities people care about.

## The Dense-To-Sparse Path

The CHTs24 paper introduces eDSrT, epitopology Dynamic Sparse re-Training.

The idea is to transition a dense model into semi-structured sparse form, then retrain under CHT-guided topology rules. This is different from one-shot pruning. The sparse structure is not merely imposed and frozen. The topology can be adjusted during recovery.

A practical dense-to-sparse path looks like this:

1. Start from a validated dense baseline.
2. Choose sparse target: unstructured, 2:4, 1:4, or another N:M format.
3. Decide which layers are sparse-able.
4. Create the initial sparse mask.
5. Retrain active weights and allow topology updates.
6. Compare against the dense baseline on quality and real efficiency.
7. Iterate on sparsity level, mask constraints, loss mix, optimizer, and schedule.

This is the basic workflow a new hire should expect in CHT recovery work.

## Where Distillation Fits

Distillation uses a teacher model to guide a student model.

For SparseMind, the dense original model can become the teacher, and the sparse CHT model becomes the student. The teacher helps preserve behavior that may not be fully captured by labels alone.

The practical mixed objective often looks like:

```text
loss = CE(real data) + alpha * KL(sparse_logits, dense_teacher_logits)
```

This means the sparse model learns from real data and from the dense teacher's output distribution.

The important word is **mixed**.

Teacher-only distillation can be tempting, especially when the original pretraining dataset is unavailable. But teacher-only training may not cover the full behavior surface. The safer default is dataset plus dense teacher plus CHT topology learning.

## What Distillation Can And Cannot Promise

Distillation can help recover:

- output behavior;
- dark knowledge in probability distributions;
- instruction style;
- broad teacher preferences;
- performance after pruning or sparsification.

Distillation cannot automatically guarantee:

- full factual preservation;
- long-context preservation;
- coding and math retention;
- Chinese capability retention;
- reasoning retention;
- better-than-dense behavior;
- hardware speedup.

These must be tested.

## Recovery Data Matters

If the original pretraining mixture is unavailable, the recovery set must be chosen carefully.

A narrow generic web corpus may improve perplexity while damaging important capabilities. A stronger recovery mixture should intentionally cover:

- general English;
- Chinese;
- code;
- math and STEM;
- factual QA;
- instruction-like prompts;
- long-context samples;
- domain-specific customer tasks if available.

This is especially important for Qwen-family or other open-weight models whose original full data mixture is not public.

## Teacher Preservation Metrics

Do not only ask whether the sparse model is good.

Ask whether it preserves the teacher where preservation matters.

Useful evaluation gates include:

- dense teacher baseline;
- sparse student before recovery;
- sparse student after recovery;
- teacher-student KL or agreement;
- task-level accuracy;
- perplexity;
- instruction-following benchmarks;
- Chinese benchmarks if relevant;
- code and math checks;
- long-context retrieval or reasoning;
- factuality and refusal behavior;
- deployment metrics.

The goal is to know what changed.

## When Can Sparse Beat Dense?

CHT papers report settings where sparse models outperform dense baselines. That is important.

But for retraining and distillation, the safe claim is:

Sparse CHT models can sometimes match or surpass dense baselines, but this is an empirical result that depends on architecture, data, sparsity level, training recipe, and evaluation.

Do not promise a sparse student will beat its dense teacher by default.

The right product posture is:

First preserve capability and unlock efficiency. Then test whether topology learning gives quality upside.

## A Recommended Recovery Recipe

For an internal SparseMind experiment, start conservative:

1. Dense warm start.
2. Moderate sparsity target first.
3. Mixed data and teacher loss.
4. CHT topology updates under the chosen sparse constraint.
5. Broad capability evaluation.
6. Increase sparsity only after recovery is stable.
7. Report real hardware metrics only on a supported execution path.

This avoids the common failure mode of jumping to extreme sparsity and then not knowing whether failure came from the mask, data, optimizer, topology updates, teacher loss, or hardware fallback.

## What To Remember

Dense-to-sparse recovery is central to productization.

Use teacher distillation as a preservation tool, not as magic.

The safest default is real data plus dense teacher plus CHT topology learning, verified across broad capabilities and real efficiency metrics.

## New-Hire Exercise

Design a recovery evaluation for a Chinese coding assistant model.

Your checklist must include quality, teacher preservation, Chinese, code, math, long context, and speed or memory.

Previous: [06-hardware-friendly-nm-sparsity.md](06-hardware-friendly-nm-sparsity.md)  
Next: [08-snn-edge-and-energy.md](08-snn-edge-and-energy.md)

