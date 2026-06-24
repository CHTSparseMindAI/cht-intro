# Lab 2: CHT Loop Pseudocode

Goal: translate the CHT concept into an implementation-shaped mental model.

Estimated time: 45 minutes

## Task

Write pseudocode for the CHT learning loop in 20 lines or fewer.

Your pseudocode must include:

- sparse topology initialization;
- active-weight training;
- link removal;
- CH-style link scoring or topology-guided regrowth;
- sparse mask update;
- evaluation checkpoint.

## Submission Template

```python
def train_with_cht(model, data, sparsity_budget):
    # 1. initialize sparse topology
    ...

    for step, batch in enumerate(data):
        # 2. train active weights
        ...

        if should_update_topology(step):
            # 3. remove links
            ...

            # 4. score missing links
            ...

            # 5. regrow links
            ...

        if should_eval(step):
            ...
```

## Reflection Questions

1. Which line makes this CHT rather than ordinary pruning?
2. Which line is most expensive at scale?
3. What changes if the sparse pattern must obey 2:4 N:M constraints?
4. What metric tells you whether topology updates are helping?

## Quality Bar

A good answer shows that CHT is not only a mask. It is a recurring topology update process.

