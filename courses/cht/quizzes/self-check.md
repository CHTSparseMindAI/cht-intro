# Self-Check Quiz

Use this after finishing lessons 0-9. Try answering before opening the expandable answers.

## 1. What Is CHT?

Write a one-sentence definition.

<details>
<summary>Suggested answer</summary>

CHT is topology-guided dynamic sparse training: it trains active weights while periodically removing and regrowing sparse connections using Cannistraci-Hebb network-science link prediction.

</details>

## 2. How Is CHT Different From Pruning?

<details>
<summary>Suggested answer</summary>

Pruning usually starts from a dense trained model and removes weights. CHT keeps the model sparse during training and learns which sparse connections should exist through topology updates.

</details>

## 3. Why Can Unstructured Sparsity Fail To Speed Up On GPUs?

<details>
<summary>Suggested answer</summary>

Arbitrary sparse patterns can create irregular memory access, index overhead, poor load balance, and fallback paths that are slower than dense matrix multiplication.

</details>

## 4. Why Does N:M Sparsity Matter?

<details>
<summary>Suggested answer</summary>

N:M sparsity constrains sparse patterns into hardware-friendly local groups, such as 2:4, making it more realistic for GPU acceleration than arbitrary unstructured sparsity.

</details>

## 5. What Is Unsafe About Saying "CHT Sparse Beats Dense"?

<details>
<summary>Suggested answer</summary>

Some papers report sparse-over-dense gains in specific settings, but the result depends on model, data, sparsity level, training recipe, and hardware path. It should be treated as an empirical result, not a universal guarantee.

</details>

## 6. Why Use Dense-Teacher Distillation In Recovery?

<details>
<summary>Suggested answer</summary>

The dense teacher can help the sparse model preserve output behavior after sparsification. It is especially useful when the original training data is unavailable or incomplete.

</details>

## 7. Why Is Teacher-Only Distillation Risky?

<details>
<summary>Suggested answer</summary>

Teacher-only training may not cover the full capability surface, such as Chinese, code, math, factuality, instruction following, or long context. A safer default is real data plus dense teacher plus CHT topology learning.

</details>

## 8. What Metrics Matter For Product Proof?

<details>
<summary>Suggested answer</summary>

Quality, dense baseline comparison, sparse-able and total sparsity, memory, optimizer memory, throughput, latency, TTFT, energy if relevant, hardware/kernel path, and deployment stability.

</details>

## 9. How Do SNNs Connect To CHT?

<details>
<summary>Suggested answer</summary>

SNNs provide temporal activation sparsity. CHT provides structural connection sparsity. Together they can improve accuracy-energy trade-offs in edge or neuromorphic settings.

</details>

## 10. What Is The Best Short Customer-Safe CHT Claim?

<details>
<summary>Suggested answer</summary>

SparseMind develops topology-guided sparse training and recovery methods that aim to preserve model quality while reducing real compute, memory, or energy on target hardware, with claims validated per model and deployment path.

</details>

