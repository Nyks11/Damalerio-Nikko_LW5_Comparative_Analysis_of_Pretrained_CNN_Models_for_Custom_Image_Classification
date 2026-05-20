# Damalerio-Nikko_LW5_Comparative_Analysis_of_Pretrained_CNN_Models_for_Custom_Image_Classification

---

## 🔗 Google Colab Notebook

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1IZ0atnjOWi0o12ZCs08se3yJgIMACbTe)

https://drive.google.com/drive/folders/1DPVb5nO0JwZMxCC3br5sZHZqKjWo1wjy?usp=sharing

---

## 📘 Project Overview

This project presents a comparative analysis of multiple pre-trained Convolutional Neural Network (CNN) models for custom image classification using TensorFlow and Keras in Google Colab.

The study evaluates and compares:

- VGG16
- ResNet50
- EfficientNetB0
- Previous LW3 and LW4 custom models

The models were evaluated using:

- Accuracy
- Loss
- Precision
- Recall
- F1-Score
- ROC-AUC
- Confusion Matrix
- Grad-CAM Explainability

---

# 📊 PART 12: Model Performance Comparison Table

> All pre-trained models trained with frozen ImageNet weights + custom classification head.  
> Dataset: 80% train / 20% validation split, 224×224 input size, batch size = 32.  
> LW3/LW4 models re-evaluated on the same validation split.

| Model | Train Acc | Train Loss | Test Acc | Test Loss | Precision | Recall | F1-score | ROC AUC |
|---|---|---|---|---|---|---|---|---|
| **VGG16** | 69.81% | 1.0391 | 82.46% | 0.7904 | 0.8276 | 0.8276 | 0.8243 | 0.9833 |
| **ResNet50** | 95.59% | 0.2100 | 96.30% | 0.1761 | 0.9623 | 0.9647 | 0.9631 | 0.9986 |
| **EfficientNetB0** ⚠️ | 5.64% | 2.9949 | 6.16% | 2.9963 | 0.0025 | 0.0500 | 0.0047 | 0.4867 |
| **Teachable Machine** | ~99.00% | ~0.0200 | ~97.20% | ~0.1800 | ~0.9720 | ~0.9700 | ~0.9710 | ~0.9990 |
| **Your 1st Model (LW3 Baseline)** | 80.55% | 0.6508 | 85.31% | 0.5903 | 0.8557 | 0.8568 | 0.8530 | 0.9865 |
| **Your 2nd Model (LW4 Enhanced)** | 38.25% | 1.9527 | 43.70% | 1.9271 | 0.5797 | 0.4327 | 0.4385 | 0.8707 |
| **Your 3rd Model — The Good Model (LW4)** | 94.38% | 0.1716 | 77.25% | 0.5795 | 0.8379 | 0.7749 | 0.7792 | 0.9851 |

---

## ⚠️ EfficientNetB0 Failure Analysis

EfficientNetB0 achieved only **6.16% accuracy** due to a preprocessing incompatibility.

The model expects raw pixel inputs `[0,255]`, but the dataset was already normalized to `[0,1]`.  
Because EfficientNetB0 internally applies its own normalization layer, the inputs became double-normalized, causing near-zero activations and complete training failure.

---

# ❓ GUIDE QUESTIONS (FINAL REFLECTION)

---

## A. Model Performance

### 1. Which pre-trained model achieved the highest accuracy? Why?

ResNet50 achieved the highest accuracy at **96.30%**.

Its residual architecture uses skip connections that allow the network to learn deeper and more complex features while avoiding vanishing gradients. This makes it highly effective for distinguishing subtle tree species characteristics.

---

### 2. Which model had the lowest performance? What could be the reason?

EfficientNetB0 had the lowest performance at only **6.16% accuracy**.

The failure was caused by double normalization during preprocessing, which corrupted the image inputs before training.

---

### 3. How did loss values compare across models?

Lower loss values generally corresponded to better prediction confidence.

- ResNet50 had the lowest test loss (**0.1761**)
- VGG16 showed moderate loss (**0.7904**)
- EfficientNetB0 produced extremely high loss (**2.9963**)

---

## B. Evaluation Metrics

### 4. Why is accuracy not enough to evaluate a model?

Accuracy alone can be misleading because some classes may dominate the dataset.

Metrics such as Precision, Recall, and F1-score help determine whether the model performs consistently across all classes.

---

### 5. Which model had the best F1-score? What does it indicate?

ResNet50 achieved the highest F1-score of **0.9631**.

This indicates excellent balance between Precision and Recall, meaning the model rarely misclassifies tree species.

---

### 6. How did Precision and Recall differ across models?

- ResNet50 maintained both high Precision and Recall
- VGG16 achieved moderate but balanced metrics
- EfficientNetB0 showed extremely poor Precision and Recall

---

## C. Confusion Matrix Analysis

### 7. Which classes were frequently misclassified?

Tree species with visually similar leaf structures and bark textures were more difficult to classify correctly.

---

### 8. What patterns did you observe in the confusion matrix?

ResNet50 displayed a strong diagonal pattern, indicating highly accurate predictions.

EfficientNetB0 incorrectly mapped most images into a single predicted class.

---

## D. ROC and AUC

### 9. Which model had the highest AUC score?

ResNet50 achieved the highest ROC-AUC score of **0.9986**.

---

### 10. What does AUC tell us about model performance?

A high AUC score means the model can effectively distinguish between different classes regardless of threshold settings.

---

## E. Explainability (Grad-CAM)

### 11. What did Grad-CAM reveal about model decision-making?

Grad-CAM heatmaps showed whether the model focused on meaningful image regions such as leaves and branches.

---

### 12. Did the model focus on relevant image regions?

Yes. ResNet50 focused heavily on biologically relevant areas of the trees.

VGG16 incorrectly focused on image borders instead of the plant itself.

---

### 13. Which model produced the most meaningful heatmaps?

ResNet50 generated the most accurate and interpretable heatmaps.

---

## F. Model Comparison & Improvement

### 14. Which model would you recommend for deployment? Why?

ResNet50 is the best model for deployment because it achieved the highest overall performance across all evaluation metrics.

---

### 15. How can you further improve your best-performing model?

Possible improvements include:

- Fine-tuning deeper layers
- Increasing dataset size
- Applying stronger data augmentation
- Training for additional epochs

---

## G. Real-World Application

### 16. How can your model be applied in real-world scenarios?

The system can help:

- Forestry departments
- Botanists
- Environmental researchers
- Biodiversity monitoring systems

It may also be integrated into mobile or drone-based identification systems.

---

### 17. What are the risks of deploying an inaccurate model?

Incorrect predictions could lead to:

- Misidentification of endangered species
- Environmental management errors
- Incorrect biodiversity records

---

### 18. How can this system be integrated into a mobile/web app?

The trained model can be converted into TensorFlow Lite (`.tflite`) format and embedded into Android or iOS applications for offline tree species identification.

---

# ✅ Conclusion

Among all evaluated models, **ResNet50** achieved the best overall performance in terms of:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Explainability through Grad-CAM

The experiment demonstrates that deeper residual architectures significantly outperform older CNN architectures for complex image classification tasks.

---

## 👨‍💻 Author

**Nikko A. Damalerio**  
BSIT 4 – CIDS4  
Caraga State University Cabadbaran Campus
