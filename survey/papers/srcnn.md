# Image Super-Resolution Using Deep Convolutional Networks

- **Model / Abbrev:** SRCNN
- **Venue:** ECCV 2014 (2014)
- **Category:** CNN (Pioneering)
- **Paper:** <https://arxiv.org/abs/1501.00092>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Single image super-resolution (SISR): recovering a high-resolution image from a single low-resolution input. Prior SR methods (notably sparse-coding/example-based pipelines) treated the problem as a sequence of separately optimized stages (patch extraction, dictionary learning, mapping, reconstruction) rather than as a single end-to-end learned mapping.

## Method
SRCNN learns a direct end-to-end mapping from a bicubic-upscaled low-resolution image to its high-resolution counterpart using a shallow fully-convolutional network with three layers. The three layers correspond to three conceptual operations: (1) patch extraction and representation (9x9 conv, 64 filters), (2) non-linear mapping of feature vectors (1x1 or 5x5 conv, 32 filters), and (3) reconstruction (5x5 conv producing the output image). The network is trained to minimize pixel-wise mean squared error (MSE) between the reconstructed and ground-truth HR images, optimized by stochastic gradient descent with backpropagation. A key insight is that this architecture is a re-interpretation of the classic sparse-coding-based SR pipeline as a convolutional network, where each pipeline stage maps onto a network layer.

## Key Contributions
- First successful CNN for single image super-resolution, learning an end-to-end LR-to-HR mapping with no hand-crafted pre/post-processing beyond bicubic upsampling of the input
- Established a conceptual bridge showing that sparse-coding-based SR methods are equivalent to a special case of a deep convolutional network, unifying example-based SR under a deep-learning framework
- A lightweight 3-layer fully-convolutional architecture (~8K params) using MSE loss that nonetheless beats prior state-of-the-art in both quality and speed
- Extended the model to jointly handle the three RGB color channels rather than processing only luminance
- Systematic study of architecture/parameter trade-offs (filter sizes, number of filters, network depth) between restoration quality and runtime

## Results
Reported state-of-the-art PSNR on standard SR benchmarks (Set5, Set14, BSD200) across upscaling factors 2x/3x/4x, e.g. roughly 32.4-32.7 dB PSNR on Set5 at 3x, surpassing prior methods such as sparse-coding (SC), A+, and KK. The network is extremely fast at test time (fully-convolutional, ~8K parameters), enabling near-real-time inference on CPU while improving over heavier example-based methods.

## Significance
SRCNN is the seminal paper that introduced deep learning to image super-resolution, demonstrating that a simple CNN trained with MSE could outperform decades of hand-engineered SR pipelines. It launched the entire deep-learning SR research line (VDSR, ESPCN, SRGAN, EDSR, and beyond) and remains the canonical baseline in virtually every SR survey.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*