# DACESR: Degradation-Aware Conditional Embedding for Real-World Image Super-Resolution

- **Model / Abbrev:** DACESR
- **Venue:** TIP 2026 (2026)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2602.23890>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Real-world image super-resolution faces complex, unknown degradations. Multimodal recognition models (RAM/DAPE) carry useful high-level semantic priors, but the authors find that directly fine-tuning RAM in the degraded space with naive contrastive learning fails to produce acceptable degradation-aware embeddings, limiting their usefulness as conditioning signals for restoration.

## Method
DACESR pairs a Real Embedding Extractor (REE) with a Mamba-based SR backbone, and is notably neither GAN- nor diffusion-based. The REE fine-tunes DAPE (derived from the Recognize Anything Model) via LoRA (rank r=8) using a degradation selection strategy: images are bucketed into mild vs. severe degradation by text-similarity thresholds (tau1=0.710, tau2=0.297) before contrastive/representation alignment, with an MSE loss on the embeddings. These degradation-aware conditional embeddings drive a Conditional Feature Modulator (CFM) that applies affine modulation CFM(x)=alpha*x+beta (alpha, beta derived from the embedding via resize/interpolation) into the SR network. The backbone uses Residual State Space Blocks (RSSBs, from DVMSR) — a Vision Mamba module with selective state-space sequence modeling — chosen because LAM analysis shows Mamba exploits a broader, more selective pixel range than CNN/Transformer baselines.

## Key Contributions
- Degradation selection strategy that splits images into mild/severe buckets by text-similarity thresholds, fixing the failure of naive contrastive fine-tuning of RAM/DAPE in the degraded space
- Real Embedding Extractor (REE): LoRA-fine-tuned (r=8) DAPE producing degradation-aware conditional embeddings, trained on COCO at 512x512 with MSE loss
- Conditional Feature Modulator (CFM) injecting REE embeddings into the backbone via affine feature modulation (alpha*x+beta)
- A lightweight (~3.65M param) Mamba/RSSB (DVMSR-style) SR backbone shown via LAM to use pixels more selectively than CNN/Transformer SR
- Demonstrates a non-GAN, non-diffusion route to balancing fidelity (PSNR) and perceptual quality (LPIPS) in real-world SR

## Results
4x SR. Reports PSNR (Y-channel) and LPIPS as primary metrics. Sample results: DIV2K-val Bicubic 28.34 PSNR / 0.139 LPIPS; Level-I degradation 28.16 / 0.142; AIM2019-val 24.01 / 0.252. Backbone ~3.65M params. Evaluated on Bicubic and synthetic Level-I/II/III degradations, RealSR-Canon/Nikon, AIM2019-val, and RealWorld38; SSIM/NIQE/MANIQA/CLIPIQA not in primary tables.

## Significance
Shows that high-level recognition-model priors can be made degradation-aware (via degradation-bucketed contrastive fine-tuning) and used as lightweight conditioning to steer a compact Mamba SR network, offering an efficient (~3.65M param) alternative to heavy GAN/diffusion real-world SR pipelines while balancing fidelity and perception.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*