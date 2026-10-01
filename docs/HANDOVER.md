# OcuVision-AI: Handover Documentation

This document contains critical information for handing over the OcuVision-AI project to the next team member. 

**IMPORTANT RULE:** 
You should be able to follow the `docs/PROJECT_ROADMAP.md` completely independently. Everything you need to know to execute the next steps is documented.

## CURRENT STATUS & BASELINE
The project is partially complete. We have established a frozen baseline of results that **must not be altered**. 

- **Completed Work:** Dataset aggregation, preprocessing, stratified split, ResNet-50 training, ViT-B/16 training, and internal test evaluation. All performance metrics, confusion matrices, and training curves have been extracted and saved in `results/`.
- **Pending Work:** Phase 1 (Checkpoint Verification), Phase 2 (Grad-CAM), Phase 3 (XAI Check), Phase 4 (External Validation), Phase 5 (Final Comparison), Phase 6 (Final Deliverables).

**Do NOT retrain the ResNet-50 or ViT-B/16 models.** Use the existing checkpoints.

## KAGGLE WORKFLOW
The local laptop does not have a working PyTorch GPU environment. All ML execution must occur on Kaggle GPU.
1. Open the appropriate Kaggle notebook (`notebooks/resnet50_ocular.ipynb` or `vit_b16_ocular.ipynb`).
2. **Enable GPU** (e.g., Tesla T4).
3. Attach the ocular dataset.
4. Attach the PyTorch checkpoint (`resnet50_best_checkpoint.pth`) via Kaggle Input.
5. Run **only the required sections**. Do not unnecessarily retrain models.
6. Download important outputs (heatmaps, CSVs) from `/kaggle/working/` after execution.

## CHECKPOINT WARNING
**PyTorch Checkpoint:**
- The correct file is `resnet50_best_checkpoint.pth` (or `vit_b16_best.pth`). 

**TensorFlow-style Checkpoints (DO NOT USE):**
- `ckpt-0.index`
- `ckpt-0.data-00000-of-00001`
*If you see an existing Kaggle dataset containing these TF files, do not use them. They are incompatible with PyTorch.*

## NEXT STEPS: GRAD-CAM
As detailed in the Roadmap, your next core AI task is explaining ResNet-50.
- **Target layer:** `model.layer4[-1]`
- Run entirely on Kaggle GPU.
- Generate a few representative Grad-CAM examples.
- **Do not overwrite existing test evaluation results.**

## NEXT STEPS: EXTERNAL VALIDATION
Use a completely separate hospital/clinical dataset to validate generalization.
- **Do not retrain** or fine-tune the model on the new dataset.
- Apply the same preprocessing.
- Evaluate and document results in `results/external_validation/`.

## IMPORTANT RULES
- Do not modify existing reported metrics unless a genuine reproducibility problem is discovered.
- If a problem is discovered, document it explicitly rather than silently changing results.
- Never push datasets or large `.pth` checkpoints to GitHub.
