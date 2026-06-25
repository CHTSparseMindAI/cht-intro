# Course Overview

**Learning objective:** Understand the structure of this course.

---

## What This Course Covers

Cannistraci-Hebb Training (CHT) is a methodology for dynamic sparse training of neural networks. Unlike pruning — which removes weights from an already-trained dense model — CHT trains sparse from the beginning, letting the network's topology evolve alongside its weights.

This course builds understanding from first principles:

1. **Why sparsity matters** — the economic and technical case for structural efficiency.
2. **What sparsity means** — the landscape of sparse methods and where CHT fits.
3. **The network-science foundation** — how link prediction and local community structure inform CHT.
4. **The CHT learning loop** — initialize, train, remove, regrow, repeat.
5. **Scaling to Transformers and LLMs** — CHTs, CHTss, CHTsL, and CHTsNM.
6. **Hardware-friendly N:M sparsity** — when and how sparse models actually run faster.
7. **Retraining and recovery** — converting existing dense models to sparse.
8. **SNN, edge, and energy** — structural sparsity meets temporal sparsity.
9. **Evaluation and engineering proof** — measuring what matters.

---

## How to Use This Course

Each lesson is self-contained and takes about 10-15 minutes to read. Lessons build on each other, so following the numbered order is recommended.

Key papers are cited throughout with links to their published versions. The **Papers** table in the repository README lists all referenced works with venues and links.

---

## What You Will Be Able to Do

After completing this course, you will be able to:

- Explain CHT as topology-guided dynamic sparse training.
- Distinguish CHT from pruning, static sparsity, and gradient-based dynamic sparse training.
- Understand the Cannistraci-Hebb link prediction formula and its role in regrowth.
- Explain why hardware-aware sparsity differs from theoretical sparsity.
- Reason about the trade-offs between CHT variants (CHT, CHTs, CHTss, CHTsL, CHTs24, CHTsNM).
- Design a principled dense-to-sparse recovery experiment.
- Evaluate sparse training results with technical rigor.

---

## Key Takeaways

- CHT is not compression — it is a training methodology where topology learns.
- The course progresses from motivation → foundations → algorithm → scaling → deployment → evaluation.
- Each lesson links to the relevant published papers so you can go deeper.

**Next:** [Why Sparsity Matters](01-why-sparsity-matters.md)
