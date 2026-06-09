# All-in-One Image Restoration via Causal-Deconfounding Wavelet-Disentangled Prompt Network

- **Model / Abbrev:** CWP-Net
- **Venue:** TIP 2026 (2026)
- **Category:** All-in-one Restoration
- **Paper:** <https://arxiv.org/abs/2603.03839>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
All-in-one image restoration (AiOIR) handles multiple unknown degradation types within a single unified model. The authors identify two defects in existing AiOIR models: (1) a spurious correlation between non-degradation semantic features and degradation patterns, and (2) biased estimation of the degradation patterns themselves, both of which hurt generalization under distribution shift.

## Method
CWP-Net is a U-shaped encoder-decoder built on a CNN backbone where each scale couples a CNNBlock with a Wavelet Attention module (WAE in the encoder, WAD in the decoder). The wavelet attention applies a discrete wavelet transform to split features into LL/LH/HL/HH subbands, runs channel+spatial attention per subband, then recombines (concat+1x1 conv for encode, inverse DWT for decode) to explicitly disentangle degradation from semantic content. To fix biased degradation estimation, it frames restoration causally: instead of a direct degradation-to-output path (T->Y) it inserts an alternative variable P (prompted wavelet subbands), giving T->P->Y, and applies a backdoor-adjustment / deconfounding formula P(Y|do(X)) = sum_i P(Y|X,P=p_i)P(P=p_i). The Wavelet Prompt Block (WPB) in the skip connections implements this via a Degradation-Based Weight Estimator (DWE, K-Means clustering on degradation representations to weight high-frequency subbands in [0,1]) and a Prompt-Guided Weighted Spatial Feature Transform (PWSFT, pixel-level modulation P_j = gamma_j (x) Z_j + beta_j + Z_j).

## Key Contributions
- Identifies and formalizes two failure modes of AiOIR: spurious semantic-degradation correlation and biased degradation estimation, and treats them through a causal-inference (backdoor adjustment) lens
- Wavelet attention modules (WAE/WAD) that decompose features into frequency subbands to explicitly disentangle degradation from semantic features
- Wavelet Prompt Block (WPB) acting as the alternative/mediator variable P for causal deconfounding, combining a K-Means-based Degradation Weight Estimator (DWE) and a prompt-guided weighted spatial feature transform (PWSFT)
- Multi-scale L1 reconstruction loss plus an FFT-domain frequency loss (L = L_rec + 0.1*L_freq)
- State-of-the-art results on both 5-degradation and 7-degradation all-in-one benchmarks at modest cost (15.5M params)

## Results
5-degradation setting: average 33.15 dB PSNR / 0.920 SSIM (dehaze 33.21, derain 38.68, denoise sigma15/25/50 = 34.14/31.48/28.22), about +0.59 dB average over prior best (Lin et al.), with +1.58 dB on dehazing. 7-degradation setting: average 30.56 dB / 0.906 SSIM (derain 38.52, dehaze 32.88, low-light 21.92, deblur 27.94, denoise 31.54), about +2.22 dB average over IDR. Efficiency: 15.50M params, 130.55 GFLOPs, 34.32 ms inference. Datasets: WED+BSD400 (denoise, test BSD68), Rain100L, SOTS-outdoor (dehaze), plus LOL-v1 (low-light) and GoPro (deblur) for the 7-task setting. Training: batch 48, lr 2e-4->1e-6 cosine, 150 epochs, 224x224 patches, RTX 3090.

## Significance
Brings explicit causal deconfounding (backdoor adjustment) together with wavelet-domain frequency disentanglement to AiOIR, offering a principled way to suppress spurious correlations rather than relying purely on learned prompts, and achieving SOTA across both 5- and 7-task settings with a lightweight model.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*