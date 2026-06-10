# 추천 Video SR 60편 전수조사 — 코드·라이선스·Pretrained·관련성 점수

> 대상: `VideoSR_for_720p_to_1080p.md`의 **비생성 fidelity 추천 60편** (중복 제거 후 58 유니크)
> 작성일: 2026-06-10 · 방법: 논문 1편당 sonnet 에이전트가 **arXiv + 공식 GitHub 리포**를 직접 확인 — 논문/코드 링크, 코드 라이선스(LICENSE 파일), pretrained 가중치 공개 여부, 프레임워크, scale, 실세계 열화 대응, 효율, 데이터셋, 관련성 점수
> 목적: 교통 PTZ/CCTV **720p→1080p 업스케일**로 객체·이벤트(사고) 검지 성능 개선

## 관련성 점수 루브릭 (0~100)
순서대로 논문을 검토할 수 있도록 통일 기준으로 채점. 5개 항목 합산:
| 항목 | 배점 | 내용 |
|---|---|---|
| 실세계/blind 열화 강인성 | 25 | H.264/265 압축·노이즈·블러 등 실제 CCTV 열화 대응 (bicubic 전용이면 낮음) |
| 배포가능성 | 25 | 효율/실시간/스트리밍/경량/online·causal (PTZ 라이브) |
| 재현성/사용성 | 20 | 코드 공개 + pretrained + 관대한 라이선스 |
| 범용 장면 적용성 | 15 | 거리/교통 등 일반 자연영상 |
| Scale/최신성 | 15 | ~×1.5–2 / arbitrary-scale, 최신·고성능 |

**표기:** Real-world ○=실세계/blind · △=합성(bicubic) | Pretrained ✅공개 ◐일부 ✗없음 | License ⚠️NC/research=비상업/연구용 제한, 미명시=LICENSE 파일 없음(상업사용 불확실)

## 요약 통계

- 총 **58편** 조사 · 코드 공개 **48편**(10편 없음) · pretrained ✅ **35** / ◐일부 4 / ✗없음 19
- 라이선스: MIT 16 · Apache-2.0 9 · 미명시 19 · ⚠️NC/research 2 · BSD 1
- real-world 대응 ○ **12편** / 합성 △ 46편

## 마스터 표 (관련성 점수 내림차순)

| # | 점수 | 등급 | Model | Year | 계열 | RW | 코드 | License | PT | Scale | 논문 |
|--:|--:|:--:|---|--:|:--:|:--:|:--:|---|:--:|---|:--:|
| 1 | **77** | S | **CDA-VSR** | 2026 | CNN | ○ | [GH](https://github.com/sspBIT/CDA-VSR) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2603.07694) |
| 2 | **72** | S | **COMISR** | 2021 | Rec-CNN | ○ | [GH](https://github.com/google-research/google-research/tree/master/comisr) | Apache-2.0 | ✅ | x4 | [링크](https://arxiv.org/abs/2105.01237) |
| 3 | **68** | S | **FTVSR** | 2022 | Transf | ○ | [GH](https://github.com/researchmm/FTVSR) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/2208.03012) |
| 4 | **65** | S | **FCA2** | 2025 | CNN | ○ | [GH](https://github.com/handsomewzy/FCA2) | 미명시 | ✗ | x4 | [링크](https://arxiv.org/abs/2506.11545) |
| 5 | **62** | A | **TURTLE** | 2024 | Hybrid | ○ | [GH](https://github.com/Ascend-Research/Turtle) | MIT | ✅ | x4 (Real-World | [링크](https://arxiv.org/abs/2410.03936) |
| 6 | **62** | A | **RealVSR** | 2021 | Rec-CNN | ○ | [GH](https://github.com/IanYeung/RealVSR) | Apache-2.0 | ✅ | x2 (iPhone 11  | [링크](https://openaccess.thecvf.com/content/ICCV2021/html/Yang_Real-World_Video_Super-Resolution_A_Benchmark_Dataset_and_a_Decomposition_Based_ICCV_2021_paper.html) |
| 7 | **58** | A | **DeepBlindVSR** | 2021 | Rec-CNN | ○ | [GH](https://github.com/csbhr/Deep-Blind-VSR) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/2003.04716) |
| 8 | **57** | A | **ST-AVSR** | 2024 | Rec-CNN | △ | [GH](https://github.com/shangwei5/ST-AVSR) | MIT | ✅ | arbitrary-scal | [링크](https://arxiv.org/abs/2407.09919) |
| 9 | **55** | A | **BasicAVSR** | 2025 | Rec-CNN | △ | [GH](https://github.com/shangwei5/BasicAVSR) | MIT | ✅ | arbitrary-scal | [링크](https://arxiv.org/abs/2510.26149) |
| 10 | **54** | A | **EAVSR** | 2022 | Rec-CNN | ○ | [GH](https://github.com/HITRainer/EAVSR) | MIT | ✗ | x2, x4 | [링크](https://arxiv.org/abs/2212.05342) |
| 11 | **52** | A | **DualX-VSR** | 2025 | Transf | ○ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2506.04830) |
| 12 | **52** | A | **FMA-Net++** | 2025 | Hybrid | ○ | [GH](https://github.com/KAIST-VICLab/FMA-Net-PlusPlus) | 미명시 | ✗ | x4 | [링크](https://arxiv.org/abs/2512.04390) |
| 13 | **52** | A | **BasicVSR / IconVSR** | 2021 | Rec-CNN | △ | [GH](https://github.com/ckkelvinchan/BasicVSR-IconVSR) | Apache-2.0 | ✅ | x4 | [링크](https://arxiv.org/abs/2012.02181) |
| 14 | **52** | A | **OVSR** | 2021 | Rec-CNN | △ | [GH](https://github.com/psychopa4/OVSR) | Apache-2.0 | ✅ | x4 | [링크](https://arxiv.org/abs/2103.15683) |
| 15 | **52** | A | **Deep Blind Video Super-resolution** | 2020 | CNN | ○ | [GH](https://github.com/csbhr/Deep-Blind-VSR) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/2003.04716) |
| 16 | **51** | A | **EDVR** | 2019 | Rec-CNN | △ | [GH](https://github.com/xinntao/EDVR) | Apache-2.0 | ✅ | x4 (primary);  | [링크](https://arxiv.org/abs/1905.02716) |
| 17 | **48** | B | **VSR-Transformer** | 2021 | Transf | △ | [GH](https://github.com/caojiezhang/VSR-Transformer) | Apache-2.0 | ✅ | x4 | [링크](https://arxiv.org/abs/2106.06847) |
| 18 | **47** | B | **BVSR-IK** | 2025 | Rec-CNN | △ | [GH](https://github.com/QZ1-boy/BVSR-IK) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2503.07856) |
| 19 | **47** | B | **MIA-VSR** | 2024 | Transf | △ | [GH](https://github.com/LabShuHangGU/MIA-VSR) | Apache-2.0 | ✅ | x4 | [링크](https://arxiv.org/abs/2401.06312) |
| 20 | **47** | B | **VRT** | 2022 | Transf | △ | [GH](https://github.com/JingyunLiang/VRT) | ⚠️ NC/research | ✅ | x4 (주), x4 BD  | [링크](https://arxiv.org/abs/2201.12288) |
| 21 | **47** | B | **PP-MSVSR / PP-MSVSR-L** | 2021 | Rec-CNN | △ | [GH](https://github.com/PaddlePaddle/PaddleGAN) | Apache-2.0 | ✅ | x4 | [링크](https://arxiv.org/abs/2112.02828) |
| 22 | **47** | B | **RRN** | 2020 | Rec-CNN | △ | [GH](https://github.com/junpan19/RRN) | 미명시 | ✅ | x2/x3/x4 (x4 p | [링크](https://arxiv.org/abs/2008.05765) |
| 23 | **46** | B | **TTVSR** | 2022 | Transf | △ | [GH](https://github.com/researchmm/TTVSR) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/2204.04216) |
| 24 | **45** | B | **RBPN** | 2019 | Rec-CNN | △ | [GH](https://github.com/alterzero/RBPN-PyTorch) | MIT | ✅ | x2, x4, x8 | [링크](https://arxiv.org/abs/1903.10128) |
| 25 | **44** | B | **TS-Mamba** | 2025 | Mamba | △ | [GH](https://github.com/QZ1-boy/TS-Mamba) | Apache-2.0 | ✗ | x4 | [링크](https://arxiv.org/abs/2508.10453) |
| 26 | **42** | B | **BF-STVSR** | 2025 | Flow/INR | △ | [GH](https://github.com/LAIT-CVLab/BF-STVSR) | 미명시 | ✅ | spatial x2–x4  | [링크](https://arxiv.org/abs/2501.11043) |
| 27 | **42** | B | **IART** | 2023 | Hybrid | △ | [GH](https://github.com/kai422/IART) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/2305.00163) |
| 28 | **42** | B | **CycMu-Net** | 2022 | Rec-CNN | △ | [GH](https://github.com/tongyuantongyu/cycmunet) | BSD | ✅ | x4 spatial + x | [링크](https://arxiv.org/abs/2205.05264) |
| 29 | **42** | B | **DAP** | 2022 | Rec-CNN | △ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2202.01731) |
| 30 | **42** | B | **SSL** | 2022 | Rec-CNN | △ | [GH](https://github.com/Zj-BinXia/SSL) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2206.07687) |
| 31 | **42** | B | **VideoINR** | 2022 | Flow/INR | △ | [GH](https://github.com/Picsart-AI-Research/VideoINR-Continuous-Space-Time-Super-Resolution) | 미명시 | ✅ | arbitrary-scal | [링크](https://arxiv.org/abs/2206.04647) |
| 32 | **42** | B | **MuCAN** | 2020 | CNN | △ | [GH](https://github.com/Jia-Research-Lab/Simple-SR) | MIT | ◐ | x4 | [링크](https://arxiv.org/abs/2007.11803) |
| 33 | **42** | B | **RSDN** | 2020 | Rec-CNN | △ | [GH](https://github.com/junpan19/RSDN) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2008.00455) |
| 34 | **42** | B | **PFNL** | 2019 | CNN | △ | [GH](https://github.com/psychopa4/PFNL) | MIT | ✅ | x4 | [링크](https://openaccess.thecvf.com/content_ICCV_2019/html/Yi_Progressive_Fusion_Video_Super-Resolution_Network_via_Exploiting_Non-Local_Spatio-Temporal_Correlations_ICCV_2019_paper.html) |
| 35 | **42** | B | **FRVSR** | 2018 | Rec-CNN | △ | [GH](https://github.com/msmsajjadi/FRVSR) | 미명시 | ✗ | x4 | [링크](https://arxiv.org/abs/1801.04590) |
| 36 | **42** | B | **TDAN** | 2018 | CNN | △ | [GH](https://github.com/YapengTian/TDAN-VSR-CVPR-2020) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/1812.02898) |
| 37 | **38** | C | **CTUN** | 2024 | Rec-CNN | △ | [GH](https://github.com/House-Leo/CTUN) | 미명시 | ◐ | x4 | [링크](https://arxiv.org/abs/2408.14244) |
| 38 | **38** | C | **U3D-RDN + MSCU** | 2021 | Rec-CNN | △ | [GH](https://github.com/iPrayerr/DSMC-VSR) | 미명시 | ◐ | x4 | [링크](https://arxiv.org/abs/2103.11744) |
| 39 | **38** | C | **VESR-Net** | 2020 | Rec-CNN | ○ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2003.02115) |
| 40 | **38** | C | **DNLN** | 2019 | CNN | △ | [GH](https://github.com/wh1h/DNLN) | MIT | ✅ | x4 | [링크](https://arxiv.org/abs/1909.10692) |
| 41 | **38** | C | **DUF** | 2018 | CNN | △ | [GH](https://github.com/yhjo09/VSR-DUF) | 미명시 | ✅ | x2, x3, x4 | [링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Jo_Deep_Video_Super-Resolution_CVPR_2018_paper.html) |
| 42 | **37** | C | **ETDM** | 2022 | Rec-CNN | △ | [GH](https://github.com/junpan19/ETDM) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2204.07114) |
| 43 | **37** | C | **VSR-TGA** | 2020 | Rec-CNN | △ | [GH](https://github.com/junpan19/VSR_TGA) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2007.10595) |
| 44 | **36** | C | **RevisitTempAlign** | 2021 | Rec-CNN | △ | [GH](https://github.com/redrock303/Revisiting-Temporal-Alignment-for-Video-Restoration) | ⚠️ NC/research | ✅ | x4 | [링크](https://arxiv.org/abs/2111.15288) |
| 45 | **34** | C | **SPMC** | 2017 | Rec-CNN | △ | [GH](https://github.com/jiangsutx/SPMC_VideoSR) | MIT | ◐ | x2, x3, x4 | [링크](https://arxiv.org/abs/1704.02738) |
| 46 | **33** | C | **MRVSR** | 2022 | Rec-CNN | △ | [GH](https://github.com/bjmch/MRVSR) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2112.08950) |
| 47 | **33** | C | **PSRT** | 2022 | Transf | △ | [GH](https://github.com/XPixelGroup/RethinkVSRAlignment) | 미명시 | ✅ | x4 | [링크](https://arxiv.org/abs/2207.08494) |
| 48 | **32** | C | **LRTI-VSR** | 2025 | Hybrid | △ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2505.02159) |
| 49 | **32** | C | **USTVSRNet** | 2021 | CNN | △ | ✗ | — | ✗ | arbitrary-scal | [링크](https://arxiv.org/abs/2102.13011) |
| 50 | **32** | C | **FSTRN** | 2019 | Rec-CNN | △ | [GH](https://github.com/lsmale/FSTRN) | Apache-2.0/MIT | ✗ | x4 | [링크](https://arxiv.org/abs/1904.02870) |
| 51 | **31** | C | **MANA** | 2022 | Transf | △ | [GH](https://github.com/jiy173/MANA) | MIT | ✗ | x4 | [링크](https://arxiv.org/abs/2108.11048) |
| 52 | **28** | C | **LDIP** | 2025 | Rec-CNN | △ | ✗ | — | ✗ | x4, arbitrary- | [링크](https://openaccess.thecvf.com/content/ICCV2025/papers/Bernasconi_LDIP_Long_Distance_Information_Propagation_for_Video_Super-Resolution_ICCV_2025_paper.pdf) |
| 53 | **28** | C | **MambaVSR** | 2025 | Mamba | △ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2506.11768) |
| 54 | **28** | C | **VSRM** | 2025 | Mamba | △ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2506.22762) |
| 55 | **28** | C | **CFDVSR** | 2024 | Rec-CNN | △ | [GH](https://github.com/House-Leo/CFDVSR) | 미명시 | ✗ | x4 | [링크](https://arxiv.org/abs/2404.04745) |
| 56 | **28** | C | **FDAN** | 2021 | Rec-CNN | △ | ✗ | — | ✗ | x4 | [링크](https://arxiv.org/abs/2105.05640) |
| 57 | **28** | C | **VESPCN** | 2017 | Rec-CNN | △ | ✗ | — | ✗ | x3, x4 | [링크](https://arxiv.org/abs/1611.05250) |
| 58 | **22** | C | **UnderstandDefAlign** | 2020 | CNN | △ | [GH](https://github.com/ckkelvinchan/offset-fidelity-loss) | 미명시 | ✗ | x4 | [링크](https://arxiv.org/abs/2009.07265) |

---
## 상세 (등급별)


### ★★★ 최우선 (≥65)

#### [77점] CDA-VSR — CVPR 2026 (2026)
- **논문:** [링크](https://arxiv.org/abs/2603.07694) · **코드:** [https://github.com/sspBIT/CDA-VSR](https://github.com/sspBIT/CDA-VSR) · **License:** none/unspecified (GitHub license field is null; NOTICE contains Apache-2.0 from TMP dependency; no project-level LICENSE file confirmed) · **Framework:** PyTorch
- **Pretrained:** yes — best.pth committed directly in pretrained_models/ directory of the GitHub repo
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** 3.3M params, ~90 FPS (10.8ms runtime) on REDS4 (320×180 input), 78G MACs; fully online/causal (no future frames)
- **Datasets:** REDS (train), REDS4 (eval), Inter4K 720p/1080p/2K (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (20/25): H.264 코덱의 motion vector·residual map·frame type을 직접 활용하여 압축 열화에 특화 대응. CCTV/교통 카메라의 주요 열화인 코덱 압축을 명시적으로 처리. 단, blind 노이즈·블러보다는 코덱 압축 특화라 만점 미달. 2. 배포가능성 (23/25): 90 FPS, 3.3M params, 10.8ms로 실시간 기준(60fps)을 훨씬 상회. online/causal 설계로 과거+현재 프레임만 사용, PTZ 라이브 스트리밍에 직접 적용 가능. 3. 재현성/사용성 (13/20): 코드 공개 및 pretrained weights(best.pth) 포함. 그러나 GitHub API가 프로젝트 라이선스를 null로 반환하여 상업적 사용 가능 여부 불명확. 훈련 데이터는 Baidu Netdisk 제공. 4. 범용 장면 적용성 (12/15): REDS(다양한 실내외 자연영상)+Inter4K로 평가. 특정 도메인 특화 아님. 교통·거리 장면에 일반 적용 가능. 5. Scale/최신성 적합 (9/15): x4 고정 배율만 지원(×1.5-2 또는 arbitrary-scale 미지원). CVPR 2026 최신 논문. REDS4에서 SOTA 대비 +0.13dB이며 >2× 속도 우위.

#### [72점] COMISR — ICCV 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2105.01237) · **코드:** [https://github.com/google-research/google-research/tree/master/comisr](https://github.com/google-research/google-research/tree/master/comisr) · **License:** Apache-2.0 · **Framework:** TensorFlow
- **Pretrained:** yes — x4 weights on Google Cloud Storage (gs://gresearch/comisr/model/), requires gcloud SDK
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** 2.63M params, 0.06T FLOPs (Vid4 x4 VSR); trained on 8x NVIDIA Tesla V100 GPUs; no FPS reported
- **Datasets:** REDS, Vimeo-90K (train); Vid4, REDS4, Vimeo-90K-T (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(20/25): H.264 압축(CRF 15~35 범위) 및 YouTube 스트리밍 압축을 명시적으로 처리하는 유일한 VSR 논문 중 하나. CCTV/PTZ의 실제 압축 열화에 직접 대응. 단, bicubic 다운샘플+H.264 압축 조합으로 blind 실세계 열화 전체를 커버하지는 않음. 2. 배포가능성(14/25): 양방향 recurrent 구조(오프라인), 2.63M 파라미터로 경량이나 bidirectional이라 스트리밍/causal 실시간 처리 불가. FPS 미보고. 3. 재현성/사용성(16/20): 코드 공개(Google Research GitHub), pretrained 가중치 공개(GCS), Apache-2.0 라이선스로 상업적 사용 가능. TF 기반으로 최신 PyTorch 생태계 대비 다소 불편. 4. 범용 장면 적용성(12/15): REDS/Vimeo-90K로 다양한 자연영상 학습. 교통/도로 영상에도 직접 적용 가능한 범용 모델. 5. Scale/최신성 적합(10/15): x4만 지원(x1.5~x2 미지원). ICCV 2021로 최신성은 다소 떨어지나 압축 특화 접근은 독보적. 합계 72점.

#### [68점] FTVSR — ECCV 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2208.03012) · **코드:** [https://github.com/researchmm/FTVSR](https://github.com/researchmm/FTVSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — FTVSR_REDS.pth and FTVSR_Vimeo90K.pth (x4) available on Google Drive and Baidu Cloud
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** n/a
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vid4 (evaluation); compressed with H.264 CRF 15/25/35
- **관련성 근거:** 1. 실세계/blind 열화 강인성(20/25): H.264 압축 아티팩트(CRF 15/25/35)를 명시적으로 다루는 Compressed Video SR 전용 모델. DCT 기반 주파수 도메인에서 압축 노이즈와 진짜 텍스처를 분리하는 핵심 설계. CCTV·교통 카메라의 H.264/265 압축 열화에 직접 대응 가능. 단, blind 랜덤 열화(블러+노이즈 혼합)보다는 압축 특화. 2. 배포가능성(10/25): Transformer 기반 무거운 오프라인 모델로 실시간/스트리밍 지원 언급 없음. 파라미터·FPS 정보 미공개. 경량화·causal 처리 없음. 3. 재현성/사용성(18/20): MIT 라이선스(상업적 사용 가능), 공식 GitHub 공개, REDS/Vimeo 학습 pretrained 가중치 Google Drive로 배포. 재현성·사용성 매우 우수. 4. 범용 장면 적용성(12/15): REDS·Vimeo-90K 등 자연영상으로 학습. 교통/거리 장면에 적용 가능하지만 PTZ·CCTV 특화 학습은 아님. 5. Scale/최신성 적합(8/15): x4 단일 스케일만 지원. ECCV 2022로 다소 연식 있음. 720p→1080p는 약 x1.5 배율이어서 x4 전용인 FTVSR과 직접 매칭 어려움.

#### [65점] FCA2 — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2506.11545) · **코드:** [https://github.com/handsomewzy/FCA2](https://github.com/handsomewzy/FCA2) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** no — No pretrained weights released; README instructs users to set checkpoint paths manually but provides no download links; no GitHub releases.
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** FCA2+BasicVSR: 9.2M params, 33ms/clip on Vid4; FCA2+BasicVSR++: 10.6M params, 57ms/clip; FCA2+FTVSR: 14.1M params, 111ms/clip (vs FTVSR baseline 428ms). Trained on A6000 GPU.
- **Datasets:** Vimeo90K, REDS (train); Vid4, REDS4, YouTube-compressed videos (eval); H.264 CRF 15/25/35 degradation settings
- **관련성 근거:** 1. 실세계/blind 열화 강인성(20/25): H.264 압축 아티팩트(CRF 15/25/35)를 명시적으로 처리하고 YouTube 실압축 영상에서도 평가. CCTV/교통 영상의 압축 열화에 직접 대응 가능. 다만 blind 미지 열화보다는 H.264 특화 설계임. 2. 배포가능성(16/25): BasicVSR 백본 기준 33ms/clip으로 빠르며 모듈형 설계로 기존 VSR 파이프라인에 통합 용이. 그러나 causal/online/스트리밍 처리 여부는 논문에서 명시되지 않아 PTZ 라이브 스트림 적용에 불확실성 있음. 3. 재현성/사용성(8/20): 코드가 공개되어 있으나 라이선스가 명시되지 않아 상업적 활용에 법적 불확실성 존재. Pretrained 가중치 미공개로 재현하려면 처음부터 학습해야 함. README에서 환경 설정이 복잡하다고 명시. 4. 범용 장면 적용성(12/15): 일반 자연영상 VSR 벤치마크(Vimeo90K, REDS, Vid4)에서 평가되어 교통/거리 장면에도 적용 가능. 특정 도메인 제한 없음. 5. Scale/최신성 적합(9/15): 2025년 최신 논문이며 IEEE TMM 투고작. 그러나 x4 스케일만 지원하여 720p→1080p(×1.5) 업스케일에는 직접 적용 불가; 별도 조정 필요.


### ★★ 유력 (50–64)

#### [62점] TURTLE (Learning Truncated Causal History Model for Video Restoration) — NeurIPS 2024 (2024)
- **논문:** [링크](https://arxiv.org/abs/2410.03936) · **코드:** [https://github.com/Ascend-Research/Turtle](https://github.com/Ascend-Research/Turtle) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — 전 task 학습 가중치 Google Drive 제공 (desnowing, deraining, deblurring, denoising, SR 포함): https://drive.google.com/drive/folders/1Mur4IboaNgEW5qyynTIHq8CSAGtyykrA
- **Scale:** x4 (Real-World SR: MVSR4×) · **Real-world:** ○ 실세계 · **효율:** 181.06G MACs @ 256×256 (단일 프레임 기준); 비교: RVRT 1182.16G, VRT 1631.67G. 추론 시 O(γ×h×w×c) 공간복잡도(γ≪T). Causal/온라인 추론 가능(recurrent inference). 정확한 params/FPS는 논문에 미표기(코드 실행으로 계산 가능).
- **Datasets:** RVSD (desnowing), VRDS (raindrop/rainstreak), NightRain (night deraining), GoPro (synthetic deblur), BSD3ms-24ms (real-world deblur), DAVIS/Set8 (denoising), MVSR4× (real-world SR)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(18/25): Real-World deblurring(BSD3ms-24ms), Real-World SR(MVSR4×), blind denoising 등 실제 열화 대응. 그러나 H.264/265 압축 아티팩트나 CCTV 특화 열화에 대한 직접적 학습/평가는 없음. 열화 prior 없이 unified 아키텍처로 다양한 열화 처리 가능한 점은 긍정적. 2. 배포가능성(15/25): Causal/recurrent 추론으로 온라인 스트리밍 가능. MACs가 181G로 기존 방법 대비 현저히 낮으나, 여전히 실시간 PTZ CCTV 배포에는 연산 부담. 8 GPU 학습 환경. 3. 재현성/사용성(18/20): MIT 라이선스(상업 사용 가능), 공식 PyTorch 코드 공개, 전 task 사전학습 가중치 Google Drive 제공. 재현성 매우 높음. 4. 범용 장면 적용성(11/15): 다양한 자연영상 열화(눈, 비, 블러, 노이즈, SR) 포함 일반 영상에 적용 가능. CCTV/교통 특화 도메인은 아니나 범용적. 5. Scale/최신성 적합(0/15): 지원 scale이 x4 단일이며 arbitrary-scale 미지원. 교통 PTZ 업스케일에 필요한 ×1.5~2 또는 arbitrary-scale 미지원이 큰 갭. NeurIPS 2024 최신 논문이나 scale 제약으로 감점.

#### [62점] RealVSR — ICCV 2021 (2021)
- **논문:** [링크](https://openaccess.thecvf.com/content/ICCV2021/html/Yang_Real-World_Video_Super-Resolution_A_Benchmark_Dataset_and_a_Decomposition_Based_ICCV_2021_paper.html) · **코드:** [https://github.com/IanYeung/RealVSR](https://github.com/IanYeung/RealVSR) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** yes — Google Drive 및 Baidu Drive(코드: n1n0)에 다수 모델 체크포인트 공개 (EDVR, TOF, FSTRN, TDAN 등 여러 아키텍처 포함)
- **Scale:** x2 (iPhone 11 Pro Max 듀얼 카메라 기반 실세계 paired dataset, 근사 x2 배율) · **Real-world:** ○ 실세계 · **효율:** n/a (FPS/파라미터 수 미보고. EDVR 기반 오프라인 처리. NVIDIA GPU + CUDA 필요. causal/online 미지원)
- **Datasets:** RealVSR (자체 구축 실세계 LR-HR paired dataset, iPhone 11 Pro Max 듀얼 카메라, 500+ LR-HR 쌍), Vimeo-90K (합성 bicubic 비교용)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(18/25): iPhone 듀얼 카메라로 수집한 실세계 paired 데이터로 학습—실제 광학 열화(노이즈·블러·chromatic aberration) 포함. 그러나 H.264/265 압축 열화나 CCTV 특유의 열화에 특화되지 않음. 2. 배포가능성(8/25): EDVR 기반 무거운 오프라인 모델. 실시간/스트리밍/causal 처리 미지원, FPS 미보고, 경량화 미적용. 교통 PTZ 라이브 스트림에는 직접 적용 어려움. 3. 재현성/사용성(17/20): GitHub 공개 코드 + Apache-2.0 라이선스(상업적 사용 가능) + Google Drive/Baidu Drive pretrained weights 다수 공개. 코드 완성도 양호. 4. 범용 장면 적용성(11/15): 일반 자연영상(야외·실내) 대상 실세계 VSR이므로 교통/거리 장면에도 적용 가능. iPhone 카메라 도메인에 약간 편향됨. 5. Scale/최신성 적합(8/15): x2 스케일로 목표 배율(720p→1080p)에 부합. ICCV 2021로 비교적 최신이나 현재 기준으로는 구세대 아키텍처(EDVR). 합산: 18+8+17+11+8=62.

#### [58점] DeepBlindVSR — ICCV 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2003.04716) · **코드:** [https://github.com/csbhr/Deep-Blind-VSR](https://github.com/csbhr/Deep-Blind-VSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — x4 weights on Google Drive (https://drive.google.com/drive/folders/1y_MGM6YwBZjvkhHlA0OxIFsOL_s5iSRn); models for Gaussian and Non-Gaussian degradation variants
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** n/a (no FPS/params/runtime reported in README or abstract; PyTorch 0.4.1-based, offline/non-causal recurrent CNN)
- **Datasets:** REDS (training), Gaussian_REDS4, Gaussian_Vid4, Gaussian_SPMCS, NonGaussian_REDS4, NonGaussian_REDS4_L (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(18/25): Gaussian 및 Non-Gaussian 블러 커널을 blind 추정하여 복원하는 방식으로, 단순 bicubic 전용 방법보다 훨씬 강인함. 단, H.264/265 압축 아티팩트나 CCTV 노이즈 특화 학습은 명시되지 않음. 2. 배포가능성(8/25): 오프라인 비인과 recurrent CNN 구조이며 실시간 처리 관련 지표 없음. PyTorch 0.4.1이라는 구버전 프레임워크 사용으로 현대 인프라와의 호환 어려움. 스트리밍/온라인 처리 불가. 3. 재현성/사용성(17/20): 공식 코드 공개(GitHub), pretrained 가중치 Google Drive 제공, MIT 라이선스(상업적 사용 가능). 높은 점수. 단, 구버전 PyTorch 의존성으로 실용성 약간 감점. 4. 범용 장면 적용성(10/15): REDS, Vid4 등 일반 자연영상 데이터셋으로 학습/평가하여 교통/거리 영상에도 적용 가능성 있음. 도메인 특화는 아님. 5. Scale/최신성 적합(5/15): x4 단일 scale만 지원, arbitrary-scale이나 ×1.5~2 미지원. 2021년 논문으로 최신성 감점.

#### [57점] ST-AVSR — ECCV 2024 (2024)
- **논문:** [링크](https://arxiv.org/abs/2407.09919) · **코드:** [https://github.com/shangwei5/ST-AVSR](https://github.com/shangwei5/ST-AVSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — Google Drive 및 BaiduDisk에 가중치 공개 (Google Drive: https://drive.google.com/file/d/15MiOh3rdvSAKW87-Ze1HSCSrOsn0peNK/view)
- **Scale:** arbitrary-scale (x2, x3, x4, x6, x8, x2.5, x3.5, x7.2 등 비정수/비대칭 포함; 학습 시 α,β ∈ U[1,4]) · **Real-world:** △ 합성 · **효율:** x4 기준 약 0.0495초/프레임 (NVIDIA RTX A6000), "nearly real-time" 언급; 파라미터 수 미공개; 비인과(non-causal) 오프라인 처리 (미래 프레임 L=2 참조)
- **Datasets:** REDS (학습/검증), Vid4 (평가); 일반화 테스트 시 노이즈+비디오 압축 열화 추가
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0~25): 학습은 bicubic 단독이며, 논문에서 노이즈+압축 열화 일반화 실험을 보조적으로 수행했지만 H.264/265 CCTV 실열화에 특화된 설계는 없음 → 8점. 2. 배포가능성(0~25): x4에서 ~0.05초/프레임으로 거의 실시간 수준이나, 미래 프레임 2장을 참조하는 non-causal 구조라 라이브 스트리밍 PTZ에는 지연 발생; 고사양 GPU(A6000 48GB) 기반 → 12점. 3. 재현성/사용성(0~20): MIT 라이선스, 코드 공개, pretrained 가중치 공개(Google Drive/BaiduDisk) → 18점. 4. 범용 장면 적용성(0~15): REDS(자연영상 거리/야외 등) 학습, 도메인 특화 아님 → 12점. 5. Scale/최신성 적합(0~15): arbitrary-scale 지원(×1~4, 비정수/비대칭 포함), ECCV 2024 최신 고성능 방법 → 7점(×1.5~2 범위 지원하나 학습 범위가 ×1~4로 매핑되어 ×1.5 정확도 검증 미흡, 합산 57점).

#### [55점] BasicAVSR — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2510.26149) · **코드:** [https://github.com/shangwei5/BasicAVSR](https://github.com/shangwei5/BasicAVSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — Google Drive에 사전학습 가중치 공개 (bidirectional RNN, unidirectional RNN, unidirectional with lookahead 세 variant): https://drive.google.com/drive/folders/14DzE4PYXNnbcHDq7vMy5imamWMr1auum
- **Scale:** arbitrary-scale (x1–x4 학습, x2/x3/x4/x6/x8 및 비정수 스케일 평가) · **Real-world:** △ 합성 · **효율:** 6.2M params; 331.2 GFLOPs (x4); bidirectional RNN: 0.116s/frame, unidirectional RNN: 0.050s/frame (RTX A6000 GPU)
- **Datasets:** REDS (학습/평가), Vid4 (평가), GoPro (비학습 degradation 평가)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (10/25): 주요 학습은 bicubic/합성 degradation(REDS sharp) 기반이며, GoPro 비학습 실험으로 일부 강인성 시연. 그러나 H.264/265 압축 artifact, CCTV 특화 열화 대응 학습은 명시적으로 수행하지 않음 → 부분 점수. 2. 배포가능성 (15/25): 단방향 RNN(0.050s/frame)으로 온라인·스트리밍 추론 지원, causal 모드 존재, 파라미터 6.2M으로 경량. 그러나 RTX A6000 기준이며 엣지/임베디드 배포 검증은 없음 → 양호. 3. 재현성/사용성 (18/20): 코드 공개(PyTorch), MIT 라이선스(상업적 사용 가능), Google Drive에 세 variant 사전학습 가중치 모두 공개 → 거의 만점. 4. 범용 장면 적용성 (10/15): REDS·Vid4 등 일반 자연영상으로 학습/평가, 특정 도메인 특화 없음. 교통·거리 장면에도 적용 가능하나 해당 도메인 검증 없음. 5. Scale/최신성 적합 (7/15): arbitrary-scale 지원(x1.5–2 포함), 2025년 최신, PSNR +1.7dB 성능. 그러나 학습 범위 U[1,4]로 PTZ 목표 배율 x1.5에는 적합하나 x6 이상 고배율 focus 아님.

#### [54점] EAVSR — arXiv 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2212.05342) · **코드:** [https://github.com/HITRainer/EAVSR](https://github.com/HITRainer/EAVSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** no — GitHub 저장소에 ckpt 폴더 구조는 존재하나, 공개 다운로드 링크(Google Drive/Baidu 등) 없음. 데이터셋(MVSR4×)만 Baidu Pan 링크 제공.
- **Scale:** x2, x4 · **Real-world:** ○ 실세계 · **효율:** n/a (params/FPS/runtime 미보고)
- **Datasets:** MVSR4× (신규 제안, 스마트폰 듀얼카메라 실세계 4× 데이터셋), RealVSR
- **관련성 근거:** 1. 실세계/blind 열화 강인성(18/25): 스마트폰 듀얼카메라 실세계 데이터셋(MVSR4×)과 RealVSR로 학습·평가하며 실제 광학 열화 대응. 단, H.264/265 압축이나 CCTV 특유 열화에 대한 명시적 처리 없음. 2. 배포가능성(7/25): FPS/params 미보고, 오프라인 전용(non-causal bidirectional propagation), 경량화 언급 없음. PTZ 라이브 스트리밍 부적합. 3. 재현성/사용성(12/20): MIT 라이선스 코드 공개, 훈련/추론 스크립트 제공. 그러나 사전 학습 가중치 다운로드 링크 없어 즉시 사용 불가. 4. 범용 장면 적용성(10/15): 스마트폰 자연 영상 대상, 교통·거리 장면에 일반 적용 가능성 있음. 특정 도메인 특화 아님. 5. Scale/최신성 적합(7/15): x4 지원(교통 1080p 업스케일에 부합), CVPRW 2023 발표로 준최신. arbitrary-scale 미지원. 합산: 54/100.

#### [52점] DualX-VSR — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2506.04830) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — 논문에서 "코드와 사전학습 모델(MSE/GAN 모두)을 공개할 예정"이라고 명시했으나, 현재(2026-06-10) 공개된 리포지토리 없음
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** 127.95M params (MSE version); 0.40s/frame (320×180→1280×720); 99.41G MACs (16 frames of 64×64)
- **Datasets:** Training: REDS (RealBasicVSR 파이프라인); Eval: REDS4, UDM10, SPMC30 (synthetic), VideoLQ (real-world)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (18/25): random blur, resize, noise, JPEG 압축 + 동영상 압축(H.264 계열) 등 실제 CCTV 열화에 대응하는 first-order 열화 모델 사용. VideoLQ 실세계 벤치마크 평가 포함. 단, second-order 열화 미적용으로 최상위 점수는 아님. 2. 배포가능성 (8/25): 0.40s/프레임(약 2.5 FPS)으로 실시간 불가. 127.95M 파라미터로 무거운 편. Motion compensation 제거로 구조는 단순하나 연산량(99.41G MACs)이 높아 PTZ 라이브 배포에 부적합. 3. 재현성/사용성 (5/20): 코드 미공개(공개 예정만 명시), pretrained 가중치 없음. 현재 재현 불가. 4. 범용 장면 적용성 (13/15): REDS 학습 데이터는 다양한 자연 영상 포함. 거리/교통 장면에 일반적으로 적용 가능하며 도메인 특화 아님. 5. Scale/최신성 적합 (8/15): x4 단일 스케일만 지원(720p→1080p에는 x1.5 필요). 2025년 최신 논문이나 arbitrary-scale 미지원.

#### [52점] FMA-Net++ — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2512.04390) · **코드:** [https://github.com/KAIST-VICLab/FMA-Net-PlusPlus](https://github.com/KAIST-VICLab/FMA-Net-PlusPlus) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** no — Repository states "The full code and pretrained models will be released soon." — no weights available as of repo creation (Dec 4, 2025).
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** 12.8M params; 0.074s per LR frame at 180×320 resolution; over 5.2× speedup vs. recurrent approach (RVRT); offline/batch processing on 4× NVIDIA A6000 GPUs
- **Datasets:** REDS-ME (introduced, multi-exposure), REDS-RE (introduced, random-exposure), REDS4, GoPro
- **관련성 근거:** 1. 실세계/blind 열화 강인성(16/25): 합성 데이터(REDS 기반)로만 학습하지만, 동적 노출 변화·모션 블러 등 실제 환경 열화를 명시적으로 모델링함. H.264/265 압축 노이즈 등 전형적인 CCTV 열화는 직접 다루지 않고, 모션+노출 커플링에 특화됨. 실세계 Samsung Galaxy S23+ 영상으로 정성 평가는 수행. 2. 배포가능성(10/25): x4 전용, 오프라인 양방향 전파(bidirectional propagation) 구조로 온라인/실시간 스트리밍에 부적합. 0.074s/프레임은 약 13FPS 수준이나, 교통 CCTV 라이브 처리를 위한 causal 구조가 아님. 3. 재현성/사용성(6/20): GitHub 리포는 존재하나 코드 미공개(inference/train/weights 모두 "released soon" 상태). 라이선스 미지정. 현시점에서 실사용 불가. 4. 범용 장면 적용성(12/15): GoPro 데이터셋으로 일반화 성능을 검증하고, 자연영상 전반에 적용 가능한 범용 프레임워크. 교통·거리 장면에 별도 제약 없음. 5. Scale/최신성 적합(8/15): x4 단일 스케일로 720p→1080p 업스케일에는 적합하나 arbitrary-scale 미지원. 2025년 arXiv 최신 논문으로 성능은 SOTA 수준.

#### [52점] BasicVSR / IconVSR — CVPR 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2012.02181) · **코드:** [https://github.com/ckkelvinchan/BasicVSR-IconVSR](https://github.com/ckkelvinchan/BasicVSR-IconVSR) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** yes — x4 weights for REDS and Vimeo-90K (BI/BD variants) publicly downloadable via OpenMMLab CDN (https://download.openmmlab.com/mmediting/restorers/basicvsr/); also available in MMEditing/MMagic model zoo
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** ~6.3M params (incl. SPyNet optical flow); ~63ms per frame reported; offline bidirectional recurrent — not causal/online
- **Datasets:** REDS, Vimeo-90K (training); REDS4, UDM10, Vimeo-90K-T, Vid4 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (10/25): 합성 bicubic(BI) 및 blur+downsampling(BD) 열화만 학습. H.264/265 압축 노이즈 등 실제 CCTV 열화는 대응하지 않음. RealBasicVSR이 별도 후속작으로 존재. 2. 배포가능성 (10/25): 약 6.3M 파라미터로 경량이나, 양방향(bidirectional) 순환 구조로 미래 프레임이 필요한 오프라인 방식. 실시간/라이브 스트리밍 PTZ 환경에 직접 적용 불가. ~63ms/frame이므로 속도 자체는 허용 범위나 causal 미지원이 치명적. 3. 재현성/사용성 (18/20): 공식 GitHub 코드 공개(Apache-2.0, 상업적 사용 가능), MMEditing 통합, 다수 pretrained 가중치(REDS/Vimeo90K 변종) 공개. 재현성 매우 우수. 4. 범용 장면 적용성 (9/15): 일반 자연영상 대상으로 도메인 특화 없음. 교통/거리 영상에 원칙적 적용 가능하나 실세계 열화 미대응이 현장 성능 저하 요인. 5. Scale/최신성 적합 (5/15): x4 단일 스케일만 지원(720p→1080p는 x1.5 업스케일이므로 스케일 불일치), arbitrary-scale 미지원, 2021년 논문으로 최신성 다소 부족. 합계 52점.

#### [52점] OVSR (Omniscient Video Super-Resolution) — ICCV 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2103.15683) · **코드:** [https://github.com/psychopa4/OVSR](https://github.com/psychopa4/OVSR) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** yes — Pretrained weights available via Baidu Pan (mainland China) and TeraBox (other regions); multiple variants (GOVSR-8+4-80, etc.)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** Light model OVSR-4+2-56 capable of real-time 4x VSR for 720p. Medium OVSR-8+4-56 ~1.897M params, ~109.7 GFLOPs per 1280x720 frame. Heavy OVSR-8+4-80 has more filters (80) but slower. Exact FPS values not extracted from tables.
- **Datasets:** Training: MM522, Vimeo-90K; Eval: 20 sequences from PFNL; Test: Vid4, UDM10, Vimeo-90K-T
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (8/25): 학습 시 Gaussian blur(σ=1.6) + 4× bicubic 다운샘플링 방식만 사용하며, H.264/265 압축 아티팩트·CCTV 노이즈 등 실제 열화는 전혀 다루지 않음. 합성 열화 전용이므로 낮게 채점. 2. 배포가능성 (14/25): 경량 모델(OVSR-4+2-56)이 720p 실시간 4× VSR 가능하다고 명시되어 배포 잠재력은 있음. 단, Local Omniscient(causal) 변형은 라이브 스트리밍에 적합하나 Global 변형은 미래 프레임 필요 → PTZ 라이브에는 LOVSR만 적용 가능. 3. 재현성/사용성 (16/20): Apache-2.0 라이선스(상업 사용 허용), 공식 GitHub 코드 공개, pretrained 가중치 Baidu/TeraBox 제공. 사용성 양호. 4. 범용 장면 적용성 (9/15): MM522, Vimeo-90K 등 일반 자연영상 데이터셋으로 학습되어 교통·거리 장면에도 적용 가능. 도메인 특화 없음. 5. Scale/최신성 적합 (5/15): x4 단일 scale 지원, arbitrary-scale 없음. 1.5×~2× 업스케일에는 미지원. ICCV 2021로 다소 구형. 합산: 8+14+16+9+5 = 52점.

#### [52점] Deep Blind Video Super-resolution (Deep-Blind-VSR) — arXiv 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2003.04716) · **코드:** [https://github.com/csbhr/Deep-Blind-VSR](https://github.com/csbhr/Deep-Blind-VSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — x4 모델 가중치(kernel_x4.pt 포함)를 Google Drive에서 공개 제공 (https://drive.google.com/drive/folders/1y_MGM6YwBZjvkhHlA0OxIFsOL_s5iSRn)
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** n/a (FPS/params/runtime 미공개)
- **Datasets:** REDS (train), REDS4, Vid4, SPMCS (eval) — Gaussian 및 Non-Gaussian blur kernel 조건 모두 평가
- **관련성 근거:** 1. 실세계/blind 열화 강인성(16/25): 합성 bicubic이 아닌 motion blur kernel을 명시적으로 추정하는 blind VSR로 실제 열화에 일부 대응. 단, H.264/265 압축 노이즈나 CCTV 특화 열화는 직접 다루지 않고 Gaussian/Non-Gaussian blur 위주. 2. 배포가능성(8/25): x4 단일 스케일, offline 배치 처리 구조, 광류 추정(PWC-Net) 등 무거운 파이프라인으로 실시간/스트리밍 적용 어려움. FPS/latency 정보 없음. 3. 재현성/사용성(16/20): MIT 라이선스, 공식 GitHub 코드 공개, Google Drive 사전학습 가중치 제공으로 재현성 높음. 4. 범용 장면 적용성(8/15): 일반 자연영상(REDS, Vid4) 대상으로 범용성 있으나 교통/CCTV 특화 실험 없음. 5. Scale/최신성 적합(4/15): x4 단일 스케일만 지원(교통 PTZ 720p→1080p의 x1.5 업스케일에 직접 적합하지 않음), 2021년 발표로 최신 방법 대비 성능 열위 가능성.

#### [51점] EDVR — CVPRW 2019 (2019)
- **논문:** [링크](https://arxiv.org/abs/1905.02716) · **코드:** [https://github.com/xinntao/EDVR](https://github.com/xinntao/EDVR) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** yes — x4 (REDS, Vimeo-90K) and x2 variants available on Google Drive and Baidu Cloud; ModelZoo lists EDVR_L and EDVR_M variants
- **Scale:** x4 (primary); x2 also available · **Real-world:** △ 합성 · **효율:** ~20.6M params; 640–939G FLOPs at 1280×720 HR resolution; offline/non-causal, no real-time capability reported
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vid4, Vimeo-90K-T (evaluation)
- **관련성 근거:** 1) 실세계/blind 열화 강인성(8/25): 주로 bicubic 합성 열화로 학습됨. NTIRE19 competition에서 blur/compression 트랙도 있었으나 CCTV/H.264 압축 특화 학습은 아님. 2) 배포가능성(8/25): ~20.6M 파라미터에 1280×720 기준 640~939G FLOPs로 매우 무거움. 오프라인 비인과적(offline/non-causal) 구조로 실시간 PTZ 라이브 스트리밍에 부적합. 3) 재현성/사용성(18/20): Apache 2.0 라이선스(상업적 사용 가능), 코드 공개(BasicSR에 통합), pretrained weights 공개(Google Drive/Baidu). 재현성 매우 높음. 4) 범용 장면 적용성(12/15): REDS, Vimeo-90K 등 다양한 자연 영상으로 학습되어 교통/거리 장면에도 일반적으로 적용 가능. 도메인 특화 아님. 5) Scale/최신성 적합(5/15): x4가 주요 스케일로 1080p→720p 업스케일에는 적합하나 ×1.5나 arbitrary-scale 미지원. 2019년 논문으로 최신성 다소 부족.


### ★ 후보 (40–49)

#### [48점] VSR-Transformer — arXiv 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2106.06847) · **코드:** [https://github.com/caojiezhang/VSR-Transformer](https://github.com/caojiezhang/VSR-Transformer) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** yes — x4 REDS and Vimeo-90K weights on Google Drive (https://drive.google.com/drive/folders/1HFZbuYq54U9mz_ngAqfW3pRMcry7XWx3)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** ~32.6–43.8M params (config-dependent); FPS/runtime not reported; trained on 8x NVIDIA TITAN RTX GPUs; offline/non-causal (bidirectional optical flow)
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vimeo-90K-T, Vid4 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): bicubic 다운샘플링만으로 학습하며 H.264/265 압축·CCTV 노이즈 등 실제 열화 대응 연구가 전혀 없음. 교통 PTZ/CCTV 환경에 직접 적용 시 성능 저하 예상. 점수: 4/25. 2. 배포가능성(7/25): 양방향 광류(bidirectional optical flow) 기반 피드포워드 레이어를 사용하므로 오프라인·비인과적 구조임. 32M+ 파라미터로 무거우며 FPS/실시간 정보 없음. 라이브 PTZ 스트리밍에 부적합. 점수: 7/25. 3. 재현성/사용성(16/20): GitHub 공식 코드 공개, Apache-2.0 라이선스(상업적 사용 허용), Google Drive pretrained 가중치(REDS/Vimeo-90K x4) 제공. 재현성은 우수함. 점수: 16/20. 4. 범용 장면 적용성(12/15): REDS·Vimeo-90K 등 일반 자연영상 데이터셋으로 학습되어 도메인 특화 없이 교통 영상에도 원칙적으로 적용 가능. 점수: 12/15. 5. Scale/최신성 적합(9/15): x4 고정 스케일만 지원하여 720p→1080p(약 x1.5) 용도에는 scale 불일치. 2021년 발표로 최신은 아니지만 Transformer 기반 VSR의 선구적 연구임. 점수: 9/15. 총합: 48/100.

#### [47점] BVSR-IK — ICCV 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2503.07856) · **코드:** [https://github.com/QZ1-boy/BVSR-IK](https://github.com/QZ1-boy/BVSR-IK) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — x4 weights for Gaussian and Realistic Motion Blur models on Baidu Cloud (pan.baidu.com)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 9.30M params; 459.23G FLOPs @64×64 LR; 1.58s per 5-frame sequence on RTX 3090 (offline, non-causal)
- **Datasets:** REDS (train), REDS4, Vid4, UDM10 (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (14/25): blind VSR로 spatially-varying 블러와 모션 블러를 다루나, 학습은 Gaussian blur + synthetic motion blur로 제한됨. H.264/265 압축 열화나 실제 CCTV 노이즈는 직접 다루지 않으며, 실제 열화 데이터셋(Real-world)으로의 평가 없음. 반합성(semi-blind) 수준으로 평가. 2. 배포가능성 (8/25): 5프레임당 1.58초(RTX 3090)로 실시간 불가. recurrent Transformer 구조로 오프라인 전용. PTZ CCTV 라이브 스트리밍에는 부적합. 3. 재현성/사용성 (12/20): 코드 공개 + pretrained 가중치 공개(Baidu Cloud). 그러나 LICENSE 파일 없어 라이선스 불명확(상업적 사용 불확실). Baidu Cloud는 해외 접근성 제한. 4. 범용 장면 적용성 (9/15): 교통/감시 등 일반 자연 영상(REDS, Vid4, UDM10)으로 학습/평가되어 범용성 있음. 도메인 특화 아님. 5. Scale/최신성 적합 (4/15): x4 단일 스케일만 지원. 720p→1080p(x1.5)나 arbitrary-scale 불가. ICCV 2025 최신 논문이나 스케일 제약이 큼.

#### [47점] MIA-VSR — arXiv 2024 (2024)
- **논문:** [링크](https://arxiv.org/abs/2401.06312) · **코드:** [https://github.com/LabShuHangGU/MIA-VSR](https://github.com/LabShuHangGU/MIA-VSR) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** yes — REDS-trained and Vimeo-trained weights on Google Drive (https://drive.google.com/drive/folders/1DvsUP-FVwENIpLyeQPQMHc7QnxX1F5Pd)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 16.5M params (full); small 6.3M, tiny 4.8M. ~822ms/frame on RTX 4090 at 720×1280 output. 1.61T FLOPs. ~40% less compute than RVRT/PSRT. Offline/non-causal.
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vid4, Vimeo-90K-T (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0~25): 8/25 — bicubic 다운샘플링 기반 합성 열화만 사용하며, H.264/265 압축 아티팩트·CCTV 노이즈 등 실제 열화에 대한 처리 없음. 표준 벤치마크(REDS, Vimeo) 위주 학습·평가로 실세계 적용 시 성능 저하 가능성 높음. 2. 배포가능성 (0~25): 10/25 — 822ms/프레임(RTX 4090 기준)으로 실시간 처리 불가. 오프라인·비인과적(non-causal) 방식이며, 라이브 PTZ/CCTV 스트리밍에는 부적합. 효율 개선(40% FLOPs 절감)이 있으나 절대 속도는 배포에 무리. 3. 재현성/사용성 (0~20): 17/20 — 코드 공개(GitHub), Apache-2.0 라이선스(상업적 사용 허용), pretrained 가중치(Google Drive) 모두 공개. 재현성·사용성 우수. 4. 범용 장면 적용성 (0~15): 7/15 — 일반 자연영상(REDS, Vimeo) 기반으로 특정 도메인 특화는 아니지만, 교통/CCTV 데이터로 파인튜닝 없이 직접 적용 시 성능 보장 어려움. 5. Scale/최신성 적합 (0~15): 5/15 — x4 단일 스케일만 지원. 720p→1080p(x1.5) 업스케일에는 직접 대응 불가이며 arbitrary-scale 미지원. CVPR 2024로 최신성은 양호하나 스케일 미스매치가 핵심 약점.

#### [47점] VRT (Video Restoration Transformer) — arXiv 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2201.12288) · **코드:** [https://github.com/JingyunLiang/VRT](https://github.com/JingyunLiang/VRT) · **License:** CC BY-NC 4.0 (non-commercial) · **Framework:** PyTorch
- **Pretrained:** yes — GitHub Releases에서 공개: video SR (bi/bd, REDS/Vimeo), video deblurring (DVD/GoPro/REDS), video denoising (DAVIS), frame interpolation 등 총 9개 이상의 .pth 가중치 파일 자동 다운로드 지원
- **Scale:** x4 (주), x4 BD (blur-downsampling) · **Real-world:** △ 합성 · **효율:** 30.7M–35.6M params; video SR ~236ms/frame, video deblurring ~2.2s/frame at 1280×720; offline batch 처리 전용 (non-causal, non-real-time)
- **Datasets:** REDS, Vimeo-90K, Vid4, UDM10 (video SR); DVD, GoPro, REDS (deblurring); DAVIS-2017, Set8 (denoising); UCF101 (frame interpolation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (8/25): 합성 열화(bicubic BI, blur-downsampling BD) 위주로 학습·평가됨. H.264/265 압축 아티팩트나 실제 CCTV 열화에 대한 blind 복원 실험 없음. BD 모델이 일부 블러를 다루지만 실세계 압축 노이즈 대응은 미흡. 2. 배포가능성 (7/25): video SR에서 ~236ms/frame(약 4 FPS 미만), 1280×720 deblurring은 2.2s/frame. 비인과적(non-causal) 오프라인 처리 구조로 PTZ 라이브 스트리밍 적용 불가. 모델 크기 30–35M로 중간 수준이나 속도가 실시간과 거리가 멈. 3. 재현성/사용성 (13/20): 코드 공개(GitHub) + pretrained 가중치 공개(yes)로 재현성은 우수. 단, CC BY-NC 4.0 라이선스로 상업적 활용 불가하여 실 배포에 법적 제약 존재. 코드 품질 및 문서화는 양호. 4. 범용 장면 적용성 (12/15): REDS, Vimeo-90K, DAVIS 등 자연영상 일반 데이터셋 사용. 교통/거리 장면에 특화되진 않았지만 일반 자연영상에 광범위 적용 가능. 도메인 편향 낮음. 5. Scale/최신성 적합 (7/15): x4 단일 스케일 위주. 720p→1080p(x1.5)나 arbitrary-scale은 미지원. 2022년 발표로 상당히 최신이며 당시 SOTA이나, 목표 스케일(x1.5)과 불일치.

#### [47점] PP-MSVSR / PP-MSVSR-L — arXiv 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2112.02828) · **코드:** [https://github.com/PaddlePaddle/PaddleGAN](https://github.com/PaddlePaddle/PaddleGAN) · **License:** Apache-2.0 · **Framework:** PaddlePaddle (PaddleGAN)
- **Pretrained:** yes — 3 variants publicly downloadable on paddlegan.bj.bcebos.com: PP-MSVSR_reds_x4 (1.45M), PP-MSVSR-L_reds_x4 (7.42M), PP-MSVSR_vimeo90k_x4 (.pdparams format)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** PP-MSVSR: 1.45M params, 111G FLOPs; PP-MSVSR-L: 7.42M params, 543G FLOPs. Recurrent (bidirectional propagation) architecture — offline/non-causal by default.
- **Datasets:** REDS, Vimeo90K (training); REDS4, Vid4, UDM10 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): 학습 열화가 bicubic 다운샘플링(REDS, Vimeo90K 기반 합성 LR) 전용이며, H.264/265 압축·CCTV 노이즈 등 실세계 열화를 명시적으로 다루지 않음. 교통 CCTV 적용 시 도메인 갭 큼. → 5점. 2. 배포가능성(10/25): PP-MSVSR은 1.45M 파라미터, 111G FLOPs로 경량이나, 양방향 recurrent 구조라 온라인/인과(causal) 스트리밍에 적합하지 않고 오프라인 배치 처리 전용. → 10점. 3. 재현성/사용성(18/20): 코드 공개(Apache-2.0 관대한 상용 가능 라이선스), pretrained 가중치 3종 공개(paddlegan.bj.bcebos.com). PaddlePaddle 기반이라 PyTorch 생태계 대비 사용성 약간 낮음. → 18점. 4. 범용 장면 적용성(9/15): REDS, Vimeo90K 등 자연영상 일반 데이터로 학습되어 교통/거리 장면에도 원칙적으로 적용 가능. 도메인 특화는 아니지만 bicubic 가정으로 실제 CCTV 영상엔 성능 저하 예상. → 9점. 5. Scale/최신성 적합(5/15): x4 단일 스케일만 지원(720p→1440p이므로 목표인 720p→1080p 즉 ×1.5에는 직접 미매칭), arbitrary-scale 미지원. 2021년 논문으로 최신성 보통. → 5점. 합산: 47점.

#### [47점] RRN (Recurrent Residual Network) — BMVC 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2008.05765) · **코드:** [https://github.com/junpan19/RRN](https://github.com/junpan19/RRN) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — RRN-5L.pth and RRN-10L.pth included directly in the GitHub repo (each ~13MB, x4 scale)
- **Scale:** x2/x3/x4 (x4 primary; code supports all three via Gaussian downsampling) · **Real-world:** △ 합성 · **효율:** Recurrent/online-causal architecture (frame-by-frame with hidden state propagation); 4× GTX-1080Ti for training, 1× for inference; no explicit FPS/params reported in README. Model file ~13MB (~3.25M params estimated).
- **Datasets:** Vimeo-90K (training), Vid4 (test), UDM10 (test), SPMC_test (test)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (7/25): Gaussian 다운샘플링 기반 합성 열화만 사용. H.264/265 압축, CCTV 노이즈 등 실제 열화는 전혀 다루지 않아 교통/PTZ 환경에 직접 적용 시 성능 저하 예상. 2. 배포가능성 (14/25): 재귀(recurrent) 구조로 프레임별 순차 처리(causal/online) 가능하여 스트리밍에 적합. 그러나 실시간 벤치마크 수치 미제공, 구버전 PyTorch 1.1 기반. 3. 재현성/사용성 (12/20): 공식 코드 공개 및 pretrained 가중치(x4) 포함. 단 LICENSE 파일 없어 상업적 사용 법적 불명확. 코드 자체는 간결하고 재현 가능. 4. 범용 장면 적용성 (9/15): Vimeo-90K(자연 영상)로 학습하여 교통·거리 장면 포함 일반 영상에 적용 가능. 도메인 특화 아님. 5. Scale/최신성 적합 (5/15): x4 지원(1080p 목표엔 ×1.5 부족), 2020년 발표로 최신 기법 대비 성능 열위 예상. arbitrary-scale 미지원. 합산: 7+14+12+9+5=47.

#### [46점] TTVSR (Trajectory-aware Transformer for Video Super-Resolution) — CVPR 2022 oral (2022)
- **논문:** [링크](https://arxiv.org/abs/2204.04216) · **코드:** [https://github.com/researchmm/TTVSR](https://github.com/researchmm/TTVSR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — TTVSR_REDS.pth 및 TTVSR_Vimeo90K.pth 제공 (Google Drive / OneDrive / Baidu Cloud)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 파라미터 수/FPS 미공개. 오프라인(비인과) 방식으로 전체 비디오 시퀀스 처리. mid_channels=64, num_blocks=60.
- **Datasets:** REDS, Vimeo-90K (학습); REDS4, Vid4, UDM10, Vimeo-90K-T (평가)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): BI(bicubic)와 BD(blur-downsampling) 두 가지 합성 열화만 사용. H.264/265 압축, 실제 CCTV 노이즈 등 실세계 열화 대응 실험 없음. 점수: 5/25. 2. 배포가능성 (5/25): 오프라인(비인과) transformer로 전체 시퀀스에 trajectory attention 수행. 실시간/스트리밍/PTZ 라이브 적용 어려움. 점수: 5/25. 3. 재현성/사용성 (17/20): MIT 라이선스로 상업 사용 가능. 공식 코드 공개 + pretrained weights(x4 REDS/Vimeo90K) 제공. 점수: 17/20. 4. 범용 장면 적용성 (12/15): 일반 자연영상(REDS, Vimeo-90K) 학습으로 교통/거리 영상에도 적용 가능한 범용 모델. 점수: 12/15. 5. Scale/최신성 적합 (7/15): x4만 지원(x1.5~x2, arbitrary-scale 미지원). 2022년 CVPR oral로 당시 SOTA급이나 목표 배율(720p→1080p ≈ x1.5)과 불일치. 점수: 7/15. 합계: 46/100.

#### [45점] RBPN (Recurrent Back-Projection Network) — CVPR 2019 (2019)
- **논문:** [링크](https://arxiv.org/abs/1903.10128) · **코드:** [https://github.com/alterzero/RBPN-PyTorch](https://github.com/alterzero/RBPN-PyTorch) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — x2 및 x4 가중치 Google Drive 공개 (https://drive.google.com/drive/folders/1sI41DH5TUNBKkxRJ-_w5rUf90rN97UFn)
- **Scale:** x2, x4, x8 · **Real-world:** △ 합성 · **효율:** n/a (FPS/runtime 미보고; 광학흐름(Pyflow) 기반 recurrent 구조로 연산량 많음)
- **Datasets:** Vimeo-90K (학습), Vid4, SPMCS (평가)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(5/25): bicubic 다운샘플링 합성 열화만 사용; H.264/265 압축, CCTV 노이즈·블러 등 실제 열화 대응 학습 없음. 2. 배포가능성(7/25): recurrent 구조이나 offline 배치 처리 방식이며 광학흐름(Pyflow) 연산 포함으로 실시간/스트리밍 배포에 부적합; online·causal 지원 없음. 3. 재현성/사용성(16/20): 공식 PyTorch 코드 공개, MIT 라이선스(상업적 사용 가능), Google Drive에 pretrained 가중치(x2, x4) 제공으로 재현성 높음. 4. 범용 장면 적용성(10/15): 일반 자연 영상 대상으로 도메인 특화 없음; 교통/거리 장면에 적용 가능하나 실세계 열화 미처리. 5. Scale/최신성(7/15): x4 지원하나 2019년 논문으로 비교적 구형; arbitrary-scale 미지원, 최신 VSR 대비 성능 갭 존재.

#### [44점] TS-Mamba — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2508.10453) · **코드:** [https://github.com/QZ1-boy/TS-Mamba](https://github.com/QZ1-boy/TS-Mamba) · **License:** Apache-2.0 · **Framework:** PyTorch
- **Pretrained:** no — README의 체크포인트 링크가 /xxx 플레이스홀더로 비어 있음 — 가중치 미공개
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 3.0M params, 112G MACs (180×320 LR), 29ms runtime, 33.5 FPS (online/causal)
- **Datasets:** REDS, Vimeo-90K (train); REDS4, Vid4, Vimeo-90K-T (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0~25): 10점 — BI(bicubic)와 BD(Gaussian blur) 두 가지 합성 열화만 다루며, H.264/265 압축 아티팩트나 CCTV 실제 열화에 대한 언급 없음. 실세계 blind 대응은 없음. 2. 배포가능성(0~25): 17점 — online/causal 구조이며 33.5 FPS(29ms), 3.0M 파라미터로 경량. 실시간 스트리밍에 적합한 편이나 CUDA 커스텀 커널 의존성(mamba-ssm, causal_conv1d) 있어 엣지 배포는 추가 작업 필요. 3. 재현성/사용성(0~20): 8점 — 코드는 Apache-2.0으로 공개되어 있으나 pretrained 가중치가 미공개(플레이스홀더). 직접 사용을 위해 처음부터 학습해야 함. 4. 범용 장면 적용성(0~15): 9점 — 일반 자연영상 벤치마크(REDS, Vid4)에서 평가되어 범용성은 있으나, 교통/CCTV 특화 실험 없음. 5. Scale/최신성 적합(0~15): 0점 — x4 단일 scale만 지원하며 목표인 720p→1080p(×1.5) 또는 arbitrary-scale 미지원. ICLR 2026으로 최신이나 scale 불일치로 감점.

#### [42점] BF-STVSR — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2501.11043) · **코드:** [https://github.com/LAIT-CVLab/BF-STVSR](https://github.com/LAIT-CVLab/BF-STVSR) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — x4 spatial / x8 temporal GoPro & Adobe weights on Google Drive (https://drive.google.com/drive/folders/1s8ciKJ4ccnumqaVw8_zvHvTh7TuULF2F)
- **Scale:** spatial x2–x4 (arbitrary continuous), temporal arbitrary (x2/x4/x6/x8/x12/x16 tested) · **Real-world:** △ 합성 · **효율:** 13.47M params; claims lowest computational cost and fastest inference among C-STVSR methods via custom CUDA kernel for B-spline evaluation; offline/non-causal
- **Datasets:** Adobe240 (train), GoPro, Vid4, Adobe240 (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (5/25): 학습 및 평가 모두 bicubic 다운샘플링만 사용. H.264/265 압축 아티팩트, CCTV 노이즈 등 실제 열화에 대한 대응이 전혀 없음. 2. 배포가능성 (8/25): 13.47M 파라미터로 상대적으로 경량이고 커스텀 CUDA 커널로 빠른 추론을 주장하나, offline/non-causal 구조로 실시간 스트리밍 PTZ 배포에 부적합. 9프레임 입력(1번·9번 참조프레임) 방식으로 라이브 스트림 적용 어려움. 3. 재현성/사용성 (12/20): 코드 공개(GitHub)되어 있고 pretrained 가중치(Google Drive)도 제공되나 라이선스 미지정으로 상업적 사용 불명확. 환경 설정이 복잡(DCNv2, CUDA 커스텀 빌드 필요). 4. 범용 장면 적용성 (12/15): 일반 자연영상(Adobe240, GoPro, Vid4) 기반 학습으로 교통/거리 장면에도 원칙적으로 적용 가능. 도메인 특화는 아님. 5. Scale/최신성 적합 (5/15): 공간 arbitrary-scale(x2–x4)과 시간 arbitrary 지원으로 scale 적합성은 양호. CVPR 2025 최신 논문. 그러나 교통 CCTV 720p→1080p 목표에서 가장 중요한 실세계 열화 대응과 배포가능성이 모두 취약하여 실용적 활용에는 상당한 추가 작업 필요.

#### [42점] IART (Implicit Resampling-based Alignment for Video Super-Resolution) — arXiv 2023 (2023)
- **논문:** [링크](https://arxiv.org/abs/2305.00163) · **코드:** [https://github.com/kai422/IART](https://github.com/kai422/IART) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — Google Drive에 공개 (https://drive.google.com/drive/folders/1MIUK37Izc4IcA_a3eSH-21EXOZO5G5qU). REDS(BI, N6/N16), Vimeo-90K(BI N14, BD) 다수 변형 모델 포함.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 논문 업데이트(2024.01)에서 speed comparison 추가 언급. 정확한 FPS/params 수치는 공개된 abstract 및 README에서 확인 불가. "minimal impact on both compute and parameters"라고 서술됨. 오프라인 배치 처리(N6/N16 프레임 윈도우), 실시간/causal 처리 미지원.
- **Datasets:** REDS, Vimeo-90K (학습); REDS4, Vid4, Vimeo-90K-T, UDM10 (평가)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (8/25): BD(blur+downsampling) 열화 모델도 지원하나, 주 학습은 BI(bicubic) 기반. H.264/265 압축 아티팩트나 CCTV 특화 열화는 다루지 않음. 실세계 적용성 제한적. 2. 배포가능성 (7/25): N6/N16 프레임 윈도우 기반 오프라인 처리로 실시간/스트리밍 불가. PTZ 라이브 CCTV에 직접 배포 어려움. compute overhead는 작다고 서술되나 causal/online 모드 없음. 3. 재현성/사용성 (17/20): 코드 완전 공개(MIT 라이선스), pretrained 가중치 Google Drive 제공, 다양한 설정 제공. 상업적 사용 가능. 재현성 우수. 4. 범용 장면 적용성 (5/15): REDS, Vimeo-90K 등 일반 자연영상으로 학습하여 교통/거리 장면에도 적용 가능하나, bicubic 위주 학습으로 실제 CCTV 화질에서 성능 보장 불확실. 5. Scale/최신성 적합 (5/15): x4 단일 scale만 지원(720p→1080p는 x1.5 수준). CVPR 2024 하이라이트로 최신성은 우수하나, 목표 scale과 불일치. Arbitrary-scale 미지원.

#### [42점] CycMu-Net (CycMuNet+) — arXiv 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2205.05264) · **코드:** [https://github.com/tongyuantongyu/cycmunet](https://github.com/tongyuantongyu/cycmunet) · **License:** BSD-2-Clause · **Framework:** PyTorch
- **Pretrained:** yes — models.zip (v0.0.1, May 2023) on GitHub Releases; described as "weights suitable for TensorRT plugin"
- **Scale:** x4 spatial + x2 temporal (ST-VSR); code also supports x2 spatial · **Real-world:** △ 합성 · **효율:** Trained on 4x NVIDIA 2080Ti; TensorRT + VapourSynth inference plugin available; explicit FPS/params not reported in accessible sources
- **Datasets:** Vimeo-90K (training), Adobe240, Vimeo-90K (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (5/25): 학습 및 평가가 Vimeo-90K/Adobe240 기반 bicubic 다운샘플링 합성 열화 전용. H.264/265 압축 아티팩트, CCTV 노이즈 등 실제 열화를 명시적으로 다루지 않음. 다만 코드 내 "cctv-scaled" 데이터셋 인덱스 참조가 있어 저자가 CCTV 도메인에 적용 시도를 한 흔적은 있음. 2. 배포가능성 (8/25): TensorRT + VapourSynth 플러그인이 있어 추론 가속이 가능하나, ST-VSR 구조 특성상 오프라인·배치 처리 위주이며 온라인/causal 스트리밍 지원 여부는 불분명. 실시간 PTZ CCTV 라이브 스트림 적용에는 추가 엔지니어링 필요. 3. 재현성/사용성 (14/20): 코드 공개(PyTorch+MindSpore+TensorRT), pretrained weights 공개(GitHub Releases), BSD-2-Clause 라이선스(상업적 사용 가능). 단, 공식 저자 리포(hhhhhumengshun/CycMuNet)는 empty이고 실제 구현은 제3자 리포(tongyuantongyu/cycmunet)에 있어 사용성이 다소 분산됨. 4. 범용 장면 적용성 (10/15): 일반 자연영상(Vimeo-90K) 학습 기반으로 범용 적용 가능. 교통/거리 장면에 특별한 제약 없음. 5. Scale/최신성 적합 (5/15): x4 spatial + x2 temporal의 ST-VSR 태스크로 720p→1080p(x1.5배) 업스케일 목표와 정확히 맞지 않고 x4가 과도한 편. arbitrary-scale 미지원. CVPR 2022로 최신은 아님.

#### [42점] DAP (Deformable Attention Pyramid) — arXiv 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2202.01731) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — No public weights found; author's GitHub (dariofuoli) only has RLSP repo, no DAP repo; CatalyzeX shows "Request Code" button indicating no release.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** DAP-128: 38ms/frame, 26.3 FPS on 180x320 LR frames, 165 GMACs (330 GFLOPs); online/causal processing (no future frames)
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vimeo-90K-T, Vid4 (evaluation); BI (bicubic) and BD (blur+downsample) degradation
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (7/25): BI(bicubic)와 BD(blur+서브샘플링) 두 가지 합성 열화만 다루며, H.264/H.265 압축 아티팩트·CCTV 실제 열화에 대한 학습·검증 없음. 실세계 대응력 낮음. 2. 배포가능성 (18/25): online/causal 아키텍처(미래 프레임 불필요), 26.3 FPS·38ms로 실시간 처리 가능, 165 GMACs로 경쟁력 있는 연산량. PTZ 라이브 스트리밍 배포 적합성은 높으나 GPU 필요. 3. 재현성/사용성 (2/20): 공식 코드 미공개(GitHub에 DAP 리포 없음), pretrained 가중치 없음. 재현성 매우 낮음. 4. 범용 장면 적용성 (10/15): REDS·Vimeo-90K 등 일반 자연영상 데이터셋으로 학습되어 교통·거리 장면에도 적용 가능. 도메인 특화 아님. 5. Scale/최신성 적합 (5/15): x4 단일 스케일만 지원(720p→1080p 목적인 x1.5 미지원), arbitrary-scale 없음. 2022년 논문으로 비교적 최신이나 이후 개선 모델 다수 존재.

#### [42점] SSL (Structured Sparsity Learning for Efficient Video Super-Resolution) — RSC/RSCL variant — arXiv 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2206.07687) · **코드:** [https://github.com/Zj-BinXia/SSL](https://github.com/Zj-BinXia/SSL) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — x4 weights for BI and BD variants on Google Drive: https://drive.google.com/drive/folders/1of6pPD1exn_VWnekIW-gmaY7jHLOlZvm
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 0.5–1.3M params; 18–24ms on V100; 62.4–92.1G FLOPs; BasicVSR-uni (unidirectional) variant supports online/causal inference
- **Datasets:** REDS, Vimeo-90K (train); REDS4, REDSval4, Vid4, UDM10, Vimeo-90K-T (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (6/25): BI(bicubic)와 BD(blur downscale) 두 가지 열화 모드를 지원하지만, H.264/265 압축·CCTV 노이즈 같은 실제 블라인드 열화는 다루지 않음. 합성 열화 전용 학습이라 CCTV 실환경 적용 시 성능 저하 가능성 높음. 2. 배포가능성 (14/25): 경량 모델(0.5–1.3M params, 18–24ms on V100)이며 단방향 BasicVSR-uni 변형이 온라인/스트리밍 추론을 지원하여 PTZ 라이브 환경 배포 가능성 있음. 그러나 x4 단일 스케일만 지원하여 720p→1080p(x1.5) 업스케일에 직접 활용하기 어려움. 3. 재현성/사용성 (10/20): 공식 코드와 pretrained 가중치(Google Drive)가 공개되어 있으나 LICENSE 파일이 없어 상업적 사용 가능 여부 불명확. 라이선스 명시 부재로 재사용 시 법적 리스크 존재. 4. 범용 장면 적용성 (9/15): 일반 자연영상(REDS, Vimeo-90K)으로 학습되어 교통/거리 장면에도 기본 적용 가능. 도메인 특화는 아님. 5. Scale/최신성 적합 (3/15): x4 단일 스케일만 지원. 목표 업무(720p→1080p, 약 x1.5)에는 스케일 불일치로 직접 활용 불가. CVPR 2023 논문으로 비교적 최신이나 스케일 갭이 치명적.

#### [42점] VideoINR — CVPR 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2206.04647) · **코드:** [https://github.com/Picsart-AI-Research/VideoINR-Continuous-Space-Time-Super-Resolution](https://github.com/Picsart-AI-Research/VideoINR-Continuous-Space-Time-Super-Resolution) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — x4 spatial / x8 temporal weights on Google Drive (linked in README)
- **Scale:** arbitrary-scale spatial (x1~x4, trained with random sampling U(1,4)); arbitrary temporal (demo: x8); in-distribution standard: x4 spatial, x8 temporal · **Real-world:** △ 합성 · **효율:** Training requires 4x RTX 2080Ti, 600k iterations two-stage training. Inference faster than fixed-scale models for multi-frame synthesis (MLP decoding after encoding). No reported FPS or params count.
- **Datasets:** Training: Adobe240; Evaluation: Vid4, GoPro, Adobe240; Degradation: bicubic downsampling (MATLAB imresize)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (8/25): bicubic 합성 열화 전용으로 학습됨. H.264/265 압축 아티팩트·CCTV 노이즈 대응 전혀 없음. 교통 CCTV 실환경 적용 시 성능 저하 예상. 2. 배포가능성 (6/25): 4x RTX 2080Ti로 훈련, 추론 시간 미공개. INR 기반 MLP 디코딩 구조로 오프라인 처리 방식. 실시간/스트리밍/causal 지원 없음. 교통 PTZ 라이브 배포 어려움. 3. 재현성/사용성 (13/20): 코드 공개(GitHub), 사전학습 가중치 Google Drive 제공. 라이선스 파일 없음(상업적 사용 불명확). 단, 재현성 자체는 가능. 4. 범용 장면 적용성 (9/15): Adobe240·GoPro·Vid4 일반 자연영상으로 학습/평가. 도메인 특화 아님. 교통 장면에도 구조적으로 적용 가능하나 실세계 열화 없이 일반성 제한. 5. Scale/최신성 적합 (6/15): arbitrary-scale 지원(x1~x4 공간, arbitrary 시간)으로 x1.5~x2 업스케일 가능. CVPR 2022로 비교적 최신이나 INR 방식 특성상 연산 비용 높고, 현재(2026) 기준 후속 방법들에 성능 추월됨. 합산: 8+6+13+9+6=42

#### [42점] MuCAN (Multi-Correspondence Aggregation Network) — ECCV 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2007.11803) · **코드:** [https://github.com/Jia-Research-Lab/Simple-SR](https://github.com/Jia-Research-Lab/Simple-SR) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** partial — x4 only: MuCAN_REDS.pth (5-frame input, REDS dataset) and MuCAN_Vimeo90K.pth (7-frame input, Vimeo-90K dataset) on Google Drive
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** n/a
- **Datasets:** REDS, Vimeo-90K (training and evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (5/25): bicubic 합성 열화 전용으로 학습되어 H.264/265 압축, CCTV 노이즈 등 실제 열화에 대한 대응 능력 없음. 2. 배포가능성 (7/25): 오프라인 슬라이딩 윈도우 방식(5~7프레임 입력), 비인과적(non-causal) 구조이며 FPS/params 미공개로 실시간 PTZ 배포에 부적합. 3. 재현성/사용성 (17/20): MIT 라이선스로 상업적 사용 가능, 코드 공개, x4 pretrained weights(REDS/Vimeo90K)도 Google Drive에서 제공되어 재현성 우수. 4. 범용 장면 적용성 (8/15): REDS/Vimeo-90K는 다양한 자연영상 포함으로 일반성 있으나, 교통 특화 학습은 없음. 5. Scale/최신성 적합 (5/15): x4 단일 스케일만 지원, 2020년 논문으로 최신성 낮음, 1.5×~2× 임의 스케일 미지원. 합계: 42점.

#### [42점] RSDN (Recurrent Structure-Detail Network) — ECCV 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2008.00455) · **코드:** [https://github.com/junpan19/RSDN](https://github.com/junpan19/RSDN) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — RSDN.pth (~24MB) included directly in the GitHub repo under RSDN/RSDN.pth; single x4 model only
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** Recurrent (online/causal) inference; tested on 1x P100 GPU; trained on 8x Tesla V100; no explicit FPS or param count published (model prints size at runtime: RSDN9_128)
- **Datasets:** Training: Vimeo-90K; Eval: Vid4, UDM10, Vimeo-90K-T
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0~25): 5점. Gaussian blur + bicubic x4 합성 열화만 사용. H.264/265 압축 아티팩트·CCTV 노이즈 등 실세계 열화 대응 없음. 2. 배포가능성(0~25): 14점. Recurrent(순환) 구조로 online·causal 추론 가능하여 스트리밍에 유리. 단, 훈련에 8xV100 필요하고 실시간 FPS 수치 미공개, 경량화 설계 아님. 3. 재현성/사용성(0~20): 10점. 코드와 pretrained 가중치(RSDN.pth) 모두 공개되어 재현 가능. 그러나 LICENSE 파일 없음(상업적 사용 불명확), README의 학습 부분이 TODO 상태로 완성도 낮음. 4. 범용 장면 적용성(0~15): 8점. Vid4·UDM10 등 일반 자연영상 벤치마크 평가. 특정 도메인 특화 아님. 교통/거리 영상에 직접 적용 가능하나 실제 CCTV 열화에는 추가 파인튜닝 필요. 5. Scale/최신성 적합(0~15): 5점. x4 단일 스케일만 지원(720p→1080p는 약 x1.5로 x4와 미스매치), arbitrary-scale 미지원. 2020년 논문으로 최신 모델 대비 성능 격차 있음.

#### [42점] PFNL (Progressive Fusion Video Super-Resolution Network via Exploiting Non-Local Spatio-Temporal Correlations) — ICCV 2019 (2019)
- **논문:** [링크](https://openaccess.thecvf.com/content_ICCV_2019/html/Yi_Progressive_Fusion_Video_Super-Resolution_Network_via_Exploiting_Non-Local_Spatio-Temporal_Correlations_ICCV_2019_paper.html) · **코드:** [https://github.com/psychopa4/PFNL](https://github.com/psychopa4/PFNL) · **License:** MIT · **Framework:** TensorFlow
- **Pretrained:** yes — Pretrained checkpoints available via TeraBox (dubox.com), including eval/test/checkpoint files for x4 model
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** ~3.0M params, ~295ms per frame, ~3.4 FPS; offline batch processing only
- **Datasets:** MM522 (training), Vid4, UDM10 (testing)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (8/25): 학습은 bicubic 다운샘플링 기반이며 실제 CCTV H.264/265 압축 열화나 노이즈 대응 설계가 없음. 평가 데이터셋에서 Gaussian blur+다운샘플링도 일부 사용하지만 실세계 열화 시나리오는 아님. 2. 배포가능성 (7/25): ~3.4 FPS로 실시간 처리 불가, TF 1.x 기반 레거시, PTZ 라이브 스트리밍이나 온라인·causal 처리 구조 없음. 오프라인 배치 전용. 3. 재현성/사용성 (14/20): MIT 라이선스로 상업적 이용 가능, GitHub 코드 공개, TeraBox에 pretrained 가중치 존재하나 TF 1.12 레거시로 실용성 다소 제한. 4. 범용 장면 적용성 (8/15): 자연영상 비디오 SR 일반 모델로 특정 도메인에 국한되지 않음. 교통/CCTV 영상에도 적용 가능하나 검증 없음. 5. Scale/최신성 적합 (5/15): x4만 지원, arbitrary-scale 및 x1.5~x2 미지원, 2019년 논문으로 최신 방법 대비 성능 격차 있음. 합산: 42점.

#### [42점] FRVSR (Frame-Recurrent Video Super-Resolution) — CVPR 2018 (2018)
- **논문:** [링크](https://arxiv.org/abs/1801.04590) · **코드:** [https://github.com/msmsajjadi/FRVSR](https://github.com/msmsajjadi/FRVSR) · **License:** none/unspecified · **Framework:** TensorFlow
- **Pretrained:** no — 공식 리포(msmsajjadi/FRVSR)에는 pretrained 가중치 없음. 코드 자체도 학습 데이터셋 다운로드 스크립트(YouTube-dl)와 README만 포함, 학습/추론 코드 없음.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** TensorFlow, Nvidia P100 기준: FRVSR 3-64 (3 residual blocks, 64 filters) → 74ms/frame (Full HD, 4x); FRVSR 10-128 (10 residual blocks, 128 filters) → 191ms/frame. 온라인/causal 처리 가능(recurrent, frame-by-frame feedforward).
- **Datasets:** Training: Vimeo.com에서 수집한 40개 고해상도 영상(720p/1080p/4K); Evaluation: YT10 (YouTube 1080p 10개 클립), Vid4 benchmark
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (8/25): 학습 시 Gaussian blur + 4픽셀 간격 서브샘플링(bicubic 유사)로 LR 생성. 논문 내 Gaussian noise와 JPEG 압축에 대한 robustness 실험이 포함되어 있어 완전히 합성전용은 아니지만, H.264/H.265 압축이나 실제 CCTV 열화에 특화된 학습은 없음. 2. 배포가능성 (12/25): recurrent 구조로 frame-by-frame 추론 가능(causal/online). 74ms~191ms/frame으로 실시간 대응은 어렵지만 스트리밍 가능. 경량 모델(3-64) 존재. 3. 재현성/사용성 (4/20): 공식 GitHub 리포 존재하지만 실제 학습/추론 코드 없음(데이터셋 다운 스크립트만). pretrained weights 없음. 라이선스 미지정. 사실상 코드 없는 것과 유사. 4. 범용 장면 적용성 (10/15): Vimeo, YouTube의 다양한 자연영상으로 학습. 특정 도메인 특화 없음. 5. Scale/최신성 적합 (8/15): x4 고정 지원, arbitrary-scale/x1.5~x2 미지원. 2018년 CVPR 논문으로 최신성 낮음.

#### [42점] TDAN (Temporally-Deformable Alignment Network) — arXiv 2018 (2018)
- **논문:** [링크](https://arxiv.org/abs/1812.02898) · **코드:** [https://github.com/YapengTian/TDAN-VSR-CVPR-2020](https://github.com/YapengTian/TDAN-VSR-CVPR-2020) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — model.pt and model_gaussian.pt included directly in the GitHub repo under /model directory
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** n/a
- **Datasets:** Vimeo-90K (training), Vid4, SPMC-30 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0~25): 5점. 학습 데이터가 bicubic downsampling 기반("LR_bicubic")이며, H.264/265 압축 열화·CCTV 노이즈 등 실제 열화에 대한 대응은 없음. model_gaussian.pt가 존재해 가우시안 노이즈 일부 고려하나 실제 압축 아티팩트 대응은 미흡. 2. 배포가능성(0~25): 7점. 오프라인 배치 방식, 5프레임 윈도우 사용이나 실시간 스트리밍 지원 없음. PyTorch 0.3.1 등 구형 의존성이 있어 현재 환경 배포가 까다로움. 파라미터/FPS 정보 미제공. 3. 재현성/사용성(0~20): 15점. 공식 GitHub에 코드와 pretrained 가중치(model.pt, model_gaussian.pt) 모두 공개, MIT 라이선스로 상업적 사용 가능. 단 구형 PyTorch(0.3.1) 의존성이 재현 걸림돌. 4. 범용 장면 적용성(0~15): 10점. Vimeo-90K로 학습된 일반 자연영상 대상 VSR 모델이며 특정 도메인 제한 없음. 교통/거리 장면에도 직접 적용 가능. 5. Scale/최신성 적합(0~15): 5점. x4 단일 스케일만 지원하며 arbitrary-scale 미지원. 2018년 arXiv 제안(CVPR 2020 발표)으로 이후 EDVR 등에 영감을 준 기초 모델이나 성능 면에서 후속작 대비 열위.


### △ 참고 (<40)

#### [38점] CTUN (Cascaded Temporal Updating Network) — arXiv 2024 (2024)
- **논문:** [링크](https://arxiv.org/abs/2408.14244) · **코드:** [https://github.com/House-Leo/CTUN](https://github.com/House-Leo/CTUN) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** partial — Vid4 (BI degradation) x4 weights released (March 2025); training code not yet released
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 2.2M params (unidirectional); 21ms per frame at 180x320 resolution; ~30% of BasicVSR params and runtime
- **Datasets:** REDS, Vimeo-90K (train); REDS4, Vid4, Vimeo-T, UDM10 (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0~25): 3점. BI(bicubic) 및 BD(Gaussian blur) 열화만 다루며 H.264/265 압축 노이즈 등 실제 CCTV 열화는 전혀 다루지 않음. 완전히 합성 열화 기반 학습. 2. 배포가능성(0~25): 16점. 2.2M 파라미터, 21ms/frame으로 매우 경량·고효율. 단방향(unidirectional) 전파 구조로 스트리밍/온라인 추론 가능. PTZ 라이브 환경에 적합한 구조. 3. 재현성/사용성(0~20): 7점. GitHub 코드 공개되어 있고 pretrained 가중치 일부(Vid4 BI) 존재하지만, 라이선스 미지정(null)이어서 상업적 사용 여부 불명확. 훈련 코드 미공개. 4. 범용 장면 적용성(0~15): 10점. 일반 자연영상(REDS, Vimeo-90K) 기반 학습으로 특정 도메인에 특화되지 않아 교통/거리 영상에도 적용 가능. 5. Scale/최신성 적합(0~15): 2점. x4 단일 스케일만 지원. 목표인 720p→1080p(x1.5)에는 직접 대응하지 못하며 arbitrary-scale 미지원. 2024년 논문으로 최신성은 보통.

#### [38점] U3D-RDN + MSCU (DSMC) — AAAI 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2103.11744) · **코드:** [https://github.com/iPrayerr/DSMC-VSR](https://github.com/iPrayerr/DSMC-VSR) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** partial — Vimeo90K 학습 가중치 1종(Google Drive, .pkl 형식). REDS 학습 가중치는 미제공.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** n/a
- **Datasets:** Vimeo90K, REDS (훈련); REDS4, Vid4 (평가)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(5/25): Vimeo90K·REDS 표준 bicubic 다운샘플 기반 학습이며, CCTV 압축 노이즈·H.264/265 열화에 대한 대응 없음. 2. 배포가능성(6/25): Deformable Conv V2 + 3D conv 기반 오프라인 배치 처리 구조로 실시간/스트리밍 설계 없음; FPS·파라미터 수 미보고. 3. 재현성/사용성(9/20): 공식 PyTorch 코드와 Google Drive pretrained 가중치 존재하나 LICENSE 파일 미포함으로 상업적 사용 법적 불명확. 4. 범용 장면 적용성(12/15): REDS·Vimeo90K 일반 자연영상 도메인 학습, 특정 도메인 특화 아님. 5. Scale/최신성(6/15): x4 단일 스케일만 지원(720p→1080p의 ~x1.5 미지원), 2021년 발표로 최신성 다소 부족.

#### [38점] VESR-Net — arXiv 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2003.02115) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — 논문 및 공개 자료 어디에도 pretrained 가중치 다운로드 링크 없음
- **Scale:** x4 · **Real-world:** ○ 실세계 · **효율:** VESR-Net_small: 15.96M params, 162.14G FLOPs; VESR-Net: 21.65M params, 186.26G FLOPs; 4× NVIDIA Titan 1080Ti로 학습; FPS/실시간 수치 미보고
- **Datasets:** Youku-VESR Challenge dataset (1000개 1080p 영상 클립, LR-HR 쌍; 실제 온라인 스트리밍 열화 포함)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(15/25): Youku 온라인 스트리밍 플랫폼에서 수집된 실제 열화(압축·노이즈 포함) 데이터로 학습하여 bicubic 전용이 아님. 그러나 교통 CCTV/PTZ 특화가 아닌 일반 동영상 스트리밍 환경이므로 부분 점수. 2. 배포가능성(5/25): 21.65M params / 186G FLOPs로 무거운 편이며, 7프레임 입력 기반 오프라인 처리 방식. 실시간·스트리밍·경량화 관련 내용 없음. 3. 재현성/사용성(0/20): 공식 코드 미공개, pretrained 가중치 없음, 라이선스 불명. 재현성 0점. 4. 범용 장면 적용성(10/15): 다양한 카테고리의 영상 콘텐츠로 학습했으나, Youku 스트리밍 도메인에 특화되어 교통 CCTV 장면 일반화 보장 불명확. 5. Scale/최신성 적합(8/15): x4 단일 스케일 지원으로 720p→1080p 요구에 부합하나, 2020년 논문으로 최신성 낮고 arbitrary-scale 미지원.

#### [38점] DNLN (Deformable Non-local Network) — arXiv 2019 (2019)
- **논문:** [링크](https://arxiv.org/abs/1909.10692) · **코드:** [https://github.com/wh1h/DNLN](https://github.com/wh1h/DNLN) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** yes — x4 pretrained weights on Baidu Pan (code: prl6); pre-computed results also available (code: edc9)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** DNLN: 0.119s/frame (112×64 input), 19.74M params; S-DNLN: 0.095s/frame, 12.39M params
- **Datasets:** Vimeo-90K (train), Vid4, SPMCS-11, Vimeo-90K-T (eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (5/25): 논문이 bicubic 다운샘플링만으로 학습/평가하며 실제 CCTV H.264/265 압축·노이즈·블러 등 실세계 열화는 전혀 다루지 않음. 합성 열화 전용으로 실제 교통 CCTV 환경과의 갭이 큼. 2. 배포가능성 (8/25): 112×64 소형 입력 기준 0.119s/frame(~8fps)으로 실시간에 미치지 못하며, 720p→1080p 적용 시 속도가 크게 저하될 것으로 예상. 오프라인 오픈루프 배치 처리에만 적합. 3. 재현성/사용성 (14/20): GitHub 공개 코드(MIT 라이선스) + Baidu Pan 사전학습 가중치 제공으로 재현성 우수. 단, Baidu Pan 접근성 제한과 Deformable Conv V2 별도 설치 필요가 약점. 4. 범용 장면 적용성 (8/15): 일반 자연영상(Vimeo-90K, Vid4) 기반 학습으로 교통·거리 장면에도 기본 적용 가능. 도메인 특화 없음. 5. Scale/최신성 적합 (3/15): x4 단일 스케일만 지원하며 교통 CCTV 720p→1080p(~x1.5) arbitrary-scale 수요와 불일치. 2019년 논문으로 최신 방법 대비 성능 열세.

#### [38점] DUF (Deep Video Super-Resolution with Dynamic Upsampling Filters) — CVPR 2018 (2018)
- **논문:** [링크](https://openaccess.thecvf.com/content_cvpr_2018/html/Jo_Deep_Video_Super-Resolution_CVPR_2018_paper.html) · **코드:** [https://github.com/yhjo09/VSR-DUF](https://github.com/yhjo09/VSR-DUF) · **License:** none/unspecified · **Framework:** TensorFlow
- **Pretrained:** yes — 5개 .h5 가중치 파일이 리포에 직접 포함: params_16L_x2.h5, params_16L_x3.h5, params_16L_x4.h5, params_28L_x4.h5, params_52L_x4.h5
- **Scale:** x2, x3, x4 · **Real-world:** △ 합성 · **효율:** 7프레임 입력(비인과적/오프라인), 16/28/52 레이어 변형 존재; FPS/params 수치 공개 미확인
- **Datasets:** 훈련: 인터넷 수집 351개 비디오(160,000 패치, 144×144), bicubic 다운샘플링; 평가: Vid4, Val4(coastguard/foreman/garden/husky)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): bicubic 합성 열화만으로 학습; H.264/265 압축, CCTV 노이즈 등 실세계 열화 미대응. 2. 배포가능성(5/25): 7프레임 비인과적 오프라인 처리, TF 1.3 구버전, 무거운 52L 모델; 라이브 PTZ 스트리밍에 부적합. 3. 재현성/사용성(14/20): 코드+가중치 모두 공개이나 라이선스 미명시로 상업적 사용 불명확, TF 1.3/Python 2.7 의존성이 현재 환경에서 재현 어려움. 4. 범용 장면 적용성(10/15): 일반 자연영상용으로 도메인 특화 아님, 교통 영상에도 적용 가능하나 real-world 성능 미검증. 5. Scale/최신성 적합(9/15): x2~x4 지원, CVPR 2018로 2018년 기준 SOTA였으나 현재는 구식; arbitrary-scale 미지원. 합계 38점: 코드/가중치는 공개되어 있으나 bicubic 전용 학습, 구버전 프레임워크, 오프라인 처리로 교통 CCTV 실배포 요건에 크게 미달.

#### [37점] ETDM (Look Back and Forth) — CVPR 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2204.07114) · **코드:** [https://github.com/junpan19/ETDM](https://github.com/junpan19/ETDM) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — x4 weight (X4_18L_64_best.pth, ~34MB) directly bundled in repo under ETDM-CVPR2022/model/
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 14 fps at 320x180 for x4 SR (buffer N=3); ~4x faster than EDVR; offline/non-causal recurrent with past+future frames
- **Datasets:** Vimeo-90K (train), Vid4, UDM10, SPMCS (eval); BI and BD degradation modes
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0~25): 5점 — bicubic(BI)과 blur-downsampling(BD) 합성 열화만 사용. H.264/265 압축 아티팩트, CCTV 노이즈 등 실제 열화에 대한 학습/대응이 없음. 2. 배포가능성 (0~25): 8점 — 14fps @320×180 x4로 준실시간에 가깝지만, SpyNet 광학류 추정을 포함한 recurrent 구조로 과거+미래 프레임을 모두 참조(비인과적). PTZ 라이브 스트리밍에는 부적합. 3. 재현성/사용성 (0~20): 12점 — 공식 코드 및 x4 pretrained weight가 repo에 직접 포함되어 재현성은 양호. 단, 라이선스 파일 없음(상업적 사용 불명확). 4. 범용 장면 적용성 (0~15): 8점 — Vimeo-90K, Vid4 등 일반 자연 영상으로 학습·평가. 교통/도로 특화 아님. 5. Scale/최신성 적합 (0~15): 4점 — x4 고정, arbitrary-scale 미지원. 1.5×/2× 업스케일 미지원. CVPR 2022로 다소 오래됨. 합산: 5+8+12+8+4=37점. 합성 열화 전용, 비인과 recurrent 구조, 라이선스 불명이 주요 감점 요인.

#### [37점] VSR-TGA — CVPR 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2007.10595) · **코드:** [https://github.com/junpan19/VSR_TGA](https://github.com/junpan19/VSR_TGA) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — TGA-without-align-dla.zip included directly in the repository under code/ directory; x4 only
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 5.8M params; 0.07T FLOPs on 112×64 LR input; trained on 8× V100 GPUs; offline/non-causal (7-frame sliding window)
- **Datasets:** Training: Vimeo-90K; Evaluation: Vid4, Vimeo-90K-T
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): 학습 열화가 Gaussian blur(σ=1.6) + bicubic 4× 다운샘플링으로만 구성된 순수 합성 열화 전용 모델. H.264/265 압축 노이즈·CCTV 특유의 열화에 대한 대응 전무. 2. 배포가능성 (5/25): 5.8M 파라미터, 0.07T FLOPs로 상대적으로 가볍지만, 7프레임 입력 기반 오프라인 처리 구조(causal/streaming 불가). 실시간·PTZ 라이브 스트리밍에 직접 적용 어려움. 3. 재현성/사용성 (12/20): 코드 공개(GitHub) 및 pretrained 가중치(in-repo zip)가 있어 재현 가능. 다만 라이선스 미지정(null)으로 상업적 사용 법적 불확실성 존재. 4. 범용 장면 적용성 (12/15): Vid4·Vimeo-90K 등 일반 자연영상 벤치마크에서 평가. 도메인 특화 아님. 5. Scale/최신성 적합 (8/15): x4 단일 스케일만 지원(x1.5~x2 미지원), 2020년 CVPR 발표로 비교적 오래된 모델. 합산: 0+5+12+12+8 = 37점. 실세계 열화 대응 부재와 비인과적 구조가 교통 CCTV 배포 시나리오와 가장 큰 갭.

#### [36점] RevisitTempAlign — arXiv 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2111.15288) · **코드:** [https://github.com/redrock303/Revisiting-Temporal-Alignment-for-Video-Restoration](https://github.com/redrock303/Revisiting-Temporal-Alignment-for-Video-Restoration) · **License:** research/non-commercial only · **Framework:** PyTorch
- **Pretrained:** yes — x4 VSR (REDS, Vimeo-90K), deblurring, denoising weights on Google Drive: https://drive.google.com/drive/folders/1_em2Z1gUe9K3rbEFFvVENUcL4cY22Tkq
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** n/a (offline batch processing; no FPS/params reported in README)
- **Datasets:** REDS, Vimeo-90K, DAVIS (denoising), GoPro-based video deblurring dataset
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): 합성 bicubic 열화만 학습. H.264/265 압축·노이즈·CCTV 실세계 열화 대응 훈련 없음. 2. 배포가능성(5/25): 오프라인 전용 비실시간 모델. DCNv2 기반 변형 합성곱으로 무거운 편이며, online/causal/스트리밍 지원 없음. 5점은 코드가 존재해 로컬 배포 시도는 가능하다는 최소 점수. 3. 재현성/사용성(12/20): 코드 공개 + pretrained 가중치 Google Drive 공개. 단, 라이선스가 "research only"로 상업적 사용 불가. 코드+가중치 공개로 재현성은 양호하나 라이선스 제약으로 감점. 4. 범용 장면 적용성(10/15): VSR·deblurring·denoising 등 일반 자연영상 대상. 도메인 특화 아님. 5. Scale/최신성 적합(9/15): x4 단일 스케일만 지원, 1.5x~2x 또는 arbitrary-scale 미지원. 2021년 논문으로 최신성 다소 낮음. 합산: 0+5+12+10+9 = 36.

#### [34점] SPMC (Sub-Pixel Motion Compensation) — ICCV 2017 (2017)
- **논문:** [링크](https://arxiv.org/abs/1704.02738) · **코드:** [https://github.com/jiangsutx/SPMC_VideoSR](https://github.com/jiangsutx/SPMC_VideoSR) · **License:** MIT · **Framework:** TensorFlow
- **Pretrained:** partial — TensorFlow .pb weights for x2 (spmc_240_320_2x3f.pb) and x4 (spmc_120_160_4x3f.pb) in official repo; x3 not included; training code not released
- **Scale:** x2, x3, x4 · **Real-world:** △ 합성 · **효율:** n/a (no params/FPS/runtime reported; offline batch processing only)
- **Datasets:** SPMCS (custom 30-video test set, 31 frames each, bicubic downsampled x2/x3/x4); training dataset not publicly specified
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (5/25): bicubic 다운샘플링 전용으로 학습되어 H.264/265 압축, CCTV 노이즈 등 실제 열화에 대한 대응이 없음. 2. 배포가능성 (5/25): 추론 코드만 공개, 훈련 코드 미공개, 실시간/온라인 처리 지원 없음, TF .pb 파일로 오프라인 배치 전용. 3. 재현성/사용성 (12/20): MIT 라이선스로 상업 사용 가능, x2/x4 사전학습 가중치 부분 공개, 코드 공개됨. 단 훈련 코드 없고 x3 가중치 미포함. 4. 범용 장면 적용성 (8/15): 자연 영상 다중 프레임 VSR로 일반 장면에 적용 가능하나 교통/CCTV 특화 실험 없음. 5. Scale/최신성 적합 (4/15): x2/x4 지원은 교통 720p→1080p에 어느정도 부합하나 2017년 논문으로 최신성 낮고 arbitrary-scale 미지원. 합계 34점: bicubic 전용 학습으로 실CCTV 열화 대응이 핵심 약점.

#### [33점] MRVSR (Middle Recurrent Video Super-Resolution) — CVPR 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2112.08950) · **코드:** [https://github.com/bjmch/MRVSR](https://github.com/bjmch/MRVSR) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — MRVSR, RFS3, RLSP weights available on Google Drive (weights/mrvsr_weights.tar). Note: SRNL training layer code excluded for proprietary reasons, but inference-ready weights are provided.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** Recurrent (causal/online-capable) architecture; tested on NVIDIA TITAN RTX GPU. No FPS/params reported in accessible materials. Inference is sequential (recurrent state propagation).
- **Datasets:** Quasi-Static Video Set (new dataset introduced by authors), Vid4
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): 합성 열화(Gaussian blur + 4× stride 다운샘플링)만 사용. H.264/265 압축·CCTV 실제 열화 대응 없음. 0점. 2. 배포가능성 (12/25): 순방향 순환(recurrent) 구조로 online/causal 처리 가능하나, 학습 코드 일부(SRNL layer)가 독점 사유로 미공개. 추론은 가능하나 전체 학습 파이프라인 재현 불가. 중간 점수. 3. 재현성/사용성 (8/20): 코드 공개, 가중치 공개(Google Drive). 단, 라이선스 미지정(상업적 사용 불명확), 핵심 학습 코드 일부 누락. 부분 점수. 4. 범용 장면 적용성 (8/15): Vid4 등 일반 영상에 평가. 그러나 주력 기여가 quasi-static(저모션) 장면 안정성이므로, 다양한 교통/거리 장면 범용 적용성은 보통. 5. Scale/최신성 적합 (5/15): x4만 지원, 2022년 CVPR 논문이나 실용적 arbitrary-scale 미지원. 교통 CCTV 720p→1080p(약 x1.5) 요구에 부합하지 않음.

#### [33점] PSRT (RethinkAlign / Patch alignment Super-Resolution Transformer) — arXiv 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2207.08494) · **코드:** [https://github.com/XPixelGroup/RethinkVSRAlignment](https://github.com/XPixelGroup/RethinkVSRAlignment) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** yes — PSRT-recurrent and PSRT-sliding weights on Google Drive (REDS and Vimeo90K variants)
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** PSRT-recurrent: 13.4M params, 812ms/frame, 1.5T FLOPs; offline/non-causal; ~18 days training on A100
- **Datasets:** REDS, Vimeo-90K, Vid4
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): 오직 bicubic 합성 열화만 다루며, H.264/265 압축·CCTV 노이즈·블러 등 실제 열화에 대한 처리가 전혀 없음. 2. 배포가능성(3/25): 프레임당 812ms로 실시간 불가, 오프라인 전용, 비인과적 구조 — PTZ CCTV 라이브 스트리밍 적용 불가. 3. 재현성/사용성(12/20): 코드와 pretrained 가중치 모두 공개되어 있으나 LICENSE 파일이 없어 상업 활용 법적 불확실성이 있음. 4. 범용 장면 적용성(10/15): 일반 자연 영상(REDS, Vimeo90K) 기반 학습으로 교통·거리 영상에 원칙적 적용 가능, 다만 도메인 특화 학습은 없음. 5. Scale/최신성 적합(8/15): x4 단일 스케일 지원(교통 CCTV 720p→1080p에 필요한 x1.5 지원 없음), NeurIPS 2022 최신 고성능 transformer 계열이지만 임의 스케일 미지원.

#### [32점] LRTI-VSR — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2505.02159) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — 코드 미공개 상태 ("code is coming soon..."). 가중치 없음.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 12.9M params, 1.54T FLOPs (180×320 frame), 848ms runtime (RTX 4090). 오프라인/비인과적 양방향 재귀 구조.
- **Datasets:** REDS (train), REDS4 (eval), ToS3 (eval), VideoLQ (real-world eval)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): 주요 학습 및 평가가 bicubic 합성 열화(x4) 기반. VideoLQ로 실세계 테스트를 일부 언급하나 RealBasicVSR 적용 실험 수준이며 CCTV 압축 열화 특화 대응 없음. → 5점. 2. 배포가능성 (3/25): 848ms/frame(RTX 4090)으로 실시간 불가, 양방향 재귀 구조로 온라인/스트리밍 처리 불가. 매우 무거운 오프라인 전용. → 3점. 3. 재현성/사용성 (0/20): 코드 "coming soon" 상태, LICENSE 없음, pretrained 가중치 없음. 현재 완전히 미공개. → 0점. 4. 범용 장면 적용성 (12/15): REDS/ToS3 등 일반 자연 영상 기반으로 학습·평가되어 교통·거리 장면에 적용 가능성은 있음. 도메인 특화 없음. → 12점. 5. Scale/최신성 적합 (12/15): 2025년 최신 논문, SOTA 성능 주장. 그러나 x4 단일 스케일만 지원(arbitrary-scale 미지원). → 12점. 총합: 32점.

#### [32점] USTVSRNet — arXiv 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2102.13011) · **코드:** ✗ 없음 · **License:** — · **Framework:** unknown
- **Pretrained:** no — No public code repository found; no weights available
- **Scale:** arbitrary-scale (spatial), arbitrary temporal frame rate · **Real-world:** △ 합성 · **효율:** Claims fewer parameters and less running time than prior STVSR methods; specific numbers not confirmed without full PDF access
- **Datasets:** Vimeo-90K septuplet (training), Vid4 and Vimeo-90K test set (Fast/Medium/Slow) (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): 합성(bicubic) 기반 열화로만 학습된 방법으로, H.264/265 압축·CCTV 실제 열화에 대한 대응이 없음. 2. 배포가능성 (7/25): arbitrary-scale 지원으로 PTZ 줌 변화 대응 가능성은 있으나, 공간+시간 동시 처리 구조로 실시간 CCTV 스트리밍 배포에는 부적합. 효율성 주장은 있으나 구체적 수치 미확인. 3. 재현성/사용성 (0/20): 공식 코드 리포지토리 없음, pretrained 가중치 없음. 재현 불가 수준. 4. 범용 장면 적용성 (10/15): Vimeo-90K 자연영상으로 학습되어 교통/거리 영상에도 적용 가능성 있음. 5. Scale/최신성 적합 (15/15): Arbitrary-scale 공간 업스케일링 지원으로 x1.5, x2 등 PTZ 목표 배율에 유연하게 대응 가능. 2021년 IEEE Trans. Broadcasting 게재. 총합: 32점. 코드/가중치 미공개가 결정적 약점이며, 실세계 열화 대응도 없어 CCTV 직접 적용에 상당한 추가 작업 필요.

#### [32점] FSTRN (Fast Spatio-Temporal Residual Network) — CVPR 2019 (2019)
- **논문:** [링크](https://arxiv.org/abs/1904.02870) · **코드:** [https://github.com/lsmale/FSTRN](https://github.com/lsmale/FSTRN) · **License:** Apache-2.0 / MIT (LICENSE file has unresolved merge conflict containing both; both are permissive open-source) · **Framework:** TensorFlow
- **Pretrained:** no — No pretrained weights in repo; only train/test placeholder .txt files; checkpoint saving code uses .npz format but no weights distributed
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** Factorized 3D convolutions (1×3×3 + 3×1×1) reduce computation vs. full 3×3×3; no formal FPS/params/runtime numbers published in code or abstract. Paper claims low computational load. Offline, non-causal sliding window (5-frame window), not real-time.
- **Datasets:** Training: custom bicubic-degraded YUV .mat sequences (BRCN-style); Evaluation: Vid4 (standard x4 VSR benchmark); REDS and Vimeo-90K referenced in survey comparisons
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): bicubic 합성 열화만 사용하여 학습; H.264/265 압축 아티팩트, 실제 CCTV 노이즈 등에 대한 대응 없음. 점수: 3/25. 2. 배포가능성 (5/25): 5프레임 슬라이딩 윈도우 기반의 오프라인 처리, 비인과적(non-causal) 구조로 라이브 스트리밍 불가. TensorFlow 1.x + TensorLayer 기반으로 현재 환경 호환성 낮음. 경량화 시도는 있으나 실제 배포 수준 미달. 점수: 5/25. 3. 재현성/사용성 (5/20): 코드는 GitHub에 공개되어 있으나 README 없음, pretrained 가중치 미제공, TensorFlow 1.x 기반으로 현재 재현 난이도 높음. 라이선스는 permissive이나 실질적 재현성 낮음. 점수: 5/20. 4. 범용 장면 적용성 (9/15): 도메인 특화 없이 일반 자연 영상 비디오에 학습/평가됨(Vid4). 교통/거리 영상에도 적용 가능하나 실제 실험 미확인. 점수: 9/15. 5. Scale/최신성 적합 (10/15): x4 고정 스케일만 지원, arbitrary-scale 미지원. 2019년 CVPR 논문으로 최신성 낮음; 당시 기준 경쟁력 있었으나 현재는 구형 모델. 점수: 10/15. 합산: 3+5+5+9+10 = 32/100.

#### [31점] MANA (Memory-Augmented Non-Local Attention for Video Super-Resolution) — CVPR 2022 (2022)
- **논문:** [링크](https://arxiv.org/abs/2108.11048) · **코드:** [https://github.com/jiy173/MANA](https://github.com/jiy173/MANA) · **License:** MIT · **Framework:** PyTorch
- **Pretrained:** no — README에 "Test code will be uploaded soon" 언급만 있으며 체크포인트 없음. 리포에 훈련 코드만 존재.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** n/a (파라미터 수/FPS 미보고; non-local attention은 공간 차원에 대해 2차 복잡도로 무거운 구조; 오프라인 배치 전용)
- **Datasets:** Vimeo-90K (훈련), Parkour dataset (14 videos, 자체 제작), Vid4, 11개 실세계 영상 (보충 자료 정성 평가용)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): Vimeo-90K bicubic degradation으로만 훈련. H.264/265 압축·CCTV 노이즈 대응 학습 없음. 실세계 영상 11개에 정성 평가만 수행. 2. 배포가능성 (3/25): non-local attention의 2차 공간 복잡도, 오프라인 배치 처리 전용. 실시간/스트리밍/causal 구조 아님. 경량화 보고 없음. 3. 재현성/사용성 (8/20): 코드 공개(MIT 라이선스, 상업 허용)되어 있으나 pretrained 가중치 미제공(훈련 코드만). 직접 재훈련 필요. 4. 범용 장면 적용성 (12/15): Vimeo-90K 기반으로 일반 자연영상에 적용 가능. 도메인 특화 아님. 5. Scale/최신성 (8/15): x4 단일 스케일만 지원(720p→1080p = ×1.5에 해당하지 않음), arbitrary-scale 미지원. CVPR 2022로 비교적 최신이나 실제 교통/CCTV 용도에는 scale 불일치. 합산: 31점.

#### [28점] LDIP — ICCV 2025 (2025)
- **논문:** [링크](https://openaccess.thecvf.com/content/ICCV2025/papers/Bernasconi_LDIP_Long_Distance_Information_Propagation_for_Video_Super-Resolution_ICCV_2025_paper.pdf) · **코드:** ✗ 없음 · **License:** — · **Framework:** unknown
- **Pretrained:** no — 공개된 pretrained 가중치 없음. Disney Research Studios 논문으로 코드/가중치 미공개.
- **Scale:** x4, arbitrary-scale (x3.25, x5.5 등) · **Real-world:** △ 합성 · **효율:** n/a
- **Datasets:** REDS, Vimeo-90K, Vid4
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): 표준 합성(bicubic) 열화 기반 VSR 학습이며, H.264/265 압축 열화나 CCTV 열화에 대한 대응은 언급되지 않음. 2. 배포가능성 (4/25): recurrent 구조로 온라인/스트리밍 처리가 이론적으로 가능하나, 효율성(FPS, 파라미터) 정보 미공개이며 Disney Research 기원으로 실용 배포 어려움. 3. 재현성/사용성 (0/20): 코드 미공개, pretrained 가중치 없음. Disney Research Studios 논문으로 오픈소스 배포 없음. 4. 범용 장면 적용성 (12/15): REDS, Vimeo-90K 등 자연영상 데이터셋에서 평가되어 일반 자연영상에 적용 가능. 5. Scale/최신성 적합 (12/15): arbitrary-scale (x3.25, x5.5 등) 지원, ICCV 2025 최신 논문, 기존 SOTA 대비 우수한 성능 주장. 총합: 코드/가중치 미공개로 재현성 점수가 0이고, 실세계 열화 대응도 부재하여 실용적 활용이 매우 어려움.

#### [28점] MambaVSR — arXiv 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2506.11768) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — no weights available; no public repository found
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 14.1M params; 2.46T FLOPs per single 180×320 LR frame; FPS not reported
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vimeo90K-T, Vid4 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): 논문이 bicubic(BI) 열화만을 학습·평가에 사용하며 H.264/265 압축, CCTV 노이즈 등 실제 열화에 대한 대응이 전혀 없음. 0점. 2. 배포가능성(5/25): 파라미터 14.1M으로 VRT 대비 경량이지만 FLOPs 2.46T(단일 프레임 기준)로 무겁고, 온라인/causal 처리나 실시간 스트리밍에 대한 언급이 없음. 오프라인 슬라이딩 윈도우 방식. 5점. 3. 재현성/사용성(3/20): 공개 코드 없음, pretrained 가중치 없음. 논문 PDF에 CC-BY 4.0 라이선스가 명시되어 있으나 코드 자체가 공개되지 않아 실질적 사용 불가. 3점(arXiv 공개만). 4. 범용 장면 적용성(10/15): REDS/Vimeo-90K 등 자연영상 일반 벤치마크에서 평가되어 교통/거리 장면에 원칙적으로 적용 가능. 특정 도메인에 특화되지 않음. 10점. 5. Scale/최신성 적합(10/15): x4 단일 스케일만 지원(arbitrary-scale 미지원), 2025년 최신 논문으로 PSNR 성능은 우수(VRT 대비 +0.58dB). 10점. 합계: 28점.

#### [28점] VSRM — ICCV 2025 (2025)
- **논문:** [링크](https://arxiv.org/abs/2506.22762) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — No weights released; no public code repository found as of June 2026.
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 17.1M params, 217.4G FLOPs, 223ms runtime per frame on RTX A6000 48GB; offline/non-causal
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vimeo-90K-T, Vid4 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): H.264/265 압축, CCTV 노이즈 등 실제 열화 대응 실험 없음. 오직 bicubic 합성 열화만 사용하여 교통/PTZ CCTV 환경에 직접 적용 불가. 2. 배포가능성 (3/25): 223ms/frame(RTX A6000)으로 실시간 불가, online/causal 구조 아님, 오프라인 배치 처리 전용. 3. 재현성/사용성 (0/20): 공식 코드 미공개, pretrained 가중치 없음, GitHub 리포 없음. 재현 불가 수준. 4. 범용 장면 적용성 (12/15): REDS, Vimeo-90K, Vid4 등 일반 자연영상 벤치마크로 학습/평가하여 교통 영상에 원칙적 적용 가능. 5. Scale/최신성 적합 (13/15): ICCV 2025 최신 논문, Mamba 기반 state-of-the-art 성능, x4 단일 scale은 목표(720p→1080p, x1.5)와 다소 불일치하나 최신성은 높음. 코드 미공개가 가장 큰 감점 요인이며 실세계 열화 미대응으로 실용성도 낮음.

#### [28점] CFDVSR (Collaborative Feedback Discriminative Propagation for Video Super-Resolution) — arXiv 2024 (2024)
- **논문:** [링크](https://arxiv.org/abs/2404.04745) · **코드:** [https://github.com/House-Leo/CFDVSR](https://github.com/House-Leo/CFDVSR) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** no — README의 "To do" 목록에 "Release testing code and pre-trained models"로 명시되어 있어 가중치 미공개 상태
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** CFD-BasicVSR: 6.6M params; CFD-BasicVSR++: 7.5M params, 87ms per frame on 180×320 (100 frames avg); VRT(35.6M, 243ms) 대비 39% 런타임
- **Datasets:** REDS, Vimeo-90K (training); REDS4, Vid4, Vimeo-T, UDM10 (evaluation)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): Bicubic(BI)과 Blur Downsampling(BD) 합성 열화만 다루며, H.264/265 압축이나 CCTV 실제 열화에 대한 대응 없음. 2. 배포가능성(8/25): CFD-BasicVSR++ 기준 87ms/frame(180×320)으로 비교적 가볍지만, bidirectional 전파 방식으로 온라인/실시간 스트리밍 적용이 어려움. 3. 재현성/사용성(3/20): 코드 리포는 공개되어 있으나 라이선스 미지정, pretrained 가중치 미공개(To-do 상태), 실제 재현 불가. 4. 범용 장면 적용성(10/15): 표준 VSR 벤치마크(REDS, Vimeo, Vid4) 기반으로 일반 자연영상에 적용 가능한 범용 설계. 5. Scale/최신성 적합(7/15): x4 단일 스케일만 지원(교통 CCTV 업스케일 목적에는 x1.5~x2가 더 적합), 2024년 arXiv 논문으로 최신성은 양호.

#### [28점] FDAN (Flow-guided Deformable Alignment Network) — arXiv 2021 (2021)
- **논문:** [링크](https://arxiv.org/abs/2105.05640) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — 논문 및 검색 결과 어디에도 pretrained 가중치 공개 링크 없음
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** 총 파라미터 8.97M (정렬 모듈 3.20M), FLOPs: 0.23T (Vimeo90K-T), FPS 미보고
- **Datasets:** Vimeo90K (학습), Vimeo90K-T, UDM10, SPMCS, Vid4 (평가)
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): Gaussian blur(σ=1.6)+bicubic 4× 다운샘플링 합성 열화만 사용. H.264/265 압축·CCTV 실제 열화 미대응. 2. 배포가능성 (5/25): 파라미터 8.97M으로 비교적 소형이나, FPS 미보고, 온라인/스트리밍/실시간 언급 없음. 오프라인 배치 처리 방식. 3. 재현성/사용성 (0/20): 공식 코드 저장소 없음, pretrained 가중치 공개 없음. 4. 범용 장면 적용성 (13/15): Vimeo90K·UDM10·Vid4 등 일반 자연영상 벤치마크 사용, 도메인 특화 아님. 5. Scale/최신성 적합 (10/15): x4 단일 스케일(×1.5~×2 미지원), 2021년 논문이나 코드 미공개로 재현 불가. 총합: 약 28점. 합성 열화 전용 + 코드/가중치 미공개로 실제 CCTV 적용성 매우 낮음.

#### [28점] VESPCN — CVPR 2017 (2017)
- **논문:** [링크](https://arxiv.org/abs/1611.05250) · **코드:** ✗ 없음 · **License:** — · **Framework:** PyTorch
- **Pretrained:** no — No official pretrained weights from authors; unofficial reimplementations (JuheonYi/VESPCN-tensorflow, JuheonYi/VESPCN-PyTorch) also do not provide pretrained weights.
- **Scale:** x3, x4 · **Real-world:** △ 합성 · **효율:** ~0.88M params; 24.23 GOps/1080p frame at x3, 14.00 GOps/1080p frame at x4; ESPCN baseline runs at ~29ms/frame on K2 GPU; designed for real-time processing
- **Datasets:** Training: CDVL; Evaluation: Vid4
- **관련성 근거:** 1. 실세계/blind 열화 강인성 (0/25): bicubic 합성 다운샘플링만 사용하며 H.264/265 압축, CCTV 노이즈 등 실제 열화에 대한 대응은 없음. 2. 배포가능성 (12/25): 경량 설계(~0.88M params)로 실시간 처리를 목표로 하며 K2 GPU에서 29ms/frame 수준이나, 2017년 설계로 최신 실시간 기준에는 미흡. online/causal 방식으로 볼 수 있어 PTZ 라이브 적용 가능성 있음. 3. 재현성/사용성 (2/20): 공식 코드 및 pretrained 가중치 미공개. 비공식 구현(JuheonYi)만 존재하며 불완전함. 라이선스도 불명확. 4. 범용 장면 적용성 (8/15): Vid4 등 일반 자연영상 비디오 벤치마크로 평가하여 거리/교통 장면에도 적용 가능하나 특별한 도메인 최적화 없음. 5. Scale/최신성 적합 (6/15): x3, x4 지원으로 목표 해상도(720p→1080p, ~x1.5) 대비 오버스케일이며 2017년 논문으로 최신성 낮음. 총합: 28/100 — 실세계 열화 미대응, 공식 코드/가중치 없음, 오래된 논문으로 실제 CCTV 업스케일 활용에 약함.

#### [22점] UnderstandDefAlign — arXiv 2020 (2020)
- **논문:** [링크](https://arxiv.org/abs/2009.07265) · **코드:** [https://github.com/ckkelvinchan/offset-fidelity-loss](https://github.com/ckkelvinchan/offset-fidelity-loss) · **License:** none/unspecified · **Framework:** PyTorch
- **Pretrained:** no — No pretrained weights in the repo; only the offset_fidelity_loss.py utility module is provided for integration into other codebases (MMEditing/BasicSR).
- **Scale:** x4 · **Real-world:** △ 합성 · **효율:** n/a
- **Datasets:** REDS, Vimeo-90K (experiments conducted on top of EDVR)
- **관련성 근거:** 1. 실세계/blind 열화 강인성(0/25): 논문 전체가 bicubic 열화 기반 합성 벤치마크(REDS, Vimeo-90K x4) 위에서 deformable alignment 메커니즘을 분석하는 연구로, H.264/265 압축·CCTV 실제 열화 대응은 전혀 고려하지 않음. 2. 배포가능성(2/25): 분석 논문이며 전체 모델 코드가 없음. loss 함수 하나만 제공하므로 배포·실시간 활용 불가. 3. 재현성/사용성(8/20): 코드 공개(loss 모듈 단독)되어 있으나 전체 모델/학습 코드 미제공, pretrained 가중치 없음, 라이선스 미명시. 부분적 재현성. 4. 범용 장면 적용성(7/15): EDVR 기반 분석이므로 일반 자연영상에 원리적으로 적용 가능하나, 독립 모델이 아닌 분석+loss 기여에 그침. 5. Scale/최신성 적합(5/15): x4 단일 scale 지원, 2020년 arXiv/AAAI 2021 발표로 비교적 구형. arbitrary-scale 미지원.
