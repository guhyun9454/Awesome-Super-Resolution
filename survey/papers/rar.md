# Restore, Assess, Repeat: A Unified Framework for Iterative Image Restoration

- **Model / Abbrev:** RAR
- **Venue:** CVPR 2026 (2026)
- **Category:** All-in-one Restoration
- **Paper:** <https://arxiv.org/abs/2603.26385>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
All-in-one / unified image restoration methods generalize poorly and run inefficiently on unknown or composite degradations (mixed weather, blur, low light, noise). Treating restoration and quality assessment as separate stages incurs latency and pixel-space encode/decode information loss, and lacks a principled stopping signal for how much restoration is enough.

## Method
RAR fuses Image Quality Assessment (IQA) and Image Restoration (IR) into one model operating entirely in latent space, run as a closed Restore-Assess-Repeat loop. A Latent Quality Assessment (LQA) module adapts the VLM-based DepictQA to the restoration latent space via an image adapter, then a text/conditioning adapter maps its embeddings directly into the restoration backbone's conditioning space, avoiding lossy text decoding. The Unified Image Restoration (UIR) backbone is Stable Diffusion 3.5 reformulated with noise-free flow matching that directly maps degraded latents to high-quality latents (velocity ||v_theta(z_t,Q_deg,t)-(z_hq-z_deg)||^2), which keeps intermediate states clean enough for re-assessment. Each iteration runs T=4 restoration steps then LQA compares old vs new latents to emit a binary CONTINUE/STOP decision (adaptive stopping, N_max=8, ~2.4-2.9 rounds average).

## Key Contributions
- Unified latent-space framework jointly doing degradation identification, restoration, and quality verification in one end-to-end-trainable model
- Direct embedding alignment: maps LQA/IQA output embeddings to the restoration conditioning branch, eliminating lossy text encoding/decoding (+13.63% PSNR on unknown degradations vs text conditioning; text encoders CLIP-L/CLIP-G/T5-XXL removed at inference)
- Noise-free flow-matching restoration that maps degraded to HQ latents directly, enabling stable intermediate re-assessment (iterative training helps FM but harms standard diffusion)
- Quality-aware adaptive stopping criterion via the Restore-Assess-Repeat loop, removing external supervision and over-/under-restoration
- Three-stage training: image-adapter alignment, text-adapter training, then joint end-to-end fine-tuning; LoRA used for IQA fine-tuning

## Results
Composite degradations (MiO100 Groups A/B/C): PSNR 20.46/21.04/19.33, SSIM 0.714/0.733/0.658, LPIPS 0.130/0.127/0.149, MANIQA ~0.46; vs AgenticIR +72.4% MANIQA (Group A) and 11.27x faster. Unknown degradations (UDC): MUSIQ 55.97 vs 52.76, CLIP-IQA 0.602 vs 0.358 (AgenticIR). Single degradation (averaged over SIDD/Kodak24/DIV2K/GoPro/LOL/SOTS/Rain100/Raindrop): PSNR 25.88, SSIM 0.838, LPIPS 0.070, CLIP-IQA 0.557, MUSIQ 56.00. Efficiency: 6.29-6.92s wall-clock (vs AutoDIR 10.6-18.1s, AgenticIR 48-78s), ~2.8 iterations.

## Significance
RAR shows that coupling a learned IQA critic with a generative restorer in a shared latent loop yields a self-regulating restoration agent that adaptively decides when to stop, achieving SOTA perceptual quality on composite/unknown degradations while being an order of magnitude faster than agentic baselines like AgenticIR. It advances all-in-one restoration toward unified perception-action models and away from brittle multi-stage pixel-space pipelines.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*