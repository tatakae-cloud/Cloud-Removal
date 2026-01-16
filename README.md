# 🌍 Cloud-Free Satellite Imagery Generation Using Deep Learning

This repository presents a deep learning–based pipeline for generating cloud-free
optical satellite imagery from multi-temporal satellite image time series.

The project is inspired by the research paper  
**“Creating Cloud-Free Satellite Imagery from Image Time Series with Deep Learning”**
(Oehmcke et al., BIGSPATIAL 2020), and extends it through a dual-path reconstruction
strategy combining classical convolutional architectures with modern foundation
models for Earth observation.

---

## 📌 Project Overview

Optical satellite imagery is frequently affected by clouds, shadows, and missing
data, significantly limiting its usability for environmental monitoring,
agriculture, urban analysis, and disaster management.

This project addresses the problem by:
- Leveraging temporal information from satellite image time series
- Learning cloud-aware temporal aggregation instead of static compositing
- Reconstructing high-quality cloud-free images using deep neural networks

The pipeline is designed to handle **variable-length time series** and does not
require pre-filtered cloud-free imagery during training.

---

## 🧠 Methodology

### 🔹 TimeGate Temporal Aggregation

A TimeGate mechanism is used to aggregate information from multiple time steps:
- Assigns pixel-wise importance weights to each temporal observation
- Suppresses cloud-covered and missing regions using learned gating
- Produces:
  - A weighted composite image
  - Abstract temporal feature maps encoding long-term information

This aggregation acts as a learned alternative to median-based compositing.

---

### 🔹 Reconstruction Pathways

Two complementary reconstruction strategies are implemented:

#### 1. TimeGate + U-Net
- Residual U-Net architecture with skip connections
- Learnable scalar γ controls the strength of residual corrections
- Focuses on refining cloud-covered and distorted regions
- Provides an efficient and stable reconstruction pipeline

#### 2. TimeGate + TerraMind
- Fine-tuning of the TerraMind foundation model for Earth observation
- Uses TimeGate aggregates instead of raw time series
- Enables exploration of large-scale pretrained models for cloud removal

---

## 📊 Evaluation

The system is evaluated using both quantitative and qualitative measures:

- **Mean Squared Error (MSE)**
- **Structural Similarity Index (SSIM)** (primary metric)
- **Peak Signal-to-Noise Ratio (PSNR)**
- Visual comparison of reconstructed outputs

The TimeGate + U-Net approach achieves strong structural similarity
(SSIM > 0.90) on validation data.

---

## ⚠️ Limitations

The approach relies on the availability of at least some cloud-free observations
within the temporal window. When the entire time series is affected by **very thick
or persistent cloud cover**, the model may fail to accurately reconstruct the
underlying surface.

This limitation is inherent to temporal reconstruction methods that depend on
visual information from optical imagery.

---

## 📂 Repository Structure

```
Cloud-Removal/
│
├── 2020_AMC_Creating_Cloud_Free_Satellite_Imagery.pdf
│   # Reference research paper (AMC 2020)
│
├── PE_Report_Cloud_Removal.pdf
│   # Detailed project / performance evaluation report
│
├── README.md
│   # Project overview, methodology, and usage instructions
│
├── Temporal_Features_extractor.ipynb
│   # Extraction of temporal features from multi-date satellite imagery
│
├── Time_Gate_Algo_CNN/
│   ├── TimeGate_ALGO_CNN.ipynb
│   # CNN-based TimeGate aggregation approach (work in progress)
│
├── Timegate_ALGO_determenistic2.ipynb
│   # Deterministic (non-learning) TimeGate baseline method
│
├── UNET_Training.ipynb
│   # U-Net model training and validation for cloud removal
│
├── Terramind_Finetune.ipynb
│   # Fine-tuning TerraMind model for cloud-free image generation
│
├── Testing_using_GlobalTerramind_weights.ipynb
│   # Evaluation using pretrained/global TerraMind weights
│
└── .gitignore


```

---

## 📁 Dataset

The satellite image time series dataset used in this project was obtained from the
Technical University of Munich’s Mediatum repository:

🔗 https://mediatum.ub.tum.de/1639953

The dataset contains multi-temporal optical satellite imagery along with cloud
annotations suitable for cloud removal and reconstruction tasks.

---

## 🔗 References

- Oehmcke et al., *Creating Cloud-Free Satellite Imagery from Image Time Series with
  Deep Learning*, BIGSPATIAL 2020
- TerraMind Foundation Model – IBM & ESA  
  https://huggingface.co/ibm-esa-geospatial/TerraMind-1.0-base

---

## 🚀 Future Directions

- Integration of SAR imagery to handle persistent cloud cover
- Expansion to additional satellite platforms
- Real-time cloud-free image generation pipelines
- Ensemble approaches combining multiple reconstruction models
