# Activating More Pixels in Image Super-Resolution Transformer

- **Model / Abbrev:** HAT (Hybrid Attention Transformer)
- **Venue:** CVPR 2023 (2023)
- **Category:** Transformer SR
- **Paper:** <https://arxiv.org/abs/2205.04437>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Window-based SR transformers like SwinIR only exploit a limited spatial range of input pixels (verified via LAM attribution analysis), leaving much of the available input information unused. The paper aims to activate more input pixels so the network can reconstruct high-resolution detail from a wider, more informative receptive context.

## Method
HAT is a hybrid attention transformer that combines, within each Residual Hybrid Attention Group (RHAG), window-based self-attention (Swin-style shifted windows) with a parallel channel attention block (CAB) so the model captures both strong local fitting and global channel statistics. The key new module is the Overlapping Cross-Attention Block (OCAB), which computes cross-attention between partially overlapping query/key-value windows (the K/V windows are enlarged with overlap) to enrich cross-window information flow and break window-boundary isolation. Training uses L1 pixel loss, and the paper introduces a same-task pre-training scheme: pretrain the full SR model on the large ImageNet dataset and then fine-tune on the target SR benchmark, which they argue is more effective than multi-task or no pretraining. The architecture follows the standard shallow-feature conv + deep transformer body + pixel-shuffle upsampler design.

## Key Contributions
- Hybrid Attention Transformer combining window self-attention with channel attention (CAB) to activate a much larger range of input pixels (shown via LAM)
- Overlapping Cross-Attention Block (OCAB) that uses overlapping K/V windows to strengthen interaction across adjacent windows
- Same-task pre-training on ImageNet followed by fine-tuning, instead of multi-task pretraining, to unlock further gains
- Provides scaled variants (HAT-S 9.6M / HAT 20.8M / HAT-L) and shows transformer SR keeps improving with capacity
- Open-source code/models in XPixelGroup/HAT; extended TPAMI version generalizes to broader restoration

## Results
Reported to outperform prior SOTA (e.g., SwinIR) by more than 1 dB. x4 PSNR without ImageNet pretraining: Set5 33.04 dB, Set14 29.23 dB, Urban100 27.97 dB, Manga109 32.48 dB (HAT); HAT-S (9.6M, 54.9G MAdds) reaches 27.87 dB on Urban100, beating SwinIR (11.9M) while being smaller. ImageNet pretraining and HAT-L push results further. Benchmarks: Set5, Set14, BSD100, Urban100, Manga109; training on DF2K (DIV2K+Flickr2K) with ImageNet for pretraining.

## Significance
HAT set a new state of the art for classical image SR, demonstrating that simply enlarging the activated pixel region (via channel attention + overlapping cross-attention) plus same-task ImageNet pretraining yields large gains over SwinIR. It became a widely used strong backbone/baseline for subsequent SR and restoration research.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*