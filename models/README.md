# OcuVision-AI Models

This directory is intended for storing trained model checkpoints (e.g., PyTorch `.pth` files) when running locally. 

**IMPORTANT NOTE FOR GITHUB:**
Large model files and weights are strictly and intentionally excluded from the GitHub repository via `.gitignore` due to size constraints. 

## Checkpoint Usage
The expected, correct PyTorch checkpoint file is:
**`resnet50_best_checkpoint.pth`**

### To the next team member:
If you need to load the model on Kaggle for Phase 2 (Grad-CAM) or Phase 4 (External Validation), you must obtain `resnet50_best_checkpoint.pth` from the team's approved cloud storage (or Kaggle output) and attach it to your Kaggle notebook as a dataset input.

## Warning: PyTorch vs TensorFlow Checkpoints
If you find a Kaggle dataset named *"resnet50 pretrained model checkpoint"* that contains the following files:
- `ckpt-0.index`
- `ckpt-0.data-00000-of-00001`

**DO NOT use them.** 
These are TensorFlow-style checkpoint files and are entirely incompatible with our PyTorch codebase (`torch.load()`). You must use the `.pth` file.
