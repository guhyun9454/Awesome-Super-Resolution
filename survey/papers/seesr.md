# SeeSR: Towards Semantics-Aware Real-World Image Super-Resolution

- **Model / Abbrev:** SeeSR
- **Venue:** CVPR 2024 (2024)
- **Category:** Diffusion SR
- **Paper:** <https://arxiv.org/abs/2311.16518>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion-prior-based real-world super-resolution (Real-ISR) leverages pretrained T2I models for realistic detail generation, but heavy LR degradation corrupts the semantic cues these models rely on. This causes semantic errors (wrong objects/textures, hallucinated content) and loss of fidelity, since text prompts extracted from degraded inputs are unreliable.

## Method
SeeSR conditions a ControlNet-style Real-ISR pipeline on degradation-robust semantic prompts. A Degradation-Aware Prompt Extractor (DAPE), built by LoRA-fine-tuning the RAM (Recognize Anything Model) tagger, produces both "hard" prompts (discrete image tags/text) and "soft" prompts (representation embeddings) that stay accurate under strong degradation; DAPE is trained to align LR-image encodings to the corresponding HR encodings. These prompts drive a frozen Stable Diffusion 2-base UNet via a trainable ControlNet branch augmented with Representation Cross-Attention (RCA) modules inserted after the text cross-attention layers, so soft semantic representations inject fine-grained local guidance. At inference, an LR Embedding (LRE) trick embeds the LR latent into the initial Gaussian sampling noise per the training noise schedule, suppressing spurious random details in smooth regions. Sampling uses 50-step spaced DDPM.

## Key Contributions
- Degradation-Aware Prompt Extractor (DAPE): RAM tagger fine-tuned with LoRA (r=8, 20K iters) to emit degradation-robust hard tags + soft representation embeddings aligned to HR features
- Dual prompting: hard prompts (tags) for global/semantic correctness plus soft prompts (embeddings) injected via Representation Cross-Attention (RCA) for local detail guidance
- LR Embedding (LRE) inference trick: initialize sampling noise from the LR latent to curb hallucinated details in flat regions and improve fidelity
- ControlNet conditioning on a frozen SD 2-base T2I prior, trained on LSDIR + 10K FFHQ faces with the Real-ESRGAN degradation pipeline

## Results
RealSR: PSNR 25.18 / SSIM 0.7216 / LPIPS 0.3009 / MUSIQ 69.77 / CLIPIQA 0.6612. DRealSR: PSNR 28.17 / SSIM 0.7691 / LPIPS 0.3189 / MUSIQ 64.93 / CLIPIQA 0.6804. DIV2K-Val: PSNR 21.19 / SSIM 0.5386 / LPIPS 0.3843 / MUSIQ 68.33 / CLIPIQA 0.6946. Reported best LPIPS/DISTS on DIV2K-Val and best FID, CLIPIQA and MUSIQ across all four benchmarks; 50 DDPM steps.

## Significance
SeeSR set a strong standard for semantic fidelity in diffusion-based Real-ISR by making the conditioning prompts robust to degradation rather than relying on raw text from corrupt inputs, reducing semantic hallucination while keeping high perceptual quality. It became a widely cited baseline that subsequent one-step and re-focused-conditioning Real-ISR methods (e.g., SRSR) compare against.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*