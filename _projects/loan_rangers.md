---
layout: page
title: Predicting Loan Applicant Risk
description: Using competitino Kaggle data, we built models to classify loan applications as safe or risky.
img: assets/img/projects/loan_rangers/cover_image.png
importance: 1
category: school
related_publications: false
toc:
  sidebar: left
---

This was a major project for my Machine Learning course, where I was part of a 3-person team, **Loan Rangers**, where we explored credit risk prediction using the Home Credit Default Risk dataset from [Kaggle](https://www.kaggle.com/competitions/home-credit-default-risk/data). The goal was to build a model that could classify loan applications as high or low risk, helping banks balance profitability with caution.

# Problem Space

Banks face tough choices when approving loans. If they deny credit to worthy applicants, they lose revenue. If they approve risky loans, they may suffer major losses. Our goal was to create a classifier that assesses both **risk** and **potential value** of loan applications using machine learning.

# Dataset & Preprocessing

The dataset included 7 relational tables and was highly imbalanced (90% safe loans, 10% risky). To handle this, we engineered features by aggregating child tables with statistical summaries: mean, median, variance — resulting in three data tranches with up to **1,266 features**.

We also had to deal with missing data. Many features were dropped due to excessive NaNs, and others were imputed or encoded appropriately.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/projects/loan_rangers/dataset_structure.png" title="Dataset Table Relationships" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  Diagram of the original dataset structure showing relationships between main and auxiliary tables.
</div>

# Resampling Techniques

Given the imbalance, we tried several resampling strategies:

- **Random Undersampling**
- **Tomek Links (cleaning borderline samples)**
- **KMeans SMOTE (clustered synthetic oversampling)**
- **Combined Undersampling + KMeans SMOTE**

We experimented with these across all three feature tranches, ensuring robust validation using hold-out sets.

# Models & Evaluation

We focused on **Random Forests (with Gradient Boosting)** and a **2-layer Neural Network**. The primary evaluation metric was **ROC-AUC**, as it’s insensitive to class imbalance and aligns with the Kaggle competition.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/projects/loan_rangers/roc_curve.png" title="ROC Curve - Best Model" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
  ROC Curve from the best-performing model (Random Forest, Tranche 1).
</div>

### Random Forest Results (Best Case)

- **Data**: Tranche 1, Random Undersampling + KMeans SMOTE  
- **ROC-AUC**: 0.724  
- **Kaggle Rank**: #5932  

We also performed a **Bayesian Search** for hyperparameter optimization, tuning `max_depth`, `learning_rate`, `n_estimators`, `scale_pos_weight`, and more.

### Neural Network Results

- Slightly lower ROC-AUC (~0.70 range), and no hyperparameter search was done due to time constraints.

# Future Directions

Our work revealed several areas for further research:

- **Advanced Feature Engineering**: Go beyond simple aggregation—capture sequence and time-based patterns.
- **Model Explainability**: Use SHAP or LIME to interpret which features drive predictions.
- **Cost-Sensitive Learning**: Integrate expected monetary loss/profit into predictions.
- **Smarter Resampling**: Try more nuanced oversampling or cost-weighted losses.
- **AutoML and Cloud Compute**: Use Bayesian optimization or cloud-based tuning to speed up model development.

# Conclusion

This project laid the groundwork for building data-driven credit risk models. While our best results approached the 0.72 ROC-AUC mark, top Kaggle entries achieve over 0.80, showing there’s still room to grow. But we learned a ton about handling real-world datasets, resampling, ensemble modeling, and more.

You can view the original dataset [here](https://www.kaggle.com/competitions/home-credit-default-risk/data). You can view our [github](https://github.com/nikko-guy/Loan-Rangers) too!

# Slide Deck

You can view our full project slides below.

<iframe src="/assets/pdf/loan_rangers_slides.pdf" width="100%" height="600px" style="border: none;">
    This browser does not support PDFs. Please download the PDF to view it: 
    <a href="/assets/pdf/loan_rangers_slides.pdf">Download PDF</a>.
</iframe>