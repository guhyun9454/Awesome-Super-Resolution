# Compressed-Domain-Aware Online Video Super-Resolution

- **Model / Abbrev:** CDA-VSR
- **Venue:** CVPR 2026 (2026)
- **Category:** Video SR
- **Paper:** <https://arxiv.org/abs/2603.07694>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Online (causal, low-latency) video super-resolution is bottlenecked by the cost of optical-flow/offset estimation for temporal alignment, so existing methods fail to hit real-time at higher resolutions. CDA-VSR exploits side information already produced by video codecs to cut this cost in bandwidth-limited streaming.

## Method
A unidirectional recurrent VSR network (3 residual-block shallow feature extractor, 64 channels) that consumes compressed-domain metadata parsed for free from the H.264 bitstream during decoding. Motion-Vector-Guided Deformable Alignment (MVGDA) initializes deformable-conv offsets with codec motion vectors o_MV and only learns small residual offsets Δo, avoiding expensive flow estimation. A Residual-Map Gated Fusion (RMGF) module turns coded residual maps into spatial gating weights that suppress unreliable/mismatched aligned regions. A Frame-Type-Aware Reconstruction (FTAR) module allocates compute by frame type — a heavy 24-residual-block branch for I-frames and a lightweight 12-block branch for P-frames. Trained with Charbonnier loss, Adam (0.9/0.99), cosine-annealed LR from 2e-4, 300k iters, batch 8, 15-frame 64x64 clips, x4 SR, on RTX 3090.

## Key Contributions
- Reuses codec-side compressed-domain information (motion vectors, residual maps, frame types) obtained essentially for free during decoding instead of computing optical flow
- MV-Guided Deformable Alignment that seeds deformable offsets with motion vectors and only predicts residual offsets, cutting alignment cost while keeping accuracy
- Residual-Map Gated Fusion that converts coded residual maps into spatial gating weights to suppress mismatched regions and emphasize reliable detail
- Frame-Type-Aware Reconstruction with a heavy I-frame branch (24 blocks) and lightweight P-frame branch (12 blocks) for adaptive per-frame compute allocation
- Causal unidirectional recurrent design satisfying online VSR latency constraints

## Results
REDS4 (CRF18): 25.81 dB PSNR, 0.7085 SSIM, 0.3324 LPIPS. Inter4K (2K): 29.98 dB PSNR, 0.8868 SSIM. Beats SOTA online method TMP by up to +0.13 dB PSNR. Efficiency: 3.3M params, 78G MACs, 10.8 ms/frame -> ~93 FPS on REDS4 (320x180) and 25.1 FPS at 2K, more than 2x faster than TMP.

## Significance
Shows that codec metadata, which prior VSR pipelines discard, can replace costly motion estimation to deliver real-time online VSR with better quality, advancing efficient compressed/streaming-oriented restoration. At only 3.3M params it more than doubles throughput over the prior online SOTA while still improving fidelity.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*