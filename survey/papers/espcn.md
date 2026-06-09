# Real-Time Single Image and Video Super-Resolution Using an Efficient Sub-Pixel Convolutional Neural Network

- **Model / Abbrev:** ESPCN
- **Venue:** CVPR 2016 (2016)
- **Category:** Efficient/Lightweight SR
- **Paper:** <https://arxiv.org/abs/1609.05158>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Prior CNN-based SR methods (e.g., SRCNN) first upscale the low-resolution (LR) input to the target resolution via bicubic interpolation and then perform all convolutions in the high-resolution (HR) space, which wastes computation and increases complexity, making real-time inference (especially for 1080p video) infeasible.

## Method
ESPCN keeps the entire feature-extraction network operating directly in the low-resolution input space, using a stack of standard convolutional layers without any pre-upsampling. The key trick is a learned "sub-pixel convolution" (pixel shuffle) layer at the very end: the final conv layer produces r^2 feature maps (where r is the upscale factor), and these are periodically rearranged/reshuffled into a single HR image of r-times the spatial resolution. This is mathematically equivalent to a learned deconvolution but far cheaper since all costly convolutions happen at LR resolution. The network is trained end-to-end with a pixel-wise mean squared error (MSE) loss on the luminance channel.

## Key Contributions
- Introduces the efficient sub-pixel convolution (pixel-shuffle) layer that performs upscaling as the last step via periodic reshuffling of r^2 LR feature channels, replacing handcrafted bicubic upscaling or expensive HR-space deconvolution
- Performs all convolutions in low-resolution space, drastically reducing computational cost and memory versus SRCNN-style HR-space processing
- Achieves real-time SR: roughly an order of magnitude faster than SRCNN while improving reconstruction accuracy, enabling 1080p HD video super-resolution on a single GPU in real time
- Demonstrates the approach on both single images and video, establishing sub-pixel convolution as a standard upsampling primitive widely adopted in later SR networks (e.g., SRGAN, EDSR)

## Results
Reports state-of-the-art accuracy at the time on standard benchmarks (Set5, Set14, BSD300/BSD500 and video datasets), with PSNR gains over SRCNN of roughly +0.15 to +0.3 dB at 3x/4x. On Set5 at 3x it reaches around 32.5 dB PSNR. It runs about an order of magnitude faster than SRCNN (the LR-space network with sub-pixel conv processes a single image roughly 10x faster), enabling real-time 1080p video upscaling on a K2 GPU.

## Significance
ESPCN's sub-pixel/pixel-shuffle upsampling became the de facto efficient upscaling layer in deep super-resolution, adopted by nearly all subsequent SR architectures (SRGAN, EDSR, RCAN, etc.), and it was among the first methods to make CNN-based real-time HD video super-resolution practical.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*