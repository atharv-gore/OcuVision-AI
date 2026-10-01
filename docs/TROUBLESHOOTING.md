# OcuVision-AI: Troubleshooting Guide

If you encounter issues during execution (especially on Kaggle), refer to this guide first. **Never recommend retraining as the first solution. First diagnose the problem.**

### 1. Checkpoint not found
- **Symptom:** `FileNotFoundError: [Errno 2] No such file or directory: '/kaggle/working/resnet50_best.pth'` or similar.
- **Likely Cause:** The Kaggle session disconnected and `/kaggle/working/` was wiped, or the checkpoint was not uploaded as an input dataset.
- **Safe Fix:** Upload the local `resnet50_best_checkpoint.pth` file as a new private dataset to Kaggle, attach it to the notebook, and update the `checkpoint_path` variable to point to `../input/dataset-name/resnet50_best_checkpoint.pth`.
- **What NOT to do:** Do not rerun the training loop to generate a new checkpoint.

### 2. Wrong checkpoint format
- **Symptom:** `RuntimeError: Invalid magic number; corrupt file?` or errors when running `torch.load()`.
- **Likely Cause:** Attempting to load TensorFlow-style checkpoint files (`ckpt-0.index` / `ckpt-0.data-00000-of-00001`) into PyTorch.
- **Safe Fix:** Ensure you are loading the `.pth` file (`resnet50_best_checkpoint.pth`). Remove the Kaggle dataset containing the TF checkpoints to avoid confusion.
- **What NOT to do:** Do not write custom extraction code to parse TF weights into PyTorch.

### 3. CUDA unavailable
- **Symptom:** Notebook runs extremely slowly, or errors state `Expected all tensors to be on the same device, but found at least two devices, cuda:0 and cpu!`.
- **Likely Cause:** GPU accelerator is not enabled in the Kaggle session.
- **Safe Fix:** Go to the Kaggle session settings, select **Accelerator**, and choose **GPU T4 x2** (or similar). Restart the session.
- **What NOT to do:** Do not manually change `device = 'cpu'` throughout the code to force it to run.

### 4. Dataset path incorrect
- **Symptom:** `FileNotFoundError` when executing the Dataset parsing block (Phase 0/1).
- **Likely Cause:** The dataset was attached under a different name or path in Kaggle (e.g., `../input/ocular-disease-recognition-dataset` instead of what's in the notebook).
- **Safe Fix:** Print `os.listdir('../input')` to find the exact path, and update the `base_path` variable.
- **What NOT to do:** Do not reorganize the Kaggle dataset structure.

### 5. Class mapping mismatch
- **Symptom:** External validation fails with `KeyError` or mismatch in class size (e.g., model outputs 7, dataset has 8).
- **Likely Cause:** The external hospital dataset uses different folder names or has missing disease classes.
- **Safe Fix:** Manually map the external dataset folder names to the `['N', 'D', 'G', 'C', 'A', 'H', 'M']` array using a dictionary before passing them to the DataLoader.
- **What NOT to do:** Do not invent labels for images that don't fit the 7 classes. Ignore them or document the limitation.

### 6. Missing variable in notebook
- **Symptom:** `NameError: name 'test_dataset' is not defined` when running the Grad-CAM block.
- **Likely Cause:** You jumped straight to the Grad-CAM cell without running the prerequisite setup blocks.
- **Safe Fix:** Run the initial cells that define the imports, dataset loaders, and model architecture (Sections 1 through 7).
- **What NOT to do:** Do not run the Training loop cell (Section 9) just to populate variables.

### 7. GPU out-of-memory
- **Symptom:** `CUDA out of memory.`
- **Likely Cause:** Trying to push a massive batch size for Grad-CAM or the external validation set.
- **Safe Fix:** Reduce the `batch_size` in the DataLoader (e.g., to 16 or 8) and clear cache using `torch.cuda.empty_cache()`.
- **What NOT to do:** Do not modify the image resolution (`224x224`), as this will break the pretrained weights.

### 8. Grad-CAM hook error
- **Symptom:** Error accessing `model.layer4[-1]` or `forward_hook` fails.
- **Likely Cause:** Incorrect layer reference or trying to run Grad-CAM on ViT-B/16 using CNN-specific code.
- **Safe Fix:** Ensure Grad-CAM is strictly running on ResNet-50. Check that `type(target_layer).__name__ == 'Bottleneck'`.
- **What NOT to do:** Do not change the ResNet architecture or replace the `layer4` block.

### 9. External dataset labels incompatible
- **Symptom:** External dataset is missing several classes (e.g., only has Glaucoma and Normal).
- **Likely Cause:** Clinical datasets often don't contain exactly the 7 diseases we modeled.
- **Safe Fix:** Filter the internal test metrics to only compare the classes that exist in the external dataset. Document the missing classes as a limitation in the final report.
- **What NOT to do:** Do not label unknown diseases as "Normal".

### 10. Kaggle session disconnect
- **Symptom:** Disconnected from session, lost variables.
- **Likely Cause:** Kaggle 12-hour timeout or 40-minute idle timeout.
- **Safe Fix:** Re-run the notebook from the top (skipping training cells). Always save Grad-CAM outputs and CSV files to `/kaggle/working/` and download them locally before walking away.
- **What NOT to do:** Do not rely on `/kaggle/working/` surviving a disconnected session.
