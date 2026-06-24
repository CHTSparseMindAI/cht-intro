# CHT New-Hire Curriculum

Read time: 6 minutes  
Audience: new research, engineering, product, and BD hires  
Goal: understand SparseMind's CHT technology well enough to read the papers, follow engineering discussions, and explain the technology without overclaiming.

## Welcome To The Core Idea

Most people meet sparsity through compression: take a large dense model, delete weights, hope performance does not fall too much, and then ask hardware to make the zeros useful.

CHT asks a deeper question: what if sparsity is not only a way to shrink a model after the fact? What if the sparse structure itself is part of learning?

Cannistraci-Hebb Training is SparseMind's central technical language for that idea. In CHT, a neural network is treated as a changing graph. Training is not only about updating weight values. Training is also about changing the topology: which connections exist, which connections disappear, and which missing connections deserve to be regrown.

That sounds abstract at first, but the practical version is simple:

- keep only a sparse set of active connections;
- train the active weights with ordinary gradient-based learning;
- periodically remove weak or unhelpful links;
- regrow new links using network-science rules rather than dense gradient search;
- measure whether the resulting sparse model preserves, matches, or sometimes exceeds the dense baseline.

The company opportunity is that this changes the efficiency problem from "can we compress a model?" to "can we train useful sparse structure from the beginning, and can that structure run efficiently on real hardware?"

## What This Course Will Teach

This course is ordered like a ramp. The first pages build the mental model. The middle pages explain CHT and its LLM extensions. The later pages translate the research into product proof, evaluation, and engineering judgment.

You do not need to understand every equation on day one. You do need to understand the claims, the boundaries of those claims, and the vocabulary the team uses.

## Lesson Path

1. **Why Sparsity Matters**  
   The economic and technical reason SparseMind exists.

2. **Sparsity Is Not Just Compression**  
   The difference between pruning, dynamic sparse training, semi-structured sparsity, low-rank training, and CHT.

3. **Network Shape Intelligence**  
   The network-science foundation behind CHT: local communities, link prediction, topology, and the idea that useful structure can be learned.

4. **The CHT Learning Loop**  
   The actual loop: initialize, train weights, remove links, regrow links, repeat.

5. **Scaling CHT To Transformers And LLMs**  
   CHT, CHTs, CHTss, CHTsL, and why LLMs force CHT to become scalable and hardware-aware.

6. **Hardware-Friendly N:M Sparsity**  
   Why unstructured sparsity can look great on paper but disappoint on GPUs, and how 2:4 and 1:4 sparsity change the story.

7. **Retraining, Distillation, And Recovery**  
   How to convert dense models into sparse models, how to use the original dense model as teacher, and why we should not say teacher-only distillation is enough by default.

8. **SNN, Edge, And Energy**  
   Why CHT naturally connects to spiking neural networks, neuromorphic computing, and edge deployment.

9. **Evaluation And Product Proof**  
   How to move from paper claims to industrial evidence: accuracy retention, latency, throughput, memory, energy, and deployment stability.

10. **Source Map And Reading Plan**  
   What to read, in what order, and what each paper contributes.

## The Claims We Should Be Careful With

CHT papers report exciting cases where sparse models match or surpass dense baselines. That is important. It is not a license to say every sparse CHT model will beat every dense model.

The safe internal framing is:

- CHT is a topology-learning method for dynamic sparse training.
- It has evidence across MLPs, CNNs, Transformers, LLM-scale experiments, SNNs, and N:M semi-structured settings.
- Dense-over-sparse comparison must be empirical for each model, dataset, sparsity pattern, hardware path, and training recipe.
- In product discussions, "real efficiency" means wall-clock speed, memory, energy, stability, and quality retention, not only parameter count.

## How To Use This Course

For a new research hire, read all pages and then pick two papers to reproduce or summarize.

For an engineering hire, focus on lessons 4 through 9, especially N:M sparsity, retraining, and evaluation.

For product, BD, or strategy hires, focus on lessons 1, 2, 6, 8, and 9. These pages explain how to talk about CHT without collapsing the technical proof into a vague efficiency story.

## End Of Page Check

You are ready for the next page if you can explain this in one sentence:

CHT is not just deleting weights; it is learning sparse network topology during training.

Next: [01-why-sparsity-matters.md](01-why-sparsity-matters.md)

