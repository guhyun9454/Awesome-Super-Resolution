# BasicVSR: The Search for Essential Components in Video Super-Resolution and Beyond

- **Model / Abbrev:** BasicVSR (and extension IconVSR)
- **Venue:** CVPR 2021 (2021)
- **Category:** Video SR
- **Paper:** <https://arxiv.org/abs/2012.02181>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Video super-resolution (VSR) pipelines had grown increasingly complex with diverse, hard-to-compare designs. The paper decomposes VSR into four essential functional components and asks what minimal set of well-chosen modules is actually needed for high-quality, efficient VSR.

## Method
Reframes VSR around four functions: Propagation, Alignment, Aggregation, Upsampling. BasicVSR uses bidirectional recurrent propagation (forward + backward passes over the whole sequence) rather than a sliding-window design, so each frame aggregates information from the entire video. Alignment is done by optical-flow estimation (SPyNet) followed by feature-level spatial warping of the propagated features (not deformable convolution, unlike EDVR/TDAN). Aggregation and upsampling use simple residual blocks and pixel-shuffle. IconVSR extends BasicVSR with two add-ons: (1) an information-refill mechanism using sparsely sampled keyframes (via an EDVR-style extractor) to mitigate flow/occlusion error accumulation, and (2) coupled propagation that lets forward and backward branches exchange information.

## Key Contributions
- Decomposes VSR into four reusable components (Propagation, Alignment, Aggregation, Upsampling) as an analysis framework
- Shows bidirectional long-range recurrent propagation beats sliding-window aggregation
- Uses optical-flow feature-warping alignment instead of deformable convolution, achieving competitive accuracy with a far simpler, lighter pipeline
- Introduces IconVSR with information-refill (keyframe) and coupled propagation to reduce error accumulation in long sequences
- Establishes a strong, simple baseline that became the basis for BasicVSR++ (CVPR 2022)

## Results
On REDS4 (BIx4) BasicVSR reaches ~31.42 dB PSNR; on Vimeo-90K-T (BIx4) ~36.28 dB; on Vid4 (BIx4) ~27.27 dB. It outperforms heavier methods such as EDVR while being substantially more efficient in speed and parameters; IconVSR improves further on these benchmarks. Trained/tested on REDS (REDS4 test split), Vimeo-90K (Vimeo-90K-T test), and Vid4.

## Significance
Demonstrated that a succinct, well-designed recurrent pipeline with optical-flow alignment can match or beat complex deformable-conv, sliding-window VSR models at much lower cost. Became the canonical VSR baseline and the foundation for BasicVSR++ and subsequent propagation-based VSR research.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*