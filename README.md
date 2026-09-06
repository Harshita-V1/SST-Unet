# SST-UNet: Hyperspectral WEEE Material Segmentation

This repository contains the implementation of **SST-UNet (Spectral-Spatial Transformer U-Net)** for hyperspectral image-based semantic segmentation and identification of materials in **Waste Electrical and Electronic Equipment (WEEE)**.

SST-UNet combines spectral band attention, a U-Net encoder-decoder, and a spectral-spatial Transformer bottleneck to learn discriminative spectral information and long-range spatial dependencies for automated WEEE material identification and sorting.

## Overview

Accurate identification of materials in WEEE is important for automated recycling, material separation, and resource recovery. Hyperspectral imaging provides rich spectral information that can help distinguish materials with similar visual appearance.

The proposed SST-UNet framework includes:

- Spectral Band Attention for learning the importance of spectral bands.
- U-Net based encoder-decoder architecture for pixel-level segmentation.
- Transformer bottleneck for modelling long-range spectral-spatial dependencies.
- Auxiliary material-presence detection branch for feature learning during training.
- Overlapping patch-based processing for training and full-image reconstruction.
- Four-way flip test-time augmentation (TTA) during inference.

## Dataset

Experiments are conducted on the **TECNALIA WEEE Hyperspectral Dataset**.

The dataset contains hyperspectral images of WEEE materials with:

- 76 spectral bands
- Spectral range of approximately 415–1008 nm
- 13 hyperspectral scenes
- Six material classes used in this work:
  - Background
  - Copper
  - Brass
  - Aluminum
  - Steel
  - White Copper

The experimental setup uses 10 scenes for training and 3 scenes for validation.

## Model Architecture

The SST-UNet architecture consists of:

1. Spectral Band Attention
2. U-Net Encoder
3. Spectral-Spatial Transformer Bottleneck
4. U-Net Decoder
5. Pixel-wise Segmentation Head
6. Auxiliary Material-Presence Detection Head

The spectral band attention module learns adaptive weights for the available spectral bands before spatial feature extraction. The Transformer bottleneck uses multi-head self-attention to model long-range spatial dependencies.

## Training Configuration

| Parameter | Value |
|---|---|
| Input spectral bands | 76 |
| Number of classes | 6 |
| Patch size | 64 × 64 |
| Patch stride | 32 |
| Training epochs | 150 |
| Optimizer | AdamW |
| Learning rate | 1 × 10⁻⁴ |
| Weight decay | 1 × 10⁻⁴ |
| Gradient clipping | 1.0 |
| Learning-rate schedule | Linear warm-up + Cosine decay |
| Loss function | Cross-Entropy + Dice + 0.3 × BCE |
| Test-time augmentation | Four-way flip TTA |

## Results

The proposed SST-UNet achieves the following performance on the TECNALIA WEEE hyperspectral dataset:

| Metric | Performance |
|---|---:|
| **mIoU** | **0.8100** |
| **Pixel Accuracy** | **94.4%** |

Final evaluation is performed using full-image inference with overlapping patch reconstruction. Predictions from overlapping patches are probability-averaged to obtain the final segmentation map.

## Ablation Study

The contribution of the main architectural components was evaluated using controlled ablation experiments.

| Model | mIoU | Pixel Accuracy |
|---|---:|---:|
| U-Net | 0.5847 | 89.24% |
| U-Net + Band Attention | 0.7097 | 92.75% |
| U-Net + Transformer | 0.6687 | 92.27% |
| **Proposed SST-UNet** | **0.7273** | **93.52%** |

The ablation results demonstrate the complementary contribution of spectral band attention and Transformer-based global contextual modelling.

## Dataset-Specific Implementations

The core SST-UNet architecture and training procedure remain consistent across the experiments.

Dataset-specific `.py` files are provided with minor parameter or preprocessing changes as required by the respective dataset. The corresponding implementation files follow the same overall architecture and processing pipeline.

## Reproducibility

To reproduce the experiments:

1. Install the required Python packages.
2. Download and prepare the TECNALIA WEEE hyperspectral dataset.
3. Update the dataset path in the corresponding code or configuration file.
4. Run the training notebook or Python implementation.
5. Evaluate the trained model using the full-image inference procedure.
6. Generate the qualitative segmentation results using the saved checkpoint.

The inference configuration and saved predictions can also be used to reproduce the qualitative segmentation outputs without retraining the model.

## Inference Pipeline

The inference procedure consists of:

1. Hyperspectral image loading.
2. Spectral preprocessing and normalization.
3. Overlapping 64 × 64 patch extraction.
4. SST-UNet prediction for each patch.
5. Four-way flip test-time augmentation.
6. Probability averaging across overlapping patches.
7. Full-image segmentation reconstruction.
8. Pixel-wise material classification.

## Qualitative Results

Representative qualitative results include:

- False-color HSI composite
- Ground-truth material labels
- SST-UNet predicted segmentation
- Error map

The qualitative outputs demonstrate the ability of SST-UNet to recover the spatial structure of different WEEE material regions.

## Application

The proposed framework is intended as an AI-based material identification component for automated WEEE sorting and resource recovery systems.

Potential applications include:

- Automated WEEE sorting
- Robotic material picking
- Material-specific separation
- Electronic waste recycling facilities
- Material-flow characterization
- Metal and resource recovery systems

The present work focuses on algorithmic validation using the TECNALIA WEEE hyperspectral dataset. Practical deployment would require integration with hyperspectral sensing, conveyor systems, and/or robotic sorting equipment.

## Requirements

The implementation uses Python and PyTorch together with commonly used scientific and machine-learning libraries.

Install the required dependencies using:

```bash
pip install -r requirements.txt
