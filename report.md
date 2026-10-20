# CV Bootcamp Report: Learning from Noisy Labels (CIFAR-10N)

## Q1
Noise is highly structured. Four confusable pairs cause 32% of the 18,109 wrong training labels: automobile↔truck (2,373; 13.1%), cat↔dog (1,758; 9.7%), deer↔horse (916; 5.1%, mostly deer→horse) and airplane↔ship (748; 4.1%). Deer→dog and bird↔frog add another 7%. Cat and deer are the noisiest classes (51% wrong) and horse the cleanest (28%). The remaining errors are spread thinly (50–400 per cell). In contrast, symmetric synthetic noise puts 168–232 images in every off-diagonal cell.

## Q2
Memorization starts at epoch 41. Training accuracy on mislabeled images, measured against their wrong labels, stays at 0.10–0.17 for epochs 1–40, then climbs to 0.66 by epoch 60. Over the same period clean-label validation accuracy falls from 0.79 to 0.69 and noisy-label validation from 0.55 to 0.50. The onset coincides with the cosine learning rate falling to about 0.023, when small steps let the network fit individual wrong labels. Noisy validation selects epoch 39 (clean accuracy 0.785), just before the onset.

## Q3
The best single signal was 1 − P(given label) from the ELR model (AUROC 0.964, precision@k 0.884). The submitted score, fixed in advance, averages early-stopped CE and ELR with flip augmentation (AUROC 0.962, precision@k 0.877), versus 0.944 / 0.844 for CE alone. Failure mode: systematic human confusions. Only 37% of cat→dog errors reach the top-k, versus 92% of dog→cat and 88% overall, because the network learns the annotators' confusion and trusts the wrong label. Also, 2,236 correct but ambiguous images are flagged.

## Q4
Relabel + ELR was best: noisy validation 0.580 (clean, measured only, 0.861), ahead of ELR alone (0.577 / 0.851) and early-stopped CE (0.550 / 0.785). ELR keeps a running average of each image's early, mostly correct predictions and penalizes moving away from them, so wrong labels stop pulling the network once the learning rate decays: CE's clean accuracy falls from 0.79 to 0.69 after epoch 41, while ELR keeps rising. Relabeling with Task 3 scores raised kept-label accuracy from 60% to 89% (measured only), leaving fewer wrong labels to resist.

## Q5
The ranking did not change: relabel + ELR (synthetic validation 0.538; clean, measured only, 0.907) beat ELR (0.535; 0.899) and CE (0.502; 0.839), as on human noise. All methods reach higher clean accuracy under synthetic noise, and its errors are easier to find (detection AUROC 0.989 vs 0.962; relabelled labels 96% vs 89% correct). Symmetric noise is random, so wrong labels share no visual pattern and the network learns true classes first. Human noise sits in confusable pairs such as cat/dog, which the network itself confuses, so early predictions and relabels inherit part of it.
