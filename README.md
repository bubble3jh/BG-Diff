<h1 align="center">Few-Shot PPG Signal Generation<br>via Guided Diffusion Models</h1>

<p align="center">
  <b>IEEE Sensors Journal, 2024</b><br>
  Jinho Kang, Yongtaek Lim, KyuHyung Kim, Hyeonjeong Lee, KwangYong Kim, Minseong Kim, Jiyoung Jung, Kyungwoo Song
</p>

<p align="center">
  <a href="https://ieeexplore.ieee.org/document/10680298"><img src="https://img.shields.io/badge/Paper-IEEE%20Xplore-00629B" alt="Paper"></a>
  <img src="https://img.shields.io/badge/IEEE%20Sensors%20Journal-2024-8A2BE2" alt="IEEE Sensors Journal 2024">
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?logo=pytorch&logoColor=white" alt="PyTorch">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/dac35b7c-00ba-4050-b8cf-3a501ae2b85f" width="90%" alt="Overall framework of the Bi-Guided Diffusion Model">
</p>

Official PyTorch implementation of **BG-Diff**, a Bi-Guided Diffusion Model that generates PPG signals with the
blood pressure characteristics of a chosen group from only a few samples per group.

- **Few-shot generation for every blood pressure group.** Guidance steers the diffusion model toward minority
  groups that an imbalanced dataset would otherwise leave out.
- **Bi-guidance.** A discrete group condition and a continuous label condition guide the sampling together, so the
  generated signals are valid for the target group.
- **Post-processing that helps every training recipe.** The generated data improve training from scratch, transfer
  learning, and linear probing on BCG, PPGBP, and SENSORS.

## Overview

<p align="center">
  <img src="https://github.com/user-attachments/assets/a5f33682-04f3-4c07-a1b5-a2d593cd1914" width="85%" alt="Guided generation toward minority groups">
</p>

<p align="center"><em>In an imbalanced dataset, guidance directs the diffusion model to generate for minority groups.</em></p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/a754732f-798b-4cc6-9248-4fa5bb720b84" width="85%" alt="Generated data cover the distribution of unseen real data">
</p>

<p align="center"><em>Trained and sampled in a few-shot setting, the model still covers the distribution of unseen real data.</em></p>

## Getting started

### Installation

```bash
conda env create -f environment.yml
```

### Data

Datasets follow the layout of [bp-benchmark](https://github.com/inventec-ai-center/bp-benchmark).

```
├── bp-algorithm
├── datasets
│   ├── splits
│   │   ├── bcg_dataset
│   │   │   ├── feat_fold_0.csv
│   │   │   ├── ...
│   │   │   └── feat_fold_4.csv
```

### Running

```bash
# train and generate synthetic PPG with the default options
python main.py
```

To evaluate the generated PPG signals, use [bp-benchmark](https://github.com/inventec-ai-center/bp-benchmark).

### Arguments

| Argument | Default | Description |
|---|---|---|
| `--seed` | `1000` | Random seed |
| `--benchmark` | `bcg` | Dataset to use |
| `--num_samples` | `5` | Number of samples in the few-shot setting |
| `--seq_length` | `625` | Sequence length of the signal |
| `--train_batch_size` | `32` | Training batch size |
| `--diffusion_time_steps` | `2000` | Number of diffusion steps |
| `--train_num_steps` | `32` | Number of training steps |
| `--train_lr` | `8e-5` | Learning rate |
| `--sample_only` | off | Skip training and only sample |
| `--sample_batch_size` | | Batch size for sampling |
| `--target_group` | `-1` | Group to sample: `-1` all, `0` hypo, `1` normal, `2` prehyper, `3` hyper2, `4` crisis |
| `--regressor_scale` | `1.0` | Scale of the regressor guidance |

## Abstract

Recent advancements in deep learning for predicting Arterial Blood Pressure (ABP) have prominently featured photoplethysmography (PPG) signals. Notably, PPG signals exhibit significant variability due to differences in measurement environments, alongside stark disparities in the distribution of collected signal data among different labels. To address these challenges, this study introduces a Bi-Guided Diffusion Model designed to generate PPG signals with expected features of ABP within a few-shot setting for each label group. We propose a guided diffusion model architecture that simultaneously considers both the determinant group condition and the continuous label condition for each group in a few-shot setting. To our knowledge, this is the first study to use a diffusion model for generating PPG signals with a limited dataset. Initially, we categorized them into four groups based on SBP and DBP values: Hypo, Normal, Prehyper, and Hyper2. In each group, we sample an equal number of data points according to the few-shot setting and then generate appropriate PPG signals for each group through guidance. Additionally, our study proposes a post-processing technique to address the limitations of generative models in few-shot settings, consistently boosting performance across various methods such as training from scratch, transfer learning, and linear probing. When benchmarked, our methodology demonstrated performance improvements across all datasets, including BCG, PPGBP, and SENSORS. We confirmed data quality by comparing training, generated, and actual data. We analyzed error cases, morphology features, and t-SNE distribution to highlight the role of synthetic data in enhancing performance.

## Citation

```bibtex
@article{kang2024few,
  title={Few-Shot PPG Signal Generation via Guided Diffusion Models},
  author={Kang, Jinho and Lim, Yongtaek and Kim, KyuHyung and Lee, Hyeonjeong and Kim, KwangYong and Kim, Minseong and Jung, Jiyoung and Song, Kyungwoo},
  journal={IEEE Sensors Journal},
  year={2024},
  publisher={IEEE}
}
```

The lab-maintained copy of this repository is at [MLAI-Yonsei/BG-Diff](https://github.com/MLAI-Yonsei/BG-Diff).
