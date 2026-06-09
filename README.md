# Glioma_MDC_Mitosis_Detection
# Detection and Classification of Mitotic Figures in Glioma Histopathology Images Using Deep Learning

## Overview

This project presents a deep learning pipeline for automated detection and classification of mitotic figures in H&E-stained glioma histopathology images.

The work was developed for the **Glioma Mitosis Domain Generalization Challenge 2025 (Glioma MDC 2025)** and focuses on building a robust system capable of generalizing across unseen clinical domains.

Manual mitosis counting is a critical component of glioma grading but is time-consuming and subject to inter-observer variability. This project leverages modern computer vision architectures and ensemble learning techniques to provide an automated and scalable solution.

---

## Results

| Metric | Score |
|----------|----------|
| EfficientNet-B4 Baseline F1 | 0.90 |
| ConvNeXtV2 F1 | 0.951 |
| Vision Transformer F1 | 0.955 |
| Vision Mamba F1 | 0.953 |
| Final Ensemble Validation F1 | 0.965 |
| Kaggle Public Leaderboard F1 | 0.944 |

---

## Project Architecture

![Pipeline](assets/architecture.png)

The pipeline consists of:

1. Data Loading
2. Annotation Parsing
3. ROI Extraction
4. Data Augmentation
5. Model Training
6. Cross Validation
7. Ensemble Learning
8. Test Time Augmentation
9. Final Inference

---

## Dataset

### Source

Glioma Mitosis Domain Generalization Challenge (Glioma MDC 2025)

### Training Data

- 1,439 Histopathology Images
- 1,616 Annotated ROIs
- 889 Mitotic Regions
- 727 Non-Mitotic Regions

### Image Information

- Original Image Size: 512×512
- ROI Patch Size: 224×224

### Classes

| Label | Class |
|---------|---------|
| 0 | Non-Mitosis |
| 1 | Mitosis |

---

## Data Preprocessing

### Polygon to Bounding Box Conversion

The dataset annotations are provided in LabelMe JSON format using polygon coordinates.

Each polygon is converted into an axis-aligned bounding box:

- Extract polygon vertices
- Compute xmin, ymin, xmax, ymax
- Remove degenerate bounding boxes
- Crop ROI
- Resize to 224×224

### Augmentations

Training augmentations:

- Horizontal Flip
- Vertical Flip
- Random Rotation
- Random Resized Crop
- Color Jitter
- ImageNet Normalization

These augmentations improve robustness against stain variations and domain shifts.

---

## Exploratory Data Analysis

Performed:

- ROI Class Distribution Analysis
- Image-Level Distribution Analysis
- ROI Size Distribution
- ROIs per Image Distribution
- Dataset Statistics

---

## Validation Strategy

### Stratified Group K-Fold

A 5-Fold Stratified Group K-Fold strategy was used.

Advantages:

- Maintains class balance
- Prevents data leakage
- Ensures ROIs from the same image remain within the same fold

This was critical for obtaining reliable validation scores.

---

## Models

### EfficientNet-B4 (Baseline)

Used as an initial benchmark to validate the preprocessing and training pipeline.

Features:

- Compound scaling
- ImageNet pretrained weights
- Efficient architecture

Validation F1:

```text
0.90
```

---

### ConvNeXt V2

Modern CNN architecture incorporating:

- Global Response Normalization (GRN)
- FCMAE Pretraining
- Improved feature extraction

Validation F1:

```text
0.951
```

---

### Vision Transformer (ViT)

Features:

- Self-attention
- Global contextual learning
- ImageNet21K pretraining

Validation F1:

```text
0.955
```

---

### Vision Mamba

Features:

- Selective State Space Models
- Long-range dependency modeling
- Linear complexity

Validation F1:

```text
0.953
```

---

## Ensemble Learning

Fifteen models were used:

```text
5 ConvNeXtV2
5 Vision Transformers
5 Vision Mamba
```

Final weighted ensemble:

```python
Ensemble =
0.32 × ConvNeXtV2
+
0.36 × Vision Transformer
+
0.32 × Vision Mamba
```

---

## Test Time Augmentation (TTA)

Seven augmentations were used during inference:

1. Original
2. Horizontal Flip
3. Vertical Flip
4. 90° Rotation
5. 180° Rotation
6. 270° Rotation
7. Horizontal + Vertical Flip

Predictions from all augmentations were averaged.

---

## Repository Structure

```text
Glioma-Mitosis-Detection/
│
├── notebooks/
│   ├── 01_EDA.ipynb
│   ├── 02_Data_Preprocessing.ipynb
│   ├── 03_Stratified_Group_KFold.ipynb
│   ├── 04_EfficientNet_Baseline.ipynb
│   ├── 05_ConvNeXtV2_Training.ipynb
│   ├── 06_VisionTransformer_Training.ipynb
│   ├── 07_VisionMamba_Training.ipynb
│   ├── 08_Ensemble_TTA.ipynb
│   └── 09_Final_Inference.ipynb
│
├── src/
├── assets/
├── results/
├── docs/
├── requirements.txt
└── README.md
```

---

## Technologies Used

### Programming

- Python 3.12

### Deep Learning

- PyTorch
- timm

### Data Processing

- NumPy
- Pandas
- Pillow

### Augmentation

- Albumentations

### Evaluation

- Scikit-Learn

### Visualization

- Matplotlib

### Platform

- Kaggle GPU (Tesla T4)

---

## Key Contributions

- Designed complete preprocessing pipeline
- Developed ROI extraction system
- Implemented Stratified Group K-Fold validation
- Trained 15 deep learning models
- Built weighted ensemble framework
- Implemented Test Time Augmentation
- Performed extensive experimentation
- Improved robustness against domain shift

---

## Future Work

Potential improvements include:

- Whole Slide Image (WSI) Analysis
- Stain Normalization
- Detection + Classification Framework
- Self-Supervised Learning
- UNI Foundation Models
- CONCH Foundation Models
- Multi-Domain Training

---

## Applications

- Computer-Aided Diagnosis
- Automated Tumor Grading
- Digital Pathology
- Clinical Decision Support
- Computational Pathology Research

---

## Author

Namra Maniar

B.Tech Computer Science and Engineering

Ahmedabad University

2026
