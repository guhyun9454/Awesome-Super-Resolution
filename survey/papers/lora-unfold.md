# LoRun: Deep LoRA-Unfolding Networks for Image Restoration

- **Model / Abbrev:** LoRun
- **Venue:** TIP 2026 (2026)
- **Category:** Other/Tangential
- **Paper:** <https://arxiv.org/abs/2602.18697>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Deep unfolding networks (DUNs) for image restoration cascade K stages, each with its own full Proximal Mapping Module (denoiser). This causes two problems: every stage uses an identical PMM architecture (ignoring stage-specific noise-level differences) and the per-stage replicated denoisers create severe parameter redundancy and high memory cost that scales linearly with the number of stages.

## Method
LoRun unfolds a proximal gradient descent (PGD) algorithm into K stages, each with a Gradient Descent Module (GDM, z = x - rho*Phi^T(Phi*x - y)) and a Proximal Mapping Module (PMM, a denoiser handling Gaussian noise at level sqrt(rho*lambda)). Instead of replicating the denoiser per stage, a single pretrained base denoiser backbone is shared across all stages with its weights frozen; lightweight, stage-specific LoRA adapters (y = (W0 + AB)x, A in R^{n1xr}, B in R^{rxn2}, r << min) are injected into the PMMs to dynamically modulate denoising behavior to each stage's noise level. Rank is set adaptively as r = ceil(min(in,out) * gamma/100), gamma typically 10. Training is two-phase: pretrain the backbone on a single block, then freeze it and fine-tune only the per-stage LoRA modules across K stages, with an L2 reconstruction loss.

## Key Contributions
- First to apply LoRA inside deep unfolding networks: one shared frozen backbone denoiser plus K lightweight stage-specific LoRA adapters, replacing K full per-stage denoisers
- Achieves up to N-times parameter reduction for an N-stage DUN while matching or exceeding full-model performance
- Adaptive LoRA rank rule r = ceil(min(in_dim,out_dim) * gamma/100) for per-layer rank selection
- Two-phase training (backbone pretrain, then frozen-backbone LoRA fine-tuning) that lets each stage adapt to its own noise level
- Architecture-agnostic: demonstrated with U-Net, Transformer, and CNN backbones across three restoration tasks

## Results
9-stage networks. Compressive Sensing (General100, ratio 25%): 38.16 dB PSNR with 5.23M params vs Block-9 baseline's 15.96M (~33% of params, ~33% GPU memory cut). Tested on Set11, Set14, General100 at ratios {1,4,10,25}%. CASSI (10 KAIST scenes, 256x256x28): 40.27 dB PSNR with 1.62M params vs RCUMP's 9.03M (17.9% of params, +1.3 dB). SR (BSD68, Set5; scales x2/x3/x4 across 12 blur kernels): params cut from 5.3M to 1.96M (~63% reduction) with comparable PSNR. Loss is mean squared error.

## Significance
Reframes parameter efficiency in deep unfolding by borrowing LoRA from LLM fine-tuning, decoupling the number of unfolding stages from parameter/memory growth. This makes deeper, higher-accuracy unfolding networks feasible under tight memory budgets and is generic across restoration tasks and backbones.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*