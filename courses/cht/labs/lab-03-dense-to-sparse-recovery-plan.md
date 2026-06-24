# Lab 3: Dense-To-Sparse Recovery Plan

Goal: design a conservative recovery experiment for converting a dense model into a sparse model.

Estimated time: 60-90 minutes

## Scenario

SparseMind is asked to convert a dense Chinese coding assistant into a sparse model. The customer cares about serving cost, memory, Chinese quality, code generation, math reasoning, and long-context retrieval.

## Task

Design a recovery plan.

Your plan must include:

- dense baseline;
- sparse target;
- layers to sparsify;
- recovery data mix;
- teacher distillation setup;
- CHT topology update plan;
- evaluation gates;
- hardware metrics;
- stop/go criteria.

## Submission Template

```markdown
# Dense-To-Sparse Recovery Plan

## Model And Baseline

## Sparse Target

## Sparse-Able Layers

## Initial Mask Strategy

## Recovery Objective

Use:

- CE(real data):
- KL(student, dense teacher):
- Other losses:

## Recovery Data Mix

## CHT Topology Schedule

## Evaluation Gates

Quality:

Teacher preservation:

Chinese:

Code:

Math:

Long context:

Safety/factuality:

## Hardware Metrics

## Stop Criteria

## Go Criteria

## Risks
```

## Quality Bar

A good plan does not assume distillation is enough. It uses real data plus dense teacher guidance and broad evaluation.

