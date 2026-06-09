# Empowering Semantic-Sensitive Underwater Image Enhancement with VLM

- **Model / Abbrev:** UIE-VLM (semantic-sensitive "-SS" plug-in strategy)
- **Venue:** AAAI 2026 (2026)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2603.12773>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Underwater image enhancement (UIE) networks pursue uniform global restoration, producing outputs whose distribution shifts from natural images and that neglect semantically important objects, which limits downstream vision-task performance (detection, segmentation). The goal is to make UIE semantic-sensitive, restoring key-object features faithfully.

## Method
A VLM (LLaVA) first generates textual descriptions of key objects from a degraded underwater image. A text-image alignment model (BLIP) then remaps these descriptions onto the image via patch-feature/text-embedding cosine similarity, producing a spatial semantic guidance map (sharpened with a power-law transform: s' = (max(0, N(s)-delta))^gamma, gamma>1). This map steers a standard encoder-decoder UIE network through a dual-guidance mechanism: (1) cross-attention injection where decoder features query encoder skip-connection features modulated by the semantic map at each stage, and (2) a semantic alignment loss combining background suppression and foreground enhancement terms. It is a model-agnostic plug-in strategy rather than a new backbone.

## Key Contributions
- First framework to use VLMs (LLaVA) plus text-image alignment (BLIP) to inject object-level semantic guidance into underwater image enhancement
- Spatial semantic guidance map built from BLIP patch-text cosine similarity with a power-law sharpening function (threshold delta, exponent gamma); ablation shows BLIP beats ViT and CLIP for this alignment
- Dual-guidance mechanism: semantic-modulated cross-attention on encoder skip connections + a two-term semantic alignment loss (background suppression ||F*(1-M)||_F^2 and foreground enhancement eta<F,M>), with L_total = L_recon + 0.1 * sum L_align
- Model-agnostic plug-in: applied to five UIE baselines (PUIE, SMDR, UIR, PFormer, FDCE), consistently boosting perceptual quality and downstream detection/segmentation

## Results
On UIEB (790 train / 100 test pairs), adding the semantic-sensitive strategy improves all five baselines, e.g. PFormer-SS 24.97 PSNR (+1.44) / 0.933 SSIM (+0.056) / 0.087 LPIPS; PUIE-SS 23.20 (+2.15) / 0.884 (+0.015); UIR-SS 24.62 (+1.73). Consistent UIQM/UCIQE gains on no-reference U45 and Challenge60. Downstream: object detection mAP +0.88 to +1.37 on Trash-ICRA19; semantic segmentation mIoU +1.93 to +5.41 on SUIM.

## Significance
Shows that VLM-derived object semantics can be cheaply injected into existing UIE networks to bias restoration toward task-relevant regions, improving not just image quality but downstream detection/segmentation, bridging low-level enhancement and high-level perception.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*