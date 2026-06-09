# BasicVSR++: Improving Video Super-Resolution with Enhanced Propagation and Alignment

- **Model / Abbrev:** BasicVSR++
- **Venue:** CVPR 2022 (2022)
- **Category:** Video SR
- **Paper:** <https://arxiv.org/abs/2104.13371>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Video super-resolution (VSR): recover a high-resolution video from a low-resolution input by exploiting long-range temporal information. Builds on BasicVSR, addressing two bottlenecks — suboptimal information propagation across frames and inaccurate alignment under large motion/occlusion.

## Method
Recurrent (bidirectional) framework, NOT sliding-window. Two enhancements over BasicVSR: (1) Second-order grid propagation — bidirectional propagation arranged in a grid that allows information to flow back and forth more aggressively, with second-order connections that aggregate features from the two preceding/following frames (relaxing the strict first-order Markov assumption) for more robust feature refinement. (2) Flow-guided deformable alignment — uses optical flow to pre-warp features as guidance for deformable convolution offsets, predicting residual offsets and modulation masks. This stabilizes the otherwise unstable training of deformable alignment while retaining its expressive, offset-diversity benefits over pure flow warping.

## Key Contributions
- Second-order grid propagation scheme: grid-like bidirectional propagation with second-order connections enabling more effective aggregation of information across the whole sequence
- Flow-guided deformable alignment: couples optical-flow warping with deformable convolution (flow as base offset + learned residual offsets/masks) to get accurate, stable alignment
- Demonstrates these two generic modules are the main drivers of gains, improving over BasicVSR by 0.82 dB PSNR with comparable parameters and computation
- Won 3 champions and 1 runner-up in NTIRE 2021 VSR and compressed video quality enhancement challenges

## Results
Headline: +0.82 dB PSNR over BasicVSR at comparable parameter count. Reported benchmark PSNR (4x, from the paper): REDS4 ~32.39 dB (BIx4); Vid4 ~27.79 dB; Vimeo-90K-T ~37.79 dB (BI degradation). (Exact per-dataset values were not in the fetched abstract; these are the figures associated with the published paper — treat as approximate.)

## Significance
Set a new state of the art in VSR at publication and became the dominant baseline/backbone for subsequent VSR work. Showed that strong propagation + alignment design, rather than heavier networks, drives VSR quality; the two proposed modules generalize to other restoration tasks (e.g., compressed video enhancement).

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*