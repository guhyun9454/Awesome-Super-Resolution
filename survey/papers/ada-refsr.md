# Trust but Verify: Adaptive Conditioning for Reference-Based Diffusion Super-Resolution via Implicit Reference Correlation Modeling

- **Model / Abbrev:** Ada-RefSR (AdaRefSR)
- **Venue:** ICLR 2026 (2026)
- **Category:** Reference-based SR
- **Paper:** <https://arxiv.org/abs/2602.01864>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
In reference-based super-resolution (RefSR), real-world degradations make correspondences between low-quality (LQ) inputs and high-quality reference (Ref) images unreliable, so naive reference fusion injects wrong details. The paper tackles how to adaptively decide when to trust a reference and when to suppress it, while keeping diffusion-based SR fast.

## Method
A single-step diffusion RefSR framework built on the S3Diff backbone (distilled from SD-Turbo/Stable Diffusion), with frozen base weights. The core is Adaptive Implicit Correlation Gating (AICG): a few learnable summary tokens distill dominant reference patterns from the reference keys via attention aggregation; softmax attention between the LQ query and these token summaries produces per-token gates through sigmoid modulation; the reference attention output is then elementwise-modulated by these gates. This implicitly models LQ-Ref correlation by reusing existing attention projections rather than doing explicit dense patch matching, acting as a built-in "verify" safeguard against erroneous fusion. RAM Swin-L and SeeSR's DAPE provide semantic conditioning.

## Key Contributions
- Adaptive Implicit Correlation Gating (AICG): learnable summary tokens + sigmoid per-token gates that implicitly model LQ-Ref correlation and adaptively regulate reference contribution, replacing explicit/dense correspondence matching
- A 'Trust but Verify' single-step diffusion RefSR design that aggressively injects reference patterns then verifies/suppresses unreliable ones, giving >30x speedup over multi-step RefSR baselines
- Parameter-efficient: only 62.2M trainable params (reference attention + summary tokens) out of a 2.68B total frozen model
- New/curated evaluation benchmarks: Bird (8,460 images, CLIP/DINOv2-paired) and Face (162 pairs, 40 identities), plus training with ~20% deliberately irrelevant Ref pairings to stress-test robustness
- Robust performance in both well-aligned and mismatched reference conditions

## Results
Single-step inference: 0.41s at 512x512 (12.66 GB) and 1.35s at 1024x1024 (15.54 GB); >30x faster than multi-step RefSR. CUFED5: PSNR 20.48 / SSIM 0.5461 / LPIPS 0.2894. WRSR: PSNR 21.97 / SSIM 0.5777 / LPIPS 0.3061. Bird: PSNR 25.30 / SSIM 0.729. Face: PSNR 27.13 / SSIM 0.752 / LPIPS 0.175. Training loss = lambda1*L_rec(L2) + lambda2*L_per(VGG) + lambda3*L_adv. Training data: DIV2K, DIV8K, Flickr2K (512x512), CelebFaceRef-HQ. Reported to outperform ReFIR in leveraging reference detail.

## Significance
Addresses a central failure mode of RefSR (unreliable correspondences under degradation) with a lightweight gating mechanism instead of costly explicit matching, while making RefSR practical via one-step diffusion (sub-second inference). It advances robust, efficient reference conditioning that degrades gracefully to single-image SR when references are untrustworthy.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*