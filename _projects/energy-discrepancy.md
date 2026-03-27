---
title: "How to train your EBM without Markov Chain Monte Carlo"
image: "/Images/ComparisonED_SM_CD.png"
excerpt: "We propose Energy Discrepancy (ED), a new training methodology for energy-based models that avoids sampling-based methods like contrastive divergence and Stein-score-based approaches, enabling robust and unbiased models for high-dimensional data."
order: 1
---

We propose a new training methodology for energy-based models (EBMs) based on **Energy Discrepancy (ED)** — a loss function that does not rely on sampling (like contrastive divergence, short CD) or Stein scores (as in score-based methods, short SM). The goal is to enable robust, unbiased models for high-dimensional data without the computational overhead and approximation errors introduced by Markov Chain Monte Carlo methods.

![EBM comparison]({{ "/Images/ComparisonED_SM_CD.png" | relative_url }})

## Key Contributions

- A score-independent training loss for energy-based models
- Avoids MCMC sampling during training, removing a major bottleneck
- Competitive or superior performance compared to contrastive divergence and score-matching

## Papers

- **"Energy Discrepancies: A Score-Independent Loss for Energy-Based Models"** — [arXiv:2307.06431](https://arxiv.org/abs/2307.06431)
- Extension to **discrete spaces**, presented at the ICML 2023 workshop *Sampling and Optimisation in Discrete Spaces* — [arXiv:2307.07595](https://arxiv.org/abs/2307.07595)
