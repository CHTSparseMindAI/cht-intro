# Lab 1: Paper Claim Audit

Goal: learn to separate a paper result from a product-safe claim.

Estimated time: 45-60 minutes

## Task

Choose one paper from [sources/source_index.md](../sources/source_index.md).

Audit one claim from the paper using this structure:

1. What exactly does the paper claim?
2. What model, data, sparsity level, and hardware path does the claim depend on?
3. Is the claim about quality, efficiency, memory, energy, interpretability, or theory?
4. What would be unsafe to say in a customer conversation?
5. What is the customer-safe version?
6. What evidence is missing before productizing the claim?

## Submission Template

```markdown
# Claim Audit

Paper:

Claim:

Context:

What the claim proves:

What the claim does not prove:

Customer-safe wording:

Missing measurements:

Recommended next experiment:
```

## Quality Bar

A good audit is specific. It does not say "needs more testing" in general. It names the exact missing test.

Example missing tests:

- target hardware latency;
- dense baseline on same code path;
- quality retention after 2:4 conversion;
- Chinese/code/math capability drift;
- total model sparsity rather than sparse-able layer sparsity;
- energy measurement on device.

