# OmniFM: Toward Modality-Robust and Task-Agnostic Federated Learning for Heterogeneous Medical Imaging

- **Model / Abbrev:** OmniFM
- **Venue:** CVPR 2026 (2026)
- **Category:** Other/Tangential
- **Paper:** <https://arxiv.org/abs/2603.21660>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Federated learning frameworks for medical imaging are tightly coupled to task-specific backbones and fragile under heterogeneous imaging modalities (MRI/PET/CT/SPECT/pathology), which blocks real-world deployment across institutions with differing modality distributions and downstream tasks. OmniFM targets a single FL framework that is both modality-robust and task-agnostic.

## Method
OmniFM exploits a frequency-domain insight: low-frequency spectral components show strong cross-modality consistency and encode modality-invariant anatomical structure. It plugs four spectral modules onto arbitrary task backbones: (1) Global Spectral Knowledge Retrieval to inject global frequency priors, (2) Embedding-wise Cross-Attention Fusion to align local/global representations, (3) Prefix-Suffix Spectral Prompting to jointly condition global and personalized cues, and (4) a Spectral-Proximal Alignment regularizer. Training optimizes task loss + lambda * alignment, where the alignment term is an L2 distance between local spectral embeddings and a global prototype centroid (Eq. 5), stabilizing aggregation across non-IID clients. The framework is backbone-agnostic, attaching to ResNet-18 (classification), UNet (segmentation), LLaVA-1.5 with CLIP ViT-B/32 + LLaMA-3.2-3B (VQA), and U2Fusion (multimodal fusion) without re-engineering the optimization pipeline.

## Key Contributions
- Unified FL across five heterogeneous tasks (classification, segmentation, super-resolution, VQA, multimodal fusion) with one task-agnostic, backbone-agnostic optimization scheme
- Frequency-domain modality-invariance principle: low-frequency spectral components are cross-modality consistent, used as the bridge for heterogeneous clients
- Four spectral mechanisms: Global Spectral Knowledge Retrieval, Embedding-wise Cross-Attention Fusion, Prefix-Suffix Spectral Prompting, and Spectral-Proximal Alignment regularization
- Broad evaluation against 12 FL baselines (FedAvg, FedProx, Ditto, SCAFFOLD, MOON, GPFL, FedDyn, FedRep, FedPer, FedAdam, FedAvgM, FedYogi) under intra- and cross-modality heterogeneity, both fine-tuning and from-scratch

## Results
SR (BreaKHis histopathology, scales x2/x4/x8): x2 average PSNR 42.21 dB / SSIM 0.9693 vs FedAvg 35.95 dB / 0.9505. Classification (MedMNIST-v2 subsets, hardest case at 20% participation): 96.85% acc vs FedPer 84.47%. Segmentation (FeTS2022 glioma mp-MRI): 77.41% Dice vs 73.86%. VQA (SLAKE/VQA-RAD/VQA-Med, 8 modalities): 79.27% vs 78.40%. Multimodal fusion (Harvard MRI/PET/CT/SPECT, U2Fusion): VIF 0.269 vs 0.221. Consistently beats SOTA FL baselines across all settings.

## Significance
Decouples federated medical imaging from task-specific backbones, offering a single framework deployable across modalities and tasks - a step toward practical, heterogeneous multi-institution FL. Its frequency-domain alignment trick is a generalizable principle for cross-modality FL robustness.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*