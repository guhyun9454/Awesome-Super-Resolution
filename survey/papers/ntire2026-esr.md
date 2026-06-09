# The Eleventh NTIRE 2026 Efficient Super-Resolution Challenge Report

- **Model / Abbrev:** NTIRE2026-ESR
- **Venue:** CVPR 2026 (2026)
- **Category:** Efficient/Lightweight SR
- **Paper:** <https://arxiv.org/abs/2604.03198>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Designing efficient single-image super-resolution networks (x4) that minimize inference runtime, parameters, and/or FLOPs while maintaining a fixed reconstruction-quality threshold. The challenge benchmarks the practical efficiency frontier of SR rather than peak fidelity.

## Method
A challenge report aggregating 15 valid team solutions (95 registrants). One main track with three sub-tracks: Inference Runtime, FLOPs, and Parameters, all gated by a quality floor (~26.90 dB val / ~26.99 dB test PSNR). Scoring is runtime-dominant (weight 0.8) vs FLOPs (0.1) and Parameters (0.1), pushing teams toward real wall-clock speed over theoretical complexity. SPAN (Swift Parameter-free Attention Network) serves as the baseline. Dominant recipes: structural reparameterization/operator fusion, custom fused CUDA kernels, channel pruning with distillation-based recovery, teacher-student knowledge distillation, parameter-free attention, plus exploratory Mamba/state-space gating branches and FFT frequency-domain losses.

## Key Contributions
- Benchmarks 15 SOTA efficient-SR designs against a fixed PSNR floor (~26.90 dB val / 26.99 dB test) across three efficiency axes
- Winner XiaomiMM (SPANV2): 5.26 ms runtime, 0.139M params, 9.11G FLOPs, 26.92 dB val / 27.00 dB test — improves on SPAN baseline (0.151M, 7.65 ms, 9.83G, 26.94 dB) via a custom fused span_attn_op CUDA kernel cutting DRAM round-trips
- Per-axis leaders: ZenoSR sets the smallest model (0.038M params) and lowest compute (2.68G FLOPs); XiaomiMM leads runtime (5.26 ms)
- Runner-ups BOE_AIoT (6.72 ms, 0.142M, 9.25G) and PKDSR (6.76 ms, 0.144M, 9.40G) win via pruning + progressive channel reduction (32->28->24) with distillation recovery
- Establishes runtime-weighted scoring (0.8) to prioritize deployable inference speed over FLOP/param counts

## Results
Winner XiaomiMM/SPANV2: 5.26 ms, 0.139M params, 9.11G FLOPs, 26.92 dB (val) / 27.00 dB (test). Baseline SPAN: 7.65 ms, 0.151M, 9.83G, 26.94 dB val. Sub-track records: ZenoSR 0.038M params and 2.68G FLOPs. Trained on DIV2K (800) + LSDIR (~85K) images; eval on 200-image DIV2K_LSDIR val/test splits.

## Significance
As the flagship efficient-SR benchmark, it sets the yearly state-of-the-art for lightweight x4 SR and signals a shift from FLOP/parameter optimization toward hardware-aware, fused-kernel runtime engineering (sub-6 ms inference at ~0.14M params) as the practical efficiency frontier.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*