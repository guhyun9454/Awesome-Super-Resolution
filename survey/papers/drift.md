# DRIFT: Deep Restoration, ISP Fusion, and Tone-mapping

- **Model / Abbrev:** DRIFT (DRIFT-MFP + DRIFT-TM)
- **Venue:** CVPR 2026 (2026)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2604.03402>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Smartphone cameras need high-quality, high-dynamic-range RGB output from noisy hand-held raw bursts, but mobile ISPs must run a full restoration + tone-mapping pipeline under tight compute/latency and NPU operator constraints. DRIFT targets an efficient, deployable end-to-end mobile camera pipeline from raw bursts to tone-mapped RGB.

## Method
Two-stage pipeline. Stage 1, DRIFT-MFP, is a NAFNet backbone (activation-free, only simple norm+conv, no deformable convs or transformers for Snapdragon NPU compatibility) that ingests 11 RGB frames (33 channels) and jointly performs multi-frame alignment, denoising, demosaicing, and 4x super-resolution, outputting one RGB image. It is trained with an L1 fidelity loss plus GAN loss plus a novel Adversarial Perceptual Loss (APL) that uses the discriminator's own pre-activation features rather than a frozen VGG to avoid the classification-vs-restoration domain gap that causes LPIPS grid artifacts. Stage 2, DRIFT-TM, is a deep tone-mapper with a dual encoder (global low-res + local full-res tile) plus a metadata encoder (sensor type, ISO, exposure); it predicts residual luma/chroma fusion weights and a luma contrast gain-map on top of a lightweight 'Tone-map Lite' baseline, trained against dual contrast-off/contrast-on references with L1+SSIM losses.

## Key Contributions
- Unified mobile-efficient MFP network (NAFNet) doing alignment+denoise+demosaic+4x SR on 11 raw frames with no NPU-unsupported ops
- Adversarial Perceptual Loss (APL) using discriminator pre-activation features instead of frozen VGG, removing LPIPS-induced grid artifacts in raw restoration
- Realistic handheld motion modeling via homography extraction with temporally-correlated sampling (groups of 10 sequential homographies)
- DRIFT-TM tone-mapper with global+local+metadata encoders predicting residuals over a Tone-map Lite baseline, with LUT-based tunability (G_phi, H_theta0/1) at inference without retraining
- Full pipeline runs <4s on a Snapdragon 8 Elite NPU at 12MP

## Results
MFP denoising: PSNR 37.49, SSIM 0.97, LPIPS 0.05, FID 10.73 (competitive with NAFNet+LPIPS/Burstormer but artifact-free; user study 63% preferred DRIFT-MFP over NAFNet+LPIPS). 4x SR: PSNR 36.22, SSIM 0.94, LPIPS 0.10, FID 20.84. Tone-mapping vs GT: PSNR 40.59, SSIM 0.99 (vs LLF-LUT 30.89/0.95, TMO-GAN 30.83/0.96); TMQI-Q 0.845 (GT 0.847). Latency: MFP 3.2s, TM 0.5s, total <4s on Snapdragon 8 Elite NPU for 11-frame 12MP burst. Trained on Galaxy S24/S25 Ultra captures (1,300 tripod pairs + 1,200 handheld for homographies; 2,000 raw captures for TM).

## Significance
Demonstrates a practical, NPU-deployable end-to-end raw-to-RGB camera pipeline that fuses learned restoration, super-resolution, and tunable tone-mapping at smartphone latency, while showing that a discriminator-feature perceptual loss can match LPIPS-style perceptual quality without its characteristic grid artifacts.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*