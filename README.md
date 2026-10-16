# Identifying Significant Predictors of Industrial Equipment Failure

AAI-500 Final Team Project, Team 1: Justin Nguyen, Kyle Abaya

## Project Overview

This project identifies which operating conditions are statistically
significant predictors of machine failure, and builds models that predict
failure so maintenance can be done before a breakdown happens.

## Dataset

AI4I 2020 Predictive Maintenance Dataset (UCI Machine Learning Repository, id 601)

- 10,000 records of simulated industrial sensor data
- Features: product type, air temperature, process temperature, rotational speed, torque, tool wear
- Target: machine failure (about 3.4% of records)
- Source: https://doi.org/10.24432/C5HS5C (CC BY 4.0)

The notebook downloads the data automatically with `ucimlrepo`.

## Repository Contents

- `data_analysis.ipynb`: data cleaning, exploratory data analysis, and modeling

## How to Run

1. Install the required packages:
   pip install numpy pandas matplotlib seaborn scipy scikit-learn statsmodels pgmpy ucimlrepo
2. Open `data_analysis.ipynb` in Jupyter and select **Run All**.

## Methods

- Data cleaning: removed 9 failure records with no recorded cause
- Exploratory data analysis: distributions, chi-square test, Welch's t-tests, correlation, and physics-based features
- Models: two Bayesian networks (Hill Climb, PC) and two logistic regressions (raw sensors, physics-based features)

## Key Findings

- Final model: logistic regression with physics-based features (PR-AUC 0.50, ROC-AUC 0.93)
- Significant predictors of failure: power, temperature difference, strain (tool wear × torque), and rotational speed
- Product quality type is not significant once operating conditions are known
