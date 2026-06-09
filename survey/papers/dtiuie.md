# Downstream Task Inspired Underwater Image Enhancement: A Perception-Aware Study from Dataset Construction to Network Design

- **Model / Abbrev:** DTI-UIE (DTIUIE)
- **Venue:** TIP 2026 (2026)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2603.01767>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Underwater image enhancement (UIE) is usually optimized for human visual perception, but such enhancement does not necessarily help — and can hurt — downstream recognition tasks (semantic/instance segmentation, object detection). The paper reframes UIE as a task-driven preprocessing problem, tackling both the absence of task-oriented ground-truth datasets and the lack of task-aware network design.

## Method
A dual-branch enhancement network is trained with a three-stage scheme. A Feature Restoration Branch (FRB) is a UNet-shaped encoder-decoder built from Conv-attention Transformer Blocks (CTBs) with channel dims {32,64,128,256} and a global residual connection; Task-Aware CTBs (TA-CTBs) inject task-specific priors via cross-modal attention that builds a mixed query from image and task features. A parallel Detail Enhancement Branch (DEB) runs at full resolution with four residual conv blocks plus channel- and pixel-attention to preserve high-frequency detail. Training combines pixel MSE loss, a Task-Driven Perceptual (TDP) loss measuring feature discrepancy in a frozen task network's latent space, and a task loss (cross-entropy) that uses 8x8 block-level mixed images for robustness.

## Key Contributions
- TI-UIED dataset: task-driven ground truth auto-constructed by enhancing SUIM images with 5 traditional + 4 deep UIE methods, then selecting the enhanced variant that most consistently improves segmentation across 7 architectures (~210 models trained)
- Dual-branch architecture (FRB + DEB) separating semantic feature restoration from high-frequency detail preservation for downstream tasks
- Task-Aware CTB (TA-CTB) that injects task priors via cross-modal mixed-query attention with convolutional relative position encoding
- Task-Driven Perceptual loss + task loss with 8x8 block-level image mixing for robust task-oriented training
- Demonstrates that human-perception-optimized UIE metrics (UIQM/UCIQE) do not correlate with downstream task gains, motivating a task-first design philosophy

## Results
Semantic segmentation (SUIM): improves UNet mIoU 66.52->67.61, Dice 72.29->74.48 over raw; LIACi UNet Dice 75.55->78.02. Object detection (RUOD): Faster R-CNN mAP 52.1->53.0, DINO 59.0->59.2. Instance segmentation (UIIS): Mask R-CNN mAP 21.9->22.3, WaterMask R-CNN 23.3->23.6. On perception metrics (SUIM-E) DTI-UIE is only comparable (UIQM 2.79, UCIQE 54.18), by design. Complexity: 9.5M params, 361 GFLOPs. Ablations: removing TA-CTB drops Dice -3.33, removing TDP loss -2.77, training on SUIM-E instead of TI-UIED -3.01; 8x8 mixing optimal.

## Significance
It shifts UIE evaluation from human-perceptual quality to downstream-task utility, providing both a task-driven dataset construction recipe and a task-aware architecture, and empirically showing that enhancement choices optimal for perception metrics are not optimal for recognition.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*