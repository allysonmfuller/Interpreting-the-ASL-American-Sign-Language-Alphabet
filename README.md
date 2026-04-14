# 🤟 Interpreting the ASL (American Sign Language) Alphabet

> Deep Learning image classification of the ASL alphabet using PyTorch and transfer learning with ResNet.

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-orange?logo=pytorch)
![Accuracy](https://img.shields.io/badge/Test%20Accuracy-78.06%25-brightgreen)
![F1 Score](https://img.shields.io/badge/F1%20Score-78.43%25-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 📖 Overview

This project applies deep learning with PyTorch to classify hand gesture images across 29 classes of the American Sign Language (ASL) alphabet (A–Z, space, delete, nothing).

For many deaf or hard-of-hearing individuals, ASL is their **first language**. Real-time image-to-text or image-to-speech tools can bridge communication gaps, foster independence, and are especially critical in emergency situations. This project is a step toward that goal.

**Final model:** ResNet50 | **Test Accuracy:** 78.06% | **F1 Score:** 78.43%

---

## 📁 Dataset

- **Source:** [ASL (American Sign Language) Alphabet Dataset – Kaggle](https://www.kaggle.com/datasets/debashishsau/aslamerican-sign-languageaplhabet-dataset/data)
- **Classes:** 29 (A–Z, space, delete, nothing)
- **Original training samples:** 223,074 images
- **Subsampled:** 750 images per class (to reduce compute time)
- **Split used:** Training set only (500 train / 250 test per class), as the provided test set contained only 29 images

---

## ⚙️ Preprocessing

- Images resized to **224 × 224**
- Pixel values normalized using **ImageNet mean and standard deviation**
- **Data augmentation** applied to training set only:
  - Random horizontal flip
  - Random rotation
  - Random brightness and contrast adjustments
- Final split: **80% Training / 20% Validation** from the 500 training images per class

---

## 🏗️ Model Architecture

Two baseline models were compared:

| Feature | ResNet18 | ASL CNN (Custom) |
|---|---|---|
| Layers | 18 | 14 |
| Fully Connected Layers | 1 | 3 |
| Filter Depth | 64→512 | 32→256 |
| Skip Connections | ✅ | ❌ |
| Pooling | Global Avg + 1 MaxPool | MaxPool after every block |
| Batch Normalization | ✅ | ✅ |
| Dropout | ❌ | ✅ (50%, 30%) |
| Starting Weights | Pre-trained (ImageNet) | Random |
| Optimizer | Adam | Adam |

**Result:** ResNet18 (74% val accuracy) dramatically outperformed the custom ASL CNN (~3% val accuracy). All further tuning was done on the ResNet family.

---

## 🔬 Hyperparameter Tuning

Parameters were changed one at a time to isolate their effect:

| Model Version | Val Acc (%) | Accuracy (%) | F1 (%) |
|---|---|---|---|
| ResNet18 Baseline | 74.17 | 61.50 | 35.35 |
| CNN Baseline | 3.28 | 1.39 | 1.01 |
| ResNet34 | 75.24 | 69.00 | 39.82 |
| SGD Optimizer | 47.48 | 19.50 | 10.83 |
| 20 Epochs (vs 10) | 77.00 | 69.50 | 35.54 |
| Batch Size 50 (vs 32) | 73.28 | 69.80 | 60.08 |
| LR 0.01 (vs 0.001) | 69.59 | 62.50 | 37.84 |

**Key takeaways:**
- Adam optimizer significantly outperformed SGD
- Deeper models (ResNet34/50), more epochs, and larger batch sizes improved performance
- These parameters were prioritized for fine-tuning

---

## 🎯 Fine-Tuning Results

| Config | Val Acc (%) | Accuracy (%) | F1 (%) | Train Time (min) |
|---|---|---|---|---|
| ResNet34 | 74.52 | 74.52 | 74.35 | 11.19 |
| **ResNet50** | **76.34** | **76.34** | **76.39** | **12.29** |
| 25 Epochs | 77.41 | 77.41 | 77.29 | 28.04 |
| 30 Epochs | 77.59 | 78.59 | 77.78 | 33.57 |
| Batch Size 30 | 74.38 | 74.38 | 74.40 | 11.22 |

**Final configuration chosen:** ResNet50 + 25 Epochs + Batch Size 30 + Early Stopping + Adam Optimizer

---

## ✅ Final Results

The final model was evaluated on the held-out test set:

| Metric | Score |
|---|---|
| **Accuracy** | 78.06% |
| **Precision** | 80.98% |
| **Recall** | 78.06% |
| **F1 Score** | 78.43% |

The training and validation loss converged cleanly, and the close alignment between training, validation, and test accuracy indicates **strong generalization with no significant over- or underfitting**.

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install torch torchvision kaggle matplotlib scikit-learn
```

### Download Dataset

```bash
# Set up Kaggle API credentials first
mkdir -p ~/.kaggle
# Place your kaggle.json in ~/.kaggle/

kaggle datasets download -d debashishsau/aslamerican-sign-language-aplhabet-dataset
unzip -q aslamerican-sign-language-aplhabet-dataset.zip -d asl_dataset
```

### Run the Notebook

```bash
jupyter notebook INFO6147_PYTORCH_CAPSTONE_Allyson_Fuller.ipynb
```

---

## 📂 Repository Structure

```
├── INFO6147_PYTORCH_CAPSTONE_Allyson_Fuller.ipynb   # Main notebook
├── INFO6147_CAPSTONE_REPORT_Allyson_Fuller.pdf       # Full project report
├── INFO6147_PYTORCH_CAPSTONE_PRESENTATION_...pptx    # Presentation slides
└── README.md
```

---

## 📚 References

- [Deep Residual Learning for Image Recognition (He et al., 2015)](https://arxiv.org/abs/1512.03385)
- [ASL Alphabet Dataset – Kaggle](https://www.kaggle.com/datasets/debashishsau/aslamerican-sign-languageaplhabet-dataset/data)
- [ImageNet Dataset](https://www.image-net.org/download.php)
- [PyTorch CrossEntropyLoss Docs](https://docs.pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html)
- [History of Closed Captions – Rev](https://www.rev.com/blog/history-of-closed-captions)

---

## 👩‍💻 Author

**Allyson Fuller** | Student ID: 0763664  
INFO6147 – Deep Learning With PyTorch | April 2026
