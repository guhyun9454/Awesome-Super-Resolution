# 초해상도(Super-Resolution) 연구 동향 보고서

> 출처: [ChaofWang/Awesome-Super-Resolution](https://github.com/ChaofWang/Awesome-Super-Resolution)
> 작성일: 2026-06-09
> 범위: **2026년 논문 전체(35편)** + **시대별 핵심 landmark 논문(27편)** — 모두 원문(arXiv/openaccess/project page)을 직접 확인하여 정리
> 방법: 논문 1편당 1개의 리서치 에이전트를 배정해 초록·도입·방법·결과를 직접 읽고 구조화 요약 → 본 보고서로 통합

---

## 목차

1. [한눈에 보는 요약 (TL;DR)](#1-한눈에-보는-요약-tldr)
2. [Part I — 2026년 논문 전체 리뷰 (35편)](#part-i--2026년-논문-전체-리뷰-35편)
3. [Part II — 시대별 Landmark 논문 (27편)](#part-ii--시대별-landmark-논문-27편)
4. [Part III — 큰 그림: landmark에서 2026으로 이어지는 흐름](#part-iii--큰-그림-landmark에서-2026으로-이어지는-흐름)
5. [부록 — 전체 논문 인덱스 & 링크](#부록--전체-논문-인덱스--링크)

---

## 1. 한눈에 보는 요약 (TL;DR)

2026년 상반기 SR 분야는 **"생성 prior를 어떻게 빠르고 충실하게 쓸 것인가"** 라는 한 가지 질문으로 수렴하고 있습니다.

- **One-step diffusion이 표준이 됨.** 2024년 SinSR가 연 "1-step diffusion SR"이 2026년에는 기본기가 됨 (FiDeSR, GenDR-Pix, DUO-VSR, Face2Scene 모두 1-step). 이제 경쟁축은 "몇 step이냐"가 아니라 **fidelity와 detail을 1-step에서 동시에 잡기**, 그리고 **VAE/타일링 같은 진짜 병목 제거**(GenDR-Pix)로 옮겨감.
- **Visual Autoregressive(VAR)가 diffusion의 대항마로 본격 등장.** 2025년 VARSR의 next-scale prediction이 2026년에 AlignVAR(전역 일관성 보강), Real-IISR(적외선)로 확산. diffusion 대비 10배 이상 빠른 비반복 추론이 셀링포인트.
- **T2I prior 의존을 깨려는 움직임.** VOSR는 "텍스트-이미지 사전학습 없이 vision-only로 from-scratch 학습"해도 SD 기반 SOTA를 따라잡음을 보임 (학습비용 1/10). 텍스트 conditioning이 복원 과제에 부적합하다는 문제의식.
- **"Ground-truth를 넘어서"라는 새 패러다임.** IQPIR는 GT 자체가 불완전하므로 NR-IQA prior로 GT 품질 상한을 넘자고 제안. RAR는 복원기와 품질평가기를 한 latent loop에 묶어 "언제 멈출지"를 스스로 판단.
- **효율성 경쟁의 무게중심이 FLOPs→실측 런타임으로.** NTIRE 2026 efficient SR 챌린지가 런타임 가중치 0.8을 부여, 우승팀이 **fused CUDA 커널**로 5.26ms 달성. ShiftLUT·UHDPromer·UCAN은 sub-1M 파라미터로 4K/엣지 복원을 노림.
- **도메인 확장.** 적외선(Real-IISR), 유체 시뮬레이션(ReMD, 물리 일관성), 3D Gaussian Splatting(SR3R), 수중(UIE-VLM·DTIUIE), 광학 수차(UniCAC), 텐서 복원(RepTRFD)으로 SR 기법이 전방위 확산.
- **Reference-based SR의 부활.** Ada-RefSR("trust but verify" 게이팅), LiveMoments(Live Photo), Face2Scene(얼굴을 열화 oracle로) — 모두 "신뢰할 수 있는 reference를 언제·어떻게 쓸 것인가"에 집중.

---

# Part I — 2026년 논문 전체 리뷰 (35편)

리포지토리의 2026 섹션에 등재된 35편을 **방법론 계열별로** 묶어 정리합니다. 각 항목은 [모델] 제목 / 학회 / 문제 / 핵심 방법 / 의의 / 대표 수치 순서입니다. (SR과 다소 거리가 있는 3편 — EgoXtreme, OmniFM, XA-SSL — 은 §1.10에 따로 묶었습니다.)

## 1.1 One-step Diffusion (1-step 생성 SR)

### [FiDeSR] High-Fidelity and Detail-Preserving One-Step Diffusion SR — CVPR 2026
- **문제:** OSEDiff·PiSA-SR 같은 1-step diffusion SR이 "디테일 보존"과 "고충실도"를 동시에 못 잡음.
- **방법:** Frozen SD 2.1-base + rank-8 LoRA를 1-step으로 distill. 세 가지 장치 — ① **Detail-Aware Weighting(DAW):** Sobel/Laplacian + error map으로 난이도 맵을 만들어 손실을 공간적으로 재가중, ② **Latent Residual Refinement Block(LRRB):** latent 공간 RRDB가 U-Net의 거친 residual을 Δr로 보정, ③ **Latent Frequency Injection(LFIM):** FFT/Butterworth 기반 학습-불요 주파수 강조(추론 시 제어 가능).
- **의의:** error/detail 인식 손실 재가중 + latent residual 보정 + 학습불요 주파수 주입의 조합으로 PSNR/SSIM(충실도)와 LPIPS/FID/MANIQA(지각품질)를 동시에 끌어올린 새 1-step SOTA.
- **수치:** DRealSR PSNR 28.90 / LPIPS 0.284 / MANIQA 0.624 (PiSA-SR-1s 대비 전 지표 우위), ~0.078s/이미지(H100), ~1.29B params.

### [GenDR-Pix] Eliminating VAE for Fast and High-Resolution Generative Detail Restoration — ICLR 2026
- **문제:** 1-step diffusion SR에서조차 4K는 타일 처리가 필요. 저자들은 **병목이 U-Net이 아니라 VAE 인코더/디코더**임을 규명.
- **방법:** Latent 1-step 모델(GenDR)을 **pixel-space로 전환** — VAE를 ×8 pixel-(un)shuffle로 대체. 2단계 적대적 distillation으로 인코더→디코더 순차 제거(직전 단계 모델을 discriminator로 재사용), random padding 증강 + masked Fourier 손실.
- **의의:** "효율 병목 = VAE"라는 재프레이밍. latent teacher 품질을 유지하면서 **타일링 없이 4K를 ~1초/~6GB**로 복원.
- **수치:** GenDR 대비 2.8배 빠름·메모리 60%↓, 866M params, RealSet80 MUSIQ 69.92, 32ms(A100, ×4).

### [Face2Scene] Using Facial Degradation as an Oracle for Diffusion-Based Scene Restoration — CVPR 2026
- **문제:** 인물 사진은 얼굴엔 고품질 reference(신원)가 있지만 배경엔 없음. 전체 장면의 미지 열화를 어떻게 추정할까?
- **방법:** ① reference 기반 얼굴 복원(FaceMe), ② 복원된 HQ-LQ 얼굴 쌍에서 **열화 코드를 추출(FaDeX, contrastive)** → MapNet이 21개 multi-scale 토큰으로 변환 → 1-step SD-Turbo가 이 토큰을 조건으로 **장면 전체를 1 step에 복원**. 얼굴이 "열화 oracle" 역할.
- **의의:** 인물 사진의 비대칭성(얼굴=reference 有, 장면=無)을 활용해 blind 전장면 복원을 reference-grounded 문제로 전환.
- **수치:** InScene real에서 S3Diff 대비 LPIPS 0.250 vs 0.515, PSNR 22.90 vs 17.14, 3.3s/1024px.

> (영상 1-step diffusion **DUO-VSR**은 §1.7 Video SR에서 다룸.)

## 1.2 Diffusion SR (multi-step / bridge / vision-only)

### [E-Bridge] Energy-oriented Diffusion Bridge for Image Restoration — arXiv 2026
- **문제:** diffusion bridge는 열화↔클린을 직접 잇지만 trajectory의 transport energy가 커서 step이 많이 필요. 대형 foundation diffusion 활용도 비자명.
- **방법:** **FLUX-dev** 위에 energy 지향 bridge 구축. 저비용 manifold geodesic을 근사 — 짧은 time horizon + 열화이미지와 가우시안 노이즈를 섞은 entropy-정규화 시작점. consistency-model 정식화로 5–10 NFE 샘플링. trajectory 길이를 과제별 조절 knob로.
- **의의:** 거대 T2I foundation diffusion(FLUX)을 5–10 step 복원 bridge로 전환, bridge 길이를 에너지 최소화/geodesic 문제로 재해석.
- **수치:** 10 NFE에서 SR PSNR 21.28 / MUSIQ 70.87 / NIQE 3.84, denoising PSNR 26.06. SR·denoise·raindrop·low-light·demoiréing 전반.

### [VOSR] A Vision-Only Generative Model for Image SR — CVPR 2026
- **문제:** 생성 SR이 SD 같은 T2I prior에 의존 → 큰 학습비용, 복원에 부적합한 텍스트 conditioning, hallucination. **텍스트 없이 vision-only로 from-scratch** 학습 가능한가?
- **방법:** LightningDiT 백본(0.5B/1.4B) from-scratch. 텍스트 prompt 대신 frozen **DINOv2**로 LR의 공간적 semantic 추출. 표준 CFG의 unconditional branch가 from-scratch 복원에 부적합함을 보이고, **복원지향 guidance**(LR 구조 anchor를 약하게 유지하는 부분조건 branch)로 대체. multi-step 학습 후 **recursive-consistency distillation**으로 1-step화.
- **의의:** "T2I prior가 생성 SR에 필수"라는 통념을 반박 — vision-only가 SeeSR/OSEDiff/StableSR를 지각품질·효율에서 따라잡으면서 **학습비용 1/10**, hallucination 적음.
- **수치:** RealSR ×4 multi-step VOSR-1.4B: LPIPS 0.296 / MUSIQ 70.47 (SeeSR 상회). 1-step 0.095s. 신규 ScreenSR 벤치마크.

### [DTPSR] Disentangled Textual Priors for Diffusion-based Image SR — CVPR 2026
- **문제:** 텍스트 가이드 SR이 전역 캡션 또는 지역 태그만 쓰고, 구조와 텍스처를 한 latent에 뒤섞음 → 제어성↓, 심한 열화 시 hallucination.
- **방법:** ControlNet-style SD-2-base. 텍스트 prior를 **공간 계층(전역/지역) × 주파수(저/고)** 두 축으로 분리. Mask2Former 분할 + LLaVA-7B가 전역 캡션·세그먼트별 저주파(형태/배치)·고주파(텍스처) 설명 생성 → 4개 전용 cross-attention(GTCA/LFCA/HFCA/LRCA)로 순차 주입. 3종 negative prompt로 multi-branch CFG.
- **의의:** 텍스트 prior를 스케일·주파수로 disentangle하면 전역 일관성과 텍스처 충실도를 동시에 개선하고 semantic-drift hallucination 감소. DisText-SR(~9.5만 쌍) 데이터셋 공개.
- **수치:** DIV2K-Val/RealSR/DRealSR 무참조 지각지표 전부 최고 (RealSR MUSIQ 71.84, MANIQA 0.602). SUPIR보다 작고 빠름(10.5B, 14.94s).

## 1.3 Reference-based SR

### [Ada-RefSR] Trust but Verify: Adaptive Conditioning for Reference-Based Diffusion SR — ICLR 2026
- **문제:** RefSR에서 실제 열화는 LQ-Ref 대응을 불안정하게 만들어, 순진한 reference 융합이 잘못된 디테일을 주입.
- **방법:** S3Diff(SD-Turbo distill) 기반 **1-step** RefSR. 핵심은 **Adaptive Implicit Correlation Gating(AICG)** — 학습형 summary token이 reference 패턴을 distill, LQ query와의 attention이 sigmoid per-token gate를 만들어 reference 기여를 조절. 명시적 dense matching 대신 기존 attention 사영 재활용 → "verify" 안전장치.
- **의의:** RefSR의 핵심 실패모드(열화 하 대응 불안정)를 경량 게이팅으로 해결하고, reference가 못 믿을 땐 단일영상 SR로 우아하게 퇴화. 1-step으로 sub-초 추론.
- **수치:** 512px 0.41s(>30배 빠름), 62.2M 학습 params/2.68B frozen. CUFED5 PSNR 20.48, 신규 Bird/Face 벤치.

### [LiveMoments] Reselected Key Photo Restoration in Live Photos via Reference-guided Diffusion — ICLR 2026
- **문제:** Live Photo에서 사용자가 다른 프레임을 키포토로 재선택하면, 원본 ISP 키포토 대비 품질 열화. 원본 키포토를 reference로 복원.
- **방법:** SD3-medium 기반 두-branch (ReferenceNet + RestorationNet), cross-attention K/V 융합. **Motion Alignment**이 두 순간의 변위 처리 — latent: RAFT flow를 attention bias로, image: Patch Correspondence Retrieval로 4K 타일 추론. ~20% 노이즈만 섞는 bridge-matching으로 6-step 샘플링.
- **의의:** Live Photo의 고유한 paired-reference 구조를 최초로 활용, 모바일 카메라 실데이터에서 4K 가능한 reference 복원.
- **수치:** SynLive260 PSNR 31.65 / LPIPS 0.083 (CoSeR 상회), 6 step, 4K ~40s(H20), 인간선호 69.6%. 신규 벤치 3종.

## 1.4 Generative / Visual Autoregressive (VAR)

### [AlignVAR] Towards Globally Consistent Visual Autoregression for Image SR — CVPR 2026
- **문제:** VAR 기반 SR의 두 약점 — 국소 편향 attention(공간구조 파편화)과 residual-only(next-scale) 감독(스케일 간 오차 누적).
- **방법:** 10개 점진 스케일에 걸친 scale-wise AR transformer(~1.06B, VAR 가중치 초기화). ① **Spatial Consistency AR(SCA):** LR Laplacian edge 가이드로 intra-scale attention을 학습형 마스크로 재가중, ② **Hierarchical Consistency Constraint(HCC):** residual-only CE에 전-스케일 latent 재구성 감독을 추가해 오차 조기 교정.
- **의의:** 적절히 제약한 scale-wise VAR이 diffusion의 고품질 대안임을 입증 — 비반복 추론으로 **10배 이상 빠르고** 파라미터 적으면서 지각·충실도 모두 VARSR 상회.
- **수치:** 512px 0.43s (PASD 5.94s/StableSR 15.32s 대비). DIV2K-Val FID 25.71(VARSR 28.64), RealSR MUSIQ 68.53.

### [Real-IISR] Real-world Infrared Image SR: Unified Autoregressive Framework + Benchmark — CVPR 2026
- **문제:** 실세계 적외선 SR은 미개척. 광학+센싱 열화가 구조 선명도와 열(radiometric) 충실도를 동시에 파괴, 실제 paired 벤치 부재.
- **방법:** pretrained VAR(next-scale) 백본. ① **Thermal-Structural Guidance(TSG):** heat map + edge map을 DINOv3로 인코딩·attention 융합·cross-attention 주입, ② **Condition-Adaptive Codebook(CAC):** 열화 조건부 저랭크(r=8) codebook 섭동, ③ **Thermal Order Consistency(TOC) loss:** 패치 단위 온도-강도 단조성 강제.
- **의의:** 최초 실세계 paired IR SR 벤치(**FLIR-IISR** 1,457쌍)와, 구조+온도 충실도를 동시 복원하는 thermally-guided VAR로 diffusion·기존 VAR SR 상회.
- **수치:** FLIR-IISR ×4 PSNR 29.51 / LPIPS 0.134 (DifIISR·VARSR 상회), ~1.14B params, 2.45 FPS(A800).

## 1.5 Transformer SR

### [SAT] Selective Aggregation Transformer for Image SR — CVPR 2026
- **문제:** Transformer SR의 self-attention은 O(N²) — 전역 문맥 vs 효율의 trade-off.
- **방법:** **Selective Aggregation Attention(SAA)** — query는 full(N×d) 유지, key/value는 K×d로 압축(전체 토큰의 ~3%) → O(N²d)→O(NKd). K개 KV 토큰은 **Density-driven Token Aggregation(DTA)**: 밀도 ρ × 고립도 δ로 중심 선택 + stratified subsampling + 유사도 가중 집계 + Feature Norm Restoration. Local/Selective Transformer block 교대.
- **의의:** KV 토큰을 97% 줄이는 밀도/고립 기반 클러스터링으로 무거운 transformer SR을 능가하면서 FLOPs 25–27%↓. 속도이론(정리 3.1)·오차한계(정리 3.2)까지 제시.
- **수치:** ×4 Manga109 32.85dB(19.5M/0.94T) vs PFT 32.63dB(19.8M/1.26T). DF2K 학습, Set5/14/B100/Urban100/Manga109.

## 1.6 Efficient / Lightweight SR

### [NTIRE 2026 Efficient SR Challenge Report] — CVPR 2026 Workshops
- **내용:** ×4 효율 SR 챌린지(15팀). 품질 하한(~26.9dB) 고정 후 **런타임(0.8)·FLOPs(0.1)·파라미터(0.1)** 가중 — 이론 복잡도보다 **실측 속도** 우선. baseline은 SPAN.
- **결과:** 우승 **XiaomiMM(SPANV2)**: 5.26ms / 0.139M / 9.11G — fused `span_attn_op` CUDA 커널로 DRAM 왕복 절감. 최소모델 ZenoSR 0.038M, 최소연산 2.68G. pruning+distillation·parameter-free attention·Mamba 분기·FFT 손실 등.
- **의의:** 경량 SR 효율 프런티어가 **FLOPs/파라미터 최적화 → 하드웨어 인식 fused-kernel 런타임 엔지니어링**(sub-6ms@0.14M)으로 이동했음을 공식화.

### [UCAN] Unified Convolutional Attention Network for Lightweight SR — CVPR 2026
- **문제:** hybrid CNN-Transformer SR이 정확도를 위해 window/kernel을 키우면 연산·메모리가 2차로 증가 → 엣지 부적합.
- **방법:** conv+attention 통합으로 effective receptive field를 싸게 확대. ① **Hedgehog Attention**(대칭 지수 feature map으로 rank collapse 방지, 선형 복잡도 전역 모델링), ② **BERFG**(Sharing Block이 attention map 계산, Receiving Block이 재사용 — semi-parameter sharing), ③ **Large Kernel Distillation**(fine 채널만 대형/dilated conv 분기), ④ ESA.
- **의의:** 선형 attention + 대형커널 distillation + 반-공유로 sub-1M 파라미터에서 transformer급 정확도 → 경량 SR Pareto 전선 확장.
- **수치:** ~0.7M params. ×4 Urban100 26.89/27.06, Manga109 31.50/31.63dB @ 38.1G MACs.

### [UHDPromer] Neural Discrimination-Prompted Transformers for Efficient UHD Restoration — IJCV 2026
- **문제:** 4K(3840×2160) full-res Transformer 복원은 연산 비현실적. low-light/dehaze/deblur를 효율적으로.
- **방법:** HR-LR feature 격차("neural discrimination")를 명시적으로 활용. **Neural Discrimination Prior(NDP)**를 ① **NDPA**(self-attention이 discrimination을 전역 인지), ② **NDPN**(NDP 가이드 연속 게이팅)에 주입. 입력을 ×8 downshuffle해 H/8×W/8에서 무거운 연산 후 **Feature SR + SR-Guided Recon**으로 디테일 복원.
- **의의:** HR-LR feature 불일치를 명시 모델링하면 sub-1M Transformer가 무거운 UHD baseline을 능가 → 4K 복원을 상용 HW에서 실현.
- **수치:** UHD-LL 27.16dB @ 0.743M / 32.56G / 0.12s(1024px). UHDformer 대비 FLOPs ~37%↓.

### [ShiftLUT] Spatial Shift Enhanced Look-Up Tables for Efficient Image Restoration — CVPR 2026
- **문제:** LUT 기반 복원은 초고속·HW친화적이나, 인덱싱 비용이 픽셀 수에 지수적이라 **수용영역이 작고 고정**.
- **방법:** ① **Learnable Spatial Shift(LSS):** 채널별 학습형 공간 오프셋으로 연속 LUT 조회가 이동된 위치를 집계 → 인덱스 크기 증가 없이 수용영역 확대, ② 비대칭 dual-branch(정보밀집 분기에 연산 집중), ③ **Error-bounded Adaptive Sampling(EAS):** 오차한계 내 LUT 압축.
- **의의:** LUT 복원의 고질적 수용영역 병목 극복 — 엣지 즉시 복원을 저장/속도 손해 없이 더 정확하게.
- **수치:** 직전 SOTA TinyLUT 대비 수용영역 3.8배·평균 PSNR +0.21dB, 저장·추론시간 동등.

## 1.7 Video SR

### [DUO-VSR] Dual-Stream Distillation for One-Step Video SR — CVPR 2026
- **문제:** diffusion VSR은 고품질이나 ~50 step으로 느림. 1-step distill 시 디테일·시간 일관성 손실, DMD-only는 과평활.
- **방법:** 50-step·1.3B video DiT를 1-step student로 3단계 distill. ① **Progressive Guided Distillation Init**(CFG distill + step 64→1 반감), ② **Dual-Stream Distillation**(DMD stream + **RFS-GAN** stream — score 모델을 discriminator 백본으로 재사용해 고주파 복원), ③ DPO 기반 **Preference-Guided Refinement**.
- **의의:** 분포매칭(DMD)과 적대(GAN)를 한 distillation에 결합해 DMD의 과평활을 해소 → diffusion VSR을 사실상 실시간급으로. AI생성 영상 복원(AIGC60)까지 확장.
- **수치:** 1 step, 21프레임 1080p 11.3s(SeedVR-7B 대비 ~50배, 50-step 대비 ~90배). YouHQ40 LPIPS 0.315/MUSIQ 66.91, 인간선호 전 baseline 상회.

### [CDA-VSR] Compressed-Domain-Aware Online Video SR — CVPR 2026
- **문제:** 온라인(causal·저지연) VSR은 flow/offset 추정 비용이 병목 → 고해상도 실시간 불가.
- **방법:** 단방향 recurrent. 코덱이 H.264 디코딩 중 **무료로 주는 메타데이터** 활용 — ① **MVGDA**: 모션벡터로 deformable offset 초기화·잔차만 학습, ② **RMGF**: 코딩 residual map을 공간 게이팅으로, ③ **FTAR**: I-프레임은 24블록·P-프레임은 12블록 적응 할당.
- **의의:** 기존 VSR이 버리던 코덱 메타데이터로 비싼 모션추정을 대체 → 스트리밍 지향 실시간 온라인 VSR.
- **수치:** 3.3M params, REDS4 ~93 FPS, 2K 25.1 FPS (직전 SOTA TMP 대비 2배+), 품질도 +0.13dB.

### [CubeComposer] Spatio-Temporal Autoregressive 4K 360° Video Generation — CVPR 2026
- **문제:** 일반 영상→몰입형 360° 파노라마 영상. 기존(Argus)은 ~1K + 사후 업스케일. native 4K는 메모리·seam 문제로 불가.
- **방법:** Wan 2.2 5B video DiT(flow-matching). 파노라마를 cubemap 6면×시간창으로 분해, **작은 시공간 블록을 coverage 순서로 autoregressive 생성**(peak 메모리 격감→native 4K). sparse context attention(O(C²)→O(CK)), cube-aware PE·topology padding·overlap blending으로 seam 제거.
- **의의:** 최초 native 4K 360° 영상 합성(기존 ≤1K+업스케일). AR cube-face/time-window 생성으로 메모리 제약 하 교차면·시간 일관성 유지.
- **수치:** 3840×1920, 4K FVD 2.22 (Argus 1K 대비 전 지표 우위). 신규 4K360Vid(11,832 클립). TencentARC 코드 공개.

## 1.8 Real-world SR / ISP / 광학

### [DRIFT] Deep Restoration, ISP Fusion, and Tone-mapping — CVPR 2026
- **문제:** 스마트폰이 노이즈 raw burst에서 HDR RGB를 만들어야 하나, 모바일 ISP는 NPU 연산자·지연 제약이 큼.
- **방법:** 2-stage. ① **DRIFT-MFP**: NAFNet 백본(deformable/transformer 없이 NPU 호환)이 11프레임(33ch)에서 정렬+denoise+demosaic+×4 SR 동시 수행. **Adversarial Perceptual Loss(APL)** — VGG 대신 discriminator pre-activation feature 사용으로 LPIPS grid 아티팩트 제거. ② **DRIFT-TM**: 전역+지역+메타데이터 인코더 톤매퍼.
- **의의:** NPU 배포 가능한 end-to-end raw→RGB 파이프라인(복원+SR+가변 톤매핑)을 스마트폰 지연 내에. discriminator-feature 손실이 LPIPS 아티팩트 없이 지각품질 달성.
- **수치:** ×4 SR PSNR 36.22, 톤매핑 PSNR 40.59. Snapdragon 8 Elite NPU 11프레임 12MP **<4초**.

### [IQPIR] Beyond Ground-Truth: Leveraging Image Quality Priors — CVPR 2026
- **문제:** 복원 모델은 GT 감독으로 학습되나 **GT 자체의 지각품질이 불균일** → 모델이 데이터셋 평균 품질로 수렴(GT 상한에 갇힘).
- **방법:** pretrained **NR-IQA**에서 distill한 **Image Quality Prior(IQP)**로 GT 상한 돌파. VQGAN codebook prior 위에 ① quality-conditioned Transformer(IQA 점수를 조건으로), ② dual-branch codebook(콘텐츠 vs 품질 분리, HQ+ codebook), ③ **discrete 공간**에서 IQA 최적화(픽셀공간 over-optimization 회피).
- **의의:** 복원의 감독 타깃을 재정의 — 불완전한 GT 회귀 대신 NR-IQA 지각품질로 최적화해 출력이 GT를 능가. plug-and-play, 얼굴/저조도/수중/역광 일반화.
- **수치:** LOL-v1 PSNR 25.72/SSIM 0.902, 얼굴복원 인간평가 4.03/5, ~5.63ms/이미지.

### [UniCAC] Towards Universal Computational Aberration Correction: Benchmark — CVPR 2026
- **문제:** 렌즈 광학수차(흐림·색수차·공간가변 PSF) 보정(CAC)이 특정 광학계에 과적합, 일반화 부족·표준 벤치 부재.
- **방법:** **자동 광학설계**로 다양한 렌즈를 생성(Zemax .zmx/.zda 공개)하고 공간가변 열화/클린 쌍 합성. **Optical Degradation Evaluator(ODE)**로 렌즈별 수차 난이도를 객관 정량화 → 난이도 층화 평가. transformer/CNN/GAN/diffusion 등 **24개 복원 알고리즘** 통합 비교.
- **의의:** 최초의 표준화·물리기반 cross-lens CAC 벤치 + 난이도 정량화 도구 → per-lens 과적합에서 보편 복원으로 전환. prior 활용·아키텍처·학습전략 3요소가 성능 좌우.

### [DACESR] Degradation-Aware Conditional Embedding for Real-World SR — TIP 2026
- **문제:** RAM/DAPE 같은 인식모델 semantic prior가 유용하나, 열화공간에서 naive contrastive로 fine-tune하면 쓸만한 degradation-aware embedding이 안 나옴.
- **방법:** GAN/diffusion이 **아닌** 경로. ① **Real Embedding Extractor(REE):** DAPE를 LoRA(r=8) fine-tune하되 텍스트 유사도 임계로 mild/severe 버킷팅 후 학습, ② **Conditional Feature Modulator(CFM):** affine 변조(αx+β)로 embedding 주입, ③ **Mamba(RSSB) 백본**(LAM상 픽셀을 더 선택적으로 활용).
- **의의:** 인식모델 prior를 degradation-aware하게 만들어 경량 Mamba SR을 조종 → 무거운 GAN/diffusion 실세계 SR의 ~3.65M 파라미터 대안.
- **수치:** ×4, ~3.65M params. DIV2K-val/RealSR/AIM2019 PSNR·LPIPS 보고.

## 1.9 All-in-one / Unified Restoration

### [RAR] Restore, Assess, Repeat: Unified Framework for Iterative Image Restoration — CVPR 2026
- **문제:** all-in-one 복원이 미지/복합 열화에 일반화·효율 약함. 복원과 품질평가를 분리하면 지연·정보손실↑, "얼마나 복원할지" 정지신호 부재.
- **방법:** IQA와 IR을 **하나의 latent-space 모델**로 융합, **Restore-Assess-Repeat 폐루프** 실행. **Latent Quality Assessment(LQA)**(VLM DepictQA를 latent에 적응)가 복원 백본 조건공간으로 직접 매핑(텍스트 디코딩 손실 회피). 백본은 SD3.5 + **noise-free flow matching**(degraded→HQ latent 직접). 매 iteration T=4 step 후 CONTINUE/STOP 판단(평균 ~2.8 라운드).
- **의의:** 학습형 IQA critic + 생성 복원기를 공유 latent loop에 묶어 **언제 멈출지 스스로 결정**하는 자기조절 복원 에이전트. 복합/미지 열화 SOTA, agentic baseline(AgenticIR) 대비 한 자릿수 배 빠름.
- **수치:** UDC MUSIQ 55.97/CLIP-IQA 0.602 (AgenticIR 상회), 6.3–6.9s (AgenticIR 48–78s 대비 11배+).

### [CWP-Net] All-in-One Restoration via Causal-Deconfounding Wavelet-Disentangled Prompt Network — TIP 2026
- **문제:** all-in-one 복원의 두 결함 — 비열화 semantic과 열화 패턴의 spurious correlation, 열화 추정 편향.
- **방법:** U자형 CNN + scale별 **Wavelet Attention**(DWT로 LL/LH/HL/HH 분해 후 attention)으로 열화-콘텐츠 disentangle. 편향 교정을 **인과추론(backdoor adjustment)**으로: T→Y 대신 T→P→Y(P=prompted wavelet subband), **Wavelet Prompt Block**(K-Means 기반 DWE + PWSFT).
- **의의:** 명시적 causal deconfounding + wavelet 주파수 disentangle을 all-in-one에 도입 — 학습형 prompt에만 의존하지 않고 spurious correlation 억제. 5/7-task 모두 SOTA, 15.5M params.
- **수치:** 5-task 평균 33.15dB(+0.59), 7-task 30.56dB(+2.22 over IDR), 130.55 GFLOPs.

### [DATPRL-IR] Learning Domain-Aware Task Prompt Representations for Multi-Domain All-in-One Restoration — ICLR 2026
- **문제:** all-in-one 복원이 단일 도메인(자연/의료/원격탐사 중 하나)에 국한. **multi-domain** all-in-one은 미개척.
- **방법:** task/domain 두 prompt pool(각 15개)을 이미지별 조회, **Prompt Composition Mechanism**으로 top-k softmax 가중 → instance-level 표현. domain awareness는 **LLaVA-1.5-7B**의 다관점 설명을 CLIP-text로 인코딩해 domain prompt에 cross-modal 정렬 distill.
- **의의:** all-in-one의 단일도메인 천장을 최초로 돌파 — task vs domain 지식 분리·재결합으로 자연·의료·원격탐사 동시 처리. MLLM prior distillation으로 저수준 비전에 semantic domain awareness 주입.
- **수치:** 6-task/3-domain 평균 PSNR 30.77/SSIM 0.865 (MoCEIR 대비 +0.37dB). RTX 5090, 1000K iter.

## 1.10 도메인 특화 SR / 역문제 / 텐서

### [SR3R] Rethinking Super-Resolution 3D Reconstruction With Feed-Forward Gaussian Splatting — CVPR 2026
- **문제:** 3D SR(저해상 multi-view→고해상 3D)이 dense 입력·per-scene 최적화 필요, 2D SR prior에만 의존.
- **방법:** sparse LR view → HR 3DGS **feed-forward 직접 회귀**(per-scene 최적화 제거). feed-forward 3DGS 백본(NoPoSplat/DepthSplat)에 plug-and-play. **Gaussian offset learning**(각 Gaussian을 6 sub-Gaussian으로 densify 후 PointTransformerV3가 잔차 보정) + bidirectional cross-attention.
- **의의:** 2D SR 출력을 lift하는 대신 multi-scene에서 3D 고주파를 end-to-end 학습 → 실시간 feed-forward(~1.7s vs 300–420s), zero-shot 교차데이터 일반화.
- **수치:** RE10K ×4 26.25dB/LPIPS 0.165 (DepthSplat 23.15 대비), zero-shot DTU 1.69s.

### [ReMD] Physics-Consistent Diffusion for Efficient Fluid SR via Multiscale Residual Correction — CVPR 2026
- **문제:** 유체장(대기/해양/난류) SR — 일반 이미지 SR/diffusion은 sampling 과다·물리제약 무시, 잘못된 에너지 스펙트럼·비발산 위배.
- **방법:** 유체 SR을 거친해 위 **잔차 보정(multigrid)** = few-step residual diffusion. reverse update에 **시간-게이트 multigrid V-cycle** drift(고정 parameter-free multiwavelet restriction/prolongation + 작은 학습형 smoother). 데이터 일관성 + **방정식-불요 물리 cue**(Laplacian/Perona-Malik/FFT radial spectrum) 결합. 초반 저주파·후반 고주파 강조.
- **의의:** 물리 일관성(발산/스펙트럼) + 고전 multigrid/multiwavelet 다중스케일을 diffusion reverse에 직접 내장 → 2–5 step으로 물리충실 유체 SR(ResShift 15 step 대비).
- **수치:** ReMD-5: NS PSNR 49.94, ERA5 58.19, Ocean 47.71 (ResShift-15 상회), 2–5 step. 코드 공개.

### [RepTRFD] Reparameterized Tensor Ring Functional Decomposition for Multi-Dimensional Data Recovery — CVPR 2026
- **문제:** 고전 Tensor Ring 분해는 고정 meshgrid 이산 텐서에 국한, 고주파·연속/비-meshgrid(점군) 데이터 모델링 약함.
- **방법:** 각 TR factor를 **INR로 연속 매개변수화**(공유 sinusoidal embedding + 소형 MLP). 핵심은 **reparameterization** — factor = 학습형 latent C ×₃ 고정 basis B. 주파수영역 분석으로 TR factor 스펙트럼이 재구성 주파수를 지배함을 보이고, variance 보존 Xavier 초기화 제안.
- **의의:** TR 분해를 연속 INR 기반 functional 표현으로 재해석, latent×고정basis 재매개화 + 원리적 초기화가 고주파 복원·최적화 안정성을 개선. inpainting/denoise/SR/점군을 한 프레임에.
- **수치:** LRTFR 대비 평균 ~1dB↑. SR ×4 Parrot 30.39 vs 27.81dB. per-instance ~13–56s.

### [GSNR] Graph Smooth Null-Space Representation for Inverse Problems — CVPR 2026
- **문제:** 이미징 역문제(deblur/CS/demosaic/SR)는 ill-posed — 센싱 연산자 H의 null-space("invisible" 성분)가 측정에 제약되지 않음. 표준 prior는 전체 이미지에 작용, null-space를 특정 regularize 못함.
- **방법:** range-null 분해 x=x_r+x_n에서 **null-space에만** graph-smoothness 부여. null-restricted graph Laplacian T=P_n L P_n 고유분해 → p개 최평활 모드로 저차원 사영. 소형 U-Net이 측정→null-space 계수 회귀. PnP/DIP/diffusion solver에 plug-in.
- **의의:** prior 설계를 연산자 null-space 중심으로 재프레임 — 관측불가 성분만 graph-spectral smoothness로 제약해 hallucination/편향 감소. 재학습 없이 drop-in.
- **수치:** baseline 대비 최대 +4.3dB. CelebA SR ×4 GSNR-PnP 29.42 vs 27.37dB.

### [LoRun] Deep LoRA-Unfolding Networks for Image Restoration — TIP 2026
- **문제:** Deep unfolding network(DUN)은 K단계마다 동일 denoiser를 복제 → 단계별 노이즈 차이 무시 + 파라미터 중복·메모리가 단계수에 선형 증가.
- **방법:** PGD를 K단계로 unfold하되, **단일 frozen base denoiser 공유 + 단계별 경량 LoRA adapter** 주입(LLM fine-tuning 차용). 적응형 rank 규칙. 2-phase 학습(백본 pretrain → frozen 후 LoRA만 fine-tune).
- **의의:** unfolding 파라미터 효율을 재프레임 — 단계 수와 파라미터/메모리 증가를 분리. backbone-agnostic(U-Net/Transformer/CNN).
- **수치:** 9단계 CS 38.16dB @ 5.23M(baseline 15.96M의 ~33%). CASSI +1.3dB @ 17.9% params. SR 파라미터 63%↓.

## 1.11 수중 영상 향상 (Underwater Image Enhancement)

### [UIE-VLM] Empowering Semantic-Sensitive Underwater Image Enhancement with VLM — AAAI 2026 (Oral)
- **문제:** UIE가 균일 전역복원을 추구 → 분포가 자연영상과 어긋나고 의미상 중요한 객체 무시, downstream 성능 제한.
- **방법:** LLaVA가 핵심객체 텍스트 생성 → BLIP가 patch-text 유사도로 **공간 semantic 가이드 맵** 생성(power-law sharpening). 이 맵이 표준 enc-dec UIE를 **dual-guidance**(semantic-modulated cross-attention + semantic alignment loss)로 조종. model-agnostic plug-in.
- **의의:** VLM 객체 semantic을 기존 UIE에 싸게 주입 → 품질뿐 아니라 downstream detection/segmentation 동시 개선. 저수준 향상과 고수준 인식 연결.
- **수치:** UIEB에서 5개 baseline 모두 향상(PFormer-SS +1.44dB). detection mAP +0.88~1.37, seg mIoU +1.93~5.41.

### [DTIUIE] Downstream Task Inspired Underwater Image Enhancement — TIP 2026
- **문제:** UIE는 보통 인간 지각용 최적화 → downstream 인식엔 도움 안 되거나 해로움.
- **방법:** UIE를 task-driven 전처리로 재프레임. **TI-UIED 데이터셋**(9개 UIE로 향상 후 7개 분할 아키텍처에서 일관 개선되는 변형 선택, ~210 모델 학습). dual-branch(Feature Restoration + Detail Enhancement), Task-Aware CTB(cross-modal mixed-query attention), Task-Driven Perceptual loss.
- **의의:** UIE 평가를 인간지각→downstream 효용으로 전환, UIQM/UCIQE가 task 이득과 무상관임을 실증. task-first 설계 철학.
- **수치:** SUIM seg UNet mIoU 66.52→67.61, RUOD detection mAP 52.1→53.0. 9.5M params.

## 1.12 SR과 거리가 있는 등재 논문 (참고)

리포지토리에 함께 등재됐으나 SR 기법이 아니라 복원을 **평가용 baseline으로만** 쓰거나 다른 과제를 다루는 3편:

- **[EgoXtreme] (CVPR 2026):** 극한조건(저조도·모션블러·연기) egocentric 6D 객체 자세추정 **데이터셋**. deblurring을 전처리 baseline으로 평가했으나 효과 없음으로 결론. SR 방법 제안 아님.
- **[OmniFM] (CVPR 2026):** 이종 의료영상 **연합학습(FL)** 프레임워크. 5개 task 중 하나로 SR(병리 BreaKHis ×2 PSNR 42.21dB) 포함하나 핵심은 주파수영역 modality-invariance + FL.
- **[XA-SSL / VasoMIM] (AAAI 2026):** X-ray 혈관조영 **self-supervised 사전학습**(anatomy-aware MIM). 분할/검출용 foundation model이며 SR 아님. XA-170K(17만 장) 공개.

---

# Part II — 시대별 Landmark 논문 (27편)

SR 분야의 패러다임을 정의한 핵심 논문을, **CNN기 → GAN/perceptual기 → 아키텍처 진화 → 분포모델링(Flow) → Transformer기 → 실세계 degradation → Diffusion기 → 효율(few/one-step) → 생성/AR기**의 시대 흐름으로 정리합니다. (모든 논문 원문 직접 확인)

## 2.1 CNN 도입기 (2014–2016)

### [SRCNN] Image Super-Resolution Using Deep Convolutional Networks — ECCV 2014 / TPAMI 2016
딥러닝 SR의 **시초**. bicubic 업샘플 입력→HR을 3-layer FCN으로 end-to-end 학습(MSE). sparse-coding SR 파이프라인이 CNN의 특수형임을 이론적으로 연결. ~8K 파라미터로 기존 example-based SR 능가. **이후 모든 SR 연구 라인의 출발점.** (Set5 ×3 ~32.4–32.7dB)

### [FSRCNN] Accelerating the Super-Resolution CNN — ECCV 2016
SRCNN 가속. **LR에서 직접 처리, 끝에서만 deconvolution으로 업샘플** + hourglass(shrink-map-expand) 구조. PReLU, scale-shared 백본(스케일 변경 시 deconv만 fine-tune). **"무거운 연산은 LR 해상도에서"라는 효율 SR 원칙 확립.** FSRCNN-s는 CPU 실시간(24.7fps).

### [ESPCN] Efficient Sub-Pixel Convolutional Network — CVPR 2016
**Sub-pixel convolution(pixel shuffle)** 도입 — 마지막 conv가 r² 채널 생성 후 주기적 재배열로 업샘플. 모든 conv를 LR 공간에서 수행, SRCNN보다 한 자릿수 배 빠름(실시간 1080p). **pixel-shuffle은 이후 거의 모든 SR(SRGAN·EDSR·RCAN…)의 표준 업샘플 primitive가 됨.**

### [VDSR] Accurate Image SR Using Very Deep Networks — CVPR 2016 (Oral)
**깊이(20층) + residual learning**. 고주파 residual만 예측, adjustable gradient clipping으로 SRCNN의 10⁴배 학습률 사용, **단일 multi-scale 모델**(×2/3/4 동시). "깊이+residual=고정확도 CNN SR" 확립, residual/global-skip·multi-scale 학습이 표준 패턴화. (Set5 ×2 ~37.53dB)

## 2.2 GAN / Perceptual기 (2017–2018)

### [SRGAN] Photo-Realistic SR Using GAN — CVPR 2017
**최초의 GAN 기반 4× SR.** Generator=SRResNet(16 residual block + pixel-shuffle). 핵심은 **perceptual loss = VGG feature content loss + adversarial loss** — 픽셀 MSE의 과평활(여러 해의 평균) 탈피. MOS 연구로 **PSNR/SSIM이 지각품질의 나쁜 대리지표**임을 실증. 이후 모든 perceptual/GAN SR의 토대.

### [EDSR] Enhanced Deep Residual Networks — CVPRW 2017 (NTIRE 우승)
**residual block에서 BatchNorm 제거**(범위 유연성 보존, 메모리 40%↓) → 더 크고 깊은 망(32블록/256채널/~43M). residual scaling(0.1), L1 loss, DIV2K, MDSR(multi-scale). "BN은 SR에 해롭다 + BN-free residual을 키우면 큰 이득"이라는 설계원칙 확립, **장수 baseline.**

### [RDN] Residual Dense Network — CVPR 2018 (Spotlight)
모든 conv 층의 hierarchical feature를 활용·융합. **Residual Dense Block**(dense connectivity + local feature fusion + local residual) + contiguous memory + global feature fusion. "dense+residual 융합"을 표준 블록화, RCAN의 전조. (Set5 ×2 ~38.24dB)

### [RCAN] Very Deep Residual Channel Attention Networks — ECCV 2018
**SR 최초의 channel attention.** Residual-in-Residual(~400층, 저주파는 skip으로 우회) + squeeze-excitation식 채널 재가중(RCAB). 저주파 우회로 EDSR보다 깊은 망 안정 학습. **이후 attention 기반 SR의 템플릿.** (Urban100 ×4 26.63dB)

### [ESRGAN] Enhanced SRGAN — ECCVW 2018 (PIRM 우승)
SRGAN 개선 3축 — ① **RRDB**(residual-in-residual dense, BN 제거), ② **Relativistic average GAN**(상대적 realness), ③ **VGG pre-activation feature** perceptual loss. **Network interpolation**으로 fidelity↔perception 무재학습 조절. **perceptual SR의 de-facto 백본**(Real-ESRGAN 등 무수히 재사용).

## 2.3 분포 모델링 — Normalizing Flow (2020)

### [SRFlow] Learning the SR Space with Normalizing Flow — ECCV 2020 (Spotlight)
SR을 **조건부 밀도추정 p(HR|LR)**로 재정의. RRDB 인코더 조건 + Glow식 invertible flow, **단일 NLL loss**(GAN의 brittle 다중손실 탈피). 온도 조절로 다양한 사실적 해 샘플링. flow 기반/확률적 SR 라인을 시드, diffusion의 분포모델링 전환에 영향. CelebA에서 LPIPS 절반↓.

## 2.4 Transformer기 (2021, 2023)

### [SwinIR] Image Restoration Using Swin Transformer — ICCVW 2021
Swin의 shifted-window self-attention을 저수준 비전에. **Residual Swin Transformer Block**(window attention + conv + residual)로 SR/denoise/JPEG 통합. IPT(115M)의 ~1/10 파라미터(11.8M)로 CNN·transformer SOTA 능가. **이후 거의 모든 transformer 복원의 표준 백본.** (+0.45dB)

### [HAT] Activating More Pixels in Image SR Transformer — CVPR 2023
LAM 분석으로 SwinIR이 입력 픽셀을 좁게만 쓴다는 점 규명. **Hybrid Attention**(window self-attention + channel attention) + **Overlapping Cross-Attention Block**(겹치는 K/V window) + **same-task ImageNet 사전학습**. SwinIR 대비 +1dB↑. classical SR 새 SOTA, 강력한 백본/baseline. (Urban100 ×4 27.97dB)

## 2.5 실세계 Degradation 모델링 (2021)

### [BSRGAN] Practical Degradation Model for Deep Blind SR — ICCV 2021
**무작위 셔플 degradation 모델** — blur·다중 downsample·노이즈(Gaussian/JPEG/ISP 센서노이즈)를 **고정 순서가 아닌 random order**로 합성해 degradation 공간 대폭 확대. ESRGAN/RRDB를 이 쌍으로 학습. **"더 나은 추정기보다 더 풍부한 degradation 합성"이라는 data-centric 패러다임 확립** → Real-ESRGAN에 직접 영향.

### [Real-ESRGAN] Training Real-World Blind SR with Pure Synthetic Data — ICCVW 2021
**High-order(2차) degradation** — blur-downsample-noise-JPEG를 두 번 반복, sinc 필터로 ringing/overshoot 합성. U-Net + spectral norm discriminator(픽셀 단위 피드백). 실 paired 데이터 없이 순수 합성으로 SOTA. **실세계 blind SR의 de-facto 실용 도구/baseline**, degradation 파이프라인이 후속 diffusion 복원에도 표준 채택.

## 2.6 Diffusion기 (2022–2024)

### [SR3] Image Super-Resolution via Iterative Refinement — TPAMI 2022
**최초로 DDPM을 조건부 SR에 적용.** LR을 채널 concat 조건으로 U-Net이 노이즈 예측, 가우시안 노이즈→반복 정제. cascaded 생성(64→256→1024). 인간 fool rate ~50%(GAN ~34%). **diffusion이 GAN/회귀를 능가함을 입증, diffusion 복원 물결의 토대**(Imagen 등에 영향).

### [ResShift] Efficient Diffusion SR by Residual Shifting — NeurIPS 2023 (Spotlight)
HR→노이즈 대신 **HR↔LR 잔차를 shifting하는 짧은 Markov chain**. 시작점이 이미 강한 추정치 → **15 step**(확장판 4 step)으로 SOTA급. VQGAN latent + SwinUNet. **few/one-step diffusion SR 물결의 영향력 있는 전조**, 표준 효율 baseline.

### [StableSR] Exploiting Diffusion Prior for Real-World SR — IJCV 2024
**Frozen SD + 경량 trainable time-aware encoder**(SFT 변조 주입)로 생성 prior 보존·저비용 학습. **Controllable Feature Wrapping**(스칼라 w로 fidelity↔realism 조절) + progressive patch aggregation(임의 해상도). **"frozen diffusion prior + 소형 adapter" 패러다임 확립**(DiffBIR/PASD/SeeSR로 확장). ~200 step.

### [DiffBIR] Towards Blind Image Restoration with Generative Diffusion Prior — ECCV 2024
**2-stage 분리** — ① degradation 제거(SwinIR식 regressor로 클린 조건영상), ② **IRControlNet**(frozen SD 2.1에 ControlNet)이 디테일 재생성. 핵심은 **raw 열화가 아닌 정제된 조건영상으로 ControlNet 학습** → 안정적 conditioning. region-adaptive guidance로 fidelity↔realism. blind SR/face/denoise 통합, 널리 인용되는 baseline.

### [SeeSR] Towards Semantics-Aware Real-World SR — CVPR 2024
심한 열화가 semantic cue를 손상 → semantic hallucination. **Degradation-Aware Prompt Extractor**(RAM을 LoRA fine-tune해 hard 태그 + soft embedding, 열화-강건). soft prompt를 **Representation Cross-Attention**으로 주입, **LR Embedding** 추론 트릭. **diffusion Real-ISR의 semantic 충실도 표준 확립**, 후속 one-step 방법들의 비교 대상. (RealSR PSNR 25.18/MUSIQ 69.77)

## 2.7 효율 — Few/One-step Diffusion (2024)

### [SinSR] Diffusion-Based Image SR in a Single Step — CVPR 2024
ResShift(15 step)를 **1-step**으로 distill. ResShift의 확률적 Markov reverse를 **결정적 non-Markovian(DDIM式)** 과정으로 재정식화 → 고정 invertible 매핑 학습. **consistency-preserving loss**(forward + inverse + **GT consistency**)로 teacher를 맹목 모방하지 않고 GT로 직접 감독 → teacher 능가. ~10배 가속. **one-step diffusion SR 라인 개막**(OSEDiff 등으로 확장). NFE=1.

## 2.8 생성/AR 최신기 (2025)

### [FluxSR] One Diffusion Step to Real-World SR via Flow Trajectory Distillation — ICML 2025
**FLUX.1-dev(~12B flow-matching DiT)**를 base이자 teacher로. **Flow Trajectory Distillation** — noise→image flow(고정)와 noise→LR flow를 결합해 **"noise-free flow"** 유도 → teacher ISR 모델·실 paired 데이터 불요. TV-LPIPS + Attention Diversification Loss로 고주파 아티팩트 억제. **대형 flow-matching T2I를 1-step Real-ISR로 distill한 최초 사례.** (RealSR MUSIQ ~68.95)

### [VARSR] Visual Autoregressive Modeling for Image SR — ICML 2025
SR을 **VAR next-scale prediction**으로 재정의(diffusion 대안). LR을 prefix token으로 주입, scale-aligned RoPE, VQ 양자화 손실을 보정하는 **diffusion refiner**, image-based CFG. **VAR이 diffusion의 효율적 대안임을 입증** — 다중 step 샘플링 비용 절감. (2026 AlignVAR·Real-IISR의 직접 조상)

### [DiT4SR] Taming Diffusion Transformer for Real-World SR — ICCV 2025
**DiT 기반 SD3(3.5-medium)를 Real-ISR에 적응.** ControlNet식 단방향 주입 대신 **LR latent를 DiT 고유 multi-modal self-attention에 직접 임베딩**(양방향 정보흐름, timestep-적응 conditioning). cross-stream convolution으로 국소 디테일 보강. **거대 DiT/SD3 prior를 복원에 성공 적용** — UNet/SD-2 지배 패러다임을 넘는 새 백본 방향. (RealSR MUSIQ 70.47)

## 2.9 Video SR 계보 (2019–2024) — 추가 landmark

### [EDVR] Video Restoration with Enhanced Deformable Convolutional Networks — CVPRW 2019 (NTIRE19 4관왕)
**Sliding-window** VSR. optical flow 대신 **feature-level deformable conv 정렬** — **PCD**(pyramid+cascading deformable, coarse-to-fine) + **TSA**(temporal+spatial attention 융합). 대규모/복잡 모션을 flow 없이 처리. **deformable-conv 정렬을 flow-warping의 강건한 대안으로 확립**, BasicVSR 계열의 기반/백본. (REDS4 ~31.09dB)

### [BasicVSR] The Search for Essential Components in VSR — CVPR 2021
VSR을 **Propagation/Alignment/Aggregation/Upsampling** 4요소로 분해. **양방향 recurrent 전파**(sliding-window 대신 전 시퀀스 집계) + **optical-flow feature warping** 정렬(deformable 아님). 간결·경량 파이프라인이 EDVR 능가. IconVSR(정보-refill + coupled propagation). **canonical VSR baseline, BasicVSR++의 토대.** (REDS4 ~31.42dB)

### [BasicVSR++] Improving VSR with Enhanced Propagation and Alignment — CVPR 2022
BasicVSR 개선 — ① **2차 grid propagation**(grid식 양방향 + 2차 연결로 더 공격적 집계), ② **flow-guided deformable alignment**(flow로 deformable offset 안정화). BasicVSR 대비 **+0.82dB**(동등 파라미터). NTIRE 2021 VSR 3관왕. **후속 VSR의 지배적 baseline/백본.** (REDS4 ~32.39dB)

### [VRT] A Video Restoration Transformer — TIP 2024 (arXiv 2022)
**병렬(parallel) Transformer** VSR — mutual-attention 기반 implicit alignment + flow 기반 병렬 warping. recurrent(BasicVSR)·sliding-window CNN(EDVR) 모두 능가, **deformable-conv 정렬의 강한 대안**. 5개 복원 task에서 SOTA(최대 +2.16dB), RVRT의 직접 전조. (REDS4 32.19dB/16프레임)

---

# Part III — 큰 그림: landmark에서 2026으로 이어지는 흐름

## 3.1 계보도 (어느 landmark가 어떤 2026 논문으로 이어지는가)

| 시대 / 조상 landmark | 핵심 아이디어 | 2026년 직계 후손 |
|---|---|---|
| ESPCN(pixel-shuffle), FSRCNN | "연산은 LR에서, 끝에서만 업샘플" | NTIRE2026-ESR, ShiftLUT, UHDPromer, UCAN — 효율 SR 전부 |
| SRGAN→ESRGAN | perceptual loss, discriminator-feature | DRIFT(APL: discriminator pre-act feature), DUO-VSR(RFS-GAN) |
| RCAN(channel attention)→SwinIR→HAT | attention 백본 | SAT(selective KV attention), UCAN(hybrid/Hedgehog attention) |
| BSRGAN→Real-ESRGAN | 합성 degradation 파이프라인 | VOSR·FiDeSR·GenDR-Pix·Face2Scene 학습(RealESRGAN degradation), DACESR |
| SR3→StableSR→DiffBIR→SeeSR | frozen diffusion prior + adapter, semantic prompt | DTPSR(disentangled text prior), E-Bridge(FLUX bridge), DACESR(REE) |
| ResShift→SinSR | 잔차 기반 few-step → 1-step distill | FiDeSR, GenDR-Pix, DUO-VSR, Face2Scene (모두 1-step) |
| FluxSR | flow-matching T2I distill | E-Bridge(FLUX consistency bridge), GenDR-Pix |
| VARSR | next-scale visual AR | AlignVAR(전역 일관성), Real-IISR(적외선 VAR) |
| DiT4SR | DiT/SD3 prior 적응 | LiveMoments(SD3-medium), VOSR(LightningDiT from-scratch) |
| EDVR→BasicVSR→BasicVSR++→VRT | 정렬+전파 VSR | DUO-VSR(1-step), CDA-VSR(online), CubeComposer(4K AR) |

## 3.2 2026년을 관통하는 5대 트렌드

1. **속도의 재정의 — "step 수"에서 "진짜 병목"으로.** one-step이 이미 표준이 되자, 경쟁은 ① 1-step에서 fidelity+detail 동시 달성(FiDeSR), ② VAE 같은 숨은 병목 제거(GenDR-Pix), ③ FLOPs가 아닌 실측 런타임·fused 커널(NTIRE2026, ShiftLUT)로 이동.

2. **diffusion 독점의 균열 — VAR과 vision-only의 부상.** AlignVAR·Real-IISR(VAR)은 비반복 추론으로 10배+ 속도를, VOSR은 T2I prior 없이도 SOTA를 보이며 "diffusion+T2I prior가 유일한 길"이라는 통념에 도전.

3. **감독 신호의 진화 — GT를 넘어, 평가를 내재화.** IQPIR는 NR-IQA prior로 GT 상한을 돌파하고, RAR는 복원기에 품질평가 critic을 묶어 "언제 멈출지"를 자율 결정. 복원이 perception-action loop로 진화.

4. **conditioning의 정교화.** 텍스트 prior를 스케일·주파수로 disentangle(DTPSR), reference 신뢰도를 게이팅(Ada-RefSR), 얼굴을 열화 oracle로(Face2Scene), 코덱 메타데이터 활용(CDA-VSR), MLLM domain prior distill(DATPRL-IR) — "무엇을·얼마나 믿고 주입할까"가 핵심 설계축.

5. **전방위 도메인 확장 + 물리/구조 내재화.** 적외선(Real-IISR), 유체(ReMD, 물리 일관성), 3D Gaussian(SR3R), 광학수차(UniCAC), 텐서/점군(RepTRFD), 역문제 null-space(GSNR), 수중(UIE-VLM/DTIUIE). 단순 자연영상 SR을 넘어 도메인 지식·물리 제약을 모델에 직접 내장하는 흐름.

## 3.3 비판적 메모

- **무참조 지표 의존 심화.** 2026 다수 논문이 PSNR/SSIM을 희생하고 MUSIQ/MANIQA/CLIP-IQA로 우위를 주장(SRGAN이 14년 전 경고한 perception-distortion trade-off). IQPIR처럼 아예 "GT를 넘자"는 주장까지 등장 — 평가의 신뢰성·재현성 논쟁이 격화될 여지.
- **모델 규모 양극화.** 한쪽은 FLUX(12B)/SD3 기반 거대 prior(E-Bridge, LiveMoments, FluxSR), 다른쪽은 sub-1M 엣지 모델(UHDPromer, UCAN, ShiftLUT). 중간대(EDSR/RCAN식 범용 백본)의 신규 연구는 상대적으로 희소.
- **arXiv ID 주의.** 본 보고서의 2026 논문 arXiv 링크 일부는 리포지토리 등재 시점 기준이며, 최종 게재본과 번호/수치가 다를 수 있음. 인용 전 원문 표(Table) 재확인 권장.

---

# 부록 — 전체 논문 인덱스 & 링크

## A. 2026년 논문 (35편)

| # | 모델 | 제목(약칭) | 학회 | 계열 | 링크 |
|---|---|---|---|---|---|
| 1 | Ada-RefSR | Trust but Verify (Reference SR) | ICLR 2026 | Reference/1-step | [arXiv](https://arxiv.org/abs/2602.01864) |
| 2 | LiveMoments | Live Photo 복원 | ICLR 2026 | Reference diffusion | [arXiv](https://arxiv.org/abs/2604.12286) |
| 3 | E-Bridge | Energy-oriented Diffusion Bridge | arXiv 2026 | Diffusion/FLUX | [arXiv](https://arxiv.org/abs/2604.10983) |
| 4 | SAT | Selective Aggregation Transformer | CVPR 2026 | Transformer | [arXiv](https://arxiv.org/abs/2604.07994) |
| 5 | DRIFT | Restoration+ISP+Tone-mapping | CVPR 2026 | Real-world/모바일 | [arXiv](https://arxiv.org/abs/2604.03402) |
| 6 | VOSR | Vision-Only Generative SR | CVPR 2026 | Diffusion(from-scratch) | [arXiv](https://arxiv.org/abs/2604.03225) |
| 7 | NTIRE2026-ESR | Efficient SR Challenge Report | CVPR 2026 | Efficient | [arXiv](https://arxiv.org/abs/2604.03198) |
| 8 | IQPIR | Beyond Ground-Truth (IQA prior) | CVPR 2026 | Real-world | [arXiv](https://arxiv.org/abs/2603.29773) |
| 9 | RAR | Restore, Assess, Repeat | CVPR 2026 | All-in-one | [arXiv](https://arxiv.org/abs/2603.26385) |
| 10 | EgoXtreme | Egocentric 6D pose 데이터셋 | CVPR 2026 | (Tangential) | [arXiv](https://arxiv.org/abs/2603.25135) |
| 11 | DUO-VSR | Dual-Stream 1-step Video SR | CVPR 2026 | Video/1-step | [arXiv](https://arxiv.org/abs/2603.22271) |
| 12 | OmniFM | 의료영상 연합학습 | CVPR 2026 | (Tangential) | [arXiv](https://arxiv.org/abs/2603.21660) |
| 13 | Face2Scene | 얼굴=열화 oracle | CVPR 2026 | Reference/1-step | [arXiv](https://arxiv.org/abs/2603.16570) |
| 14 | UIE-VLM | VLM 수중영상 향상 | AAAI 2026 | Underwater | [arXiv](https://arxiv.org/abs/2603.12773) |
| 15 | UCAN | Unified Conv-Attention Network | CVPR 2026 | Efficient | [arXiv](https://arxiv.org/abs/2603.11680) |
| 16 | UniCAC | 광학수차 보정 벤치마크 | CVPR 2026 | Real-world/광학 | [arXiv](https://arxiv.org/abs/2603.12083) |
| 17 | CDA-VSR | Compressed-Domain Online VSR | CVPR 2026 | Video | [arXiv](https://arxiv.org/abs/2603.07694) |
| 18 | DTPSR | Disentangled Textual Priors | CVPR 2026 | Diffusion | [arXiv](https://arxiv.org/abs/2603.07430) |
| 19 | Real-IISR | 실세계 적외선 SR + 벤치 | CVPR 2026 | VAR/적외선 | [arXiv](https://arxiv.org/abs/2603.04745) |
| 20 | CubeComposer | 4K 360° AR 영상생성 | CVPR 2026 | Video/AR | [arXiv](https://arxiv.org/abs/2603.04291) |
| 21 | CWP-Net | Causal Wavelet Prompt | TIP 2026 | All-in-one | [arXiv](https://arxiv.org/abs/2603.03839) |
| 22 | FiDeSR | High-Fidelity 1-step Diffusion | CVPR 2026 | 1-step | [arXiv](https://arxiv.org/abs/2603.02692) |
| 23 | DATPRL-IR | Multi-Domain All-in-one | ICLR 2026 | All-in-one | [arXiv](https://arxiv.org/abs/2603.01725) |
| 24 | DTIUIE | Task-Inspired 수중 향상 | TIP 2026 | Underwater | [arXiv](https://arxiv.org/abs/2603.01767) |
| 25 | RepTRFD | Tensor Ring Functional Decomp | CVPR 2026 | 텐서/도메인 | [arXiv](https://arxiv.org/abs/2603.01034) |
| 26 | UHDPromer | Neural Discrimination UHD | IJCV 2026 | Efficient/UHD | [arXiv](https://arxiv.org/abs/2603.00853) |
| 27 | ShiftLUT | Spatial Shift LUT | CVPR 2026 | Efficient/LUT | [arXiv](https://arxiv.org/abs/2603.00906) |
| 28 | AlignVAR | Globally Consistent VAR SR | CVPR 2026 | VAR | [arXiv](https://arxiv.org/abs/2603.00589) |
| 29 | SR3R | Feed-Forward 3DGS SR | CVPR 2026 | 3D/Gaussian | [arXiv](https://arxiv.org/abs/2602.24020) |
| 30 | DACESR | Degradation-Aware Embedding | TIP 2026 | Real-world/Mamba | [arXiv](https://arxiv.org/abs/2602.23890) |
| 31 | ReMD | Physics-Consistent Fluid SR | CVPR 2026 | 유체/도메인 | [arXiv](https://arxiv.org/abs/2603.00149) |
| 32 | GSNR | Graph Smooth Null-Space | CVPR 2026 | 역문제 | [arXiv](https://arxiv.org/abs/2602.20328) |
| 33 | LoRun | LoRA-Unfolding Networks | TIP 2026 | Unfolding | [arXiv](https://arxiv.org/abs/2602.18697) |
| 34 | XA-SSL | 혈관조영 self-supervised | AAAI 2026 | (Tangential) | [arXiv](https://arxiv.org/abs/2602.11536) |
| 35 | GenDR-Pix | VAE 제거 1-step SR | ICLR 2026 | 1-step | [arXiv](https://arxiv.org/abs/2602.10630) |

## B. Landmark 논문 (27편)

| 시대 | 모델 | 제목(약칭) | 학회/연도 | 링크 |
|---|---|---|---|---|
| CNN기 | SRCNN | Deep CNN for SR | ECCV 2014 | [arXiv](https://arxiv.org/abs/1501.00092) |
| CNN기 | FSRCNN | Accelerating SRCNN | ECCV 2016 | [arXiv](https://arxiv.org/abs/1608.00367) |
| CNN기 | ESPCN | Sub-Pixel Conv | CVPR 2016 | [arXiv](https://arxiv.org/abs/1609.05158) |
| CNN기 | VDSR | Very Deep SR | CVPR 2016 | [arXiv](https://arxiv.org/abs/1511.04587) |
| GAN기 | SRGAN | Photo-Realistic GAN SR | CVPR 2017 | [arXiv](https://arxiv.org/abs/1609.04802) |
| 아키텍처 | EDSR | Enhanced Deep Residual | CVPRW 2017 | [arXiv](https://arxiv.org/abs/1707.02921) |
| 아키텍처 | RDN | Residual Dense Network | CVPR 2018 | [arXiv](https://arxiv.org/abs/1802.08797) |
| 아키텍처 | RCAN | Residual Channel Attention | ECCV 2018 | [arXiv](https://arxiv.org/abs/1807.02758) |
| GAN기 | ESRGAN | Enhanced SRGAN | ECCVW 2018 | [arXiv](https://arxiv.org/abs/1809.00219) |
| Flow | SRFlow | Normalizing Flow SR | ECCV 2020 | [arXiv](https://arxiv.org/abs/2006.14200) |
| Transformer | SwinIR | Swin Transformer 복원 | ICCVW 2021 | [arXiv](https://arxiv.org/abs/2108.10257) |
| 실세계 | BSRGAN | Practical Degradation | ICCV 2021 | [arXiv](https://arxiv.org/abs/2103.14006) |
| 실세계 | Real-ESRGAN | Pure Synthetic Blind SR | ICCVW 2021 | [arXiv](https://arxiv.org/abs/2107.10833) |
| Diffusion | SR3 | Iterative Refinement | TPAMI 2022 | [arXiv](https://arxiv.org/abs/2104.07636) |
| Transformer | HAT | Activating More Pixels | CVPR 2023 | [arXiv](https://arxiv.org/abs/2205.04437) |
| Diffusion | ResShift | Residual Shifting | NeurIPS 2023 | [arXiv](https://arxiv.org/abs/2307.12348) |
| Diffusion | StableSR | Diffusion Prior 활용 | IJCV 2024 | [arXiv](https://arxiv.org/abs/2305.07015) |
| Diffusion | DiffBIR | Blind Restoration prior | ECCV 2024 | [arXiv](https://arxiv.org/abs/2308.15070) |
| Diffusion | SeeSR | Semantics-Aware SR | CVPR 2024 | [arXiv](https://arxiv.org/abs/2311.16518) |
| 1-step | SinSR | Single-Step Diffusion | CVPR 2024 | [arXiv](https://arxiv.org/abs/2311.14760) |
| 1-step | FluxSR | Flow Trajectory Distill | ICML 2025 | [arXiv](https://arxiv.org/abs/2502.01993) |
| VAR | VARSR | Visual Autoregressive SR | ICML 2025 | [arXiv](https://arxiv.org/abs/2501.18993) |
| Diffusion | DiT4SR | Diffusion Transformer SR | ICCV 2025 | [arXiv](https://arxiv.org/abs/2504.20461) |
| Video | EDVR | Deformable Video Restoration | CVPRW 2019 | [arXiv](https://arxiv.org/abs/1905.02716) |
| Video | BasicVSR | Essential VSR Components | CVPR 2021 | [arXiv](https://arxiv.org/abs/2012.02181) |
| Video | BasicVSR++ | Enhanced Propagation | CVPR 2022 | [arXiv](https://arxiv.org/abs/2104.13371) |
| Video | VRT | Video Restoration Transformer | TIP 2024 | [arXiv](https://arxiv.org/abs/2201.12288) |

---

*본 보고서는 62편의 논문 원문을 병렬 리서치 에이전트로 직접 읽고 통합 작성했습니다. 정량 수치는 각 논문이 보고한 값이며, 일부 무참조 지표/미게재 수치는 원문 표 재확인을 권장합니다.*
