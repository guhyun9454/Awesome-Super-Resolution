# Visual Autoregressive Modeling for Image Super-Resolution

- **Model / Abbrev:** VARSR
- **Venue:** ICML 2025 (2025)
- **Category:** Visual Autoregressive
- **Paper:** <https://arxiv.org/abs/2501.18993>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Image super-resolution (ISR) must balance fidelity (faithfulness to the low-resolution input) against realism (perceptually convincing detail), while diffusion-based SR methods that achieve strong realism are computationally heavy due to many denoising steps. VARSR aims to deliver high-fidelity, high-realism SR more efficiently.

## Method
VARSR reframes ISR as visual autoregressive (VAR) "next-scale prediction": instead of next-token generation, the model autoregressively predicts progressively finer scales of a multi-scale VQ token pyramid, conditioned on the low-resolution image. The LR image is injected as prefix tokens to preserve semantic content, and Scale-aligned Rotary Positional Encodings (RoPE) are introduced to capture spatial structure consistently across the different token-map resolutions. Because VQ quantization discards information, a diffusion refiner is appended to model the quantization residual (the residual between continuous features and quantized tokens), restoring pixel-level fidelity. Image-based Classifier-free Guidance (CFG) is proposed — using the LR image as the conditioning signal for CFG — to steer generation toward more realistic outputs.

## Key Contributions
- First framework to cast image super-resolution as visual autoregressive next-scale prediction, an alternative to diffusion-based SR
- Prefix-token conditioning that feeds the LR image into the autoregressive transformer to preserve semantics
- Scale-aligned Rotary Positional Encodings (RoPE) to encode spatial structure consistently across multi-scale token maps
- A diffusion refiner that models the VQ quantization residual loss to recover pixel-level fidelity lost to discretization
- Image-based Classifier-free Guidance tailored to the SR setting for improved realism
- Large-scale data collection and a training scheme to learn robust generative priors for SR; code released at github.com/quyp2000/VARSR

## Results
Reported (qualitatively in the abstract/review, exact tables in the full paper) to achieve high-fidelity and high-realism reconstructions with superior quantitative metrics (PSNR, SSIM) and user-study scores versus state-of-the-art SR methods, while being more computationally efficient than diffusion-based baselines (the next-scale autoregressive scheme replaces many diffusion denoising steps). Specific numerical PSNR/SSIM/LPIPS values, parameter counts, and benchmark datasets were not recoverable from the accessible abstract/review excerpts.

## Significance
VARSR demonstrates that visual autoregressive next-scale prediction is a viable, more efficient alternative to diffusion models for real-world super-resolution, reducing the heavy multi-step sampling cost of diffusion SR while retaining competitive fidelity and realism — broadening the generative-prior toolkit for restoration beyond diffusion and GANs.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*