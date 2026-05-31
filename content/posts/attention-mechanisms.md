---
title: "Attention mechanisms, from scratch"
date: 2025-05-10
draft: false
tags: ["transformers", "attention", "fundamentals"]
description: "Building up intuition for self-attention and why it works."
---

The attention mechanism is one of those ideas that seems obvious in hindsight but took a while to arrive at. This post builds it up from first principles.

## The core problem

Say you're processing the sequence "The cat sat on the mat, and it was happy." When you read "it", you know it refers to the cat. How?

You retrieved a fact from earlier in the sequence by *querying* your memory. Attention is a learnable version of this.

## Queries, keys, and values

In self-attention, each token produces three vectors:

- **Q** (query) — what am I looking for?
- **K** (key) — what do I contain?
- **V** (value) — what do I return if selected?

The attention score between token `i` and token `j` is:

```
score(i, j) = Q_i · K_j / sqrt(d_k)
```

We divide by `sqrt(d_k)` to keep gradients stable as dimensionality grows. The softmax of these scores gives us weights over the values.

## Why this beats RNNs

RNNs process sequences one step at a time — information from position 0 must survive `n` steps to influence position `n`. With attention, every token can directly attend to every other token. Path length is O(1).

The tradeoff: attention is O(n²) in sequence length, which is why longer-context models are expensive.

## Multi-head attention

Instead of one attention function, run `h` attention heads in parallel with different learned projections, then concatenate. Each head can specialize in different types of relationships — some heads track syntax, others semantics.

```python
def multi_head_attention(Q, K, V, h=8):
    d_k = Q.shape[-1] // h
    heads = []
    for i in range(h):
        q = Q[..., i*d_k:(i+1)*d_k]
        k = K[..., i*d_k:(i+1)*d_k]
        v = V[..., i*d_k:(i+1)*d_k]
        scores = q @ k.T / d_k**0.5
        heads.append(softmax(scores) @ v)
    return concat(heads) @ W_o
```

## What I find interesting

Attention weights are often interpreted as "where the model looks," but this is only partially true. The weights tell you which values are aggregated, not whether those values are causally important. There's active work on disentangling these.

Also worth noting: most of the representational power isn't in the attention itself, but in the MLP layers that follow. Attention routes information; MLPs transform it.
