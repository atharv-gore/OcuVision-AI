# Ocular Disease Recognition Using Deep Learning

## Project Overview
This project performs multi-class ocular disease recognition from fundus images using deep learning.

## Models
- ResNet-50
- Vision Transformer (ViT-B/16)

## Dataset
- **Total Images**: 10,449 images
- **Number of Classes**: 7 classes
- **Classes**: N, D, G, C, A, H, M
- **Train Split**: 8,359
- **Validation Split**: 1,045
- **Test Split**: 1,045
- **Split Method**: Stratified split with `random_state=42`

## Methodology
- **Preprocessing**: Images are resized to uniform dimensions, augmented (e.g., horizontal flip, rotation, color jitter for training), converted to tensors, and normalized.
- **Data Splitting**: A stratified train/validation/test split ensures a consistent class distribution across all sets.
- **Model Training**: Models are trained using standard optimization techniques, often incorporating class weights to handle data imbalance.
- **Validation-based Checkpoint Selection**: The best model checkpoint during training is selected and saved based on the highest validation Macro F1 score.
- **Final Test Evaluation**: The saved best checkpoint is reloaded and evaluated on the unseen test set to compute the final metrics.

## ResNet-50
The best model is selected using the validation Macro F1 score.

## ViT-B/16
This model serves as the transformer-based comparison model against the ResNet-50 architecture.

## Evaluation Metrics
The models are evaluated on the test set using the following metrics:
- Accuracy
- Precision
- Recall
- Macro F1
- Weighted F1
- Confusion Matrix

## Project Structure
```text
eye_detect_v1/
├── notebooks/
│   ├── resnet50_ocular.ipynb
│   └── vit_b16_ocular.ipynb
├── models/
│   └── README.md
├── results/
│   └── README.md
├── README.md
└── .gitignore
```

## Reproducibility
The notebooks are designed to run on Kaggle with GPU support and use `random_state=42` for the dataset split and random seed initializations to ensure consistent reproducibility.

## Important
Trained model checkpoints and the original dataset are not included in GitHub because of size/storage considerations.
