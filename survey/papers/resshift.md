# ResShift: Efficient Diffusion Model for Image Super-resolution by Residual Shifting

- **Model / Abbrev:** ResShift
- **Venue:** NeurIPS 2023 (2023)
- **Category:** Diffusion SR (Efficient)
- **Paper:** <https://arxiv.org/abs/2307.12348>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion-based image super-resolution (SR) delivers strong perceptual quality but is bottlenecked by slow inference, requiring hundreds-to-thousands of sampling steps. ResShift targets drastically cutting the number of sampling steps while keeping competitive SR quality.

## Method
Instead of diffusing from the HR image to pure Gaussian noise (as in standard DDPM SR), ResShift builds a shorter Markov chain whose endpoints are the HR image and the LR image. The forward process progressively shifts the residual between HR and LR images (e_0 = y_LR - x_HR), so the chain transitions directly between the two resolution states rather than to white noise; this makes the start point already a strong estimate, slashing the steps needed. An elaborate noise schedule jointly controls the residual-shifting speed and the noise strength at each step. The model is trained in the latent space of a pretrained VQGAN autoencoder, with a SwinUNet-based denoiser/transition network optimizing a residual-prediction objective.

## Key Contributions
- Residual-shifting diffusion formulation: a Markov chain between HR and LR images (shifting the HR-LR residual) replacing the HR-to-noise chain, enabling far fewer sampling steps
- An elaborate, flexible noise schedule that controls both the shifting speed and noise strength for efficient transitions
- Only 15 sampling steps to reach SOTA-comparable SR (vs hundreds/thousands for prior diffusion SR); the journal/v3 extension pushes this to as few as 4 steps
- SwinUNet transition network operating in a pretrained VQGAN latent space for efficiency
- Strong real-world SR results on real-world benchmarks without relying on hand-tuned long schedules

## Results
Achieves SR quality comparable or superior to SOTA diffusion methods (e.g., LDM/StableSR-style baselines) using only 15 sampling steps; the extended journal version reaches similar quality in 4 steps. Evaluated on synthetic SR benchmarks (e.g., ImageNet-Test) and real-world benchmarks RealSet65/RealSet80 using no-reference perceptual metrics (CLIPIQA, MUSIQ) alongside PSNR/SSIM/LPIPS; reports favorable perception-distortion trade-off versus GAN-based (Real-ESRGAN/BSRGAN) and multi-step diffusion baselines. Exact numbers are reported in Tables 3-4 of the NeurIPS paper.

## Significance
ResShift reframes diffusion SR as a short residual-shifting Markov chain rather than a noise-to-image process, cutting sampling cost by 1-2 orders of magnitude (15, later 4, steps) while preserving quality. It is an influential precursor to the wave of few-step/one-step diffusion SR work and remains a standard efficient-diffusion-SR baseline.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*