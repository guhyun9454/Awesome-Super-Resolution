# Video SR 논문 선별 — 720p→1080p PTZ/CCTV 업스케일링 목적

> 출처: [ChaofWang/Awesome-Super-Resolution](https://github.com/ChaofWang/Awesome-Super-Resolution) 의 연도별 Video SR 논문 전수
> 작성일: 2026-06-09 · 방법: 후보 124편 → (1차) 논문 1편당 sonnet 에이전트가 arXiv 원문 확인해 **video SR 여부 + 목적 부합도** 판정 → (2차) 생존 84편을 다시 1편당 에이전트로 **생성형(GAN/Diffusion) vs fidelity 복원형 + artifact 위험도** 분류.

## 목적 및 배경 (선정 기준)

**1.1 배경** — 데이터팀이 수집 가능한 영상 최대 해상도는 **720p**. PTZ 카메라로 교통사고 등 이벤트 검지 시 줌을 쓰면 원본이 낮아 화면이 뭉개져 객체·상황이 안 보이고, 이는 객체탐지·이벤트검지 성능 저하로 이어짐.

**1.2 목적** — FHD 이상 원본 수집이 어려운 상황에서 **Super-Resolution으로 720p→FHD(1080p) 업스케일**이 검지 성능 개선에 실효성이 있는지 검증.

**선정 필터**
1. **진짜 Video SR** (다중 프레임 시간정보로 공간 해상도 향상) 일 것 — image-only / frame-interpolation 전용 / deblurring 전용 / depth·event-stream 전용 제외.
2. **목적 부합** — 범용 자연(거리/교통) 영상에 적용 가능. 도메인 한정(의료·360°·애니메이션·얼굴전용·스테레오·egocentric·event-camera 하드웨어 의존) 제외.
3. **⚠️ 비생성 우선** — GAN·Diffusion 등 **사전학습 생성 prior로 디테일을 "새로 생성"하는 계열은 없는 디테일(번호판·객체)을 hallucination할 수 있어** 사고검지엔 위험 → **fidelity 복원형(CNN/Transformer/Mamba/recurrent, 적대·확산 없이 실제 정보 복원)** 을 추천 목록으로, 생성형은 별도 "참고(제외)" 섹션으로 분리.

> 표기: Real-world ○ = 실세계/blind 열화 학습(미지 CCTV 열화에 강인, 우선 검토) · △ = 합성(bicubic) 열화 기준(실데이터 fine-tune 필요).

## ★ 우선 검토(Top picks) — 비생성 + 실세계 열화 대응

실제 720p CCTV의 미지 열화(압축·블러)에 강인하면서 디테일을 지어내지 않는 fidelity 모델. PoC 1순위.

| Model | Year | 계열 | Venue | Link | 핵심 |
|---|---|---|---|---|---|
| **CDA-VSR** | 2026 | CNN | CVPR 2026 | [arXiv](https://arxiv.org/abs/2603.07694) | 순수 CNN 회귀 모델로 Charbonnier loss만 사용; 압축 도메인 정보(모션벡터·잔차맵·프레임타입)를 활용한 결정론적 복원이므로 hallucination 위험 없음. 교통 CCTV 720p→1080p 업스케일에 적합하며 추론 속도도 기존 SOTA 대비 2배 이상 빠름. |
| **DualX-VSR** | 2025 | Transformer | arXiv 2025 | [arXiv](https://arxiv.org/abs/2506.04830) | Dual Axial Spatial×Temporal Transformer 기반 회귀형 VSR 모델로, 주력 변형(DualX-VSR MSE)은 Charbonnier loss만 사용하는 비생성형 fidelity 복원 모델임. GAN 파인튜닝 변형(DualX-VSR GAN)은 존재하나 선택적이며, CCTV/사고검지 용도에는 MSE 변형 사용 시 hallucination 위험이 낮아 적합함. 모션 보상 없이 시공간 축 분리 어텐션으로 영상 복원. |
| **FCA2** | 2025 | CNN | arXiv 2025 | [arXiv](https://arxiv.org/abs/2506.11545) | CNN 기반 모듈형 압축 인식 오토인코더로, Charbonnier 손실만 사용하는 순수 fidelity 복원형(비생성형). GAN/Diffusion/적대학습 없음. 광학흐름 기반 인코더+잔차 레이어 구조로 기존 VSR 백본(BasicVSR 등)과 모듈식 결합 가능. 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **FMA-Net++** | 2025 | Hybrid | arXiv 2025 | [arXiv](https://arxiv.org/abs/2512.04390) | Hybrid(CNN+attention) 비생성형 회귀 모델 — GAN/diffusion 없이 L1+광학흐름+warping loss만 사용하여 실제 정보만 복원하므로 번호판/객체 hallucination 위험 없음; 교통 CCTV 720p→1080p 용도에 적합. |
| **LRTI-VSR** | 2025 | Hybrid | arXiv 2025 | [arXiv](https://arxiv.org/abs/2505.02159) | BasicVSR++ 기반 재귀 CNN + 리포커스드 인트라/인터프레임 Transformer 하이브리드 백본; 학습 손실은 Charbonnier(픽셀 회귀)만 사용하며 GAN·Diffusion 없음. 장범위 시간 정보를 full-video forward + short-clip backprop 전략으로 효율 학습. 번호판/객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **TURTLE** | 2024 | Hybrid | NeurIPS 2024 | [arXiv](https://arxiv.org/abs/2410.03936) | CNN + 패치 어텐션 기반 하이브리드 회귀 모델; 학습 손실이 L1 단독이며 GAN·Diffusion 없음 → hallucination 위험 낮아 CCTV 720p→1080p 업스케일에 적합. NeurIPS 2024 최신작으로 시계열 인과 히스토리(CHM)로 시간적 일관성 확보. |
| **IART** | 2023 | Hybrid | arXiv 2023 | [arXiv](https://arxiv.org/abs/2305.00163) | CNN/Transformer 하이브리드 백본(BasicVSR 또는 PSRT-recurrent)에 좌표 네트워크(PE-MLP)+윈도우 크로스어텐션 기반 암묵적 정렬 모듈을 결합한 순수 회귀형 VSR 모델; L2 손실만 사용하며 GAN·Diffusion 없음 → 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **EAVSR** | 2022 | Recurrent-CNN | arXiv 2022 | [arXiv](https://arxiv.org/abs/2212.05342) | BasicVSR/BasicVSR++ 기반 양방향 재귀 CNN 계열; 기본 EAVSR은 L1 손실만 사용하는 fidelity 복원형으로 GAN·Diffusion 없음 — EAVSRGAN+ 변종은 제외하고 EAVSR/EAVSR+만 사용하면 번호판 hallucination 위험 없이 교통 CCTV 720p→1080p 업스케일에 적합. |
| **COMISR** | 2021 | Recurrent-CNN | ICCV 2021 | _(검색: COMISR Compression-Informed Video Super-)_ | 순수 L2 회귀 손실만 사용하는 양방향 순환 CNN(광학 흐름 워핑 + Laplacian 고주파 강화) 구조로, GAN·Diffusion 없음. 압축 아티팩트 제거에 특화되어 CCTV 영상처럼 H.264/H.265 압축된 720p→1080p 업스케일에 적합하며 번호판·객체 hallucination 위험이 낮아 교통 사고검지용으로 적극 추천. |
| **DeepBlindVSR** | 2021 | Recurrent-CNN | ICCV 2021 | _(검색: Deep Blind Video Super-Resolution Pan IC)_ | CNN 기반 blind VSR 복원형 모델 — KernelPredict·VideoSR 두 학습 단계 모두 '1*L1' 손실만 사용(GAN·Diffusion 없음). PWC-Net 광학흐름 + FFT 기반 deconvolution + RCAN 스타일 재구성 네트워크로 구성되며, 없는 디테일을 생성하지 않아 교통 CCTV 번호판·객체 hallucination 위험이 낮으므로 720p→1080p 사고검지 업스케일에 적합. |
| **RealVSR** | 2021 | Recurrent-CNN | ICCV 2021 | _(검색: RealVSR Real-World Video Super-Resolutio)_ | EDVR 백본 기반 회귀형 복원 모델로, 휘도 채널을 라플라시안 피라미드로 분해한 뒤 각 레벨에 Charbonnier 손실만 적용(GAN·Diffusion 없음). 번호판·객체 hallucination 위험이 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **Deep Blind Video Super-resolution** | 2020 | CNN | arXiv 2020 | [arXiv](https://arxiv.org/abs/2003.04716) | 순수 CNN 회귀 모델(L1 손실만 사용); blur kernel 추정(N_k) + 광류 추정(PWC-Net) + RCAN 기반 SR 복원(N_I)으로 구성된 비생성형 파이프라인. GAN·Diffusion 전혀 없음. 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **VESR-Net** | 2020 | Recurrent-CNN | arXiv 2020 | [arXiv](https://arxiv.org/abs/2003.02115) | CNN 계열 회귀 복원 모델(PCD 정렬 + Separate Non-Local + CARB); L1 픽셀 손실만 사용하며 GAN·Diffusion 없음 → hallucination 위험 낮음; 교통 CCTV 720p→1080p 업스케일 용도에 적합. |

---
## ✅ 추천 — Fidelity 복원형 (비생성, 60편)

적대학습(GAN)·확산(diffusion) 없이 pixel/perceptual/optical-flow 손실로 실제 정보를 복원 → artifact(hallucination) 위험 낮음.


#### 2017

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **SPMC** | Detail-Revealing Deep Video Super-Resolution | ICCV 2017 | Recurrent-CNN | x4 (primary), x2/x3 (ablation) | △ | [arXiv](https://arxiv.org/abs/1704.02738) | L1/L2 회귀 손실 기반 순수 fidelity 복원형 CNN+ConvLSTM 모델(SPMC 레이어로 서브픽셀 모션 보상 후 멀티프레임 융합); GAN·Diffusion 없음, hallucination 위험 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **VESPCN** | Real-Time Video Super-Resolution with Spatio-Temporal Networks and Motion Compensation | CVPR 2017 | Recurrent-CNN | x4, x3, x2 | △ | [arXiv](https://arxiv.org/abs/1611.05250) | CNN 기반 회귀 모델(ESPCN 확장)로, MSE+Huber 손실만 사용하며 GAN·Diffusion 전혀 없음. 광류 기반 공간 변환기로 프레임 간 움직임 보상 후 sub-pixel conv로 업스케일 — 없는 디테일을 만들지 않아 CCTV 720p→1080p 감시 업스케일에 적합하며 할루시네이션 위험 낮음. |

#### 2018

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **DUF** | Deep Video Super-Resolution Network Using Dynamic Upsampling Filters | CVPR 2018 | CNN | x2, x3, x4 | △ | _(검색: Jo Deep Video Super-Resolution CVPR 2018)_ | 3D CNN + DenseNet 백본으로 동적 업샘플링 필터와 잔차 맵을 생성하는 fidelity 복원형 회귀 모델. 학습 손실은 Huber loss만 사용하며 GAN/Diffusion 없음. 없는 디테일을 hallucination할 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 안전하게 사용 가능. |
| **FRVSR** | Frame-Recurrent Video Super-Resolution | CVPR 2018 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/1801.04590) | 재귀적 CNN 계열(FNet+SRNet), 순수 L2 픽셀/광학류 손실만 사용하며 GAN·Diffusion 없음 — hallucination 위험 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **TDAN** | TDAN: Temporally-Deformable Alignment Network for Video Super-Resolution | arXiv 2018 | CNN | x4 | △ | [arXiv](https://arxiv.org/abs/1812.02898) | CNN + deformable conv 기반 순수 회귀 모델(L1 손실만 사용, GAN/Diffusion 없음); 없는 디테일을 생성하지 않아 CCTV 번호판·객체 hallucination 위험 낮음. 720p→1080p 감시카메라 업스케일 용도에 적합. |

#### 2019

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **DNLN** | Deformable Non-local Network for Video Super-Resolution | arXiv 2019 | CNN | x4 | △ | [arXiv](https://arxiv.org/abs/1909.10692) | 변형가능한 컨볼루션(deformable conv) 기반 정렬 + 비국소(non-local) 모듈 + RRDB 백본의 순수 CNN 회귀 모델로, L1/L2 픽셀 손실만 사용하며 GAN·Diffusion 없음. 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **EDVR** | EDVR: Video Restoration with Enhanced Deformable Convolutional Networks | CVPRW 2019 | Recurrent-CNN | x4 (primarily; also x2 in benchmark evaluations) | △ | [arXiv](https://arxiv.org/abs/1905.02716) | CNN 계열(deformable conv + PCD/TSA 모듈) fidelity 복원형 — Charbonnier loss만 사용, GAN·diffusion 없음 — hallucination 위험 낮아 교통 CCTV 번호판 업스케일에 적합. |
| **FSTRN** | Fast Spatio-Temporal Residual Network for Video Super-Resolution | CVPR 2019 | Recurrent-CNN | x4 (주요 벤치마크 기준, abstract에 명시 없으나 당시 표준 x4 설정으로 평가됨) | △ | [arXiv](https://arxiv.org/abs/1904.02870) | 3D 분해합성곱 기반 순수 회귀형 VSR 모델(비생성형); GAN·Diffusion 없이 픽셀 복원 손실만 사용하므로 번호판/객체 hallucination 위험 없음 — CCTV 720p→1080p 업스케일에 적합. |
| **PFNL** | Progressive Fusion Video Super-Resolution Network via Exploiting Non-Local Spatio-Temporal Correlations | ICCV 2019 | CNN | x4 | △ | _(검색: PFNL ICCV 2019)_ | CNN 기반 프로그레시브 퓨전 + 비국소(non-local) 시공간 상관관계 모듈로 구성된 회귀형 VSR; Charbonnier 손실만 사용하며 GAN·diffusion 없음 → 번호판/객체 hallucination 위험 없어 CCTV 720p→1080p 업스케일에 적합. (같은 저자의 후속작 MSHPFNL은 GAN 포함이므로 혼동 주의) |
| **RBPN** | Recurrent Back-Projection Network for Video Super-Resolution | CVPR 2019 | Recurrent-CNN | x4 (주), x2 | △ | [arXiv](https://arxiv.org/abs/1903.10128) | 순수 L1 회귀 손실만 사용하는 Recurrent CNN 계열(DBPN 기반 반복 역투영 + 다프레임 인코더-디코더). GAN·Diffusion 없음 → 번호판·객체 hallucination 위험 없음. 교통 CCTV 720p→1080p 업스케일 목적에 적합하며 fidelity 복원형으로 권장. |

#### 2020

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **Deep Blind Video Super-resolution** | Blind Video Super-Resolution with motion blur estimation (blindvsr) | arXiv 2020 | CNN | x4 | ○ | [arXiv](https://arxiv.org/abs/2003.04716) | 순수 CNN 회귀 모델(L1 손실만 사용); blur kernel 추정(N_k) + 광류 추정(PWC-Net) + RCAN 기반 SR 복원(N_I)으로 구성된 비생성형 파이프라인. GAN·Diffusion 전혀 없음. 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **MuCAN** | MuCAN: Multi-Correspondence Aggregation Network for Video Super-Resolution | ECCV 2020 | CNN | x4 (primary), x2 likely evaluated | △ | [arXiv](https://arxiv.org/abs/2007.11803) | CNN 기반 fidelity 복원형(비생성형): Charbonnier + 엣지인식 손실만 사용, GAN/diffusion 없음. 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **RRN** | Revisiting Temporal Modeling for Video Super-Resolution (RRN) | BMVC 2020 | Recurrent-CNN | x4 (primary, standard VSR benchmark); x2 likely also evaluated | △ | [arXiv](https://arxiv.org/abs/2008.05765) | 순수 Recurrent CNN 계열(잔차 블록 + RNN 은닉 상태 전파) + L1 손실만 사용하는 fidelity 복원형 모델; GAN·Diffusion 없음 — 번호판/객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합하며 추천. |
| **RSDN** | Video Super-Resolution with Recurrent Structure-Detail Network (RSDN) | ECCV 2020 | Recurrent-CNN | x4 (primary), likely x2/x3 on standard benchmarks | △ | [arXiv](https://arxiv.org/abs/2008.00455) | 재귀 CNN 기반 fidelity 복원형 모델로 Charbonnier loss만 사용(GAN·diffusion 없음); 입력에 없는 내용을 hallucination할 위험이 낮아 교통 CCTV 번호판·객체 검지용 720p→1080p 업스케일에 적합하다. |
| **UnderstandDefAlign** | Understanding Deformable Alignment in Video Super-Resolution | arXiv 2020 | CNN | x4 (standard VSR benchmark protocol) | △ | [arXiv](https://arxiv.org/abs/2009.07265) | EDVR 기반 deformable conv alignment 분석 논문으로, Charbonnier + offset-fidelity loss(광학흐름 정규화)만 사용하는 순수 회귀 복원형; GAN·Diffusion 없음 → 번호판/객체 hallucination 위험 없어 CCTV 720p→1080p 업스케일에 적합. |
| **VESR-Net** | VESR-Net: Youku Video Enhancement and Super-Resolution Challenge | arXiv 2020 | Recurrent-CNN | x4 | ○ | [arXiv](https://arxiv.org/abs/2003.02115) | CNN 계열 회귀 복원 모델(PCD 정렬 + Separate Non-Local + CARB); L1 픽셀 손실만 사용하며 GAN·Diffusion 없음 → hallucination 위험 낮음; 교통 CCTV 720p→1080p 업스케일 용도에 적합. |
| **VSR-TGA** | Video Super-Resolution with Temporal Group Attention | CVPR 2020 | Recurrent-CNN | x4 | △ | _(검색: Video Super-Resolution With Temporal Gro)_ | CNN 기반 fidelity 복원형 모델로 Charbonnier(L1 변형) 손실만 사용; GAN·Diffusion 없음. 시간 프레임을 그룹으로 나눠 계층적 Attention으로 정렬·융합하는 구조(deformable-style 정렬 포함). 생성적 hallucination 위험이 없어 교통 CCTV 720p→1080p 업스케일에 적합. |

#### 2021

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **BasicVSR / IconVSR** | BasicVSR: The Search for Essential Components in Video Super-Resolution and Beyond | CVPR 2021 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/2012.02181) | 양방향 순환 CNN + SpyNet 광학 흐름 정렬 기반 fidelity 복원형 모델. Charbonnier 픽셀 손실만 사용하며 GAN/Diffusion 없음. 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합 추천. |
| **BasicVSR++** | BasicVSR++: Improving Video Super-Resolution with Enhanced Propagation and Alignment | arXiv 2021 | Recurrent-CNN | x4 (primary), x2 | △ | [arXiv](https://arxiv.org/abs/2104.13371) | 양방향 재귀 CNN + flow 기반 deformable alignment 구조로, Charbonnier loss만으로 학습하는 fidelity 복원형 모델. GAN·Diffusion 없음 → 번호판/객체 hallucination 위험 없으므로 교통 CCTV 720p→1080p 업스케일 용도에 적합. |
| **COMISR** | COMISR: Compression-Informed Video Super-Resolution | ICCV 2021 | Recurrent-CNN | x4 | ○ | _(검색: COMISR Compression-Informed Video Super-)_ | 순수 L2 회귀 손실만 사용하는 양방향 순환 CNN(광학 흐름 워핑 + Laplacian 고주파 강화) 구조로, GAN·Diffusion 없음. 압축 아티팩트 제거에 특화되어 CCTV 영상처럼 H.264/H.265 압축된 720p→1080p 업스케일에 적합하며 번호판·객체 hallucination 위험이 낮아 교통 사고검지용으로 적극 추천. |
| **DeepBlindVSR** | Deep Blind Video Super-Resolution | ICCV 2021 | Recurrent-CNN | x4 | ○ | _(검색: Deep Blind Video Super-Resolution Pan IC)_ | CNN 기반 blind VSR 복원형 모델 — KernelPredict·VideoSR 두 학습 단계 모두 '1*L1' 손실만 사용(GAN·Diffusion 없음). PWC-Net 광학흐름 + FFT 기반 deconvolution + RCAN 스타일 재구성 네트워크로 구성되며, 없는 디테일을 생성하지 않아 교통 CCTV 번호판·객체 hallucination 위험이 낮으므로 720p→1080p 사고검지 업스케일에 적합. |
| **FDAN** | FDAN: Flow-guided Deformable Alignment Network for Video Super-Resolution | arXiv 2021 | Recurrent-CNN | x4 (standard VSR benchmark scale; x2 가능성 있으나 명시 미확인) | △ | [arXiv](https://arxiv.org/abs/2105.05640) | CNN 기반 fidelity 복원형 VSR 모델. 광학 흐름(optical flow)으로 coarse offset을 추정하고 deformable conv로 fine offset을 정제하는 정렬 방식이며, 학습 손실은 pixel-wise L1만 사용 — GAN·diffusion·생성 prior 없음. hallucination 위험 없어 교통 CCTV 번호판·객체 오검지 우려 없이 720p→1080p 업스케일에 권장 가능. |
| **OVSR** | Omniscient Video Super-Resolution | ICCV 2021 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/2103.15683) | 재귀형 CNN 계열(PFRB 백본)로 과거·현재·미래 프레임을 통합하는 Precursor+Successor 이중 서브넷 구조; Charbonnier 회귀 손실만 사용해 GAN·Diffusion 없이 fidelity 복원; 번호판 hallucination 위험 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **PP-MSVSR / PP-MSVSR-L** | PP-MSVSR: Multi-Stage Video Super-Resolution | arXiv 2021 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/2112.02828) | 순수 CNN 회귀 모델(SPyNet 광학 흐름 + deformable conv 정렬 + 다단계 전파); 학습 손실은 Charbonnier Loss만 사용(GAN·diffusion·perceptual 없음) — PaddleGAN 공식 코드(msvsr_model.py, msvsr_reds.yaml)로 직접 확인. 번호판·객체 hallucination 위험 없으며 교통 CCTV 720p→1080p 업스케일에 적극 권장. |
| **RealVSR** | Real-World Video Super-Resolution: A Benchmark Dataset and Decomposition-Based Learning | ICCV 2021 | Recurrent-CNN | x2 (iPhone 11 Pro Max 듀얼 카메라 기반 실세계 쌍 데이터셋으로 구성, 근사 x2 배율) | ○ | _(검색: RealVSR Real-World Video Super-Resolutio)_ | EDVR 백본 기반 회귀형 복원 모델로, 휘도 채널을 라플라시안 피라미드로 분해한 뒤 각 레벨에 Charbonnier 손실만 적용(GAN·Diffusion 없음). 번호판·객체 hallucination 위험이 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **RevisitTempAlign** | Revisiting Temporal Alignment for Video Restoration | arXiv 2021 | Recurrent-CNN | x4 (standard REDS/Vimeo benchmarks; x4 is the dominant evaluated factor in 2021-era video SR) | △ | [arXiv](https://arxiv.org/abs/2111.15288) | CNN 기반 반복 정렬(IAM) + 적응형 재가중치(ARW) 구조로, BasicVSR/IconVSR 프로토콜(Charbonnier loss)을 따르는 순수 회귀형 복원 모델. GAN·Diffusion 없음 → 번호판 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **U3D-RDN + MSCU** | Large Motion Video Super-Resolution with Dual Subnet and Multi-Stage Communicated Upsampling | AAAI 2021 | Recurrent-CNN | x4 (AAAI 2021 표준; 논문에서 x2/x4 가능성 있으나 실제 명시값 미확인) | △ | [arXiv](https://arxiv.org/abs/2103.11744) | CNN 계열(U자형 3D-RDN + 변형 가능 합성곱 + MSCU) 회귀 복원 모델로, Charbonnier·perceptual·dual 손실만 사용하며 GAN·Diffusion 없음. hallucination 위험 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **USTVSRNet** | Learning for Unconstrained Space-Time Video Super-Resolution | arXiv 2021 | CNN | arbitrary-scale (unconstrained spatial upsampling via generalized pixel-shuffle) | △ | [arXiv](https://arxiv.org/abs/2102.13011) | CNN 기반 회귀 모델(RDN 백본 + optical flow)로, Charbonnier+Perceptual 손실만 사용하며 GAN·Diffusion 없음 — 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **VSR-Transformer** | Video Super-Resolution Transformer | arXiv 2021 | Transformer | x4 | △ | [arXiv](https://arxiv.org/abs/2106.06847) | Transformer 기반 fidelity 복원형 모델(Charbonnier loss만 사용, GAN·diffusion 없음); 광학 흐름 기반 프레임 정렬로 실제 디테일만 복원하므로 번호판·객체 hallucination 위험 없이 교통 CCTV 720p→1080p 업스케일에 권장 가능. |

#### 2022

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **CycMu-Net** | Spatial-Temporal Video Super-Resolution via Cycle-Projected Mutual Learning | arXiv 2022 | Recurrent-CNN | x4 spatial (typical for ST-VSR benchmarks); x2 temporal (frame interpolation) | △ | [arXiv](https://arxiv.org/abs/2205.05264) | CNN 계열(변형 합성곱 + 반복 업/다운 프로젝션) 순수 Charbonnier 회귀 손실 학습; GAN·Diffusion 없음 → 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **DAP** | Fast Online Video Super-Resolution with Deformable Attention Pyramid | arXiv 2022 | Recurrent-CNN | x4, x2 | △ | [arXiv](https://arxiv.org/abs/2202.01731) | 재귀적 CNN + Deformable Attention Pyramid 구조로 Smooth L1 손실만 사용하는 순수 회귀 복원 모델. GAN·Diffusion 없음. 번호판/객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합하며, EDVR-M 대비 3배 이상 빠른 온라인(인과적) 추론이 가능해 실시간 사고검지 용도에 적합. |
| **EAVSR** | Benchmark Dataset and Effective Inter-Frame Alignment for Real-World Video Super-Resolution (EAVSR) | arXiv 2022 | Recurrent-CNN | x4 | ○ | [arXiv](https://arxiv.org/abs/2212.05342) | BasicVSR/BasicVSR++ 기반 양방향 재귀 CNN 계열; 기본 EAVSR은 L1 손실만 사용하는 fidelity 복원형으로 GAN·Diffusion 없음 — EAVSRGAN+ 변종은 제외하고 EAVSR/EAVSR+만 사용하면 번호판 hallucination 위험 없이 교통 CCTV 720p→1080p 업스케일에 적합. |
| **ETDM** | Look Back and Forth: Video Super-Resolution with Explicit Temporal Difference Modeling | CVPR 2022 | Recurrent-CNN | x4 (REDS, Vimeo-90K 기준 표준; x4 위주로 평가, x2도 가능성 있음) | △ | [arXiv](https://arxiv.org/abs/2204.07114) | Recurrent-CNN 계열 순수 회귀 복원 모델 — loss.py/train.py 소스코드에서 CharbonnierLoss만 확인됨(GAN·Diffusion 없음); SpyNet 기반 옵티컬 플로우 + 명시적 시간차 모델링으로 fidelity 복원에 집중하므로, 번호판·객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일 용도에 적합. |
| **FTVSR** | Learning Spatiotemporal Frequency-Transformer for Compressed Video Super-Resolution | ECCV 2022 | Transformer | x4 | △ | [arXiv](https://arxiv.org/abs/2208.03012) | Transformer 계열 비생성형 복원 모델로, DCT 주파수 도메인 self-attention + Charbonnier 손실만 사용(GAN·Diffusion 없음); hallucination 위험 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **MANA** | Memory-Augmented Non-Local Attention for Video Super-Resolution (MANA) | CVPR 2022 | Transformer | x4 (standard VSR benchmarks; x2 likely also supported) | △ | _(검색: MANA Memory-Augmented Non-Local Attentio)_ | CNN+비로컬 어텐션 기반 회귀 복원 모델(Charbonnier loss)로, GAN·Diffusion 없이 실제 디테일만 복원하므로 CCTV 번호판/객체 hallucination 위험이 없어 교통 감시용 720p→1080p 업스케일에 적합. 웹 접근 불가로 도메인 지식 기반 판정. |
| **MANA** | Memory-Augmented Non-Local Attention for Video Super-Resolution (MANA) | CVPR 2022 | CNN | x4 (standard VSR benchmarks; x2 likely also supported) | △ | _(검색: MANA Memory-Augmented Non-Local Attentio)_ | CNN 기반(ResidualBlock + 비지역 공간 어텐션 + Pixel Shuffle) 회귀 복원 모델로, 학습 손실은 L1만 사용(GAN·Diffusion 없음). 입력에 없는 디테일을 합성하지 않아 번호판/객체 hallucination 위험이 낮으므로, 교통 CCTV 720p→1080p 업스케일 용도에 적합. |
| **MRVSR** | Stable Long-Term Recurrent Video Super-Resolution | CVPR 2022 | Recurrent-CNN | x4 | △ | _(검색: Stable Long-Term Recurrent Video Super-R)_ | 순수 MSE 회귀 기반 재귀 CNN(Lipschitz 제약 SRNL 적용); GAN·Diffusion·Perceptual loss 없음 — hallucination 위험 없고 장기 저모션 CCTV 시퀀스에서도 발산하지 않아 교통 감시 720p→1080p 업스케일에 적합. |
| **RethinkAlign** | Rethinking Alignment in Video Super-Resolution Transformers | arXiv 2022 | Transformer | x4 | △ | [arXiv](https://arxiv.org/abs/2207.08494) | Swin 스타일 Transformer 백본 + Charbonnier 손실만 사용하는 순수 회귀형 복원 모델로, GAN/Diffusion 없음. 패치 수준 정렬(patch alignment)로 다중 프레임을 처리하며 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **RSCL** | Residual Sparsity Connection Learning for Efficient Video Super-Resolution | arXiv 2022 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/2206.07687) | BasicVSR 기반 재귀 CNN에 구조적 희소성 가지치기(structured pruning)를 적용한 경량 fidelity 복원형 모델; 학습 손실은 Charbonnier + L2 정규화만 사용하며 GAN·Diffusion 없음 → 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **TTVSR** | Learning Trajectory-Aware Transformer for Video Super-Resolution | CVPR 2022 oral | Transformer | x4, x2 | △ | [arXiv](https://arxiv.org/abs/2204.04216) | Transformer 기반 fidelity 복원형 모델로, 학습 손실은 Charbonnier loss만 사용(GAN·Diffusion 없음). 궤적 기반 self-attention으로 시공간 정렬을 수행하며 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합하다. GitHub 설정 파일(TTVSR_reds4.py)로 손실 함수 직접 확인. |
| **VideoINR** | VideoINR: Learning Video Implicit Neural Representation for Continuous Space-Time Super-Resolution | CVPR 2022 | Flow/INR | arbitrary-scale (x2~x4 spatial, x8 temporal; bicubic degradation 기준) | △ | _(검색: VideoINR Video Implicit Neural Represent)_ | flow-or-inr 계열 순수 회귀 모델 — Charbonnier 재구성 손실만 사용하며 GAN·Diffusion 없음. 공간+시간 암시적 신경 표현(INR)으로 임의 해상도/프레임레이트 업스케일 가능. hallucination 위험 낮아 교통 CCTV 720p→1080p 용도에 적합. |
| **VRT** | VRT: A Video Restoration Transformer | arXiv 2022 | Transformer | x4 (primarily), x4 BD (blur-downsampling) | △ | [arXiv](https://arxiv.org/abs/2201.12288) | Transformer 기반 fidelity 복원형(Charbonnier loss만 사용, GAN·Diffusion 없음); 번호판/객체 hallucination 위험이 없어 교통 CCTV 720p→1080p 업스케일에 안전하게 권장 가능. |

#### 2023

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **IART** | An Implicit Alignment for Video Super-Resolution (IART) | arXiv 2023 | Hybrid | x4 | ○ | [arXiv](https://arxiv.org/abs/2305.00163) | CNN/Transformer 하이브리드 백본(BasicVSR 또는 PSRT-recurrent)에 좌표 네트워크(PE-MLP)+윈도우 크로스어텐션 기반 암묵적 정렬 모듈을 결합한 순수 회귀형 VSR 모델; L2 손실만 사용하며 GAN·Diffusion 없음 → 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |

#### 2024

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **CFDVSR** | Collaborative Feedback Discriminative Propagation for Video Super-Resolution | arXiv 2024 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/2404.04745) | 재귀형 CNN 백본(BasicVSR/BasicVSR++/PSRT 위에 DAC+CFP 모듈 추가)으로 Charbonnier+FFT 손실만 사용하는 fidelity 복원형 모델 — GAN·Diffusion 없음, 번호판·객체 hallucination 위험 낮아 교통 CCTV 720p→1080p 업스케일에 적합. |
| **CTUN** | Cascaded Temporal Updating Network for Efficient Video Super-Resolution | arXiv 2024 | Recurrent-CNN | x4 | △ | [arXiv](https://arxiv.org/abs/2408.14244) | 순환 CNN 계열 fidelity 복원형 모델로, Charbonnier+FFT loss만 사용하며 GAN/Diffusion 없음; 번호판·객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합. BasicVSR 대비 파라미터 30% 수준의 경량 구조라 실시간 검지 환경에도 유리. |
| **MIA-VSR** | Video Super-Resolution Transformer with Masked Inter&Intra-Frame Attention | arXiv 2024 | Transformer | x4 | △ | [arXiv](https://arxiv.org/abs/2401.06312) | Swin Transformer 기반 회귀 복원형 VSR; Charbonnier+희소마스크 손실만 사용, GAN·Diffusion 없음 — 번호판/객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **ST-AVSR** | Arbitrary-Scale Video Super-Resolution with Structural and Textural Priors | ECCV 2024 | Recurrent-CNN | x2, x3, x4, x6, x8 (arbitrary-scale, 비정수/비대칭 스케일 포함) | △ | _(검색: ST-AVSR Arbitrary-Scale Video Super-Reso)_ | CNN 기반 재귀형(recurrent) 복원 모델로 학습 손실은 Charbonnier만 사용(GAN·Diffusion 없음); VGG-19는 동결된 비생성 특징 추출기로만 활용되며 적대학습 없음. 없는 디테일을 합성하지 않아 번호판 hallucination 위험이 낮으므로 CCTV 720p→1080p 업스케일에 적합 추천. |
| **TURTLE** | Learning Truncated Causal History Model for Video Restoration (Turtle) | NeurIPS 2024 | Hybrid | x4 | ○ | [arXiv](https://arxiv.org/abs/2410.03936) | CNN + 패치 어텐션 기반 하이브리드 회귀 모델; 학습 손실이 L1 단독이며 GAN·Diffusion 없음 → hallucination 위험 낮아 CCTV 720p→1080p 업스케일에 적합. NeurIPS 2024 최신작으로 시계열 인과 히스토리(CHM)로 시간적 일관성 확보. |

#### 2025

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **BasicAVSR** | BasicAVSR: Arbitrary-Scale Video Super-Resolution via Image Priors and Enhanced Motion Compensation | arXiv 2025 | Recurrent-CNN | arbitrary-scale (x1–x4) | △ | [arXiv](https://arxiv.org/abs/2510.26149) | Charbonnier(smooth L1) 단일 손실로 학습된 recurrent-CNN 기반 임의배율 VSR 모델; GAN·Diffusion 없음, hallucination 위험 낮음 — CCTV 교통 감시 720p→1080p 업스케일에 적합 |
| **BF-STVSR** | BF-STVSR: B-Splines and Fourier for High Fidelity Spatial-Temporal Video Super-Resolution | arXiv 2025 | Flow/INR | arbitrary-scale (spatial x2–x4, temporal arbitrary) | △ | [arXiv](https://arxiv.org/abs/2501.11043) | INR(암묵신경표현) 기반 연속 시공간 VSR 모델로, B-스플라인(시간 보간)과 푸리에(공간 주파수) 매퍼를 결합; 학습 손실은 Charbonnier만 사용(GAN·Diffusion 없음) → hallucination 위험 낮아 교통CCTV 720p→1080p 업스케일에 적합. |
| **BVSR-IK** | Blind Video Super-Resolution based on Implicit Kernels (BVSR-IK) | ICCV 2025 | Recurrent-CNN | x4 | △ | _(검색: Blind Video Super-Resolution Implicit Ke)_ | Recurrent Transformer + INR 기반 fidelity 복원형 모델; GAN·Diffusion 없이 Charbonnier 손실만 사용하여 없는 디테일을 지어내지 않으므로 교통 CCTV 번호판 오검지 위험 낮고 720p→1080p 업스케일에 적합. |
| **DualX-VSR** | DualX-VSR: Dual Axial Spatial x Temporal Transformer for Real-World Video Super-Resolution without Motion Compensation | arXiv 2025 | Transformer | x4 | ○ | [arXiv](https://arxiv.org/abs/2506.04830) | Dual Axial Spatial×Temporal Transformer 기반 회귀형 VSR 모델로, 주력 변형(DualX-VSR MSE)은 Charbonnier loss만 사용하는 비생성형 fidelity 복원 모델임. GAN 파인튜닝 변형(DualX-VSR GAN)은 존재하나 선택적이며, CCTV/사고검지 용도에는 MSE 변형 사용 시 hallucination 위험이 낮아 적합함. 모션 보상 없이 시공간 축 분리 어텐션으로 영상 복원. |
| **FCA2** | FCA2: Frame Compression-Aware Autoencoder for Compressed Video Super-Resolution | arXiv 2025 | CNN | x4 | ○ | [arXiv](https://arxiv.org/abs/2506.11545) | CNN 기반 모듈형 압축 인식 오토인코더로, Charbonnier 손실만 사용하는 순수 fidelity 복원형(비생성형). GAN/Diffusion/적대학습 없음. 광학흐름 기반 인코더+잔차 레이어 구조로 기존 VSR 백본(BasicVSR 등)과 모듈식 결합 가능. 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **FMA-Net++** | FMA-Net++: Motion- and Exposure-Aware Real-World Joint Video Super-Resolution and Deblurring | arXiv 2025 | Hybrid | x4 | ○ | [arXiv](https://arxiv.org/abs/2512.04390) | Hybrid(CNN+attention) 비생성형 회귀 모델 — GAN/diffusion 없이 L1+광학흐름+warping loss만 사용하여 실제 정보만 복원하므로 번호판/객체 hallucination 위험 없음; 교통 CCTV 720p→1080p 용도에 적합. |
| **LDIP** | LDIP: Long Distance Information Propagation for Video Super-Resolution | ICCV 2025 | Recurrent-CNN | x4, arbitrary-scale (x3.25, x5.5 등) | △ | _(검색: LDIP Long Distance Information Propagati)_ | 비생성형 recurrent-CNN 계열(BasicVSR++/IART 백본 기반) 복원 모델; Charbonnier+optical-flow 손실만 사용하며 GAN·Diffusion 없음 — 번호판·객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합하며, 장거리 시간적 전파(LRRF 모듈)로 기존 BasicVSR++ 대비 LPIPS 품질이 크게 개선됨. |
| **LRTI-VSR** | Small Clips, Big Gains: Learning Long-Range Refocused Temporal Information for Video Super-Resolution | arXiv 2025 | Hybrid | x4 | ○ | [arXiv](https://arxiv.org/abs/2505.02159) | BasicVSR++ 기반 재귀 CNN + 리포커스드 인트라/인터프레임 Transformer 하이브리드 백본; 학습 손실은 Charbonnier(픽셀 회귀)만 사용하며 GAN·Diffusion 없음. 장범위 시간 정보를 full-video forward + short-clip backprop 전략으로 효율 학습. 번호판/객체 hallucination 위험 없어 교통 CCTV 720p→1080p 업스케일에 적합. |
| **MambaVSR** | MambaVSR: Content-Aware Scanning State Space Model for Video Super-Resolution | arXiv 2025 | Mamba/SSM | x4 | △ | [arXiv](https://arxiv.org/abs/2506.11768) | Mamba SSM 기반 회귀형 VSR 모델(BasicVSR++ 위에 구축); Charbonnier 픽셀 손실만 사용하여 GAN·Diffusion 없음 → 번호판/객체 hallucination 위험 낮음; CCTV 720p→1080p 업스케일 fidelity 복원 용도에 적합. |
| **TS-Mamba** | Trajectory-aware Shifted State Space Models for Online Video Super-Resolution | arXiv 2025 | Mamba/SSM | x4 | △ | [arXiv](https://arxiv.org/abs/2508.10453) | Mamba-SSM 기반 비생성 회귀 모델; Charbonnier + trajectory loss만 사용(GAN/diffusion 없음)으로 hallucination 위험 없음 — 교통 CCTV 720p→1080p 업스케일에 적합하며 22.7% MACs 절감으로 온라인(실시간) 처리에도 유리. |
| **VSRM** | VSRM: A Robust Mamba-Based Framework for Video Super-Resolution | ICCV 2025 | Mamba/SSM | x4 | △ | [arXiv](https://arxiv.org/abs/2506.22762) | Mamba SSM 기반 회귀형 VSR 모델로, Charbonnier + 주파수 도메인 Charbonnier 손실만 사용하며 GAN·Diffusion 없음. 번호판/객체 hallucination 위험이 낮아 교통 CCTV 720p→1080p 업스케일에 적합하게 권장됨. |

#### 2026

| Model | Title | Venue | 계열 | Scale | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **CDA-VSR** | Compressed-Domain-Aware Online Video Super-Resolution | CVPR 2026 | CNN | x4 (REDS4 기준, 정확한 지원 배율은 논문 본문 확인 필요) | ○ | [arXiv](https://arxiv.org/abs/2603.07694) | 순수 CNN 회귀 모델로 Charbonnier loss만 사용; 압축 도메인 정보(모션벡터·잔차맵·프레임타입)를 활용한 결정론적 복원이므로 hallucination 위험 없음. 교통 CCTV 720p→1080p 업스케일에 적합하며 추론 속도도 기존 SOTA 대비 2배 이상 빠름. |


---
## ⚠️ 참고 — 생성형(GAN/Diffusion/AR), 일단 제외 (24편)

사전학습 생성 prior로 디테일을 합성 → 지각품질은 높지만 **없는 디테일을 만들어낼 위험**. 사고검지 목적엔 비권장. (단, 지각적 품질이 압도적이라 '보기 좋은 영상' 용도로는 후보. 검지 전처리로 쓸 거면 hallucination 검증 필수.)


#### 2018

| Model | Title | Venue | 계열 | Artifact위험 | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **TecoGAN** | Temporally Coherent GANs for Video Super-Resolution (TecoGAN) | arXiv 2018 | GAN | high | △ | [arXiv](https://arxiv.org/abs/1811.09393) | GAN 기반 생성형 VSR 모델로, 시간적 적대학습(Ping-Pong loss 포함)을 핵심으로 사용하여 입력에 없는 세부 정보를 합성(hallucination 가능)하므로 교통 CCTV 번호판·객체 오검지 위험이 높아 감시/포렌식 목적에는 부적합. |

#### 2021

| Model | Title | Venue | 계열 | Artifact위험 | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **RealBasicVSR** | Investigating Tradeoffs in Real-World Video Super-Resolution (RealBasicVSR) | arXiv 2021 / CVPR 2022 | GAN | high | ○ | [arXiv](https://arxiv.org/abs/2111.12704) | GAN 적대학습 포함(2단계: stage2에서 adversarial+perceptual loss 적용) → 입력에 없는 디테일을 합성할 수 있어 번호판·객체 hallucination 위험 있음. 교통 CCTV 사고검지 용도에는 부적합하며, 비생성형 fidelity 복원 모델(BasicVSR, EDVR 등) 권장. |

#### 2023

| Model | Title | Venue | 계열 | Artifact위험 | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **SRWD-VSR** | Expanding Synthetic Real-World Degradations for Blind Video Super Resolution | arXiv 2023 | GAN | high | ○ | [arXiv](https://arxiv.org/abs/2305.02660) | GAN 계열(FRVSR + RRDBNet 백본, U-Net 판별기 + 적대적 손실): content/perceptual/adversarial loss 조합으로 학습하여 없는 디테일을 합성할 수 있으므로, 번호판·객체 hallucination 위험이 높아 교통 CCTV 사고검지용 fidelity-critical 업스케일에는 부적합. |

#### 2024

| Model | Title | Venue | 계열 | Artifact위험 | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **MGLD-VSR** | Motion-Guided Latent Diffusion for Temporally Consistent Real-world Video Super-resolution | ECCV 2024 | Diffusion | high | ○ | _(검색: MGLD-VSR Motion-Guided Latent Diffusion )_ | Stable Diffusion V2.1을 생성 prior로 사용하는 latent diffusion 모델이며 GAN 적대 손실까지 포함 — 입력에 없는 디테일을 합성(hallucination)할 가능성이 높아 번호판·객체 오검지 위험이 크므로 교통 CCTV 감시 용도에 부적합. |
| **RealViformer** | RealViformer: Investigating Attention for Real-World Video Super-Resolution | ECCV 2024 | Hybrid | medium | ○ | _(검색: RealViformer Real-World Video Super-Reso)_ | Transformer 기반 recurrent VSR이나 2단계 학습에서 GAN(적대) 손실을 사용하므로 생성형으로 분류됨. 번호판·객체 hallucination 위험이 있어 교통 CCTV 사고검지 목적의 fidelity-critical 업스케일에는 부적합하며 비권장. |
| **VideoGigaGAN** | VideoGigaGAN: Towards Detail-rich Video Super-Resolution | arXiv 2024 | GAN | high | △ | [arXiv](https://arxiv.org/abs/2404.12388) | GAN 기반 생성형 VSR 모델(GigaGAN 백본 + 시간 모듈)로, 적대적 학습을 통해 저해상도 입력에 없는 디테일을 합성(hallucination 가능)함. 교통 CCTV 번호판·객체 오검지 위험이 높아 사고검지 용도에는 부적합. |

#### 2025

| Model | Title | Venue | 계열 | Artifact위험 | Real-world | Link | 메모 |
|---|---|---|---|---|---|---|---|
| **DGAF-VSR** | Rethinking Diffusion Model-Based Video Super-Resolution: Leveraging Dense Guidance from Aligned Features | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2511.16928) | Stable Diffusion x4 Upscaler를 사전학습 prior로 사용하는 latent diffusion 기반 생성형 VSR 모델로, DDPM 노이즈 예측 손실로 학습됨. 확산 샘플링 과정에서 입력에 없는 디테일(번호판·객체 등)을 hallucination할 수 있어 교통 CCTV/사고검지 용도에 부적합하며 사용 비권장. |
| **DiffVSR** | DiffVSR: Revealing an Effective Recipe for Taming Robust Video Super-Resolution Against Complex Degradations | ICCV 2025 | Diffusion | high | ○ | _(검색: DiffVSR Taming Robust Video Super-Resolu)_ | Stable Diffusion x4 Upscaler를 생성 prior로 사용하는 latent diffusion 기반 생성형 모델; diffusion denoising 손실 + VAE 디코더에 GAN 손실 포함으로 입력에 없는 디테일을 hallucination할 위험이 높아 교통 CCTV 번호판/객체 오검지 우려가 있으므로 비추천. |
| **DiTVR** | DiTVR: Zero-Shot Diffusion Transformer for Video Restoration | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2508.07811) | Diffusion 생성형 모델(Inf-DiT 사전학습 prior 활용, 훈련 없는 zero-shot 추론) — 역확산 과정에서 입력에 없는 고주파 디테일을 합성하므로 번호판·객체 hallucination 위험이 높음. 교통 CCTV 사고검지용 720p→1080p 업스케일에는 부적합. |
| **DLoRAL** | One-Step Diffusion for Detail-Rich and Temporally Consistent Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2506.15591) | Stable Diffusion(SD) UNet을 생성 prior로 사용하는 one-step diffusion 모델이며, Classifier Score Distillation(CSD)으로 SD의 생성 능력을 활용해 없는 디테일을 합성함 — 번호판·객체 hallucination 위험이 높아 교통 CCTV 사고검지용 fidelity 복원 목적에는 부적합. |
| **DOVE** | DOVE: Efficient One-Step Diffusion Model for Real-World Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2505.16239) | DOVE는 사전학습된 T2V diffusion 모델(CogVideoX)을 백본으로 사용하는 생성형 모델로, 손실함수가 MSE+DISTS로만 구성되어 있어도 diffusion prior가 입력에 없는 디테일을 합성(hallucination)할 수 있으므로 번호판·객체 오검지 위험이 높아 교통 CCTV 용도에 부적합. |
| **FastVSR** | Asymmetric VAE for One-Step Video Super-Resolution Acceleration (FastVSR) | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2509.24142) | CogVideoX1.5 T2V 확산 모델을 frozen prior로 사용하는 one-step diffusion 기반 VSR 모델로, 손실 함수에 MSE+perceptual을 사용해 hallucination 억제를 주장하나, 생성형 확산 prior 자체가 학습 데이터에 없는 디테일을 합성할 수 있어 교통 CCTV 번호판·객체 오검지 위험이 높음 → fidelity 복원형이 아닌 생성형으로 분류되어 사용 비권장. |
| **FlashVSR** | FlashVSR: Towards Real-Time Diffusion-Based Streaming Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2510.12747) | Wan 2.1 T2V 확산 prior(DiT)를 LoRA 파인튜닝한 diffusion 기반 생성형 모델로, flow matching + DMD + L2/LPIPS 손실을 혼합 사용. LR conditioning으로 fidelity를 강화하나 확산 prior가 번호판/객체 hallucination 위험을 내포해 교통 CCTV 오검지용 업스케일에는 부적합. |
| **InfVSR** | InfVSR: Breaking Length Limits of Generic Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2510.00948) | 사전학습된 T2V diffusion 모델(Wan 2.1, 1.3B) 기반 autoregressive one-step diffusion 구조로, 분포 매칭 증류(DMD) 손실을 사용하는 생성형 모델임. 입력에 없는 디테일을 hallucination할 수 있어 번호판·객체 오검지 위험이 높으므로 교통 CCTV 업스케일 용도에는 부적합. |
| **LiftVSR** | LiftVSR: Lifting Image Diffusion to Video Super-Resolution via Hybrid Temporal Modeling | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2506.08529) | PixArt-α(DiT 기반 T2I 확산 모델)를 생성 prior로 사용하는 latent diffusion 계열 VSR 모델로, 노이즈 예측(L2) 손실로 학습하며 입력에 없는 디테일을 hallucination할 수 있어 교통 CCTV 번호판·객체 오검지 위험이 높으므로 사용 비권장. |
| **OS-DiffVSR** | OS-DiffVSR: Towards One-step Latent Diffusion Model for High-detailed Real-world Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2509.16507) | 사전학습된 latent diffusion UNet(LoRA 파인튜닝) + DINOv2 기반 적대 학습을 결합한 생성형 모델로, 입력에 없는 디테일을 hallucination할 수 있음. 번호판·객체 오검지 위험이 높아 교통 CCTV 720p→1080p 업스케일에 부적합. |
| **QuantVSR** | QuantVSR: Low-Bit Post-Training Quantization for Real-World Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2508.04485) | QuantVSR는 MGLD-VSR(사전학습된 Stable Diffusion latent diffusion 기반 VSR)을 저비트 PTQ로 압축한 모델로, diffusion 생성 prior를 사용해 없는 디테일을 합성(hallucination 가능)하므로 교통 CCTV/번호판 검지 등 fidelity-critical 용도에는 부적합하며 사용을 권장하지 않음. |
| **SCST** | Self-supervised ControlNet with Spatio-Temporal Mamba for Real-world Video Super-resolution | CVPR 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2506.02605) | Stable Diffusion V2.1 기반 latent diffusion 생성형 모델로, 논문 자체가 "hallucinate visual content"를 명시함. 입력에 없는 디테일을 만들어낼 수 있어 교통 CCTV 번호판·객체 오검지 위험이 크며, GAN·확산 등 생성형을 피해야 하는 감시/법증거 용도에 부적합. |
| **SDATC** | Spatial Degradation-Aware and Temporal Consistent Diffusion Model for Compressed Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2502.07381) | Stable Diffusion v2.1 기반 latent diffusion 생성 모델로, 입력에 없는 디테일을 확률적으로 합성(hallucination 가능)함. 교통 CCTV 번호판/객체 오검지 위험이 매우 높으므로 사고검지용 업스케일에는 부적합. |
| **STAR** | STAR: Spatial-Temporal Augmentation with Text-to-Video Models for Real-World Video Super-Resolution | ICCV 2025 | Diffusion | high | ○ | _(검색: STAR Spatial-Temporal Augmentation Text-)_ | STAR는 CogVideoX-5B 등 사전학습된 T2V diffusion 모델을 generative prior로 활용하는 생성형 모델(v-prediction 확산 역방향 학습 + Dynamic Frequency Loss)로, 입력에 없는 디테일을 합성(hallucination)할 수 있음. 교통 CCTV 번호판·객체 오검지 위험이 높아 감시/사고검지 용도에는 부적합하며, 비생성형 fidelity 복원 모델(예: RVRT, BasicVSR++ 등 regression 기반)을 대신 권장. |
| **STAR** | STAR: Spatial-Temporal Augmentation with Text-to-Video Models for Real-World Video Super-Resolution | ICCV 2025 | Diffusion | high | ○ | _(검색: STAR Spatial-Temporal Augmentation Text-)_ | CogVideoX-5B 사전학습 T2V 확산 모델을 백본으로 사용하는 생성형 모델. 논문이 직접 "강력한 T2V 생성 능력으로 인한 fidelity 저하"를 핵심 문제로 인정하며, diffusion score matching 기반 학습으로 입력에 없는 디테일 합성(hallucination) 가능. 번호판·객체 hallucination 위험이 높아 교통 CCTV 사고검지 720p→1080p 업스케일 용도로 부적합. |
| **STCDiT** | STCDiT: Spatio-Temporally Consistent Diffusion Transformer for High-Quality Video Super-Resolution | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2511.18786) | 사전학습된 Wan2.1 비디오 확산 모델(T2V/I2V)을 기반으로 하며, 추론 시 10스텝 iterative diffusion denoising을 수행하는 생성형 모델임. 훈련 손실은 flow-matching 기반 denoising MSE로 확산 목적함수이며, 순수 회귀 복원이 아님. 입력에 없는 세부사항을 합성(hallucination)할 수 있어 교통 CCTV 번호판·객체 오검지 위험이 높으므로 사고검지용 업스케일에는 부적합. |
| **Stream-DiffVSR** | Stream-DiffVSR: Low-Latency Streamable Video Super-Resolution via Auto-Regressive Diffusion | arXiv 2025 | Diffusion | high | △ | [arXiv](https://arxiv.org/abs/2512.23709) | Stable Diffusion 사전학습 prior 기반 latent diffusion 모델로, 전 학습 단계에 PatchGAN 적대 손실이 포함된 생성형 모델임. 번호판·객체 등 입력에 없는 디테일을 hallucination할 위험이 높아 교통 CCTV 사고검지용 fidelity 업스케일에는 부적합. |
| **UltraVSR** | UltraVSR: Achieving Ultra-Realistic Video Super-Resolution with Efficient One-Step Diffusion | arXiv 2025 | Diffusion | high | ○ | [arXiv](https://arxiv.org/abs/2505.19958) | Stable Diffusion 사전학습 prior 기반 one-step diffusion + temporal GAN loss 사용 생성형 모델로, 번호판·객체 등 입력에 없는 디테일을 hallucination할 위험이 높아 교통 CCTV 사고검지 용도에 부적합. |


---
## ✕ 제외 — 목적 무관/도메인 한정 (45편)

| Model | Year | 사유 | 비고 |
|---|---|---|---|
| STFAN | 2019 | Video SR 아님 | STFAN은 비디오 디블러링(모션 블러 제거) 논문으로, 공간 해상도 향상(SR)과 무관하므로 720p→1080p 업스케일링 목적에 전혀 적합하지 않다. |
| DeepTemporalSR | 2020 | Video SR 아님 | DeepTemporalSR는 단일 영상 내부 패치 반복성을 이용한 프레임레이트(시간 해상도) 향상 논문으로, 공간 해상도(픽셀)를 높이는 Video SR이 아니기 때문에 720p→1080p 업스케일링 목적에 전혀 부합하지 않는다. |
| DKC | 2020 | 목적 부합 낮음(fit=low) | AIM2020 Video Extreme SR 챌린지용으로 x16 초고배율에 특화된 Video SR 논문이며, 합성(synthetic) 열화만 다루고 x1.5(720p→1080p) 실세계 배포 시나리오와 직접 부합하지 않아 적합도 낮음. |
| FISR | 2020 | 목적 부합 낮음(fit=low) | FISR는 2K→4K 공간 SR과 30→60fps 프레임 보간을 동시에 수행하는 조인트 프레임워크이나, 합성(bicubic) 열화 기반이고 실세계/blind 열화 미지원 및 실시간 배포 불가로 교통 CCTV 720p→1080p 목적에는 부적합. |
| STARnet | 2020 | 목적 부합 낮음(fit=low) | STARnet은 공간 해상도 향상과 프레임 보간을 동시에 수행하는 시공간 SR 모델이나, 합성(bicubic) 열화 기반이고 실세계 blind 열화에 대한 강인성이 없으며 경량화·배포 고려가 부족해 교통 CCTV 720p→1080p 실적용 목적에는 부적합하다. |
| STVUN | 2020 | 목적 부합 낮음(fit=low) | STVUN은 공간(VSR)과 시간(VFI) 업샘플링을 단일 네트워크로 통합한 ECCV 2020 논문으로, 비디오 SR 자체는 맞지만 합성 열화(bicubic) 기반이고 실세계 blind 열화 대응이 없으며 프레임 보간이 주요 기여라 720p→1080p 교통 CCTV 실배포 목적에는 부적합하다. |
| AdaptedVSR | 2021 | 목적 부합 낮음(fit=low) | bicubic 합성 열화 기반 자기지도 테스트-타임 적응 VSR 방법으로, 실세계 blind 열화 미지원 및 테스트당 수분 소요의 높은 연산 비용으로 CCTV/PTZ 720p→1080p 실시간 배포 목적에 부적합. |
| E-VSR | 2021 | 도메인 한정: event-camera | 이벤트 카메라(DVS)의 비동기 이벤트 스트림을 이용해 고주파 영상을 생성하고 이를 VSR에 활용하는 방법으로, 별도 이벤트 카메라 하드웨어가 필수이므로 일반 720p CCTV/PTZ 환경에는 적용 불가. |
| EFENet | 2021 | 목적 부합 낮음(fit=low) | EFENet은 외부 HR 참조 프레임이 반드시 필요한 Reference-based VSR로, 단일 카메라 CCTV·PTZ 환경에서는 참조 프레임 확보가 불가능하고 실세계 열화에 대한 대응도 없어 목적 시나리오에 부적합하다. |
| MEGAN | 2021 | 목적 부합 낮음(fit=low) | MEGAN은 시공간 비디오 SR(STVSR) 논문으로 저해상도·저프레임 영상을 동시에 고해상도·고프레임으로 복원하지만, 프레임 보간이 목적에 불필요하고 합성(bicubic) 열화만 다루며 연산 비용이 높아 실세계 CCTV 720p→1080p 공간 업스케일링 검증 목적에는 부적합하다. |
| SRVC | 2021 | Video SR 아님 | SRVC는 영상 압축 효율을 높이기 위해 코덱 파이프라인 내부에 콘텐츠 적응형 SR을 결합한 압축 프레임워크로, 실세계 CCTV 영상의 범용 업스케일링 목적에는 부적합하다. |
| TMNet | 2021 | 목적 부합 낮음(fit=low) | TMNet은 공간 해상도 향상과 프레임 보간을 동시에 수행하는 시공간 비디오 SR 모델로, 합성(바이큐빅) 열화만 다루고 실세계 블라인드 열화를 지원하지 않으며 프레임 보간이 불필요한 CCTV 720p→1080p 목적에는 부적합하다. |
| AnimeSR | 2022 | 도메인 한정: animation | 애니메이션 비디오 전용 실세계 열화 기반 Video SR 모델로, 교통 CCTV/PTZ 자연 영상과는 도메인이 완전히 다르므로 목적에 부적합. |
| DAVSR | 2022 | 목적 부합 낮음(fit=low) | DAVSR는 공간(x4) + 시간(프레임보간) + 디블러링을 동시에 수행하는 Space-Time VSR 논문으로, 합성(bicubic) 열화만 다루고 실세계 블라인드 열화 미지원 및 720p→1080p(x1.5배) 목적에 과도한 x4 업스케일만 제공하여 교통 CCTV 실용 배포 목적에 부적합. |
| EffVSRTrain | 2022 | 목적 부합 낮음(fit=low) | VSR 모델 학습을 멀티그리드 전략으로 최대 6.2배 가속하는 훈련 기법 논문으로, 실세계 열화 대응이나 추론 경량화를 다루지 않아 720p→1080p 실배포 목적에 직접 부합하지 않음. |
| RefVSR | 2022 | 도메인 한정: stereo | 스마트폰 트리플 카메라의 참조 영상을 활용한 Reference-based Video SR 논문으로, 단일 CCTV/PTZ 카메라 환경에서는 추가 참조 카메라 영상이 없어 적용 불가능하며 목적에 부적합. |
| RSTT | 2022 | 목적 부합 낮음(fit=low) | RSTT는 공간 해상도와 프레임률을 동시에 높이는 Space-Time VSR 논문으로, 실시간 처리는 가능하지만 순수 720p→1080p 공간 업스케일링 목적과 방향이 다르고 합성(bicubic) 열화 기반이라 실세계 교통 CCTV 영상에 대한 범용성이 낮아 목적 적합도가 낮음. |
| STDAN | 2022 | 목적 부합 낮음(fit=low) | 공간+시간 해상도를 동시에 높이는 STVSR 논문으로, 목적 시나리오(720p→1080p 순수 공간 업스케일)에는 프레임 보간이 불필요하고 bicubic 합성 열화만 다뤄 실세계 CCTV 영상에 직접 적용하기 어려워 적합도가 낮음. |
| Trans-SVSR | 2022 | 도메인 한정: stereo | 스테레오(양안) 카메라 영상에 특화된 Video SR 논문으로, 단일 카메라 CCTV/PTZ 환경에는 적용 불가한 도메인 한정 모델이다. |
| EgoVSR | 2023 | 도메인 한정: egocentric | 1인칭(egocentric) 웨어러블 카메라 영상의 모션 블러와 압축 열화에 특화된 비디오 SR 논문으로, 교통 CCTV/PTZ 범용 업스케일링 목적에는 도메인 불일치로 부적합. |
| EGVSR | 2023 | 도메인 한정: event-camera | 이벤트 카메라 + RGB 프레임을 동시에 입력받아 임의 배율 Video SR을 수행하는 논문이며, 특수 하드웨어(이벤트 카메라) 의존으로 일반 CCTV/PTZ 환경에 적용 불가하여 목적에 부적합. |
| STDO | 2023 | 목적 부합 낮음(fit=low) | 네트워크 동영상 전송 시스템에서 각 청크에 SR 모델을 오버피팅하는 Video SR 논문으로, bicubic 열화만 처리하고 영상별 사전 학습이 필수적이어서 교통 CCTV 범용 업스케일링 목적에는 부적합함. |
| VQD-SR | 2023 | 도메인 한정: animation | 애니메이션 비디오 전용으로 벡터 양자화 열화 모델을 학습해 4x SR을 수행하나, 도메인이 애니메이션에 한정되어 교통 CCTV/PTZ 업스케일링 목적에는 부적합하다. |
| EATER | 2024 | 도메인 한정: event-camera | 이벤트 카메라 스트림을 활용해 기존 VSR 모델을 적응시키는 방법으로, 일반 CCTV/PTZ 카메라(단일 RGB 카메라) 환경에는 적용 불가하여 목적에 부적합. |
| EvTexture | 2024 | 도메인 한정: event-camera | 이벤트 카메라의 고주파 텍스처 정보를 활용한 Video SR 논문이나, 추론 시 이벤트 카메라 하드웨어가 필수적으로 요구되어 일반 CCTV/PTZ 환경에 적용 불가하므로 목적에 부적합. |
| KEEP | 2024 | 도메인 한정: face | 얼굴 전용 비디오 SR 논문으로, Kalman 필터 기반 시간적 특징 전파로 얼굴 영상 화질을 향상시키지만 교통/PTZ 범용 장면에는 적용 불가하여 목적에 부적합. |
| SATeCo | 2024 | 목적 부합 낮음(fit=low) | SATeCo는 확산 모델 기반의 범용 비디오 SR 논문이나, 합성(bicubic) 열화만 다루고 확산 모델 특성상 실시간 배포가 불가해 교통 CCTV 720p→1080p 실시간 업스케일링 목적에는 낮은 적합도. |
| SeeClear | 2024 | 목적 부합 낮음(fit=low) | SeeClear는 Diffusion 기반 VSR 프레임워크로 시맨틱 증류와 픽셀 압축을 결합해 4x 업스케일링을 수행하지만, 바이큐빅 합성 열화만을 대상으로 하고 Diffusion 모델의 무거운 추론 비용으로 인해 실세계 CCTV/PTZ 실시간 업스케일링 목적에는 부적합하다. |
| StableVSR | 2024 | 목적 부합 낮음(fit=low) | StableVSR는 Stable Diffusion 기반으로 시간적 일관성을 갖춘 고화질 비디오 SR을 수행하지만, x4 바이큐빅 합성 열화만 다루고 프레임당 처리 시간이 약 100초에 달해 교통 CCTV 실시간 배포에 부적합하다. |
| VIDIM | 2024 | Video SR 아님 | VIDIM은 두 프레임 사이의 중간 프레임을 생성하는 영상 보간(frame interpolation) 논문으로, 공간 해상도를 높이는 Video SR이 아니므로 720p→1080p 업스케일링 목적에 전혀 부합하지 않는다. |
| CreativeVR | 2025 | Video SR 아님 | CreativeVR는 AI 생성 및 실제 영상의 구조적·시간적 아티팩트를 복원하는 비디오 복원 논문으로, 공간 해상도를 높이는 Video Super-Resolution이 아니므로 720p→1080p 업스케일링 목적에 전혀 적합하지 않음. |
| DAM-VSR | 2025 | 목적 부합 낮음(fit=low) | 실세계 열화에 강인한 범용 Video SR 논문이지만, Stable Video Diffusion 기반의 무거운 확산 모델을 사용하여 실시간/엣지 배포가 사실상 불가능하므로 교통 CCTV 720p→1080p 실시간 업스케일링 목적에는 부적합하다. |
| DC-VSR | 2025 | 목적 부합 낮음(fit=low) | DC-VSR는 Stable Video Diffusion 기반으로 실세계 열화를 처리하는 범용 x4 비디오 SR 논문이나, 추론이 A100-80GB급 GPU를 요구하며 실시간 불가능으로 명시되어 교통 CCTV 엣지 배포에는 부적합하다. |
| DSFN | 2025 | 목적 부합 낮음(fit=low) | DSFN은 디블러링·SR·프레임 보간을 통합한 비디오 향상 네트워크지만, 합성(bicubic) 열화만 학습하고 실세계/blind 열화에 대한 일반화가 검증되지 않아 교통 CCTV 실세계 열화 업스케일링 목적에는 부적합. |
| Ev-DeblurVSR | 2025 | 도메인 한정: event-camera | 이벤트 카메라의 이벤트 스트림을 필수 입력으로 사용하는 블러리 비디오 SR 논문으로, 일반 CCTV/PTZ 카메라에는 적용 불가하여 목적에 부적합. |
| EvEnhancer | 2025 | 도메인 한정: event-camera | 이벤트 카메라 스트림을 필수 입력으로 사용하는 임의 시공간 스케일 Video SR 논문으로, 별도 이벤트 카메라 하드웨어가 없는 교통 CCTV/PTZ 환경에는 적용 불가. |
| FedVSR | 2025 | 목적 부합 낮음(fit=low) | FedVSR는 기존 VSR 모델(VRT/RVRT/IART)을 연합학습(Federated Learning)으로 훈련하는 프레임워크로, bicubic 합성 열화만 실험하고 실시간 배포 가능한 모델이 아니므로 실세계 CCTV 720p→1080p 업스케일링 목적에는 부적합하다. |
| MedVSR | 2025 | 도메인 한정: medical | 의료 영상(내시경·백내장 수술) 전용 비디오 SR 논문으로, 교통 CCTV/PTZ 카메라의 720p→1080p 범용 업스케일링 목적에는 도메인 불일치로 부적합. |
| PatchVSR | 2025 | 목적 부합 낮음(fit=low) | PatchVSR는 비디오 확산 모델 기반의 패치 단위 영상 초해상도 방법으로, 일반 자연 영상에 적용 가능하나 실세계(blind) 열화에 취약하고 추론에 40GB GPU/680초가 필요해 교통 CCTV 실시간 업스케일링 목적에는 부적합하다. |
| S3PO | 2025 | 도메인 한정: 360 | S3PO는 360도 전방위(VR) 영상의 구면 왜곡을 고려한 Video SR 모델로, 교통 CCTV/PTZ 일반 카메라 영상에는 적용 불가한 도메인 한정 모델이다. |
| SimpleGVR | 2025 | 도메인 한정: none (but AIGC/T2V pipeline-locked) | SimpleGVR는 텍스트-투-비디오(T2V) 생성 모델의 후처리용 잠재 공간 캐스케이드 비디오 SR로, AI 생성 영상 전용 합성 열화만 다루며 실세계 CCTV/PTZ 영상의 blind 열화에 대응하지 못해 교통 감시 업스케일링 목적에는 부적합. |
| STDNet | 2025 | Video SR 아님 | STDNet은 비디오 깊이(depth) 맵 초해상도 논문으로, RGB 영상 자체의 해상도를 높이는 것이 아니라 LiDAR/ToF 깊이 맵을 업스케일하는 도메인 특화 연구이며, 교통 CCTV/PTZ RGB 영상 업스케일 목적에 전혀 부합하지 않는다. |
| UniMMVSR | 2025 | 도메인 한정: AI-generated video upscaling (text-to-video / multi-ID image-guided video / video editing pipelines only) | UniMMVSR는 AI 생성 비디오(text-to-video)를 4K로 업스케일하는 멀티모달 확산 기반 캐스케이드 VSR 프레임워크로, 실제 CCTV/PTZ 카메라 영상의 실세계 열화(real-world degradation)에 대응하지 않고 도메인이 AI 생성 영상에 고정되어 교통 감시 목적에 부적합하다. |
| CubeComposer | 2026 | Video SR 아님 | 이 논문은 일반 원근 영상을 4K 360도 파노라마 영상으로 생성하는 확산 모델로, 교통 CCTV 720p→1080p 업스케일링(Video SR) 목적과 전혀 무관한 360도 도메인 한정 영상 생성 연구다. |
| DUO-VSR | 2026 | 목적 부합 낮음(fit=low) | 실세계 열화를 지원하는 확산 기반 1-step 동영상 SR이지만, 1.3B 파라미터에 21프레임당 11초 이상 소요되어 교통 CCTV 실시간 배포에는 부적합하다. |


---
## 실무 권고

- **1순위 PoC**: 위 **Top picks(비생성 + real-world)** — 특히 `DualX-VSR`(Transformer, 모션보정 없는 실세계 VSR), `IART`(implicit alignment), `EAVSR`/`COMISR`(real-world·압축 인지), `CDA-VSR`/`FCA2`(압축 도메인), `FMA-Net++`(joint deblur+SR) 계열. 720p→1080p는 ×1.5라 ×2/×4 모델을 ×2로 쓰고 다시 리사이즈하거나 arbitrary-scale 모델(`ST-AVSR`, `BasicAVSR`, `VideoINR`) 활용 가능.
  - ※ 주의: `RealViformer`·`SRWD-VSR`는 real-world VSR이지만 **GAN 적대학습 기반(생성형)** 이라 본 목적(artifact 회피)에선 ⚠️ 제외 섹션으로 분류됨.
- **경량/실시간**(엣지·실시간 검지 파이프라인): `VESPCN`, `FRVSR`, `TDAN`, `CTUN`, `RSCL` 등 경량 recurrent/CNN. 정확도-속도 트레이드오프 측정 필요.
- **공통 주의**: 대부분 bicubic 합성 열화로 학습됨(△) → 실제 CCTV 압축·노이즈 도메인갭 존재. 자체 720p/1080p 페어(또는 의사 페어)로 fine-tune 권장.
- **검증 지표**: PSNR/SSIM(충실도)뿐 아니라 **업스케일 후 객체탐지/이벤트검지 mAP 변화**를 실제 측정해야 SR의 "실효성"을 입증할 수 있음. 생성형은 NR-IQA가 좋아도 검지 성능은 떨어질 수 있으니 별도 검증.
- **생성형(⚠️ 섹션)**: 사고검지엔 비권장이나, 화질 자체가 목적이면 후보. 사용 시 hallucination이 검지에 미치는 영향(false positive)을 반드시 평가.
