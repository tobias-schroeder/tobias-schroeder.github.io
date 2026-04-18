---
title: "Near-linear softmax attention in theory and practice via weighted coresets"
image: "/Images/plot_runtime_vs_seqlen.png"
excerpt: "We introduce WILDCAT, a principled method to approximate the softmax attention mechanism from a small coreset of reweighted keys and values. This enables accurate inference for long context tasks at a fraction of the computational cost and memory footprint."
order: 1
---

The **attention mechanism** is arguably the most important building block in modern machine learning. Transformers power large language models, image generators, and protein structure predictors alike. But attention has a fundamental cost: it scales **quadratically** with the sequence length. Process a sequence twice as long, and attention takes four times as long. For long documents, high-resolution images, or extended conversations, this becomes prohibitive.

This is the problem WILDCAT solves.

## The Core Idea

WILDCAT (**W**eighted **I**terative **L**ow-rank **D**ecomposition for **C**oreset **AT**tention) approximates exact softmax attention by attending only over a small, carefully chosen subset of keys — a *coreset*. Crucially, the keys in the coreset are not just selected but also **optimally reweighted** to minimise reconstruction error, making the approximation far more accurate than simple subsampling.

The key insight is that the attention matrix can be well-approximated using a low-rank Nyström approximation of the key kernel matrix. Rather than computing attention over all *n* keys, WILDCAT selects *r ≪ n* representative keys and forms weighted compressed keys and values that summarise the full context.

## Coreset Selection via Randomly Pivoted Cholesky

The coreset is selected using a **randomly pivoted Cholesky (RPC)** algorithm, which builds a partial Cholesky decomposition of the key kernel matrix. At each step, the next coreset point is sampled proportionally to the diagonal of the *residual* kernel — favouring keys that are least well-explained by the current coreset. This simple rule turns out to be spectrally optimal: it produces the best possible Nyström approximation in expectation.

Combined with an **optimal reweighting** via Nyström weights, the resulting approximation is near-optimal among all rank-*r* approximations to the attention matrix.

## Runtime and Error Guarantees

WILDCAT enjoys the best of both worlds: it is both fast and accurate.

- **Near-linear runtime**: With a coreset of size *r ∈ n^o(1)*, WILDCAT runs in *O(nr²)* time — near-linear in the sequence length.
- **Super-polynomial error decay**: For bounded inputs, a near-constant coreset size suffices for *O(n^{−√log log n})* error decay. This is dramatically faster than the polynomial decay guaranteed by prior methods, and is the first practical algorithm to achieve super-polynomial error in near-linear time.

Prior work either required quadratic runtime for fast error decay, or only guaranteed slow near-constant decay in near-linear time. WILDCAT closes this theory-practice gap.

## Comparison to FlashAttention

The plot below shows WILDCAT runtime versus sequence length, compared to FlashAttention 2. FlashAttention already achieves impressive speed through IO-aware tiling, but its runtime still grows **quadratically** — visible as the rapidly accelerating dashed curve. WILDCAT's runtime remains nearly flat across all tested sequence lengths (up to 32,768 tokens), staying well below 100ms while FlashAttention exceeds 1,200ms at the longest sequences.

![Runtime vs sequence length]({{ "/Images/plot_runtime_vs_seqlen.png" | relative_url }})

Different curves correspond to different coreset sizes *r* (with the number of parallel bins *B* adjusted accordingly). Even the largest coreset (*r = 512*, purple) remains orders of magnitude faster than exact attention at long sequences.

## KV Cache Compression

In autoregressive language models, past keys and values are stored in a **KV cache** that grows linearly with context length. WILDCAT's COMPRESSKV algorithm compresses this cache down to *O(r)* entries by running the coreset selection during the *prefill phase*. Subsequent generation then attends only over the compressed cache.

We evaluated COMPRESSKV on 13 long-context language understanding tasks from LongBench-E, using Qwen2.5-7B-Instruct. COMPRESSKV achieved the **highest average score** across all tasks, outperforming five leading KV cache compression methods (StreamingLLM, PyramidKV, BalanceKV, Uniform, and SnapKV) — all at 25% of the original cache size.

## Image Generation and Classification

Beyond language models, WILDCAT was evaluated as a drop-in replacement for exact attention in two vision benchmarks:

- **BigGAN image generation**: WILDCAT achieved the largest speed-up (3.71× over exact attention), the lowest Inception Score degradation (1.4%), and no degradation in Fréchet Inception Distance — outperforming Reformer, ScatterBrain, Performer, KDEformer, and Thinformer on all metrics simultaneously.
- **T2T-ViT image classification**: WILDCAT achieved the highest Top-1 accuracy among all approximations (82.18% vs. 82.55% for exact attention) while also running the fastest — an 11.6× speed-up on the dominant attention layer.

## Papers and Code

- **"WILDCAT: Near-Linear Attention in Theory and Practice"** with Lester Mackey — [arXiv:2602.10056](https://arxiv.org/abs/2602.10056)
- Open-source PyTorch implementation: [github.com/microsoft/wildcat](https://github.com/microsoft/wildcat)
