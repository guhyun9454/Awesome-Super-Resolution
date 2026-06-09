# SAT: Selective Aggregation Transformer for Image Super-Resolution

- **Model / Abbrev:** SAT
- **Venue:** CVPR 2026 (2026)
- **Category:** Transformer SR
- **Paper:** <https://arxiv.org/abs/2604.07994>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Transformer-based SR models long-range dependencies well but vanilla self-attention is O(N^2), forcing a trade-off between efficiency and global context. SAT targets cheap global attention without sacrificing reconstruction fidelity / high-frequency detail.

## Method
SAT uses Selective Aggregation Attention (SAA): an asymmetric query/key-value scheme that keeps the full-resolution query (N x d) but compresses keys and values to K x d (K << N, ~3% of tokens), cutting attention from O(N^2 d) to O(N K d). The K aggregated KV tokens are produced by a Density-driven Token Aggregation (DTA) algorithm with three steps: (1) Density-Guided Center Selection picks K cluster centers using a local density rho_i (kNN cosine similarity) times an isolation metric delta_i (distance to nearest denser token), score gamma_i = rho_i * delta_i; (2) Stratified Subsampling partitions tokens into K contiguous regions and samples S = beta*K (beta>=2) per region to avoid O(N^2 C) pairwise cost, reducing center selection to O(K^2 C); (3) Similarity-Weighted Aggregation with softmax weights w_i = exp(s(x_i,c_k)/tau), plus Feature Norm Restoration (FNR) that rescales aggregated tokens to the global max norm to preserve magnitude distributions for layer norm. The backbone is a residual-in-residual stack alternating Local Transformer Blocks (LTB) and Selective Aggregation Transformer Blocks (SATB), with embedding, refinement, and upscale modules. Trained on Y-channel PSNR/SSIM with the BasicSR framework.

## Key Contributions
- Selective Aggregation Attention (SAA): asymmetric full-query / compressed-KV attention reducing KV tokens by ~97% and complexity from O(N^2 d) to O(N K d)
- Density-driven Token Aggregation (DTA) using density (rho) and isolation (delta) metrics to choose K cluster centers that preserve high-frequency detail
- Stratified subsampling to make center selection scalable (O(K^2 C) instead of O(N^2 C))
- Feature Norm Restoration (FNR) to rescale aggregated tokens to the global max norm, stabilizing layer normalization
- Theoretical guarantees: Theorem 3.1 speedup Theta(N/K); Theorem 3.2 approximation-error bound under Lipschitz assumptions

## Results
Trained on DF2K (DIV2K+Flickr2K), evaluated on Set5/Set14/BSD100/Urban100/Manga109 at x2/x3/x4. At x4: 19.5M params, 0.94T FLOPs, 32.85 dB PSNR on Manga109 vs PFT's 19.8M / 1.26T / 32.63 dB — a 0.22 dB gain with 25% fewer FLOPs (27% at x2). Reported SOTA vs EDSR, RCAN, IPT, SwinIR, CAT-A, HAT, IPG, ATD, PFT.

## Significance
Shows that aggressive (97%) KV token reduction guided by density/isolation clustering can beat heavier transformer SR baselines while cutting FLOPs ~25-27%, advancing the efficiency-vs-global-context frontier for transformer super-resolution with both empirical SOTA and theoretical error bounds.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*