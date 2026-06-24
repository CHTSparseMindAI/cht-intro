# CHT New-Hire Curriculum

Welcome to the GitHub version of the SparseMind CHT onboarding course.

This course teaches Cannistraci-Hebb Training (CHT) as a technology stack: sparse learning theory, topology-guided dynamic sparse training, Transformer and LLM scaling, N:M hardware-aware sparsity, dense-to-sparse recovery, SNN/edge energy, and product proof.

## Who This Is For

- Research hires who need to read and reproduce CHT papers.
- Engineering hires who need to understand sparse training and evaluation.
- Product and BD hires who need to explain CHT without overclaiming.
- Strategy hires who need to connect CHT research to industrial proof.

## Learning Path

Use this as your main table of contents.

| Step | Lesson | Output |
|---:|---|---|
| 0 | [Course Home](lessons/00-course-home.md) | Explain CHT in one sentence |
| 1 | [Why Sparsity Matters](lessons/01-why-sparsity-matters.md) | Five-sentence answer: why SparseMind is not just pruning |
| 2 | [Sparsity Is Not Just Compression](lessons/02-sparsity-is-not-just-compression.md) | Sparse method comparison table |
| 3 | [Network Shape Intelligence](lessons/03-network-shape-intelligence.md) | Plain-English definition of epitopological learning |
| 4 | [The CHT Learning Loop](lessons/04-the-cht-learning-loop.md) | CHT pseudocode |
| 5 | [Scaling CHT To Transformers And LLMs](lessons/05-scaling-cht-to-transformers-and-llms.md) | Variant comparison: CHTs, CHTss, CHTsL, CHTs24, CHTsNM |
| 6 | [Hardware-Friendly N:M Sparsity](lessons/06-hardware-friendly-nm-sparsity.md) | Hardware-aware sparsity explanation |
| 7 | [Retraining, Distillation, And Recovery](lessons/07-retraining-distillation-and-recovery.md) | Dense-to-sparse recovery plan |
| 8 | [SNN, Edge, And Energy](lessons/08-snn-edge-and-energy.md) | Edge deployment memo |
| 9 | [Evaluation And Product Proof](lessons/09-evaluation-product-proof.md) | Research claim vs customer-safe claim |
| 10 | [Source Map And Reading Plan](lessons/10-source-map-and-reading-plan.md) | Two-week onboarding plan and reading map |

## Interactive Materials

- [Progress Tracker](workbook/progress-tracker.md)
- [Course Workbook](workbook/course-workbook.md)
- [Lab 1: Paper Claim Audit](labs/lab-01-paper-claim-audit.md)
- [Lab 2: CHT Loop Pseudocode](labs/lab-02-cht-loop-pseudocode.md)
- [Lab 3: Dense-To-Sparse Recovery Plan](labs/lab-03-dense-to-sparse-recovery-plan.md)
- [Lab 4: Product Benchmark Report](labs/lab-04-product-benchmark-report.md)
- [Self-Check Quiz](quizzes/self-check.md)
- [Final Capstone](quizzes/final-capstone.md)

## Source Materials

- [Source Index](sources/source_index.md)
- [Extracted paper text](sources/source_text/)

The source index records which local SparseMind papers were reviewed, duplicate handling, and the one curated-list paper that was named but not found locally.

## Recommended Completion Criteria

A new hire is course-complete when they can:

- explain CHT as topology-guided dynamic sparse training;
- distinguish CHT from pruning and static sparsity;
- explain why hardware-aware sparsity is different from theoretical sparsity;
- design a conservative dense-to-sparse recovery experiment;
- identify unsafe overclaims;
- present a customer-safe CHT proof story.

