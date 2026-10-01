# OcuVision-AI: Project Roadmap & Execution Plan

This is the main execution document for the next team member. **Follow these phases sequentially.** Do not skip steps. Do not modify the existing trained models or reported results.

---

## PHASE 0 — PROJECT BASELINE & ENVIRONMENT
**STATUS: COMPLETED**

**Objective:**
Establish the existing project baseline without changing the trained models.

**Completed Tasks:**
- Dataset preparation, preprocessing, and class organization (N, D, G, C, A, H, M).
- Stratified train/validation/test split (`random_state = 42`).
- ResNet-50 training & ViT-B/16 training.
- Internal test evaluation, classification reports, confusion matrices, training curves, and model comparison.

**Expected Baseline Results:**
- **ResNet-50:** 81.24% test accuracy | 82.58% test Macro F1
- **ViT-B/16:** 77.70% test accuracy | 80.61% test Macro F1

**Action:**
- **DO NOT RETRAIN.**
- **DO NOT MODIFY** these baseline results.

**Exit Condition:**
The team member understands the existing project, has access to the GitHub repository, Kaggle, dataset, PyTorch checkpoint (`resnet50_best_checkpoint.pth`), and both notebooks.

---

## PHASE 1 — REPRODUCIBILITY & CHECKPOINT VERIFICATION
**STATUS: FIRST TASK (PENDING)**

**Goal:**
Ensure the existing trained ResNet-50 model can be loaded on Kaggle without retraining.

**Environment & Setup:**
- **Platform:** Kaggle GPU
- **Notebook:** `notebooks/resnet50_ocular.ipynb`

**Exact Steps:**
1. Open the Kaggle notebook and **enable GPU**.
2. Attach the ocular dataset.
3. Attach/upload the PyTorch checkpoint: `resnet50_best_checkpoint.pth`.
4. Confirm the file exists.
5. Execute the cells to recreate the exact ResNet-50 architecture and set the classifier to 7 classes.
6. Execute the cell that loads the checkpoint.
7. Confirm the model loads successfully.
8. Execute the cell to confirm CUDA is available and print the GPU name.
9. Load one test image and run one inference.
10. Print the image path, true class, predicted class, and prediction confidence.

**What NOT to do:**
- DO NOT retrain or modify weights.
- DO NOT modify preprocessing, class order, or test split.

**Expected Output & Where to Save:**
- **Create file:** `results/resnet50/checkpoint_verification.txt`
- **Record:** Kaggle GPU details, checkpoint path, confirmation of successful load, class order, test image used, prediction, and confidence.

**What to do if something fails:**
- **STOP.** Do not retrain automatically.
- Document the exact error in `docs/TROUBLESHOOTING.md` and consult it for fixes.

**Exit Condition:**
Checkpoint loads successfully, one test image produces a valid prediction, and `checkpoint_verification.txt` is saved.

---

## PHASE 2 — GRAD-CAM EXPLAINABILITY
**STATUS: PENDING**

**Goal:**
Explain which retinal regions contribute to ResNet-50 predictions visually.

**Environment & Setup:**
- **Notebook:** `notebooks/resnet50_ocular.ipynb`
- **Model:** ResNet-50
- **Target layer:** `model.layer4[-1]` (Expected activation: `[Batch_Size, 2048, 7, 7]`)

**Exact Steps:**
1. Load the trained checkpoint and test dataset (as verified in Phase 1).
2. Select representative test images (include correct predictions, difficult/ambiguous predictions, and examples from different disease classes).
3. Generate predictions.
4. Generate Grad-CAM heatmaps for `model.layer4[-1]`.
5. Overlay heatmaps on the original fundus images.
6. Clearly label each visualization with the true class, predicted class, and confidence.
7. Save the original image, the Grad-CAM heatmap, and the overlay.

**What NOT to do:**
- DO NOT generate hundreds of unnecessary images. A small, representative set is enough.
- DO NOT claim that Grad-CAM proves medical causality. It only indicates regions contributing to the model prediction.

**Expected Output & Where to Save:**
- **Create folder:** `results/resnet50/gradcam/`
- **Save files:** `gradcam_01.png`, `gradcam_02.png`, etc.
- **Create file:** `results/resnet50/gradcam/README.md` (Document target layer, preprocessing, image selection, prediction, and interpretation).

**What to do if something fails:**
- If hooks fail or layers mismatch, refer to `docs/TROUBLESHOOTING.md`.

**Exit Condition:**
A clean set of valid Grad-CAM visualizations has been generated and documented in the `gradcam` folder.

---

## PHASE 3 — XAI QUALITY / SANITY CHECK
**STATUS: PENDING**

**Goal:**
Check whether Grad-CAM visualizations appear meaningful rather than obviously focusing on irrelevant artifacts.

**Exact Steps:**
1. For each selected Grad-CAM example, observe the highlighted regions.
2. Check whether activation is concentrated around plausible retinal structures rather than image borders, black backgrounds, watermarks, or camera artifacts.

**What NOT to do:**
- DO NOT claim clinical validity from visual inspection alone.
- If an example looks suspicious, **DO NOT delete it**. Keep it and document it.

**Expected Output & Where to Save:**
- **Create file:** `results/resnet50/gradcam/gradcam_analysis.md`
- **Record for each example:** true class, predicted class, confidence, qualitative region observed, whether obvious shortcut/artifact focus is visible, and limitations.

**Exit Condition:**
Grad-CAM examples have been thoroughly reviewed and documented in `gradcam_analysis.md`.

---

## PHASE 4 — EXTERNAL DATASET / HOSPITAL VALIDATION
**STATUS: PENDING**

**Goal:**
Evaluate whether the trained ResNet-50 model generalizes to an independent external/hospital dataset. *(Can be started independently after Phase 1 if the external dataset is available).*

**Exact Steps:**
1. Obtain the external/hospital dataset and verify permission/privacy requirements.
2. Verify class mapping, image format, and image quality.
3. Apply the **exact same evaluation preprocessing** used by the existing model.
4. Load the frozen ResNet-50 checkpoint.
5. Run inference on the external dataset.
6. Calculate accuracy, macro precision, macro recall, macro F1, and weighted F1.
7. Generate a confusion matrix and classification report.
8. Compare external performance against the existing internal test result.

**What NOT to do:**
- **DO NOT** use the external dataset for training.
- **DO NOT** fine-tune or retrain.
- **DO NOT** optimize thresholds using the external test results.
- **DO NOT** invent labels if the external dataset does not contain compatible labels. Document the limitation instead.

**Expected Output & Where to Save:**
- **Create folder:** `results/external_validation/`
- **Save files:** `external_metrics.csv`, `external_classification_report.csv`, `external_confusion_matrix.png`, and `external_validation_summary.md`.

**Exit Condition:**
External validation results are reproducibly generated and saved, OR a documented reason explains why external validation could not be completed.

---

## PHASE 5 — FINAL COMPARATIVE ANALYSIS
**STATUS: PENDING**

**Goal:**
Produce the final analytical comparison between ResNet-50 and ViT-B/16.

**Exact Steps:**
1. Use the frozen internal test results for both models.
2. Discuss differences in accuracy, precision, recall, macro F1, and weighted F1.
3. Discuss class-wise behavior, confusion patterns, and model characteristics.
4. Discuss possible reasons for observed differences and limitations.
5. If Phase 4 is complete, compare internal test performance vs external performance and discuss possible domain shift.

**What NOT to do:**
- DO NOT create subjective rankings or invent new metrics.

**Expected Output & Where to Save:**
- **Create file:** `results/final_comparison.md`

**Exit Condition:**
A comprehensive markdown file containing the final comparison is created.

---

## PHASE 6 — FINAL REPORT + PRESENTATION
**STATUS: FINAL PHASE (PENDING)**

**Goal:**
Prepare final academic deliverables (Report and PPT). Only finalize this phase after Phases 2-5 are complete.

**Expected Output:**
- **Final Report**: Must include Introduction, Problem Statement, Aim, Objectives, Literature Review, Dataset, Data Preprocessing, Data Split, Methodology, Model Architectures, Training Methodology, Evaluation Metrics, Results, Confusion Matrices, Class-wise Analysis, Grad-CAM/XAI, External Validation, Comparative Analysis, Limitations, Future Scope, Conclusion, and References.
- **PPT Presentation (~15 slides)**: Title, Problem, Motivation, Objectives, Dataset, Preprocessing, Proposed pipeline, ResNet-50, ViT-B/16, Training methodology, Results, Confusion/class-wise analysis, Grad-CAM, External validation, Conclusion/future work.

**Exit Condition:**
The final report document and PPT file are completed and saved outside the core GitHub repository (or in a designated docs/deliverables folder).
