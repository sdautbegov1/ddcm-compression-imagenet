# DDCM Image Compression Evaluation

This repository contains notebooks used for a small-scale evaluation of **DDCM (Denoising Diffusion Codebook Models)** for image compression and image restoration tasks.

The project is based on the paper **"Compressed Image Generation with Denoising Diffusion Codebook Models"** and focuses on testing how images can be represented using sequences of selected codebook indices instead of storing the full image directly.

## Project Overview

The main goal of this project is to evaluate DDCM on a small ImageNet validation subset and analyze the trade-off between compression strength and reconstruction quality.

Two main types of experiments were performed:

1. **Latent-space image compression**

   * Input images: ImageNet validation subset resized to 512×512
   * Model: Stable Diffusion 2.1 base compatible checkpoint
   * Pipeline: image → VAE latent → DDCM compression → VAE decoder → reconstructed image

2. **Pixel-space super-resolution**

   * Input images: ImageNet validation subset resized to 256×256
   * Model: ImageNet 256×256 pixel-space diffusion checkpoint
   * Pipeline: degraded low-resolution image → DDCM restoration → reconstructed super-resolution image

## Tested Configurations

Several parameter configurations were tested by changing:

* `T` – number of diffusion timesteps
* `K` – codebook size
* `M` – number of codebook vectors used in matching pursuit
* `C` – number of bits used for coefficient quantization

The bit-stream length was computed using:

```text
L = (T - 1)(M log2(K) + C(M - 1))
```

where `L` represents the number of bits needed to store the selected codebook indices.

## Evaluation Metrics

The experiments were evaluated using:

* **BPP (Bits Per Pixel)** – compression rate
* **PSNR (Peak Signal-to-Noise Ratio)** – pixel-level reconstruction quality
* **LPIPS (Learned Perceptual Image Patch Similarity)** – perceptual similarity using a pretrained AlexNet-based LPIPS model
* **SSIM (Structural Similarity Index Measure)** – structure similarity, used mainly for super-resolution evaluation

## Repository Contents

The repository includes Jupyter notebooks used for:

* preparing ImageNet image subsets
* running DDCM compression experiments
* running super-resolution restoration experiments
* calculating PSNR, LPIPS, SSIM and BPP
* visualizing original, degraded and reconstructed images
* comparing different DDCM parameter configurations

## Notes

The experiments were performed on a limited number of images due to computational constraints, mostly using Google Colab. Therefore, the results should be interpreted as an indicative small-scale evaluation rather than a full benchmark.

The original DDCM implementation and diffusion checkpoints are not included in this repository. The notebooks are intended to document the experimental workflow and evaluation process used in the project.

## Main Conclusion

DDCM shows that an image can be represented as a compact sequence of codebook indices while still enabling visually meaningful reconstruction. Increasing the number of timesteps, codebook size, or using matching pursuit generally improves reconstruction quality, but also increases bitrate and computational cost.
