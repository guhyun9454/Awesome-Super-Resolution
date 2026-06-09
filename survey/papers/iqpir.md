# Beyond Ground-Truth: Leveraging Image Quality Priors for Real-World Image Restoration

- **Model / Abbrev:** IQPIR
- **Venue:** CVPR 2026 (2026)
- **Category:** Real-world SR
- **Paper:** <https://arxiv.org/abs/2603.29773>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Real-world image restoration models are trained with reference (ground-truth) supervision, but GT images themselves have inconsistent perceptual fidelity, so models converge toward the dataset's average quality level rather than the maximal achievable perceptual quality, capping output quality at the GT ceiling.

## Method
IQPIR introduces an Image Quality Prior (IQP) distilled from pretrained No-Reference IQA (NR-IQA) models to guide restoration beyond the GT ceiling. It builds on a learned VQGAN-style codebook prior with three mechanisms: (1) a quality-conditioned Transformer where NR-IQA-derived quality scores act as conditioning signals steering the predicted discrete representation toward maximal perceptual quality; (2) a dual-branch codebook that disentangles common (content) features from quality-specific attributes, with a high-quality "HQ+" codebook; (3) discrete representation-based quality optimization that performs IQA-driven optimization in the discrete latent space to avoid the over-optimization/artifacts seen when maximizing IQA directly in pixel space. Stage-two training combines a feature loss (MSE with stop-gradient to the quantized codes), a cross-entropy index loss over both common and HQ+ codebooks, and a quality loss that directly maximizes ensemble NR-IQA scores (L_quality = -IQA(x_res)), with the combined objective L = L_feat + 0.5*L_index + 0.1*L_quality.

## Key Contributions
- Identifies that GT-based supervision caps restoration at the dataset's average perceptual quality and proposes leveraging NR-IQA-derived quality priors to push 'beyond ground-truth'
- Quality-conditioned Transformer: a plug-and-play module that uses an NR-IQA score as a conditioning signal to steer discrete representation prediction toward higher perceptual quality (e.g. condition score 0.9 for top quality)
- Dual-branch codebook separating common content features from quality-specific attributes, including a high-quality HQ+ codebook (1024 entries, 256-dim, 16x16x256 quantized maps)
- Discrete representation-based quality optimization that applies the IQA-maximization loss in the discrete latent space to mitigate over-optimization artifacts
- Ensemble of NR-IQA models as the prior: Musiq-GFIQA/TOPIQ-GFIQA/Q-Align for faces; Musiq/CLIP-IQA/BRISQUE for other tasks; adjustable fidelity-vs-quality weight at inference (0=quality, 1=fidelity)

## Results
Blind face restoration (LFW-Test): TOPIQ-G 0.861, Musiq-GFIQA 0.878, CLIP-IQA 0.790. Low-light enhancement (LOL-v1): PSNR 25.72, SSIM 0.902, FID 40.18. Human evaluation: 4.03/5.0 (face restoration), 4.22/5.0 (low-light). Inference ~5.63 ms/image. Trained on a single NVIDIA H200 (Adam, batch size 32).

## Significance
Reframes the supervision target for real-world restoration: instead of regressing to imperfect GT, it optimizes a learned discrete prior toward NR-IQA-defined perceptual quality, letting outputs exceed GT quality. The quality-conditioned Transformer is plug-and-play and the framework generalizes across face restoration, low-light, underwater, and backlit tasks, suggesting a broadly applicable recipe for prior-driven, GT-agnostic restoration.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*