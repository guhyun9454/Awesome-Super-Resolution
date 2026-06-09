# SinSR: Diffusion-Based Image Super-Resolution in a Single Step

- **Model / Abbrev:** SinSR
- **Venue:** CVPR 2024 (2024)
- **Category:** One-step Diffusion
- **Paper:** <https://arxiv.org/abs/2311.14760>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion-based super-resolution (SR) produces high perceptual quality but needs many iterative denoising steps, making inference slow. Even acceleration methods like ResShift still require ~15 steps. SinSR aims to collapse this to a single network forward pass without sacrificing (and ideally improving) quality.

## Method
SinSR distills the multi-step ResShift teacher (a residual-shifting Markov diffusion SR model on a VQGAN latent/image space) into a one-step student. First it reformulates ResShift's stochastic Markov reverse chain into a non-Markovian deterministic sampling process q(x_{t-1}|x_t,x_0,y)=delta(k_t·x_0 + m_t·x_t + j_t·y), giving a fixed, invertible noise->HR mapping the student can learn directly. The student is then trained to predict the teacher's deterministic output in one step (NFE=1). The key trick is a consistency-preserving loss that does not blindly imitate the teacher: it uses a forward distillation term, an inverse/bidirectional mapping term, and a ground-truth consistency term that re-derives a noise code from the GT image (x_hat_T = detach(f(x_0,y,0))) and forces the one-step output back to x_0, letting the student exceed the teacher's feature manifold.

## Key Contributions
- Reduces diffusion-based SR to a single inference step (NFE=1), giving roughly 10x speedup over the 15-step ResShift teacher
- Derives a deterministic, non-Markovian (DDIM-like) reverse process from ResShift's stochastic formulation, enabling a fixed bidirectional noise<->HR mapping for distillation
- Introduces a consistency-preserving distillation loss combining forward distillation, inverse-mapping, and ground-truth consistency terms so the student is supervised by real HR images, not only the teacher
- Shows that leveraging ground-truth during distillation lets the one-step student match or surpass the teacher on perceptual no-reference metrics

## Results
On ImageNet-Test (x4): SinSR-1 vs ResShift-15 -> PSNR 24.56 vs 24.90, SSIM 0.657 vs 0.673, LPIPS 0.221 vs 0.228, CLIPIQA 0.611 vs 0.603, with NFE 1 vs 15. Real-world RealSR: CLIPIQA 0.689 vs 0.596, MUSIQ 61.58 vs 59.87. RealSet65: CLIPIQA 0.715 vs 0.654, MUSIQ 62.17 vs 61.33. So SinSR slightly trades PSNR/SSIM for better perceptual/no-reference quality at ~10x faster inference (single step).

## Significance
Demonstrates that diffusion SR can run in a single forward pass while preserving or improving perceptual quality, removing the central efficiency barrier of diffusion SR. It established an influential distillation recipe (deterministic sampling + GT-consistency loss) that helped launch the one-step diffusion SR line later extended by methods like OSEDiff and SinSR follow-ups.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*