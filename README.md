# Uncertainty-Aware Simulation of Ultrasound-Guided Robotic Needle Insertion
This project presents a full simulation pipeline for safer robotic needle insertion guided by ultrasound, with a strong emphasis on modeling uncertainty in segmentation and control. It integrates deep learning–based nerve segmentation, particle filtering for localization, and a confidence-aware robotic planner.


## Overview
Current robotic systems often treat visual segmentation as ground truth, ignoring the uncertainty in perception. This can result in unsafe behavior, especially in safety-critical applications like needle insertion. This pipeline introduces:
- Monte Carlo Dropout for pixel-wise segmentation uncertainty
- Connected component fusion to localize nerves
- Particle filtering in robot world coordinates
- Confidence-aware trajectory planning with a "pause and re-scan" mechanism

## Key Components
1. Segmentation with MC-Dropout
- Trained a U-Net on the Kaggle Ultrasound Nerve Segmentation dataset
- Performed 20 stochastic forward passes at test time to estimate soft masks and pixel-wise uncertainty
- Achieved Dice = 0.65, IoU = 0.49 on validation set

2. Localization with Connected Components
- Computed both continuous and thresholded centroids
- Selected the best nerve estimate based on alignment with soft predictions

3. Particle Filter for World-Space Fusion
- Transformed image coordinates to world coordinates
- Modeled uncertainty using a Gaussian noise model
- Achieved localization error ≈ 139 µm

4. Robotic Planning & Simulation
- Simulated a 2-link planar arm using inverse kinematics
- Executed trajectory toward nerve with periodic uncertainty checks
- Paused and re-scanned when confidence dropped below threshold

## Results
- Final localization error reduced from 3.7px → 3.5px after a pause-and-rescan
- Expected Calibration Error (ECE) = 0.012
- System consistently fused noisy estimates into stable world-frame targets
- Trajectory simulation visualized with matplotlib in Python

## How to Run
1. Clone and Set Up
git clone https://github.com/your-username/robotic-needle-insertion.git

2. Train the U-Net
Run the notebook train.ipynb

4. Run the Simulation Pipeline
run the notebook end_to_end.ipynb
