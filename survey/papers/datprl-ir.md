# Learning Domain-Aware Task Prompt Representations for Multi-Domain All-in-One Image Restoration

- **Model / Abbrev:** DATPRL-IR
- **Venue:** ICLR 2026 (2026)
- **Category:** All-in-one Restoration
- **Paper:** <https://arxiv.org/abs/2603.01725>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Existing all-in-one image restoration models are confined to a single domain (natural scenes, or medical, or remote sensing) and a small set of degradation tasks. This work targets the harder, previously unaddressed setting of multi-domain all-in-one restoration: a single model that handles many tasks spanning visually disparate domains, where naive prompt sharing fails to capture both task-specific and domain-specific priors.

## Method
A prompt-conditioned encoder-decoder restoration backbone is guided by a "domain-aware task prompt representation." Two learnable prompt pools (task and domain, 15 prompts each, key dim 1x1024 / value 2x1024) are queried per input image; a Prompt Composition Mechanism (PCM) selects top-k prompts (k=3 task, k=5 domain) and fuses them via softmax-over-similarity weighted averaging with a temperature parameter, producing instance-level representations rather than one fixed prompt per task. Domain awareness is injected by distilling priors from a multimodal LLM (LLaVA-1.5-7B), which generates multi-perspective image descriptions (content, color richness, object category, brightness, camera/viewpoint); these are CLIP-text-encoded and aligned to the domain prompts via a cross-modal cosine alignment loss. The composed task and domain representations are fused into a single domain-aware task prompt that conditions restoration.

## Key Contributions
- First multi-domain all-in-one image restoration framework, unifying natural, medical, and remote-sensing degradations in one model
- Dual task/domain prompt-pool design with a Prompt Composition Mechanism (PCM) that adaptively selects and softmax-weights top-k prompts into instance-level representations
- Distillation of MLLM (LLaVA-1.5-7B) domain priors into domain prompts via CLIP-text cross-modal alignment loss, encoding semantic domain knowledge
- Composite training objective: L1 reconstruction in RGB + Fourier domains, cross-modal alignment, prompt diversity regularization, balance (uniform-utilization) loss, and contrastive regularization
- New multi-domain, multi-task benchmark spanning 6 tasks across 3 domains with SOTA results

## Results
On the 6-task/3-domain benchmark, average PSNR 30.77 dB / SSIM 0.8653, beating prior SOTA MoCEIR by ~0.37 dB average PSNR (~1 dB on deraining). Per-task PSNR/SSIM: natural SR 28.98/0.8191, deraining 39.56/0.9865, MRI SR 27.88/0.9053, CT denoising 33.80/0.9278, RSI SR 28.29/0.7917, cloud removal 26.12/0.7612. Datasets: DIV2K+Flickr2K, Rain100L, GoPro (natural); IXI, AAPM-Mayo CT, PolarStar m660 PET (medical); UCMerced, CUHK CR1, RICE1 (remote sensing). Trained with Adam (lr 4e-4, cosine), batch 12, 1000K iterations on RTX 5090.

## Significance
It is the first method to break the single-domain ceiling of all-in-one restoration, showing one model can jointly handle natural, medical, and remote-sensing degradations by separating and recombining task vs. domain knowledge. The MLLM-prior distillation establishes a practical route to inject semantic domain awareness into low-level vision, and the work introduces a unified cross-domain benchmark for future research.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*