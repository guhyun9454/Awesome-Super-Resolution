# Residual Dense Network for Image Super-Resolution

- **Model / Abbrev:** RDN
- **Venue:** CVPR 2018 (2018)
- **Category:** CNN Architecture
- **Paper:** <https://arxiv.org/abs/1802.08797>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Single-image super-resolution. Prior deep SR networks (e.g., VDSR, SRResNet, MemNet) do not fully exploit the hierarchical features produced by every intermediate convolutional layer; features from each layer are not directly used by later layers, limiting representational capacity. RDN aims to extract and adaptively fuse abundant local and global features from all conv layers to reconstruct high-quality HR images.

## Method
A CNN built from stacked Residual Dense Blocks (RDBs). Each RDB densely connects its internal conv layers (DenseNet-style), and—via a "contiguous memory" mechanism—the output state of the preceding RDB is fed directly to every layer of the current RDB. Inside each RDB, Local Feature Fusion (LFF) uses a 1x1 conv to adaptively fuse the concatenated dense features (enabling large growth rates and stable training), followed by Local Residual Learning (LRL). Across blocks, Global Feature Fusion (GFF) concatenates all RDB outputs and fuses them with 1x1 then 3x3 convs; Global Residual Learning then adds the shallow features extracted by the first conv layers. Upscaling is done late via sub-pixel/ESPCN upsampling, and the network is trained with an L1 reconstruction loss.

## Key Contributions
- Residual Dense Block (RDB): unifies dense connectivity, local feature fusion, and local residual learning so each conv layer's features are directly reused
- Contiguous Memory (CM): direct connection from prior RDB state to every layer of the current RDB, allowing feature reuse across blocks
- Local Feature Fusion (LFF): 1x1 conv that adaptively controls/preserves accumulated features, enabling a high growth rate and stable training of a very deep net
- Global Feature Fusion (GFF) + Global Residual Learning: jointly fuses hierarchical features from ALL RDBs and adds shallow features for the final dense feature representation
- Demonstrated on three degradation models (BI bicubic, BD blur-downsample, DN downsample-noise), showing the same architecture generalizes beyond simple bicubic SR

## Results
Default config: D=20 RDBs, C=6 conv layers per RDB, growth rate G=32 (~22M params). Trained on DIV2K (800 images), evaluated on Set5, Set14, B100, Urban100, Manga109 at x2/x3/x4. Achieves best or near-best PSNR/SSIM across benchmarks under BI degradation (e.g., Set5 x2 ~38.24 dB / 0.9614 SSIM, Urban100 x2 ~32.89 dB), competitive with the much larger EDSR/MDSR; and clearly outperforms SRCNN, FSRCNN, VDSR, IRCNN, and SRMDNF under the harder BD and DN degradation models.

## Significance
RDN showed that fully exploiting and adaptively fusing hierarchical features from every conv layer (local + global dense fusion plus residual learning) yields state-of-the-art SR, making "dense + residual" fusion a standard building block. It became a strong, widely cited baseline and a precursor to the authors' attention-based RCAN, influencing subsequent restoration architectures across SR, denoising, and deblocking.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*