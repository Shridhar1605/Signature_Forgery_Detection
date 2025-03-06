# Signature_Forgery_Detection

## 1. Decision Tree Model

### Dataset
- **Original Signatures**: 1320 samples
- **Forged Signatures**: 1320 samples
- **Total Samples**: 2640
- **Image Size**: 128x128 (Grayscale)

### Preprocessing
- Conversion to Grayscale
- Image Resizing to 128x128
- Pixel Intensity Flattening
- Feature Standardization using `StandardScaler`

### Feature Extraction
- Hu Moments: Captures geometric shape properties — helpful for distinguishing overall shape and curves of signatures.
- HOG (Histogram of Oriented Gradients): Describes local edge orientations and texture patterns — useful for identifying handwriting patterns.
- Pixel Density: Measures the proportion of inked pixels — good for detecting how heavy or light the signature is.

### Model Specification
- Classifier: Decision Tree Classifier
- Criterion: Entropy
- Maximum Depth: 5
- Pruning using Cost Complexity Parameter (ccp_alpha = 0.2)
- Cross-validation: 5-Fold

### Results
| Class       | Precision | Recall | F1-Score | Support |
|-------------|----------|-------|----------|--------|
| Forgery     | 0.99     | 0.91  | 0.95     | 281    |
| NST_forged  | 1.00     | 1.00  | 1.00     | 244    |
| Original    | 0.91     | 0.99  | 0.95     | 268    |
| **Accuracy**|          |       | **0.96** | 793    |



### Observations
- The Decision Tree model performed well with 96% accuracy.
- It achieved a perfect classification score for NST-forged samples.
- Some confusion was observed between Forged and Original signatures.

---

## 2. Decision Tree + Neural Style Transfer (NST) Model

### Dataset
- **NST Forged Signatures**: 1320 samples generated using Neural Style Transfer
- Preprocessed Generated Dataset is already attached. If the user wants to generate their own dataset, they can utilize the generate_nst_forgeries function

### Preprocessing
- Grayscale Conversion
- Image Resizing to 128x128
- Pixel Intensity Flattening
- Feature Standardization

### Model Specification
- Classifier: Decision Tree Classifier (Trained from Previous Stage)
- Criterion: Entropy
- Maximum Depth: 5

### Results
| Class       | Precision | Recall | F1-Score | Support |
|-------------|----------|-------|----------|--------|
| NST_forged  | 1.00     | 1.00  | 1.00     | 1320   |
| **Accuracy**|          |       | **1.00** | 1320   |

### Observations
- The model achieved **100% accuracy** on NST-forged samples.
- NST-based forgeries were detected flawlessly due to the texture artifacts introduced by style transfer.
- The NST based model is showing 100% accuracy because NST style transfer brush-like noise patterns to the images and this causes the Decision Tree to memorize the noise pattern instead of learning any meanful patterns and this maybe avoided by tweaking alpha and beta values of the neural_transfer_style function
