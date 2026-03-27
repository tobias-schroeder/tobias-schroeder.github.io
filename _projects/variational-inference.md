---
title: "Variational Inference as a Gradient Flow in a Kernelised Wasserstein Geometry"
image: "/Images/ParticleTransport.png"
excerpt: "For my Master's thesis, I formulated variational inference training dynamics as a gradient flow in a kernelised Wasserstein geometry, connecting Stein geometries with black-box variational inference."
order: 3
---

Variational Inference (VI) optimises a training objective with gradient descent to infer optimal parameters in a parametric family of distributions — for example, to compute an approximate Bayesian posterior.

For my Master's thesis, I formulated the training dynamics of variational inference as a **gradient flow in a kernelised Wasserstein geometry**. This builds on foundational results from two directions:

- The geometry of [Stein operators](https://arxiv.org/abs/1912.00894) and their induced kernelised geometry
- The [relationship between gradient flows and black-box variational inference](https://arxiv.org/abs/2004.01822)

![Particle transport visualisation]({{ "/Images/ParticleTransport.png" | relative_url }})

## Key Ideas

- Reformulating VI optimisation as a continuous-time gradient flow in a function space
- Using kernel-induced geometries (Stein geometry) to define the flow
- Connecting particle-based and parametric VI methods under a unified geometric framework

## Context

This work was my Master's thesis and explores the theoretical foundations of why gradient-based variational inference converges, and what geometry underlies its dynamics.
