# Retraining, Distillation, and Recovery (In progress... We are still exploring this topic)

**Learning objective:** Understand how to convert existing dense models into useful sparse models without losing critical capabilities.

---

## Why Retraining Matters

Training sparse models from scratch is scientifically clean, but the world is full of dense models that already exist. A practical question arises constantly:

> Can you take my trained dense model and make it more efficient?

This means dense-to-sparse conversion and recovery are critical. The relevant question is not only whether CHT can pretrain a sparse model from scratch. It is also whether CHT can help transform an existing dense model into a hardware-friendly sparse model without losing the capabilities that matter.

---

## The Dense-to-Sparse Path

The **[CHTs24](https://openreview.net/forum?id=oo7G9mheNo)** paper (CPAL 2026) introduces **eDSrT** — epitopology Dynamic Sparse re-Training. The idea is to transition a dense model into semi-structured sparse form, then retrain under CHT-guided topology rules.

A practical dense-to-sparse workflow:

1. Start from a validated dense baseline.
2. Choose a sparse target: unstructured, 2:4, 1:4, or another N:M format.
3. Decide which layers are sparse-able.
4. Create the initial sparse mask.
5. Retrain active weights and allow topology updates.
6. Compare against the dense baseline on quality and real efficiency.
7. Iterate on sparsity level, mask constraints, loss mix, optimizer, and schedule.

---

## Where Distillation Fits

Distillation uses a teacher model to guide a student model. In the CHT context, the dense original model becomes the teacher, and the sparse CHT model becomes the student.

The typical mixed objective:

```
loss = CrossEntropy(real data) + α · KL(sparse logits, dense teacher logits)
```

The sparse model learns from both real data and the dense teacher's output distribution.

The important word is **mixed**.

Teacher-only distillation can be tempting, especially when the original pretraining dataset is unavailable. But teacher-only training may not cover the full behavior surface. The safer default is dataset plus dense teacher plus CHT topology learning.

---

## What Distillation Can and Cannot Promise

Distillation **can** help recover:
- Output behavior and probability distributions.
- Instruction style and broad teacher preferences.
- Performance after pruning or sparsification.

Distillation **cannot** automatically guarantee:
- Full factual preservation.
- Long-context preservation.
- Coding and math retention.
- Chinese (or other language) capability retention.
- Reasoning retention.
- Better-than-dense behavior.
- Hardware speedup.

Task specific retraining data is still required.

---

## Recovery Data Matters

If the original pretraining mixture is unavailable, the recovery dataset must be chosen carefully. A narrow generic corpus may improve perplexity while damaging important capabilities.

A strong recovery mixture should intentionally cover:
- General English.
- Chinese (or target languages).
- Code.
- Math and STEM.
- Factual QA.
- Instruction-like prompts.
- Long-context samples.
- Domain-specific target tasks if available.

---

## Teacher Preservation Metrics

Do not only ask whether the sparse model is good. Ask whether it preserves the teacher where preservation matters.

Useful evaluation gates:
- Dense teacher baseline.
- Sparse student before recovery.
- Sparse student after recovery.
- Teacher-student KL divergence or agreement.
- Task-level accuracy and perplexity.
- Instruction-following benchmarks.
- Language-specific benchmarks.
- Code and math checks.
- Long-context retrieval or reasoning.
- Factuality and refusal behavior.
- Deployment metrics (speed, memory, energy).

The goal is to understand *what changed*.

---

## When Can Sparse Beat Dense?

CHT papers report settings where sparse models outperform dense baselines. That is significant.

But for retraining and distillation, a measured claim is:

> Sparse CHT models can sometimes match or surpass dense baselines, but this is an empirical result that depends on architecture, data, sparsity level, training recipe, and evaluation protocol.

Do not promise a sparse student will beat its dense teacher by default. The right posture is:

> First preserve capability and unlock efficiency. Then test whether topology learning gives quality upside.

---

## A Recommended Recovery Recipe

Start conservative:

1. Dense warm start.
2. Moderate sparsity target first.
3. Mixed data and teacher loss.
4. CHT topology updates under the chosen sparse constraint.
5. Broad capability evaluation.
6. Increase sparsity only after recovery is stable.
7. Report real hardware metrics only on a supported execution path.

This avoids the common failure mode of jumping to extreme sparsity and then not knowing whether failure came from the mask, data, optimizer, topology updates, teacher loss, or hardware fallback.

---

## Key Takeaways

- Dense-to-sparse recovery is central to practical adoption.
- Use teacher distillation as a preservation tool, not as a substitute for real data.
- The safest default: real data + dense teacher + CHT topology learning, verified across broad capabilities and real efficiency metrics.

**Previous:** [Hardware-Friendly N:M Sparsity](06-hardware-friendly-nm-sparsity.md)  
**Next:** [SNN, Edge, and Energy](08-snn-edge-and-energy.md)
