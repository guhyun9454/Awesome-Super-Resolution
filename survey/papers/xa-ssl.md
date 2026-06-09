# Vascular anatomy-aware self-supervised pre-training for X-ray angiogram analysis

- **Model / Abbrev:** VasoMIM (paper abbrev XA-SSL; dataset XA-170K)
- **Venue:** AAAI 2026 (2026)
- **Category:** Scientific/Domain SR (medical-imaging self-supervised pre-training; not image super-resolution)
- **Paper:** <https://arxiv.org/abs/2602.11536>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Deep models for X-ray angiogram analysis are bottlenecked by scarce annotations, and large-scale self-supervised learning is underexplored in this domain due to the absence of both an effective SSL framework tailored to vascular structure and a large pre-training corpus.

## Method
VasoMIM is a vascular anatomy-aware Masked Image Modeling framework built on a ViT-B/16 MAE backbone (scaled to ViT-L/ViT-H). It injects domain knowledge via two designs: (1) an anatomy-guided masking strategy that preferentially masks vessel-containing patches so the encoder must learn robust vascular semantics rather than easy background; vessel maps come from a Frangi filter (multi-scale Hessian + percentile adaptive thresholding + region growing) combined with a co-guidance UNeXt-S segmentor (0.26M params) trained on Frangi pseudo-labels, fused as G = eta*B + (1-eta)*M (eta=0.5); (2) an anatomical consistency loss L_cons = L(S(I), S(I')) (cross-entropy between segmentor outputs on original vs reconstructed image) added to the standard MAE pixel MSE, enforcing vessel structural fidelity in reconstructions.

## Key Contributions
- VasoMIM: anatomy-aware MIM that combines vessel-targeted masking with an anatomical consistency loss for X-ray angiograms
- Anatomy-guided masking that biases masked patches toward vessel regions using Frangi-filter + lightweight UNeXt-S co-guidance pseudo-masks
- Anatomical consistency loss enforcing segmentation agreement between original and reconstructed images to preserve vascular topology
- XA-170K: largest X-ray angiogram pre-training dataset to date, 171,478 images from CADICA, SYNTAX, XCAD, and CoronaryDominance (160,320)
- Validation on 4 downstream tasks across 6 datasets, beating MAE/SimMIM and even DINOv3 (1.6B-image pretrain), showing domain-specific SSL beats generic large-scale SSL here

## Results
Vessel segmentation Dice: ARCADE-V 80.25% (MAE 79.39, UNet 71.44), XCAV 86.09% (MAE 84.84), stenosis seg ARCADE-S 92.57% (MAE 91.91); centerline Dice ARCADE-V 82.06% (MAE 80.74); stenosis detection mAP50 94.91% (MAE 92.30, Faster R-CNN 88.37), overall mAP 41.07%. Outperforms DINOv3 (1.6B-image pretrain) on segmentation. Backbone ViT-B 86M params (also ViT-L 307M, ViT-H 632M).

## Significance
Demonstrates that embedding explicit vascular anatomical priors into masked image modeling yields a transferable foundation model for cardiovascular X-ray imaging that surpasses generic web-scale SSL (DINOv3) despite far less data, and releases the field's largest angiogram pre-training corpus.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*