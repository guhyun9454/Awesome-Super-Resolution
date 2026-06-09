# VRT: A Video Restoration Transformer

- **Model / Abbrev:** VRT
- **Venue:** TIP 2024 (2024)
- **Category:** Video SR
- **Paper:** <https://arxiv.org/abs/2201.12288>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Prior video restoration is dominated by either (a) sliding-window CNNs (e.g., EDVR) that restore one frame at a time without exploiting long-range temporal dependencies, or (b) recurrent networks (e.g., BasicVSR) that propagate features sequentially, preventing parallelization and struggling with long-range modeling and information loss over long sequences. The goal is a Transformer that does parallel frame prediction with long-range temporal modeling for VSR (and other restoration tasks).

## Method
Multi-scale U-shaped Transformer that processes all frames in parallel. Core block is Temporal Mutual Self Attention (TMSA): each block splits the clip into small temporal sub-clips (e.g., 2 frames) and applies (1) mutual attention across frames for joint implicit motion estimation, feature alignment and fusion, plus (2) self attention within frames for feature extraction. The video sequence is temporally shifted by half a clip every other layer so information propagates across clip boundaries (analogous to Swin's shifted windows, applied along time). A parallel warping module at the end of each scale uses optical-flow-based feature warping of neighbouring frames for explicit alignment, complementing the implicit mutual-attention alignment. Multiple scales with downsampling/upsampling capture motions of different magnitudes.

## Key Contributions
- Temporal Mutual Self Attention (TMSA): mutual attention performs implicit joint motion estimation, alignment and fusion, replacing explicit deformable-conv alignment
- Parallel (non-recurrent) frame prediction enabling full parallelization while modeling long-range temporal dependencies, unlike recurrent/sliding-window designs
- Temporal sequence shifting every other layer for cross-clip interaction (Swin-style shifting along the time axis)
- Parallel warping: optical-flow-based explicit feature warping module to further fuse neighbouring-frame information
- Single architecture (multi-scale U-shaped Transformer) generalizes across video SR, deblurring, denoising, frame interpolation, and space-time SR

## Results
Video SR PSNR (dB): REDS4 (BI) 32.19 / SSIM 0.9006 with 16 input frames (31.60 with 6 frames); Vimeo-90K-T (BD) 38.20 / 0.9530; Vid4 (BD) 27.93 / 0.8425. Reported as state-of-the-art across 5 restoration tasks with gains up to 2.16 dB overall; ~0.50-1.57 dB over EDVR on VSR.

## Significance
Demonstrated that a parallel Transformer can outperform both recurrent (BasicVSR) and sliding-window CNN (EDVR) baselines, establishing mutual-attention-based implicit alignment plus flow-based parallel warping as a strong alternative to deformable-convolution alignment. Became a key Transformer baseline for VSR and the direct predecessor to RVRT (which reintroduces recurrence with guided deformable attention).

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*