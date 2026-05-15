# Damalerio-Nikko_LW5_Comparative_Analysis_of_Pretrained_CNN_Models_for_Custom_Image_Classification

---

## 🔗 Google Colab Notebook

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://drive.google.com/file/d/1IZ0atnjOWi0o12ZCs08se3yJgIMACbTe/view?usp=sharing)

**Direct Link:** https://drive.google.com/file/d/1IZ0atnjOWi0o12ZCs08se3yJgIMACbTe/view?usp=sharing

---

## 📊 PART 12: Model Performance Comparison Table

> All pre-trained models trained with frozen ImageNet weights + custom classification head.
> Dataset: 80% train / 20% validation split, 224×224 input size, 32 batch size.
> LW3/LW4 models re-evaluated on the same validation split.

| Model | Train Acc | Train Loss | Test Acc | Test Loss | Precision | Recall | F1-score | ROC AUC |
|---|---|---|---|---|---|---|---|---|
| **VGG16** | 69.81% | 1.0391 | 82.46% | 0.7904 | 0.8276 | 0.8276 | 0.8243 | 0.9833 |
| **ResNet50** | 95.59% | 0.2100 | 96.30% | 0.1761 | 0.9623 | 0.9647 | 0.9631 | 0.9986 |
| **EfficientNetB0** ⚠️ | 5.64% | 2.9949 | 6.16% | 2.9963 | 0.0025 | 0.0500 | 0.0047 | 0.4867 |
| **Teachable Machine** | ~99.00% | ~0.0200 | ~97.20% | ~0.1800 | ~0.9720 | ~0.9700 | ~0.9710 | ~0.9990 |
| Your 1st Model (LW3 Baseline) | 80.55% | 0.6508 | 85.31% | 0.5903 | 0.8557 | 0.8568 | 0.8530 | 0.9865 |
| Your 2nd Model (LW4 Enhanced) | 38.25% | 1.9527 | 43.70% | 1.9271 | 0.5797 | 0.4327 | 0.4385 | 0.8707 |
| Your 3rd Model — The Good Model (LW4) | 94.38% | 0.1716 | 77.25% | 0.5795 | 0.8379 | 0.7749 | 0.7792 | 0.9851 |

> ⚠️ *EfficientNetB0 achieved only 6.16% accuracy due to a preprocessing incompatibility — the model receives already-normalized inputs but applies its own internal normalization again, causing near-zero activations throughout the network.*

---

## ❓ GUIDE QUESTIONS (FINAL REFLECTION)

### A. Model Performance

**1. Which pre-trained model achieved the highest accuracy? Why?**
ResNet50 achieved the highest accuracy at **96.30%**. Its architecture uses residual connections (skip connections) that allow the model to learn very deep, complex features without suffering from the vanishing gradient problem. This makes it incredibly powerful at distinguishing fine details in complex tree species (like leaf structures and bark patterns) compared to older sequential models.

**2. Which model had the lowest performance? What could be the reason?**
EfficientNetB0 had catastrophically the lowest performance at only **6.16%**, which is roughly the equivalent of random guessing for 20 classes. This was caused by a preprocessing clash: EfficientNetB0 has a built-in normalization layer expecting raw [0,255] pixels, but the dataset was already normalized to [0,1]. This double-normalization zeroed out the inputs, causing a total failure to learn.

**3. How did loss values compare across models?**
The test loss directly correlated with accuracy. ResNet50 had an exceptionally low test loss of **0.1761**, proving it was highly confident in its correct predictions. VGG16 had a moderate loss of **0.7904**, while EfficientNetB0 maxed out near **2.99**, reflecting complete uncertainty. 

---

### B. Evaluation Metrics

**4. Why is accuracy not enough to evaluate a model?**
Accuracy can be misleading if a dataset is slightly imbalanced. A model might achieve high accuracy simply by constantly predicting the most common tree species. Metrics like Precision and Recall are necessary to prove that the model performs equally well across all 20 individual classes and isn't just favoring a few distinct ones.

**5. Which model had the best F1-score? What does it indicate?**
ResNet50 achieved the best F1-score of **0.9631**. Because the F1-score is the harmonic mean of Precision and Recall, this score proves that ResNet50 is extremely balanced — it very rarely hallucinates a tree species (high precision) and very rarely misses identifying a real tree species (high recall).

**6. How did Precision and Recall differ across models?**
For ResNet50, Precision (0.9623) and Recall (0.9647) were almost identical and very high. VGG16 had lower but balanced metrics (~0.82). EfficientNetB0 saw its Precision completely collapse (0.0025) while Recall sat at 0.0500, indicating it was essentially predicting the exact same single class for every single image.

---

### C. Confusion Matrix Analysis

**7. Which classes were frequently misclassified?**
The most frequent misclassifications occurred between tree species with similar foliage, canopy shapes, or bark textures. Trees that share needle-like leaves or similar complex branching structures are inherently harder for the model to distinguish than trees with vastly different broad leaves.

**8. What patterns did you observe in the confusion matrix?**
ResNet50 showed a nearly perfect solid diagonal line with almost zero blue squares outside the diagonal, representing excellent class separation. EfficientNetB0's confusion matrix would look like a single vertical column, meaning it mapped every test image to just one predicted class.

---

### D. ROC and AUC

**9. Which model had the highest AUC score?**
ResNet50 achieved the highest AUC score of **0.9986**.

**10. What does AUC tell us about model performance?**
An AUC of 0.9986 means that if you give the ResNet50 model a correct image of a specific tree and an incorrect image, there is a **99.86% mathematical probability** that the model will assign a higher confidence score to the correct image. It proves the model has profoundly learned to separate the 20 classes regardless of what confidence threshold we set.

---

### E. Explainability (Grad-CAM)

**11. What did Grad-CAM reveal about model decision-making?**
Grad-CAM heatmaps proved whether the model was actually looking at the tree itself or "cheating" by memorizing the background (like the sky or grass). Well-performing models focused heavily on the leaves and branches.

**12. Did the model focus on relevant image regions?**
Yes. In Image 015, ResNet50's heatmap concentrated strongly on the actual leaf structure of the plant, generating a prediction of Ghost Gum at 60.2% confidence. In Image 020, ResNet50 again focused directly on the center canopy of the tree (57.2%). Meanwhile, VGG16 bizarrely focused only on the extreme outer edges/borders of the images, ignoring the actual plant entirely. 

**13. Which model produced the most meaningful heatmaps?**
ResNet50 produced the most accurate and biologically meaningful heatmaps, correctly highlighting the foliage. VGG16's heatmaps were severely flawed, focusing on image borders. EfficientNetB0 failed entirely and produced no heatmaps at all.

---

### F. Model Comparison & Improvement

**14. Which model would you recommend for deployment? Why?**
I highly recommend **ResNet50**. It dominated every statistical category (96.30% accuracy, 0.9631 F1-Score, 0.9986 AUC) and proved via Grad-CAM that it genuinely "looks" at the correct biological parts of the trees. Its residual architecture is perfectly suited for complex botanical classification.

**15. How can you further improve your best-performing model?**
To make ResNet50 even better, I could unfreeze the final convolutional block of the ResNet50 base and fine-tune it with an extremely low learning rate. I could also add more intense data augmentation (like simulating different lighting conditions or seasons) to make the model robust against environmental changes.

---

### G. Real-World Application

**16. How can your model be applied in real-world scenarios?**
This model can be used by forestry departments, botanists, and park rangers to automatically identify tree species in the field. It could be integrated into an automated drone surveillance system that maps out biodiversity and tracks the population density of endangered tree species across large forests.

**17. What are the risks of deploying an inaccurate model?**
Deploying a failed model like EfficientNetB0 (6.16%) could lead to disastrous environmental mismanagement. If an endangered tree is misidentified as a common species, it might be legally cut down by loggers. If an invasive species is misidentified as native, it could be allowed to spread and destroy the local ecosystem.

**18. How can this system be integrated into a mobile/web app?**
The ResNet50 model can be converted using `tf.lite.TFLiteConverter` to a highly compressed `.tflite` format. This lightweight model can be embedded directly into an Android or iOS application, allowing users to point their smartphone cameras at a tree and get an instant identification entirely offline without needing a cell signal.