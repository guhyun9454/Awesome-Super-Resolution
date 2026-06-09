# Toward Real-world Infrared Image Super-Resolution: A Unified Autoregressive Framework and Benchmark Dataset

- **Model / Abbrev:** Real-IISR
- **Venue:** CVPR 2026 (2026)
- **Category:** Generative/Autoregressive
- **Paper:** <https://arxiv.org/abs/2603.04745>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Real-world infrared image super-resolution (IISR) is largely unexplored: real IR images suffer coupled optical and sensing degradations (defocus, motion blur, sensor noise) that jointly destroy both structural sharpness and thermal (radiometric) fidelity, and prior IISR work relies on synthetic bicubic degradations with no real paired benchmark.

## Method
Real-IISR is a unified Visual Autoregressive (VAR) framework that reconstructs IR images via next-scale prediction (scale-by-scale), built on a pretrained VAR backbone. A Thermal-Structural Guidance (TSG) module builds dual priors from the LR input — a heat map (thermal semantics) and an edge map (geometry) — encoded by DINOv3-based encoders, adaptively fused via attention weights (F_Fused = F_Heat*W + F_Edge*(1-W)), and injected into LR features by cross-attention to bridge the thermal-radiation/structural-edge mismatch. A Condition-Adaptive Codebook (CAC) perturbs the VQ codebook embeddings with degradation-conditioned low-rank updates (rank r=8, tanh-gated) for spatially adaptive code selection. A Thermal Order Consistency (TOC) loss enforces a monotonic temperature-to-intensity ordering across 8x8 patches, robust to LR-HR misalignment.

## Key Contributions
- FLIR-IISR: first real-world paired IISR benchmark — 1,457 real LR-HR pairs (train/test 1,192/265) captured with a FLIR T1050sc at 1024x768, spanning 12 scenes, 6 cities, 3 seasons; LR obtained via automated focus variation and real object motion (1,305 defocus + 152 motion-blur), lossless BMP
- Unified autoregressive (VAR / next-scale prediction) framework for real-world IISR, the first to bring visual autoregression to this task
- Thermal-Structural Guidance (TSG) module fusing DINOv3-encoded heat-map and edge-map priors via adaptive attention and cross-attention injection
- Condition-Adaptive Codebook (CAC): degradation-aware low-rank (r=8) perturbation of discrete codebook for spatially adaptive quantization
- Thermal Order Consistency (TOC) loss preserving monotonic temperature-intensity relation, misalignment-robust

## Results
On FLIR-IISR (4x, 128->512): PSNR 29.51, SSIM 0.8895, LPIPS 0.1340, MUSIQ 57.06, beating DifIISR (28.56/0.8474/0.2739) and VARSR (28.34/0.8613/0.2003). Model ~1,144.6M params, 2.45 FPS on one A800 (~6% faster than VARSR despite larger size). Trained with AdamW (lr 5e-5, wd 5e-2), batch 4 on 4xA800, 10k fine-tuning steps, loss weights lambda1=0.2 (MSE) and lambda2=0.8 (TOC). Compared against 9 baselines (HAT, BI-DiffSR, PFT-SR, CoRPLE, InfraFFN, DifIISR, RealSR, SinSR, VARSR), all retrained on FLIR-IISR.

## Significance
Establishes the first real-world paired IR super-resolution benchmark and shows that a thermally-guided visual-autoregressive model can jointly restore structural detail and radiometric (temperature) fidelity, outperforming both diffusion-based and prior VAR SR methods in distortion and perceptual quality — providing a unified foundation for real-world IISR research and evaluation.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*