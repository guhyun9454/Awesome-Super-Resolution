# Neural Discrimination-Prompted Transformers for Efficient UHD Image Restoration and Enhancement

- **Model / Abbrev:** UHDPromer
- **Venue:** IJCV 2026 (2026)
- **Category:** Efficient/Lightweight SR
- **Paper:** <https://arxiv.org/abs/2603.00853>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Ultra-high-definition (4K, 3840x2160) image restoration/enhancement is computationally prohibitive for full-resolution Transformers. The paper targets efficient UHD restoration across low-light enhancement, dehazing, and deblurring while preserving high-frequency detail lost when operating in heavily downsampled feature space.

## Method
UHDPromer is a Transformer that exploits the observed "neural discrimination" — the feature gap between high-resolution and low-resolution features. It computes Neural Discrimination Priors (NDP) that measure HR-vs-LR feature differences and injects them into two modules: Neural Discrimination-Prompted Attention (NDPA), which reformulates self-attention so it globally perceives useful discrimination information, and the Neural Discrimination-Prompted Network/feed-forward (NDPN), which uses a continuous NDP-guided gating mechanism to selectively pass beneficial content. The model shuffles the UHD input down by a factor of 8 and does the heavy Transformer work in H/8 x W/8 x C space for efficiency, then uses a Feature Super-Resolution (FeaSR) and SR-Guided Reconstruction (SRG-Recon) path to recover detail.

## Key Contributions
- Neural Discrimination Priors (NDP): an explicit measure of the feature difference between high-res and low-res features used to prompt restoration
- Neural Discrimination-Prompted Attention (NDPA): self-attention reformulated to globally perceive discrimination information via NDP
- Neural Discrimination-Prompted Network (NDPN): NDP-guided continuous gating in the feed-forward path for selective content passage
- Super-resolution-guided reconstruction (FeaSR + SRG-Recon): super-resolving low-res features to guide the final UHD output
- Pipeline (HRFR -> 15 NDPT blocks at H/8 -> FeaSR -> SRG-Recon) operating mostly in 8x-downshuffled space for efficiency
- Released code and pretrained models (github.com/supersupercong/uhdpromer)

## Results
On UHD-LL (low-light, Setting 2): 27.159 dB PSNR / 0.9285 SSIM / 0.2118 LPIPS with only 0.743M params, 32.56 GFLOPs and 0.12s at 1024x1024. Beats UHDformer (27.113/0.9271, 0.339M) and UHDFour (26.226/0.9000, 17.5M), with ~36.9% fewer FLOPs than UHDformer. Reports SOTA across all three UHD tasks (UHD-LL, UHD-Haze, UHD-Blur, all at 3840x2160) at low compute.

## Significance
Pushes the efficiency frontier of UHD restoration by showing that explicitly modeling the HR-LR feature discrepancy lets a sub-1M-parameter Transformer match or beat heavier UHD baselines, making 4K restoration practical on commodity hardware (real-time-ish at 1024-crop) and generalizing across three distinct degradations with one architecture.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*