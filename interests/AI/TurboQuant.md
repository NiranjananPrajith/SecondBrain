---
title: TurboQuant
created: 2026-04-10
tags: [AI, ML, quantization, compression, papers]
---

# Summary: TurboQuant: Online Vector Quantization with Near-optimal Distortion Rate

**Authors:** Amir Zandieh, Majid Daliri, Majid Hadian, Vahab Mirrokni
**Institutions:** Google Research, New York University, Google DeepMind
**Date:** April 28, 2025 (arXiv preprint)

---

## 1. Executive Summary

The paper introduces **TurboQuant**, a novel, data-oblivious, online Vector Quantization (VQ) algorithm designed to compress high-dimensional vectors while minimizing both Mean-Squared Error (MSE) and Inner Product distortion. By leveraging random rotations, statistical concentration, and optimal scalar quantization, TurboQuant achieves near-optimal distortion rates that differ from theoretical information-theoretic lower bounds by a small constant factor (≈2.7). To address the inherent bias of MSE-optimized quantizers in inner product estimation, the authors propose a two-stage approach utilizing a 1-bit Quantized Johnson-Lindenstrauss (QJL) transform on the residual error.

Extensive experiments demonstrate TurboQuant's superiority in Large Language Model (LLM) KV cache quantization (achieving quality neutrality at 3.5 bits per channel and outperforming methods like KIVI, SnapKV, and PyramidKV) and Nearest Neighbor (NN) search (outperforming Product Quantization (PQ) and RabitQ with near-zero indexing time).

---

## 2. Introduction & Problem Definition

Vector Quantization is critical for handling high-dimensional vectors in domains ranging from deep learning inference (e.g., compressing LLM weights, activations, and Key-Value (KV) caches) to search retrieval systems (vector databases).

**Existing Methods' Limitations:** Existing VQ algorithms trade off between accelerator-friendliness (computation speed) and distortion optimality. Offline methods (like PQ) require heavy, data-dependent preprocessing (e.g., k-means), while existing online methods often suffer from suboptimal distortion relative to bit-width.

**Problem Definition:**
The objective is to design a randomized quantization map $Q: \mathbb{R}^d \to \{0,1\}^B$ that maps $d$-dimensional vectors to a binary string of $B = b \cdot d$ bits ($b$ is the average bits per coordinate), and a dequantization map $Q^{-1}$. The design aims to minimize two expected distortion measures for any worst-case vectors $x, y$:

- **MSE Distortion:** $\mathbb{E}[\|x - Q^{-1}(Q(x))\|_2^2]$
- **Inner-Product Distortion:** $\mathbb{E}[|\langle y, x \rangle - \langle y, Q^{-1}(Q(x)) \rangle|^2]$

For inner products, an unbiased estimator is strictly required: $\mathbb{E}[\langle y, Q^{-1}(Q(x)) \rangle] = \langle y, x \rangle$.

---

## 3. TurboQuant Methodology

The authors introduce two VQ algorithms tailored to specific optimization objectives.

### 3.1. MSE-Optimized TurboQuant (TurboQuant_mse)

This algorithm is designed to strictly minimize the reconstruction MSE.

**Key Insight:** For worst-case inputs, the algorithm applies a random rotation matrix $\Pi \in \mathbb{R}^{d \times d}$ to the input vector $x$. The rotated vector $\Pi \cdot x$ is uniformly distributed on a hypersphere.

**Statistical Property:** By measure concentration, each coordinate of the rotated vector follows a Beta distribution, which closely approximates a Gaussian $\mathcal{N}(0, 1/d)$ in high dimensions. Crucially, the coordinates become nearly independent, allowing the algorithm to apply independent, 1-dimensional optimal scalar quantizers to each coordinate.

**Optimal Scalar Quantization:** The scalar quantizer is designed by solving a continuous 1D k-means (Max-Lloyd) problem to partition the distribution into $2^b$ buckets. The optimal codebooks are precomputed and stored for varying bit-widths ($b$).

**Theoretical Guarantee (Theorem 1):** TurboQuant_mse achieves an expected MSE bounded by $\frac{3\pi^2}{4} \cdot \frac{1}{4^b}$. For small bit-widths ($b=1,2,3,4$), the bounds are specifically refined (≈0.36, 0.117, 0.03, 0.009).

### 3.2. Inner-Product-Optimized TurboQuant (TurboQuant_prod)

MSE-optimal quantizers are empirically and theoretically shown to be biased when estimating inner products (e.g., at $b=1$, the estimator has a multiplicative bias of $2/\pi$).

**Two-Stage Approach:** To obtain an unbiased estimator with low distortion, TurboQuant_prod combines TurboQuant_mse with the 1-bit Quantized Johnson-Lindenstrauss (QJL) transform.

**Mechanism:**
1. Quantize the vector using TurboQuant_mse with $b-1$ bits.
2. Calculate the residual vector $r = x - \tilde{x}_{\text{mse}}$.
3. Apply a 1-bit QJL transform to the residual $r$. QJL uses a randomized sign projection to return an unbiased 1-bit estimate.

**Theoretical Guarantee (Theorem 2):** TurboQuant_prod yields a perfectly unbiased inner product estimate, and the distortion is bounded by $\frac{3\pi^2 \|y\|_2^2}{d} \cdot \frac{1}{4^b}$.

### 3.3. Information-Theoretic Lower Bounds

Using Shannon's Source Coding Theory (Shannon Lower Bound) and Yao's Minimax Principle, the authors prove (in Theorem 3) that for any randomized quantization algorithm operating on $b$ bits, the lower bound on MSE distortion is $\frac{1}{4} \cdot \frac{1}{4^b}$.

**Conclusion:** TurboQuant's distortion is within a maximum constant factor of $\frac{3\pi^2}{4} \approx 2.7$ of the absolute theoretical limit. At $b=1$, it is just ≈1.45 times the optimal bound.

---

## 4. Experimental Results

The paper rigorously validates the theoretical claims and evaluates TurboQuant on two downstream tasks: LLM KV cache compression and Nearest Neighbor Search.

### 4.1. Empirical Validation

- **Dataset:** DBpedia Entities encoded into 1536-dimensional space (OpenAI text-embedding-3).
- **Verification:** Experimental variance histograms confirm that TurboQuant_prod is entirely unbiased for inner product estimations across all bit widths, whereas TurboQuant_mse shows bias that reduces as bit width increases. Measured errors align perfectly with the derived theoretical upper bounds.

### 4.2. LLM KV Cache Compression (Needle-In-A-Haystack & LongBench)

**Needle-In-A-Haystack Test:** Conducted on Llama-3.1-8B-Instruct across context sizes from 4k to 104k tokens. Under a memory compression ratio of 0.25 (4x compression), TurboQuant achieved perfect retrieval (Score: 0.997), matching the Full-Precision baseline and outperforming token-dropping/scalar methods like KIVI (0.981), PyramidKV (0.895), and SnapKV (0.858).

**LongBench Evaluation:** Tested on LongBench-E using Llama-3.1-8B-Instruct and Ministral-7B-Instruct.

- **Outlier Treatment Strategy:** To handle outlier channels, 32 channels were assigned 3 bits and the remaining 96 channels assigned 2 bits, yielding an effective bit rate of 2.5 bits/channel. A similar setup achieved 3.5 bits/channel.
- **Performance:** At 3.5 bits/channel, TurboQuant maintained an average score (50.06) virtually identical to the unquantized Full Cache model (50.06). At 2.5 bits, it still heavily outperformed prior art like KIVI and PolarQuant.

### 4.3. Nearest Neighbor Search

**Setup:** Evaluated on DBpedia ($d=1536$, 3072) and GloVe ($d=200$) datasets, comparing TurboQuant against Product Quantization (PQ) and RabitQ using Recall@1@k.

**Results:**
- **Accuracy:** TurboQuant consistently outperformed PQ and RabitQ across different bit widths and dimensions.
- **Latency/Indexing Time:** Because TurboQuant is data-oblivious (requires no data-specific k-means clustering or offline learning), its indexing time is essentially zero. For a $d=1536$ dataset, TurboQuant's quantization took 0.0013 seconds, compared to 239.75 seconds for PQ and 2267.59 seconds for RabitQ.

---

## 5. Summary of Main Contributions

- **Algorithmic:** Introduced a novel data-oblivious quantization algorithm combining random rotations with optimal scalar continuous 1D k-means quantization.
- **Unbiased Inner Product Estimation:** Formulated a two-stage process that leverages the QJL transform on residual errors to fix the inherent bias of MSE quantizers.
- **Theoretical:** Provided rigorous, information-theoretic mathematical proofs that establish optimal lower limits for VQ and proved TurboQuant operates within a small constant of those limits (≈2.7).
- **Practical:** Achieved state-of-the-art results for online, dynamic AI workloads requiring fast, parallelizable (AVX/GPU-friendly) vector quantization, specifically in long-context LLM KV cache management and vector database indexing.

---

Paper link: https://drive.google.com/file/d/11kaC53L3F65HL9Cy-SXaZKRxvRLaCKaJ/view?usp=sharing
