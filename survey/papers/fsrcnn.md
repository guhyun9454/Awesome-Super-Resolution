# Accelerating the Super-Resolution Convolutional Neural Network

- **Model / Abbrev:** FSRCNN
- **Venue:** ECCV 2016 (2016)
- **Category:** Efficient/Lightweight SR
- **Paper:** <https://arxiv.org/abs/1608.00367>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
SRCNN, while a strong CNN baseline for single-image SR, is too slow for real-time use: upsampling a 240x240 image by x3 runs at only ~1.32 fps (vs the 24 fps real-time target). Two structural causes are identified: (1) it operates on a bicubic-interpolated input, so all computation scales with the large HR spatial size, and (2) its non-linear mapping layer is a wide, expensive convolution.

## Method
FSRCNN redesigns SRCNN into an hourglass-shaped, fully convolutional network that takes the original LR image directly (no bicubic pre-upsampling) and upscales only at the very end via a learned deconvolution layer, so most computation runs at LR resolution. The pipeline has five parts: feature extraction Conv(5,d,1), shrinking Conv(1,s,d) to reduce feature dimension, m mapping layers Conv(3,s,s), expanding Conv(1,d,s) to restore dimension, and a final DeConv(9,1,d) for upsampling. PReLU activations replace ReLU for more stable training. The model is trained with a mean-squared (Euclidean) loss directly from LR to HR sub-images. Because all conv layers act as a shared LR feature extractor and only the deconvolution layer is upscale-specific, switching scale factors only requires fine-tuning the deconvolution layer.

## Key Contributions
- Moves the upsampling step to the network end via a learned deconvolution layer (replacing bicubic pre-interpolation), making cost proportional to LR spatial size rather than HR size
- Hourglass design with explicit shrinking (Conv 1x1) -> m small 3x3 mapping layers -> expanding (Conv 1x1) that drastically cuts parameters while keeping/raising quality
- Three governing hyperparameters d (feature dim/LR feature layers), s (shrinking dim), m (mapping depth); best setting FSRCNN(56,12,4) = 12,464 params
- PReLU activations for more stable training than ReLU
- Cross-scale transfer: only the deconvolution layer needs fine-tuning to change upscaling factor, sharing conv layers across scales
- Introduces a new General-100 training dataset (100 bmp images) used jointly with the classic 91-image set, plus data augmentation

## Results
On Set5 x3, FSRCNN(56,12,4) reaches 33.16 dB with only 12,464 parameters; the full FSRCNN attains a 41.3x speedup over SRCNN-Ex with superior PSNR. Even the smallest FSRCNN(48,12,2) hits 32.87 dB, beating SRCNN-Ex (32.75 dB). The lightweight FSRCNN-s = FSRCNN(32,5,1) has only 3,937 parameters, runs at 24.7 fps on a generic CPU (real-time, >24 fps) and still outperforms SRCNN(9-1-5). FSRCNN is at least 40x faster than SRCNN-Ex, SRF and SCN; reported as ~17.36x faster while running in real time. Evaluated on Set5, Set14, BSD200 for x2/x3/x4 (Tables 3-4); leads prior methods on PSNR especially at x2 and x3, slightly below SCN at x4 (SCN cascades two x2 models).

## Significance
FSRCNN established the now-standard efficient-SR principle of doing the heavy computation at LR resolution and upsampling only at the end with a learned (sub-pixel/deconvolution) layer, achieving real-time CPU SR with an order-of-magnitude speedup and better quality than SRCNN. Its hourglass shrink-map-expand structure and scale-shared backbone influenced many later lightweight and real-time SR networks.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*