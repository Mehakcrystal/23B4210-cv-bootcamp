# CV Bootcamp: Learning from Noisy Labels (CIFAR-10N)

## How to reproduce
1. Put the provided files in Google Drive at `MyDrive/cv_bootcamp/`.
2. Open `CV_Bootcamp.ipynb` (this folder) in Google Colab with a T4 GPU.
3. Run `Runtime → Run all`. Each 60-epoch run takes about 15–25 minutes. Finished runs are saved in
   `MyDrive/cv_bootcamp/work/` and skipped on re-run.

## Notebook order
- Step 0: mount Drive, verify files, load data, shared training setup (organizer ResNet-18 from `model.py`).
- Task 1: noise rate and confusion matrix; clean-reference and noisy CE models (final epoch).
- Task 2: per-epoch dynamics, memorization epoch, early-stopped predictions (noisy validation only).
- Task 4 method 1 (ELR) and Task 5 runs on synthetic labels: trained before Task 3, whose ensemble uses them.
- Task 3: ensemble noise scores (early-stopped CE + ELR, flip TTA), own check with clean labels, relabel files.
- Task 4 method 2 (relabel + ELR) and its synthetic-label run; best method chosen by noisy validation accuracy.
- Submission: results.json, report.md, format check, zip.

## Training setup (identical for all runs)
Seed 42; SGD lr 0.1, momentum 0.9, weight decay 5e-4; cosine schedule; 60 epochs; batch 128;
RandomCrop(32, padding=4) + horizontal flip (on GPU); mixed precision.
Clean labels are used only for the Task 1 reference model and for measurements.
Model selection: noisy validation labels (Tasks 1-4), synthetic validation labels (Task 5).
