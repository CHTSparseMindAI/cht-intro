# Lab 4: Product Benchmark Report

Goal: turn a sparse training result into a partner-ready benchmark report outline.

Estimated time: 60 minutes

## Task

Create a benchmark report outline for a CHT sparse model.

Your outline must make clear:

- what was measured;
- what was not measured;
- what can be claimed;
- what cannot yet be claimed.

## Required Sections

```markdown
# Benchmark Report Outline

## Executive Summary

## Model Setup

## Dense Baseline

## Sparse Method

## Sparsity Pattern

## Layers Sparsified

## Training Or Recovery Recipe

## Hardware And Kernel Path

## Quality Results

## Efficiency Results

## Memory Results

## Energy Results

## Failure Cases

## Product Interpretation

## Claims We Can Make

## Claims We Cannot Make Yet

## Next Experiment
```

## Quality Bar

A good benchmark report does not hide inconvenient details. If a sparse model is faster only in one kernel but not end-to-end, say that. If quality was measured only on perplexity, say that.

