# Quick navigation

- [repositories](awesome_paper_list_and_repos.md)
- [Datasets](dataset.md)
- [papers](#papers)
  - [Non-DL based approach](non_dl_papers.md)
  - [DL based approach](#DL-based-approach)
    - [2014-2016](2014-2016_papers.md)
    - [2017](2017_papers.md)
    - [2018](2018_papers.md)
    - [2019](2019_papers.md)
    - [2020](2020_papers.md)
    - [2021](2021_papers.md)
    - [2022](2022_papers.md)
    - [2023](2023_papers.md)
	- [2024](2024_papers.md)
    - [2025](2025_papers.md)
- [Super Resolution workshop papers](workshops.md)
- [Super Resolution survey](sr_survey.md)

# Awesome-Super-Resolution（in progress）

Collect some super-resolution related papers, data and repositories.

## papers

### DL based approach

Note this table is referenced from [here](https://github.com/LoSealL/VideoSuperResolution/blob/master/README.md#network-list-and-reference-updating)

### 2026
More years papers, plase check Quick navigation

| Title                  | Model                  | Published                                                    | Code                                                         | Keywords                                                     |
| ---------------------- | ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Trust but Verify: Adaptive Conditioning for Reference-Based Diffusion Super-Resolution via Implicit Reference Correlation Modeling |Ada-RefSR | [ICLR2026](https://arxiv.org/abs/2602.01864) | [code](https://github.com/vivoCameraResearch/AdaRefSR) | diffusion, one-step, reference-based, attention, image, super-resolution |
|LiveMoments: Reselected Key Photo Restoration in Live Photos via Reference-guided Diffusion |LiveMoments | [ICLR 2026](https://arxiv.org/abs/2604.12286) | [code](https://github.com/OpenVeraTeam/LiveMoments) | diffusion, flow-matching, reference-based, image, restoration, 4K |
|Energy-oriented Diffusion Bridge for Image Restoration with Foundational Diffusion Models |E-Bridge | [arxiv](https://arxiv.org/abs/2604.10983) | [code](https://jinnh.github.io/E-Bridge/) | diffusion, bridge, image, restoration, one-step, FLUX |
|SAT: Selective Aggregation Transformer for Image Super-Resolution |SAT | [CVPR 2026](https://arxiv.org/abs/2604.07994) | [code](https://github.com/PhuTran1005/SAT) | transformer, attention, image, efficient, reconstruction, super-resolution |
|DRIFT: Deep Restoration, ISP Fusion, and Tone-mapping |DRIFT | [CVPR 2026](https://arxiv.org/abs/2604.03402) |  | GAN, NAFNet, image, multi-frame, denoising, super-resolution |
|VOSR: A Vision-Only Generative Model for Image Super-Resolution |VOSR | [CVPR 2026](https://arxiv.org/abs/2604.03225) | [code](https://github.com/cswry/VOSR) | diffusion, DiT, DINOv2, distillation, one-step, super-resolution |
|The Eleventh NTIRE 2026 Efficient Super-Resolution Challenge Report |NTIRE | [CVPR 2026](https://arxiv.org/abs/2604.03198) |  | CNN, distillation, lightweight, benchmark, super-resolution |
|Beyond Ground-Truth: Leveraging Image Quality Priors for Real-World Image Restoration |IQPIR | [CVPR 2026](https://arxiv.org/abs/2603.29773) |[code](https://github.com/fengyang1399-pixel/IQPIR)  | GAN, transformer, image, real-world, codebook, perceptual |
|Restore, Assess, Repeat: A Unified Framework for Iterative Image Restoration |RAR | [CVPR 2026](https://arxiv.org/abs/2603.26385) | [code](https://restore-assess-repeat.github.io) | flow, diffusion, image, all-in-one, blind, restoration |
|EgoXtreme: A Dataset for Robust Object Pose Estimation in Egocentric Views under Extreme Conditions |EgoXtreme | [CVPR 2026](https://arxiv.org/abs/2603.25135) | [code](https://taegyoun88.github.io/EgoXtreme/) | dataset, benchmark, real-world, 6dof, egocentric, motion-blur |
|DUO-VSR: Dual-Stream Distillation for One-Step Video Super-Resolution |DUO-VSR | [CVPR 2026](https://arxiv.org/abs/2603.22271) |  | diffusion, DiT, GAN, one-step, video, super-resolution |
|OmniFM: Toward Modality-Robust and Task-Agnostic Federated Learning for Heterogeneous Medical Imaging |OmniFM | [CVPR 2026](https://arxiv.org/abs/2603.21660) |  | transformer, attention, medical, reconstruction, federated learning, frequency |
|Face2Scene: Using Facial Degradation as an Oracle for Diffusion-Based Scene Restoration |Face2Scene | [CVPR 2026](https://arxiv.org/abs/2603.16570) |[code](https://amirhossein-kz.github.io/face2scene/)  | diffusion, SD-Turbo, LoRA, one-step, blind, restoration |
|Empowering Semantic-Sensitive Underwater Image Enhancement with VLM |- | [AAAI 2026](https://arxiv.org/abs/2603.12773) |  | underwater, cross-attention, perceptual, LLaVA, image, enhancement |
|UCAN: Unified Convolutional Attention Network for Expansive Receptive Fields in Lightweight Super-Resolution |UCAN | [CVPR 2026](https://arxiv.org/abs/2603.11680) |  | attention, CNN, lightweight, large-kernel, linear-attention, super-resolution |
|Towards Universal Computational Aberration Correction in Photographic Cameras: A Comprehensive Benchmark Analysis |UniCAC | [CVPR 2026](https://arxiv.org/abs/2603.12083) | [code](https://github.com/XiaolongQian/UniCAC) | benchmark, CNN, transformer, blind, aberration correction, image |
|Compressed-Domain-Aware Online Video Super-Resolution |CDA-VSR | [CVPR 2026](https://arxiv.org/abs/2603.07694) | [code](https://github.com/sspBIT/CDA-VSR) | CNN, video, compressed-domain, motion vectors, online, super-resolution |
|Disentangled Textual Priors for Diffusion-based Image Super-Resolution |DTPSR | [CVPR 2026](https://arxiv.org/abs/2603.07430) |[code](https://github.com/JL6666JL/DTPSR)  | diffusion, cross-attention, frequency-aware, real-world, blind, super-resolution |
|Toward Real-world Infrared Image Super-Resolution: A Unified Autoregressive Framework and Benchmark Dataset |Real-IISR | [CVPR 2026](https://arxiv.org/abs/2603.04745) | [code](https://github.com/JZD151/Real-IISR) | autoregressive, codebook, infrared, real-world, super-resolution |
|CubeComposer: Spatio-Temporal Autoregressive 4K 360° Video Generation from Perspective Video |CubeComposer | [CVPR 2026](https://arxiv.org/abs/2603.04291) | [code](https://lg-li.github.io/project/cubecomposer) | diffusion, autoregressive, video, attention, 4K, 360 |
|All-in-One Image Restoration via Causal-Deconfounding Wavelet-Disentangled Prompt Network |CWP-Net | [TIP](https://arxiv.org/abs/2603.03839) |  | CNN, prompt, wavelet, blind, all-in-one, image |
|FiDeSR: High-Fidelity and Detail-Preserving One-Step Diffusion Super-Resolution |FiDeSR | [CVPR 2026](https://arxiv.org/abs/2603.02692) | [code](https://github.com/Ar0Kim/FiDeSR) | diffusion, distillation, one-step, fidelity, image, super-resolution |
|Learning Domain-Aware Task Prompt Representations for Multi-Domain All-in-One Image Restoration |DATPRL-IR | [ICLR 2026](https://arxiv.org/abs/2603.01725) | [code](https://github.com/GuangluDong0728/DATPRL-IR) | transformer, prompt, distillation, image, all-in-one, restoration |
|Downstream Task Inspired Underwater Image Enhancement: A Perception-Aware Study from Dataset Construction to Network Design |DTIUIE | [TIP](https://arxiv.org/abs/2603.01767) | [code](https://github.com/oucailab/DTIUIE) | CNN, attention, underwater, perceptual, image |
|Reparameterized Tensor Ring Functional Decomposition for Multi-Dimensional Data Recovery |RepTRFD | [CVPR 2026](https://arxiv.org/abs/2603.01034) | [code](https://github.com/YangyangXu2002/RepTRFD) | INR, tensor, reconstruction, inpainting, point cloud |
|Neural Discrimination-Prompted Transformers for Efficient UHD Image Restoration and Enhancement |UHDPromer | [IJCV](https://arxiv.org/abs/2603.00853) | [code](https://github.com/supersupercong/uhdpromer) | transformer, attention, image, lightweight, restoration, 4K |
|ShiftLUT: Spatial Shift Enhanced Look-Up Tables for Efficient Image Restoration |ShiftLUT | [CVPR 2026](https://arxiv.org/abs/2603.00906) | [code](https://github.com/Sailor-t/ShiftLUT) | LUT, CNN, efficient, lightweight, image, restoration |
|AlignVAR: Towards Globally Consistent Visual Autoregression for Image Super-Resolution |AlignVAR | [CVPR 2026](https://arxiv.org/abs/2603.00589) |  | autoregressive, attention, image, efficient, perceptual, super-resolution |
|SR3R: Rethinking Super-Resolution 3D Reconstruction With Feed-Forward Gaussian Splatting |SR3R | [CVPR 2026](https://arxiv.org/abs/2602.24020) |[code](https://xiangfeng66.github.io/SR3R/)  | transformer, 3D, gaussian splatting, feed-forward, super-resolution |
|DACESR: Degradation-Aware Conditional Embedding for Real-World Image Super-Resolution |DACESR | [TIP](https://arxiv.org/abs/2602.23890) | [code](https://github.com/nathan66666/DACESR.git) | Mamba, contrastive learning, real-world, image, super-resolution |
|Physics-Consistent Diffusion for Efficient Fluid Super-Resolution via Multiscale Residual Correction |ReMD | [CVPR 2026](https://arxiv.org/abs/2603.00149) | [code](https://github.com/lizhihao2022/ReMD) | diffusion, fluid, wavelet, multigrid, super-resolution |
|GSNR: Graph Smooth Null-Space Representation for Inverse Problems |GSNR | [CVPR 2026](https://arxiv.org/abs/2602.20328) |  | graph laplacian, plug-and-play, diffusion, image, reconstruction, inverse problems |
|Deep LoRA-Unfolding Networks for Image Restoration |LoRA | [TIP](https://arxiv.org/abs/2602.18697) |  | deep unfolding, LoRA, reconstruction, compressive sensing, image, efficient |
|Vascular anatomy-aware self-supervised pre-training for X-ray angiogram analysis |XA-SSL | [AAAI 2026](https://arxiv.org/abs/2602.11536) | [code](https://github.com/Dxhuang-CASIA/XA-SSL) | transformer, masked image modeling, medical, pre-training, reconstruction |
|Eliminating VAE for Fast and High-Resolution Generative Detail Restoration |GenDR-Pix | [ICLR 2026](https://arxiv.org/abs/2602.10630) | | diffusion, distillation, one-step, real-world, 4K, super-resolution |
