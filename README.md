# 23B4210 · CV Bootcamp: Learning from Messy Human Labels (CIFAR-10N)

Inter-IIT Tech Meet 15.0 selection bootcamp, computer vision track.
ResNet-18 (organizer's `model.py`) trained from scratch on CIFAR-10N "worst" labels (40.2% wrong).

## Repository structure
| Path | Contents |
|---|---|
| `code/CV_Bootcamp.ipynb` | Full pipeline for Tasks 1–5 |
| `code/README.md` | Exact steps to reproduce every task |
| `predictions/` | All required prediction, dynamics and noise-score files |
| `results.json` | Seed, training settings, method names, validation numbers |
| `report.md` | Answers to Q1–Q5 |

## Results (validation set)
| Model | Val acc (noisy labels) | Val acc (clean labels, measured only) |
|---|---|---|
| CE, final epoch | 0.4968 | 0.6914 |
| CE, early-stopped (epoch 39) | 0.5502 | 0.7850 |
| ELR | 0.5766 | 0.8506 |
| **Relabel + ELR (best)** | **0.5796** | **0.8614** |

- **Task 2:** memorization of wrong labels starts at epoch 41.
- **Task 3:** noise-score AUROC 0.962, precision@k 0.877.
- **Task 5 (synthetic labels):** CE 0.502 vs relabel + ELR 0.538 synthetic val acc; the method ranking matches real noise.

## Training setup
SGD (lr 0.1, momentum 0.9, weight decay 5e-4), cosine schedule, 60 epochs, batch 128, seed 42,
RandomCrop(32, padding 4) + horizontal flip. Identical for all runs.
