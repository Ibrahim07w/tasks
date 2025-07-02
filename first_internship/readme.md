# 🔊 Deep Fake Audio Classifier

A lightweight deep learning model for detecting deepfake (spoofed) audio using mel-spectrogram features and a customized SpecRNet-inspired architecture. Designed for **real-time** performance and mobile/web deployment.

---

## 📚 Part 1: Research & Model Selection

### 🎯 Goal
Detect synthetic audio with high accuracy and fast inference time, suitable for real-time applications such as live interviews, voice-based authentication, or anti-spoofing locks.

### 🔍 Research Summary

Audio Deepfake Detection (ADD) typically involves two stages:
- **Feature Extraction**: Mel-spectrograms, LFCC, MFCC, or learned embeddings via autoencoders or ResNet blocks.
- **Classification**: DNNs, RNNs, CNNs, or traditional ML models like XGBoost/SVM.

### 🧠 Model Exploration

- **Option 1: XGBoost / SVM with GMM Features**  
  ✅ Fast training and inference  
  ❌ Poor generalization, complex preprocessing, not scalable

- **Option 2: GANs**  
  ❌ Training instability, high computational cost, weak research support for ADD

- **Option 3: Deep Neural Networks (CNNs / RNNs)**  
  ✅ Better generalization, pretrained models available  
  ✅ SpecRNet with LFCC inspired architecture seemed promising  
  ✅ Optimized with mel-spectrogram input for better frequency coverage  
  ❌ Transformers perform better but are too heavy for real-time use

- **Final Decision**: Use a **lightweight CNN-based SpecRNet-inspired model** with mel-spectrogram input and custom hyperparameters (e.g., neurons per layer, number of blocks).

---

## 🛠️ Part 2: Implementation

### 🔽 Dataset
- Used a **subset** of the [ASVspoof 2021 (ASVspoof5)](https://datashare.ed.ac.uk/handle/10283/4176) dataset (limited to ~6GB due to resource constraints).

### ⚙️ Preprocessing
- Extracted and labeled audio
- Converted to **mel-spectrograms**
- Applied **augmentation techniques**:
  - Additive noise
  - Time shifting
  - Pitch variation
  - Random cropping

### 🧪 Model Architecture

- **Input**: Mel-spectrograms
- **Backbone**: Custom CNN with SpecRNet principles
  - Convolutional layers with stride 3
  - **SeLU activation** (self-normalizing, no batchnorm)
  - Doubling neurons per block
- **Classifier**:
  - Dense layer → Dropout → Sigmoid output
- **Parameters**: < 400,000

---

## 📄 Part 3: Documentation

### 🚧 Challenges
- Understanding **Fourier Transforms** and **mel scale math** for audio preprocessing
- Overcame through:
  - YouTube (especially 3Blue1Brown)
  - Medium articles
  - Research papers (2020–2024)

### 🧠 Assumptions
- Model must support **low-latency inference**
- No high computational overhead
- Preprocessing must not bottleneck real-time use

### ✅ Results

- **Accuracy**: ~85% on small data subset
- **Inference Time**: Fast and suitable for real-time environments
- **Strengths**:
  - Lightweight and fast
  - Handles basic spoofing tricks well
  - Augmentation improves generalization

- **Limitations**:
  - May fail against very advanced deepfake generators
  - Older architecture (SpecRNet 2022)
  - Only trained on small dataset

---

## 🚀 Future Work

- Train on full ASVspoof5 dataset
- Experiment with **lightweight transformers** (e.g., DistilBERT-style audio models)
- Improve augmentation with adversarial examples
- Deploy using **TensorFlow Lite** for:
  - Mobile apps
  - Web applications
  - Embedded systems

---

## 🧠 Key Takeaways

- Achieving real-time fake audio detection is possible with balanced tradeoffs.
- Lightweight CNNs with smart augmentation can outperform heavier models in real-world speed-sensitive scenarios.
- This project lays the groundwork for scalable, deployable deepfake detection tools.
