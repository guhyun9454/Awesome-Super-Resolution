# Disentangled Textual Priors for Diffusion-based Image Super-Resolution

- **Model / Abbrev:** DTPSR
- **Venue:** CVPR 2026 (2026)
- **Category:** Diffusion SR
- **Paper:** <https://arxiv.org/abs/2603.07430>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Diffusion-based SR depends heavily on how semantic (textual) priors are structured. Existing text-guided SR uses either coarse global captions or local tag descriptions, and entangles structure with texture in a single latent, mixing global layout with local detail. This conflates structural and textural cues, limits controllability/interpretability, and causes hallucinations under severe degradation (e.g. misreading smooth walls as complex textures).

## Method
DTPSR is a ControlNet-style SD-2-base diffusion SR framework that disentangles textual priors along two axes: spatial hierarchy (global vs. local) and frequency semantics (low- vs. high-frequency). Given an LR image, Mask2Former (Swin-L) segments object regions, then a frozen LLaVA-7B MLLM generates a global scene caption plus per-segment low-frequency descriptions (shape, layout, color) and high-frequency descriptions (texture, edges, details). These are CLIP-encoded into global, LF, and HF embeddings and injected sequentially through four dedicated cross-attention modules in the denoising UNet: GTCA (global), LFCA (local low-freq), HFCA (local high-freq), and LRCA (anchors identity using frozen DAPE LR image features). A multi-branch classifier-free guidance scheme uses three separate negative prompts (global, LF, HF) to suppress layout errors, structural errors, and texture artifacts independently.

## Key Contributions
- DTPSR: first SR framework to disentangle textual priors along both spatial hierarchy (global/local) and frequency (low/high) dimensions for interpretable, controllable restoration
- Disentangled semantic injection via four separate cross-attention pathways (GTCA, LFCA, HFCA, LRCA) operating coarse-to-fine and sequentially
- Multi-branch classifier-free guidance with frequency/scale-specific negative prompts to suppress hallucinations without extra training
- DisText-SR dataset: ~95,000 image-text pairs with disentangled global, region-wise low-frequency, and high-frequency annotations generated via panoptic segmentation + MLLM prompting
- Standard noise-prediction (epsilon) training objective conditioned on all disentangled priors; trained 110k iters, AdamW, batch 32, lr 5e-5 on 4 A800 GPUs

## Results
Base: SD-2-base, 50 DDPM steps, 10.5B params, 14.94s/image (512x512) — smaller/faster than SUPIR (17.8B, 100 steps, 17.25s). Best no-reference perceptual metrics across DIV2K-Val, RealSR, DRealSR. DIV2K-Val: MUSIQ 71.24, MANIQA 0.5866, CLIP-IQA 0.7549 (all best). RealSR: MUSIQ 71.84, MANIQA 0.6021, CLIP-IQA 0.7278 (best). DRealSR: MUSIQ 69.24, MANIQA 0.6011, CLIP-IQA 0.7640 (best). Trades off full-reference fidelity (lower PSNR/SSIM than GAN methods, e.g. DIV2K PSNR 22.03/SSIM 0.5471) due to perception-distortion tradeoff but stays competitive on LPIPS. Ablations confirm gains from global+local priors, LF+HF disentanglement (vs mixed), and multi-branch vs single negative CFG; robust under corrupted text inputs (DTPSR-C).

## Significance
Demonstrates that how textual priors are structured (disentangled by scale and frequency) materially improves perceptual SR, balancing global scene coherence with fine texture fidelity while reducing semantic-drift hallucinations. Establishes a reusable disentangled text-prior dataset (DisText-SR) and a branch-specific CFG strategy that advances controllable, interpretable text-guided diffusion restoration.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*