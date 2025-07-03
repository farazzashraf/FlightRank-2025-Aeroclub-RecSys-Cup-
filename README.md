# AeroClub RecSys 2025 — Flight Ranking Challenge

[![Kaggle Competition](https://www.kaggle.com/competitions/aeroclub-recsys-2025/overview)

---

## Project Overview

This repository contains a solution for the **AeroClub RecSys 2025** competition on Kaggle, focused on ranking flight options within user search sessions. The goal is to build a model that can effectively rank the flight offers to predict which option the user will select.

---

## 🚀 Current Status

* **Leaderboard Score:** 0.31126 (baseline achieved)
* The project is actively evolving with ongoing feature engineering and hyperparameter tuning.

---

## Dataset

The dataset consists of flight options for multiple user search sessions (`ranker_id`). Each session contains several flight choices, and the task is to rank these correctly based on historical selections.

**Key Columns:**

* `ranker_id`: Search session ID (grouping variable)
* `Id`: Unique flight option ID
* `selected`: Target label (1 if selected, 0 otherwise)
* Flight timings, segments, pricing, user info, company info, and cancellation policies

---

## Project Steps & Pipeline

### 1. Data Loading and Sampling

* Used **DuckDB** to efficiently query large Parquet files without loading everything into memory.
* Filtered sessions with at least 10 flight options to ensure meaningful ranking data.
* Sampled 10,000 sessions for experimentation and faster iteration.

### 2. Preprocessing & Feature Engineering

* Renamed columns to clear, snake\_case naming conventions for model clarity.
* Extracted datetime features (hour, minute, weekday, weekend flag) and applied cyclical encoding.
* Created a `trip_type` feature to differentiate between one-way and round-trip flights.
* Flattened multi-segment columns for each leg and connection.
* Handled missing values (imputation logic to be further developed).
* Target-encoded categorical variables using `category_encoders` with smoothing.

### 3. Train-Validation Split

* Applied **GroupShuffleSplit** on `ranker_id` to preserve session integrity during splitting.
* Prepared group arrays for LightGBM ranking.

### 4. Model Training & Hyperparameter Optimization

* Built a **LightGBM LambdaRank** model optimized to maximize **HitRate\@3** — the metric measuring if the correct flight is in the top 3 predictions.
* Used **Optuna** for hyperparameter tuning, including learning rate, num\_leaves, regularization, and sampling parameters.
* Employed early stopping and pruning to speed up training and avoid overfitting.

### 5. Final Model Training

* Trained the final model with the best-found hyperparameters.
* Saved the model to disk using `joblib` for reuse.

### 6. Inference & Submission Preparation

* Loaded the trained model for test prediction.
* Ensured feature order and encoding consistency on test data.
* Generated ranked predictions within each session.
* Created submission file aligned with Kaggle competition requirements.

---

## Next Steps

* Extensive **feature engineering** around flight segments, pricing policies, and cancellation rules.
* Tune hyperparameters further using longer Optuna runs or alternative samplers.
* Explore session-level temporal patterns and user behavior modeling.

---
