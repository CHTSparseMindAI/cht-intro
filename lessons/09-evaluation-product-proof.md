# Evaluation And Product Proof

Core question: how do we know CHT is working in a way customers care about?

## Research Proof Is Not Product Proof

A paper table can show that a sparse model matches a dense model on a benchmark. That is valuable. It is not enough for an industrial buyer.

Product proof asks:

- Does quality remain acceptable on the customer's actual workload?
- Does the sparse model run faster or cheaper on target hardware?
- Is memory reduced in the real deployment path?
- Is energy reduced if energy is part of the promise?
- Is the system stable under real traffic?
- Is the engineering complexity worth it?

This is why SparseMind should think in proof layers.

## The Five Proof Layers

### 1. Technical Proof

Technical proof shows the algorithm works in controlled experiments.

Examples:

- sparse model reaches dense-like loss;
- CHT beats another dynamic sparse training baseline;
- topology update does not create convergence penalty;
- 2:4 sparse model retains accuracy;
- distillation recovers performance after sparsification.

### 2. Systems Proof

Systems proof shows the method works inside a real software and hardware path.

Examples:

- sparse kernels execute correctly;
- no dense fallback hides the cost;
- memory usage decreases in measured runs;
- forward and backward pass improve;
- throughput increases under realistic batch and sequence settings.

### 3. Product Proof

Product proof shows the method solves a user problem.

Examples:

- lower serving cost per token;
- fit model into a smaller GPU memory budget;
- increase tokens per second for a partner deployment;
- reduce edge-device energy consumption;
- preserve accuracy on a customer domain task.

### 4. Industry Proof

Industry proof shows repeatability across partners or verticals.

Examples:

- validation with data-center partner;
- validation with chip or accelerator partner;
- validation on edge or robotics setting;
- reproducible benchmark package.

### 5. Commercial Proof

Commercial proof shows someone will pay or commit resources.

Examples:

- pilot contract;
- paid validation;
- cloud or hardware partnership;
- customer deployment roadmap;
- cost-saving estimate accepted by buyer.

## Metrics That Matter

For CHT and sparse LLM work, track:

- perplexity or task loss;
- benchmark accuracy;
- zero-shot and instruction tasks;
- Chinese, code, math, long context if relevant;
- dense-teacher agreement for recovery;
- active parameter count;
- total model parameter count;
- sparse-able layer sparsity;
- total model sparsity;
- training memory;
- optimizer-state memory;
- inference memory;
- throughput;
- TTFT;
- latency distribution;
- cost per token;
- energy if measured;
- hardware and kernel path;
- deployment stability.

The rule is simple:

If a metric is part of the customer promise, it must be measured.

## How To Avoid Overclaiming

Bad claim:

"CHT makes models 75% sparse, so it cuts cost by 75%."

Better claim:

"In this target layer set, the model uses a 1:4 sparse pattern. We still need to report total model sparsity, kernel support, quality retention, and measured end-to-end speed before translating that into cost."

Bad claim:

"Sparse beats dense."

Better claim:

"In these experiments, the sparse CHT variant matches or outperforms the dense baseline on selected tasks. We need to verify the same on the target model, data, and hardware."

Bad claim:

"Distillation can replace the dataset."

Better claim:

"Dense-teacher distillation can help recovery, but we prefer mixed real-data and teacher supervision unless a task-specific experiment proves teacher-only recovery is sufficient."

## What A Good Benchmark Report Looks Like

A good SparseMind benchmark report should include:

- exact model version;
- dense baseline;
- sparse method and sparsity pattern;
- layers sparsified;
- training or retraining recipe;
- dataset and recovery mixture;
- distillation setup if used;
- hardware;
- kernel path;
- quality metrics;
- efficiency metrics;
- failure cases;
- next experiment.

It should be readable by engineers and credible to external partners.

## What To Remember

CHT success must be proven across model quality, systems performance, and customer value.

The company should be proud of research claims while translating them into measured industrial proof.

Real sparse infrastructure is built by closing the gap between paper sparsity and deployment savings.

Previous: [08-snn-edge-and-energy.md](08-snn-edge-and-energy.md)  
Next: [10-source-map-and-reading-plan.md](10-source-map-and-reading-plan.md)

