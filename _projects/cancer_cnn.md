---
layout: page
title: Detecting cancerous cells with a Convolutional Neural Network
description: Using a Kaggle dataset, I achieved 90% accuracy with limited AI knowledge!
img: assets/img/projects/cancer_cnn/collage.png
importance: 1
category: school
related_publications: true
toc:
  sidebar: left
---

This project was part of my Intro to AI class at CU Boulder. I had a lot of freedom in choosing what to build, so I decided to focus on a medical imaging dataset from Kaggle that distinguishes cancerous from non-cancerous cells. I also found a great starting point by following [this beginner-friendly Kaggle guide](https://www.kaggle.com/code/gomezp/complete-beginner-s-guide-eda-keras-lb-0-93).

# Exploratory Data Analysis

## Color Analysis
A large part of my exploration involved examining color channel distributions in cancerous vs. non-cancerous images. By plotting histograms of average color values, I noticed that malignant samples skewed darker overall, especially in the green channel. Sometimes there even appeared to be a “bowl” shape in that range, like part of the green intensities were missing. Meanwhile, benign cells generally showed higher brightness, with a clearer green channel peak. These observations hinted that color could be a key factor in classification.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cancer_cnn/green_hists.png" title="Green Channel Histograms" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Average Green Value histograms. Left: Malignant, Right: Benign
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cancer_cnn/color_avg.png" title="Average Color for Each Set" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Image Brightness
I also calculated the average brightness per image. Malignant samples spread across a broader brightness spectrum, while benign ones were more clustered on the higher end. Some images were so bright or overexposed that they barely showed any cell structure—so I filtered out those “junk” samples, ensuring the model focused on more representative data.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cancer_cnn/brightness_hists.png" title="Brightness Histograms" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Distribution of mean brightness for malignant vs. benign samples.
</div>

# Creating my First Model
My initial approach was a straightforward CNN using TensorFlow/Keras:

1. **Conv2D + MaxPooling**: Several convolution layers for feature extraction, each followed by pooling to reduce dimensionality.
2. **Dense Output**: A final layer with a single sigmoid neuron for binary classification.
3. **Optimizer & Loss**: Started with `binary_crossentropy` and Adam.

```mermaid
flowchart LR
    A([Input Images]) --> B[Conv2D + ReLU]
    B --> C[MaxPooling2D]
    C --> D[Conv2D + ReLU]
    D --> E[MaxPooling2D]
    E --> F[Flatten]
    F --> G[Dense + ReLU]
    G --> H[Dense (Sigmoid)]
    H --> I((Output: Malignant or Benign))
```

By about the third epoch, I saw clear signs of overfitting, which motivated more advanced regularization and scheduling.

# Data Cleaning and Model Improvements
To combat overfitting and achieve more stable performance:

- **Filtering**: Removed heavily overexposed or otherwise “junk” images.  
- **Data Augmentation**: Incorporated random flips, small brightness changes, and occasional rotations.  
- **Learning Rate Scheduling**: One of the biggest turning points. I let the learning rate adjust dynamically (e.g., via `ReduceLROnPlateau`), improving stability and final accuracy once the model plateaued.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/cancer_cnn/training_metrics.png" title="Model Metrics Over Epochs" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A combined graph of loss, accuracy, and learning rate across epochs.
</div>

# Final Model
After multiple iterations, I landed on:

- **Architecture**: Several Conv2D + MaxPooling2D layers, then Flatten and two Dense layers (with dropout).  
- **Loss & LR Scheduling**: Retained `binary_crossentropy`, complementing it with an adaptive learning rate that automatically dropped whenever validation metrics stalled.  
- **Cleaned Dataset**: By discarding unhelpful images and focusing on genuinely informative examples, the model got clearer signals during training.

This final setup reached ~90% accuracy on a held-out test split, which was a marked improvement from my earliest attempts.

You can find all my code for this project on my [GitHub repo](https://github.com/nikko-guy/Kaggle-Cancer). Overall, combining careful data filtering, augmentation, and learning rate scheduling helped me achieve strong results with relatively little prior AI experience.