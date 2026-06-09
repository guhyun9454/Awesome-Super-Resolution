# DiT4SR: Taming Diffusion Transformer for Real-World Image Super-Resolution

- **Model / Abbrev:** DiT4SR
- **Venue:** ICCV 2025 (2025)
- **Category:** Diffusion SR (DiT)
- **Paper:** <https://arxiv.org/abs/2504.20461>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Real-world image super-resolution (Real-ISR) needs strong generative priors to hallucinate realistic detail from degraded low-resolution inputs. Diffusion Transformers (DiT) outperform UNet diffusion backbones in generation, but it was unclear how to effectively condition a large-scale DiT on LR images for Real-ISR, since the ControlNet-style additive injection used for UNets does not transfer well to the DiT architecture.

## Method
DiT4SR adapts the DiT-based SD3 (SD3.5-medium) text-to-image model for Real-ISR. Rather than injecting LR features with a separate ControlNet branch, it embeds the LR latent directly into DiT's original multi-modal self-attention so information flows bidirectionally between the LR stream and the generated latent. This lets the LR-conditioning stream co-evolve through the diffusion trajectory, yielding progressively refined guidance that stays aligned with the generated latent at each timestep. A cross-stream convolution layer injects the LR guidance into the generated latent, compensating for DiT's weak local-detail modeling (transformers favor global over local structure). Trained on LR-HR pairs at 512x512.

## Key Contributions
- One of the first works to tame a large-scale Diffusion Transformer (SD3/SD3.5-medium) for Real-ISR instead of UNet-based diffusion priors
- Integrates LR embeddings into DiT's native attention for bidirectional LR-to-generated information flow, replacing ControlNet-style one-way injection
- LR stream evolves jointly with the diffusion process, producing timestep-adaptive (progressively refined) conditioning aligned with the generated latent
- Cross-stream convolution module that injects LR guidance to recover local detail that the global attention of DiT tends to miss
- Introduces the NKUSR8K dataset and uses VLM-generated prompts (CLIP-ViT / LLaVA) for semantic guidance; releases fidelity-oriented (dit4sr_f) and quality-oriented (dit4sr_q) variants

## Results
Evaluated on DRealSR, RealSR, RealLR200, RealLQ250 with perceptual/no-reference metrics (authors argue PSNR/SSIM poorly reflect visual quality, so report LPIPS + no-ref). DRealSR: LPIPS 0.319, MUSIQ 68.07, MANIQA 0.661, CLIPIQA 0.550, LIQE 3.96. RealSR: MUSIQ 70.47, MANIQA 0.645, CLIPIQA 0.588, LIQE 3.98. RealLR200: MUSIQ 71.83, MANIQA 0.632, CLIPIQA 0.578. Trained/evaluated at 512x512. Diffusion step count not explicitly stated in accessible text (multi-step DiT sampler).

## Significance
Demonstrates that large DiT/SD3 generative priors can be successfully adapted to Real-ISR, and that attention-level bidirectional LR conditioning plus cross-stream convolution outperforms naive ControlNet-style injection on DiT, opening a strong new backbone direction for diffusion-based restoration beyond the dominant UNet/SD-2 paradigm.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*