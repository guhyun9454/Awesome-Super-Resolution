# Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network

- **Model / Abbrev:** SRGAN (generator: SRResNet)
- **Venue:** CVPR 2017 (2017)
- **Category:** GAN / Perceptual SR
- **Paper:** <https://arxiv.org/abs/1609.04802>
- **Status:** ✅ Surveyed — read in full

> Part of the **[SR Survey index](../README.md)**. See also the combined cross-paper report: **[SuperResolution_2026_Report.md](../SuperResolution_2026_Report.md)**.

## Problem
At large upscaling factors (4x), MSE-optimized SR networks achieve high PSNR but produce overly smooth, perceptually unsatisfying images lacking high-frequency texture detail. The ill-posed SR problem has many plausible HR solutions; minimizing pixel-wise MSE yields the smooth pixel-wise average of these solutions rather than a single photo-realistic one on the natural image manifold.

## Method
SRGAN is the first GAN framework for photo-realistic 4x SISR. The generator (SRResNet) is a deep residual CNN with B=16 residual blocks (3x3 conv, BN, ParametricReLU, skip-connections) plus sub-pixel (PixelShuffler) convolution layers for learned upsampling. It is trained against a VGG-style discriminator (8 conv layers, LeakyReLU alpha=0.2, strided convs, no max-pooling, two dense layers + sigmoid). The key innovation is a perceptual loss = content loss + 1e-3 * adversarial loss, where the content loss is replaced from pixel-wise MSE by a VGG feature-space loss (Euclidean distance between feature maps of a pre-trained VGG19, e.g. phi_5,4), making it invariant to pixel-space differences. The adversarial loss pushes solutions toward the natural image manifold. Generator is initialized from the trained MSE-SRResNet to avoid bad local optima.

## Key Contributions
- First GAN-based framework capable of inferring photo-realistic natural images at 4x upscaling
- SRResNet: a deep ResNet (16 residual blocks, skip-connections, sub-pixel conv upsampling) that sets new state of the art on PSNR/SSIM when optimized for MSE
- Novel perceptual loss combining a VGG19 high-level feature content loss with an adversarial loss, diverging from MSE to better match human perception
- Demonstrates via extensive MOS study that PSNR/SSIM fail to capture perceptual quality; SRGAN MOS scores are far closer to original HR than any prior method

## Results
On Set5/Set14/BSD100 at 4x: SRResNet-MSE sets SOTA PSNR/SSIM (Set5 32.05dB/0.9019; BSD100 27.58dB/0.7620). SRGAN trades PSNR for perception (Set5 29.40dB, BSD100 25.16dB) but wins MOS by a large margin: Set5 MOS 3.58, Set14 3.72, BSD100 3.56 vs original HR ~4.3-4.46 and next-best (DRCN/SRResNet) ~3.2-3.4. VGG54 (deep features) content loss gave the most convincing textures vs VGG22 (low-level). MOS rated by 26 raters scoring 1-5. Trained on 350k ImageNet images, Adam, NVIDIA Tesla M40.

## Significance
SRGAN is the seminal work introducing adversarial and perceptual (VGG feature) losses to super-resolution, shifting the field's objective from distortion (PSNR) to perceptual realism and foundational to nearly all subsequent perceptual/GAN SR (ESRGAN, Real-ESRGAN). It also empirically established that PSNR/SSIM are poor proxies for human-perceived quality.

---
*Generated from a dedicated research agent that read the paper directly (arXiv / project page). Quantitative figures are as reported by each paper; verify against the original tables before citing.*