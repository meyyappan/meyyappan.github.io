---
title: "Score matching and why diffusion works"
date: 2025-04-22
draft: false
tags: ["diffusion", "generative-models", "score-matching"]
description: "The probabilistic foundations behind diffusion models."
---

Diffusion models have become the dominant paradigm for image generation, but the theoretical foundation — score matching — is underexplained in most introductions. Here's how I think about it.

## What is a score?

The *score* of a distribution `p(x)` is the gradient of the log probability with respect to the data:

```
s(x) = ∇_x log p(x)
```

This vector points in the direction of increasing probability density. If you could follow it, you'd walk uphill toward modes of the distribution.

## Why not model p(x) directly?

For high-dimensional data, modeling `p(x)` requires a normalization constant `Z` that's intractable to compute. The score `∇_x log p(x)` bypasses this — the normalization constant disappears when you take the gradient.

## Score matching

Hyvärinen (2005) showed you can learn the score without access to the true distribution. The objective is:

```
E[||s_θ(x) - ∇_x log p(x)||²]
```

With integration by parts, this becomes tractable using only samples from `p(x)`.

## Connecting to diffusion

In diffusion models, we add Gaussian noise at multiple scales to destroy the data structure. At each noise level `σ`, we train a network to denoise — which is equivalent to estimating the score at that noise level.

The key insight from Song et al. (2020): by training across many noise levels and using Langevin dynamics, you can generate samples by starting from pure noise and following the learned score downhill.

## Practical notes

- Predicting noise (ε-prediction) vs. predicting `x0` vs. predicting the score are all equivalent given appropriate parameterization
- The noise schedule matters a lot — linear, cosine, and learned schedules have different tradeoffs
- Classifier-free guidance works by steering the score toward a conditioning signal, which is why it can be turned up arbitrarily (at the cost of diversity)

The connection between optimal denoising and score estimation is one of those results that makes the math feel inevitable once you see it.
