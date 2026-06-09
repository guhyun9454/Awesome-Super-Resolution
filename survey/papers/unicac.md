# Towards Universal Computational Aberration Correction in Photographic Cameras: A Comprehensive Benchmark Analysis

- **Model / Abbrev:** UniCAC
- **Venue:** CVPR 2026 (2026)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2603.12083>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Computational Aberration Correction (CAC) restores images degraded by the optical aberrations (blur, chromatic fringing, spatially-varying PSFs) of real camera lenses. Existing CAC methods are tailored to a single specific optical system and generalize poorly to other lenses, and there is no large-scale, standardized benchmark to study what makes universal (lens-agnostic) CAC work.

## Method
The authors build UniCAC, a large-scale CAC benchmark generated via automatic optical design: a pipeline automatically designs diverse photographic camera lenses (released as Zemax .zmx/.zda files) and synthesizes the corresponding spatially-varying aberrated/clean image pairs, covering a wide range of optical degradations. They introduce the Optical Degradation Evaluator (ODE), a framework that objectively quantifies the severity/difficulty of each lens's aberration from its optical parameters, enabling difficulty-stratified evaluation rather than ad-hoc per-lens testing. They then run a systematic comparative study of 24 image-restoration / CAC algorithms (transformer-based e.g. Restormer/SwinIR, CNN e.g. NAFNet/MPRNet, GAN and diffusion-based generative methods) on this common benchmark, dissecting performance along three axes: prior utilization, network architecture, and training strategy.

## Key Contributions
- UniCAC: a large-scale, lens-diverse CAC benchmark constructed via automatic optical design, releasing image data (train/val/test), a lens/degradation library, Zemax optical-design files, and evaluation checkers
- Optical Degradation Evaluator (ODE): a framework to objectively quantify optical aberration severity and CAC task difficulty, enabling difficulty-stratified, system-agnostic benchmarking
- Comprehensive benchmark of 24 image-restoration and CAC algorithms under a unified protocol, the first large-scale comparison toward universal (cross-lens) aberration correction
- Identification of three influential factors for universal CAC performance: prior utilization, network architecture, and training strategy, providing design guidance for future generalizable CAC models

## Results
Abstract/repo report a systematic comparison of 24 restoration/CAC methods on the UniCAC benchmark evaluated with standard image-quality metrics (PSNR/SSIM/LPIPS via the ODE difficulty stratification); the public sources do not expose specific headline numerical scores. Key qualitative finding: performance is governed by prior utilization, network architecture, and training strategy, and methods tuned to single lenses generalize poorly across the diverse lens set.

## Significance
Provides the first standardized, optically-grounded benchmark plus a difficulty-quantification tool (ODE) for cross-lens aberration correction, shifting CAC research from per-lens overfitting toward universal, generalizable restoration and giving concrete architectural/training guidance backed by Zemax-accurate physical degradation.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*