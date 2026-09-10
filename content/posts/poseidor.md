---
date: '2026-09-10T22:43:22+03:00'
draft: false
title: 'POSEIDOR (Pose Identificator)'
---

Python-based, unified exercise correctness classification, model training, evaluation,
and inference toolkit. The [source code is public](https://gitlab.com/den.ege.der/gradproj),
but the dataset used is private. It was developed as the machine-learning core of an
AI-powered physiotherapy system and is especially designed for exercise form
classification on relatively small datasets.

The dataset contains **flexion** and **abduction** exercises recorded from 10 participants
using three RGB cameras at different angles, resulting in approximately 1,500 anonymized
videos. The pipeline extracts body keypoints using **MediaPipe**, **AlphaPose**, or
**OpenPose** and converts their different output formats into a common **COCO-17**
skeleton representation. Videos are temporally standardized, normalized relative to the
subject's bounding box, smoothed, and optionally augmented using temporal warping,
coordinate noise, affine transformations, skeletal proportion changes, and posture
offsets. The original 1080p recordings are also downscaled to 360p before pose extraction,
which drastically reduces processing time without a noticeable loss in classification
performance for this dataset.

Instead of relying only on raw X/Y coordinates, POSEIDOR can extract a 28-dimensional
biomechanical feature vector for every frame. It contains eight joint angles for the
elbows, shoulders, hips, and knees, their angular velocities and accelerations, and four
left-right symmetry measurements. Classical models further summarize an entire video
into a 196-dimensional vector using minimum, maximum, mean, and standard deviation
statistics together with temporal checkpoints from the beginning, middle, and end of
the movement. Predictions from the three camera views can then be combined using
majority voting or probability averaging. The following classification models were tested:

- Deep-learning experiments included GRU, LSTM, Transformer, TCN, and CTR-GCN
- Classical methods included KNN, PCA-KNN, SVM, RBF SVM, Extra Trees, Random Forest,
  HistGradientBoosting, and ensembles of multiple classical models

The experiments showed that representation quality had a major effect on performance.
Training directly on raw pose coordinates produced a best F1-score of only about **66%**
for flexion. After normalization and biomechanical feature extraction, several neural
models reached above **80% F1**. Classical models were particularly effective on the
smaller dataset: an **AlphaPose-based multi-camera classical ensemble with probability
averaging reached 88.19% F1 on flexion**. User-grouped five-fold testing also produced
individual held-out-user results above **90% F1**, with the strongest flexion folds
reaching about **96.4% F1**. However, performance varied considerably depending on the
held-out participants, and abduction remained substantially more difficult than flexion.
This suggests that compact biomechanical features are very effective when data is
limited, while more complex temporal models still require a larger and more diverse
dataset to generalize consistently.

Future work mainly focuses on increasing both the size and diversity of the dataset,
including more patients, body types, exercise types, camera placements, and recording
conditions. Other directions include testing newer pose-estimation models, evaluating
Kalman filtering as an alternative to the current Savitzky-Golay smoothing, and treating
incorrect form as an anomaly-detection or one-class classification problem. More detailed
expert labels could identify the body part, movement phase, and severity of an error,
allowing POSEIDOR to move from binary correct/incorrect classification toward generating
specific exercise feedback. The web platform could also be extended to retrain models
directly from newly labeled recordings, with the longer-term goal of evaluating the
system with physiotherapists and patients in a real clinical workflow.
