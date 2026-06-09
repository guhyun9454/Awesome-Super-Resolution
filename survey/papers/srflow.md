# SRFlow: Learning the Super-Resolution Space with Normalizing Flow

- **Model / Abbrev:** SRFlow
- **Venue:** ECCV 2020 (2020)
- **Category:** Normalizing Flow
- **Paper:** <https://arxiv.org/abs/2006.14200>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Super-resolution is ill-posed: a single low-resolution (LR) input maps to many plausible high-resolution (HR) images, yet standard SR models learn a deterministic one-to-one mapping. GAN-based perceptual methods produce a single hallucinated output, are hard to train (adversarial + reconstruction + perceptual loss balancing), and cannot represent or sample the full space of valid SR solutions.

## Method
SRFlow is a conditional normalizing flow that directly learns the conditional distribution p(HR | LR) of high-res images given the LR input. An LR encoder g_theta (built on RRDB blocks, the ESRGAN backbone) extracts conditioning features; an invertible flow network f_theta maps an HR image to a Gaussian latent z conditioned on those features, using a multi-scale (squeeze + split) Glow-style architecture with conditional affine coupling layers, conditional affine injectors, actnorm, and invertible 1x1 convolutions. Being fully convolutional, it handles arbitrary input sizes. Training uses a single principled loss — negative log-likelihood (exact, via the change-of-variables formula with tractable Jacobian log-determinant) — with no adversarial or perceptual losses. At inference, sampling z at a chosen temperature and running the flow in reverse yields diverse photo-realistic SR outputs.

## Key Contributions
- First conditional normalizing flow for super-resolution that learns the full distribution p(HR|LR) rather than a deterministic mapping, enabling sampling of diverse, photo-realistic solutions
- Trained with a single negative-log-likelihood loss — removing the brittle multi-loss/adversarial training of GAN-based SR (ESRGAN-style)
- Conditional flow architecture: RRDB-based LR encoder feeding conditional affine coupling/injector layers in a multi-scale (squeeze/split) invertible network with tractable Jacobian
- Exact-likelihood model supports flexible image manipulation/editing in latent space and content-faithful sampling at controllable temperature
- Outperforms state-of-the-art GAN-based SR in both PSNR/fidelity and perceptual (LPIPS) quality while uniquely providing output diversity

## Results
Evaluated on faces (CelebA) and general images (DIV2K) at 4x and 8x scale. On CelebA, SRFlow more than halves the LPIPS distance versus its RRDB backbone (much better perceptual quality), trading off slightly lower PSNR/SSIM. On DIV2K, SRFlow achieves significantly better PSNR and LPIPS than ESRGAN, while also generating a diverse set of plausible SR samples. Beats SOTA GAN-based approaches on both fidelity and perceptual metrics simultaneously.

## Significance
Reframed super-resolution as conditional density estimation, showing a flow can match or beat GANs on perceptual quality while being stable to train (one NLL loss) and uniquely able to sample the SR solution space. It seeded a line of flow-based and stochastic SR work (e.g., SRFlow-DA, normalizing-flow restoration) and influenced the broader shift toward generative, distribution-modeling SR that diffusion models later extended.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*