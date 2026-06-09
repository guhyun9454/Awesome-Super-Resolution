# EgoXtreme: A Dataset for Robust Object Pose Estimation in Egocentric Views under Extreme Conditions

- **Model / Abbrev:** EgoXtreme
- **Venue:** CVPR 2026 (2026)
- **Category:** Other/Tangential
- **Paper:** <https://arxiv.org/abs/2603.25135>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
Existing 6D object pose estimation benchmarks are captured under clean, well-lit conditions and do not reflect the perceptual degradations of real-world egocentric (first-person) deployment. EgoXtreme targets robust 6D pose estimation when egocentric views suffer extreme conditions: low light / dynamic illumination, heavy motion blur from fast head/hand motion, and smoke-induced occlusion.

## Method
This is a dataset/benchmark paper, not a new model. The authors capture a large-scale egocentric dataset using Project Aria glasses (1408x1408 raw fisheye RGB plus undistorted versions, 30 fps) across three real-world scenarios — industrial maintenance, sports, and emergency rescue — deliberately injecting severe degradations (8 illumination conditions, motion blur, smoke). They benchmark recent state-of-the-art generalizable / RGB-only zero-shot 6D pose estimators on the data, and additionally probe two mitigation strategies: (1) classical image-restoration / deblurring as a preprocessing front-end, and (2) temporal tracking-based pose approaches that exploit inter-frame information for fast-motion clips.

## Key Contributions
- EgoXtreme dataset: ~1.3M frames, ~775.5 min (~12.9 h) at 30 fps, captured with Aria glasses; 1408x1408 raw fisheye RGB plus undistorted views
- 15 participants interacting with 13 objects (sports gear, assembly blocks, emergency supplies) across 3 scenarios (industrial maintenance, sports, emergency rescue)
- Controlled extreme-condition coverage: 8 illumination settings, heavy motion blur, and smoke in specific scenes — degradations absent from prior 6D pose benchmarks
- Benchmark of SOTA generalizable RGB-only zero-shot pose estimators showing generalization collapses under extreme conditions, especially low light
- Evaluation of mitigation baselines: image restoration/deblurring (found ineffective) and temporal tracking (promising for motion-heavy cases)

## Results
Qualitative/diagnostic findings rather than a single headline metric on the public pages: existing pose estimators perform reasonably under standard conditions but degrade sharply under motion blur, low light, and smoke (worst under low light). Naive image-restoration/deblurring preprocessing gave no improvement; temporal tracking-based methods showed promise in fast-motion scenarios. (Numeric ADD/AR/recall tables are in the paper PDF, not surfaced on the project page.) Reported as a CVPR 2026 Highlight.

## Significance
Establishes the first egocentric 6D pose benchmark explicitly stress-testing extreme perceptual conditions, exposing that current zero-shot/generalizable pose estimators are not robust to real-world degradation and that off-the-shelf image restoration does not bridge the gap — motivating degradation-aware and temporally-grounded pose methods. For an SR/restoration survey it is tangential: it uses deblurring only as an evaluated (and failing) baseline rather than proposing a restoration method.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*