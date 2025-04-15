# 🧫 Automated Mitosis Detection using Bag-of-Visual-Words and Classical Image Processing

This project presents a classical computer vision pipeline for detecting mitosis instances in breast cancer histopathological images. It combines background segmentation, handcrafted features, and a Bag-of-Visual-Words (BoVW) model with a Random Forest classifier to automate mitotic figure identification.

## 📌 Table of Contents

- [Introduction](#introduction)
- [Dataset](#dataset)
- [Pipeline Overview](#pipeline-overview)
  - [1. Preprocessing & Segmentation](#1-preprocessing--segmentation)
  - [2. Feature Extraction](#2-feature-extraction)
  - [3. Classification](#3-classification)
- [Results](#results)
- [Installation](#installation)
- [Usage](#usage)
- [Future Work](#future-work)

---

## 📖 Introduction

Mitotic figures in breast cancer histology slides are key indicators of tumor proliferation. Manual counting is time-consuming and inconsistent. This project proposes a low-resource automated detection approach using classical image processing and machine learning techniques.

---

## 📂 Dataset

We use breast cancer histopathology slides from five patients (A00–A04). Each image is 2084 × 2084 pixels and comes with CSV annotations of mitosis coordinates.

- **Train**: A01, A03, A04  
- **Validation**: A02  
- **Test**: A00

---

## 🔍 Pipeline Overview

### 1. Preprocessing & Segmentation

- Convert image to grayscale and apply Gaussian blur.
- Segment regions that are significantly darker than the local mean intensity using adaptive thresholding.
- Apply morphological operations (opening, closing, dilation) to clean up noise and isolate nuclei.

### 2. Feature Extraction

Each segmented cell region is represented by a hybrid feature vector containing:

- **Texture Features**: LBP, GLCM (contrast, energy, homogeneity, correlation)
- **Shape Features**: Aspect ratio, solidity, perimeter, circularity
- **Color Features**: Mean and std in HSV space
- **BoVW Histogram**: Using SIFT descriptors clustered into 100 visual words via KMeans

### 3. Classification

- Random Forest Classifier with class balancing:
  ```python
  RandomForestClassifier(
      n_estimators=200,
      max_depth=20,
      min_samples_leaf=3,
      min_samples_split=4,
      class_weight="balanced"
  )
