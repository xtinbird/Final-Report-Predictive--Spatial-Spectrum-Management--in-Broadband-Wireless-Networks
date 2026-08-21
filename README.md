# Final Report Predictive Spatial Spectrum Management in Broadband Wireless Networks

**UC Berkeley Professional Certificate in Machine Learning and Artificial Intelligence**

**Required Capstone Project 24.1: Final Report**

---

## Executive Summary

- Mobile networks in dense urban areas struggle to maintain signal quality as users move.
- This project builds a machine learning system that predicts signal strength, recommends the best antenna beam, and flags users at high risk of interference.
- Models were trained on synthetic data and validated on real O-RAN measurements.
- The best model (Random Forest) produces highly reliable high-risk alerts with very few false alarms.
- A performance gap exists between synthetic and real data, but the system remains practically useful.
- With further fine-tuning on real network data, this approach can support proactive and more efficient network management.

**Jupyter Notebook:** [Capstone_Final_and_Model_Comparision.ipynb](./Capstone_Final_and_Model_Comparision.ipynb)

---

## Business Problem

In dense urban mobile networks, users move frequently and antenna beams must continuously track them. When the network reacts too late:

- Signal quality drops
- Interference increases
- User experience degrades

This project develops a proactive machine learning pipeline to address these challenges.

---

## Project Goal

Build a system that can:
1. Forecast future signal strength
2. Predict the optimal antenna beam in advance
3. Identify users at high risk of interference

---

## Data Sources

- **Synthetic Data (Primary):**  
  Generated in Python to simulate user mobility, multi-cell interference, RSRP, RSRQ, SINR, speed, heading, Doppler shift, beam indices, and resource block utilization.

- **Real-World Data (Validation):**  
  Public “Mobility Dataset from a 7.2 O-RAN deployment” containing real base-station measurements of RSRP, RSRQ, and SINR.  
  Source: https://data.mendeley.com/datasets/khxgr6m8wz/1

---

## Methods

### Classification (High Interference Risk)
- Random Forest
- Decision Tree
- Logistic Regression
- K-Nearest Neighbors (KNN)

### Regression (Next Beam Prediction)
- Linear Regression
- Random Forest Regressor
- Decision Tree Regressor

### Time-Series Forecasting
- ARIMA
- LSTM

### Model Validation
- Stratified K-Fold Cross-Validation
- Grid Search hyperparameter tuning
- Real O-RAN external validation
- Decision threshold analysis

---

## Handling Imbalanced Data

Class imbalance is an important challenge in this project, especially in the real O-RAN dataset, where Low Risk samples far outnumber High Risk samples.

To address this, the following techniques were used:

- **Stratified train-test splits and Stratified K-Fold Cross-Validation** to preserve class proportions during training and evaluation
- **Class weighting (`class_weight="balanced"`)** in Random Forest, Logistic Regression, and Decision Tree models so the minority High Risk class receives greater emphasis
- **Focus on Precision, Recall, and F1-Score** instead of Accuracy alone, because Accuracy can be misleading under imbalance
- **Decision threshold tuning** on real data to improve the practical balance between detecting high-risk users and limiting false alarms

These steps are especially important for real-world deployment, where high-risk events are rare but operationally important, and false alarms can be costly.

---

## Key Findings

- **SINR** and **RSRP** are the strongest predictors of high-interference risk.
- On synthetic data, tree-based models achieved near-perfect classification performance.
- For beam prediction, the tuned **Random Forest Regressor** performed best (lowest MAE).
- ARIMA provided a solid baseline for SINR forecasting; LSTM was also explored.
- On real O-RAN data:
  - Precision was extremely high (≈ 0.998–1.000)
  - Recall was moderate (≈ 0.55)
  - F1-Score was approximately 0.71
- Additional hyperparameter optimization did not improve real-world recall, suggesting the main limitation is the sim-to-real gap and limited shared features rather than model settings alone.
- A clear **sim-to-real gap** exists, which is expected when moving from simulation to real measurements.
- A decision threshold between **0.40 and 0.50** offers the best practical balance.
- The model remains useful in practice because its High Risk predictions are highly reliable.

---

## Business Recommendations

1. Use the model to flag high-risk users early, before service quality drops.
2. Trust high-precision alerts to avoid unnecessary network interventions.
3. Keep SINR and RSRP as core input features.
4. Operate with a decision threshold around 0.40–0.50.
5. Integrate forecasting, beam prediction, and risk detection into one pipeline.

---

## Next Steps

- Fine-tune the best model on real O-RAN measurements to reduce the sim-to-real gap.
- Improve synthetic data realism (for example, using MATLAB’s 5G Toolbox).
- Obtain richer real-world data from a professional network or operator testbed.
- Enhance multi-step signal strength forecasting.
- Connect model outputs to network control systems for automated action.
- Measure impact using operational metrics such as reduced interference events or improved user throughput.

---

## How to Run

1. Clone this repository
2. Open `Capstone_Final_and_Model_Comparision.ipynb`
3. Run all cells in order
