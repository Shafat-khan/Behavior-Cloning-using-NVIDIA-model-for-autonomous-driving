# Autonomous Driving with Behaviour Cloning

## Project Overview

This project explores autonomous driving through **behaviour cloning**, a machine learning approach in which a model learns to imitate driving behaviour from recorded examples. Using a CNN-based architecture inspired by NVIDIA's end-to-end self-driving model, the system processes front-facing driving images and predicts a continuous steering angle.

The project was completed for the CSE 445.6 Machine Learning course. Training data was collected from the Udacity driving simulator and supplemented with images captured during driving. The dataset includes centre, left, and right camera images paired with steering-angle and vehicle-control information.

## Objectives

- Build a convolutional neural network to predict steering angles from road images.
- Prepare image and steering-angle data for model training.
- Reduce steering-angle imbalance caused by tracks dominated by straight driving.
- Use image preprocessing and augmentation to improve robustness.
- Train and validate a regression model using the Adam optimiser and mean squared error loss.

## Methodology

### 1. Data preparation

The data-processing pipeline extracts image filenames from recorded paths and organises centre, left, and right camera views with their corresponding steering angles.

The report identifies a strong imbalance around zero steering angle because the training track contains many straight sections. To address this, the dataset is filtered and shuffled to reduce the dominance of centre-steering examples.

The report records the following dataset counts after this process:

- Initial dataset: **4,053 samples**
- Removed: **2,590 samples**
- Remaining: **1,463 samples**

The data is then divided into training and validation subsets, with a reported 20% holdout for testing/validation.

### 2. Image preprocessing

The model uses the following image-processing pipeline:

1. Crop the image to focus on the relevant road region (`img[60:135, :, :]`).
2. Convert the image from RGB to YUV colour space.
3. Apply a 3 × 3 Gaussian blur.
4. Resize the image to **200 × 66 pixels**.
5. Normalise pixel values to the range 0–1.

These transformations reduce irrelevant visual information and standardise the inputs expected by the NVIDIA-inspired network.

### 3. Data augmentation

To increase training-data diversity, the project applies augmentation techniques including:

- **Zoom:** simulates changes in distance and scale.
- **Pan:** shifts the image horizontally or vertically.
- **Brightness adjustment:** helps represent different lighting conditions.
- **Random flipping:** creates additional image variations, with corresponding steering labels handled during data preparation.

Augmented images are used for training rather than validation so that validation remains a more consistent measure of performance on unmodified data.

### 4. CNN model architecture

The model is inspired by NVIDIA's self-driving-car architecture and treats steering prediction as a **continuous regression task**, rather than a classification problem.

The network described in the report contains:

- Five convolutional layers for visual feature extraction.
- A flattening layer.
- Four fully connected (dense) layers.
- ELU activation functions.
- Dropout to help reduce overfitting.

The report lists **252,219 trainable parameters** for the model.

### 5. Training and optimisation

The model is trained with:

- **Optimiser:** Adam
- **Learning rate:** 0.001
- **Loss function:** Mean Squared Error (MSE)
- **Batch generation:** loads and preprocesses image batches during training

MSE measures the difference between predicted and recorded steering angles. The network learns to map road-scene images to steering commands by minimising this regression loss.

## Results and Observations

The report presents training and validation loss/accuracy plots and describes a relatively small gap between training and validation curves after augmentation and preprocessing.

The project concludes that the model learned to predict steering angles from camera images in the simulator-based environment. However, the report does not provide a clearly stated numerical test-set score or a quantitative steering-error metric, so no specific accuracy or real-world autonomous-driving performance claim is made here.

## Technology Stack

The report describes a Python-based machine learning workflow using:

- Python
- Convolutional Neural Networks (CNNs)
- NumPy and Pandas for data handling
- OpenCV for image preprocessing
- Keras / TensorFlow-style model training
- Matplotlib for visualising steering-angle distributions and training curves

Confirm the exact library versions against the source code before adding pinned dependencies.

## Repository Structure

A suggested repository structure is:

```text
autonomous-driving-behaviour-cloning/
├── README.md
├── CNN-Convolution-Neural-Network.pdf
├── notebooks/
│   └── behaviour_cloning.ipynb
├── src/
│   ├── data_preprocessing.py
│   ├── augmentation.py
│   └── model.py
├── requirements.txt
└── .gitignore
```

Adjust the structure to match the files actually included in the repository. The PDF is the project report; source scripts and notebooks should only be listed if you upload them.

## Getting Started

The report documents the methodology but does not, by itself, provide a complete executable codebase or an exact dataset download location. To make this repository reproducible:

1. Upload the original notebook or Python source files.
2. Document the dataset source and expected folder structure.
3. Add the exact dependencies and versions used by the code.
4. Provide training and evaluation commands.
5. Keep large driving-image datasets and trained model weights outside Git if they exceed repository limits; document how to obtain them instead.

Once the source files are available, create a Python environment and install the dependencies listed in `requirements.txt`.

## Limitations and Future Improvements

- Report numerical test-set metrics such as MAE and RMSE for steering-angle predictions.
- Evaluate performance on different tracks, road layouts, and lighting conditions.
- Compare predictions against ground-truth steering angles using plots and error analysis.
- Verify that data balancing and augmentation preserve correct steering labels.
- Add reproducible train/validation/test splits and random seeds.
- Test recovery from sharp turns and unusual road conditions.
- Treat simulator performance as a research result, not evidence of safety on real roads.

## Skills Demonstrated

Machine Learning · Deep Learning · Computer Vision · Convolutional Neural Networks · Behaviour Cloning · Regression · Image Preprocessing · Data Augmentation · Data Balancing · Model Training · Model Evaluation · Python

## Academic Context

Developed as a group project for **CSE 445.6 – Machine Learning**. The work investigates imitation learning for autonomous-driving behaviour using an NVIDIA-inspired CNN architecture and simulator/driving image data.

## Safety Disclaimer

This project is an educational prototype. It is not validated for use in a real vehicle and must not be used to control a physical car without extensive safety engineering, testing, and appropriate safeguards.
