# VOSR: A Vision-Only Generative Model for Image Super-Resolution

- **Model / Abbrev:** VOSR
- **Venue:** CVPR 2026 (2026)
- **Category:** Diffusion SR
- **Paper:** <https://arxiv.org/abs/2604.03225>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
State-of-the-art generative SR methods inherit heavy pretrained text-to-image (T2I) diffusion models (e.g. Stable Diffusion), which carry large training cost, a text-conditioning paradigm mismatched to restoration, and a tendency to hallucinate. VOSR asks whether competitive generative SR can be built from scratch using only visual data, with no text-image multimodal supervision.

## Method
VOSR is a vision-only diffusion-transformer SR model built on a LightningDiT backbone (0.5B and 1.4B variants) trained from scratch, with no T2I prior. Instead of text prompts, it uses a frozen pretrained vision encoder (DINOv2; CLIP/SigLIPv2/DINOv3 also ablated) to extract spatially grounded semantic features from the LR image as conditioning. It revisits classifier-free guidance and shows the standard unconditional branch is ill-suited to from-scratch restoration; it is replaced by a restoration-oriented guidance strategy whose "negative" branch is a partially conditioned branch that drops semantic guidance but retains a weakened LR structural anchor (scaled by alpha in [0.05,0.25]), steering toward fidelity at high scales and generation at low scales. A multi-step model is trained first, then distilled into a one-step model via a recursive-consistency (RC) scheme (teacher-guided warm start with multi-step ODE rollout and corrected targets), which beats shortcut-style distillation.

## Key Contributions
- First competitive generative SR model trained entirely from scratch on vision-only data (no T2I prior, no text supervision), using a frozen vision encoder (DINOv2) for LR semantic guidance instead of text prompts
- Restoration-oriented guidance: replaces the standard CFG unconditional branch with a partially-conditioned branch that retains a weakened LR anchor (alpha in [0.05,0.25]) and removes semantics, fixing the failure mode of vanilla CFG in from-scratch restoration models
- Recursive-consistency (RC) distillation of the multi-step teacher into a one-step model, outperforming shortcut-based distillation in the perception-fidelity tradeoff
- LightningDiT-based DiT backbone at 0.5B and 1.4B scales; trains at <1/10 the cost of representative T2I-based SR methods
- New ScreenSR benchmark (130 paired smartphone captures of screens) plus evaluation on RealSR and synthetic LSDIR

## Results
On RealSR (4x), multi-step VOSR-1.4B: LPIPS 0.2961, MUSIQ 70.47, PSNR 25.29, beating SeeSR (LPIPS 0.3007/MUSIQ 69.82) and DiT4SR; ResShift (vision-only) lags at LPIPS 0.3468/MUSIQ 58.47 (higher PSNR 26.26). One-step VOSR-1.4B: LPIPS 0.2732, MUSIQ 70.58 at 0.095s, competitive with PiSA-SR (0.2672/70.14) and ahead of OSEDiff (0.2920/69.08). Efficiency: multi-step 512x512 25-step inference 2.114s vs StableSR 10.036s; one-step ~0.094-0.095s. Trained on ~100M filtered web images with Real-ESRGAN degradation, progressive 256->512 schedule. Human studies favored VOSR-1.4B for both perceptual quality and LR consistency.

## Significance
Demonstrates that the dominant T2I-prior paradigm for generative SR is not necessary: a from-scratch vision-only model matches or beats T2I-based methods (SeeSR, OSEDiff, PiSA-SR, StableSR) in perceptual quality and efficiency at roughly one-tenth the training cost, while producing more faithful structures with fewer hallucinations. It reframes guidance design specifically for restoration and offers a cheaper, more controllable alternative to SD-based SR.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*