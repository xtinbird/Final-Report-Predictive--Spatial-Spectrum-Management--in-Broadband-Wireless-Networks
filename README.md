# Final Report Predictive Spatial Spectrum Management in Broadband Wireless Networks

**UC Berkeley Professional Certificate in Machine Learning and Artificial Intelligence**

**Required Capstone Project 24.1: Final Report**

---

## Project Summary

In dense urban mobile networks, users are constantly moving. To maintain fast and reliable connections, antenna beams must continuously track these users. When the network reacts too late, signal quality drops, users experience slower speeds, and interference between nearby users increases.

This project develops a machine learning system that helps the network act **before** problems occur. The system forecasts future signal strength, recommends the best antenna beam in advance, and identifies users who are at high risk of interference.

**Jupyter Notebook:** [01_EDA_and_Baseline_Model.ipynb](./01_EDA_and_Baseline_Model.ipynb)

---

## Business Problem

Mobile operators in crowded city environments face three recurring challenges:

- Users move quickly between coverage areas
- Antenna beams must constantly adjust
- Interference rises when the network only reacts after signal quality has already declined

If left unsolved, these issues lead to poorer user experience, lower data speeds, and inefficient use of limited radio spectrum.

**Project Goal:**  
Build a proactive machine learning pipeline that predicts signal problems early and supports smarter network decisions.

---

## How the Solution Works

The system performs three complementary tasks:

1. **Forecasts future signal strength**  
   Uses time-series models (including LSTM) to predict how signal quality is likely to change.

2. **Recommends the optimal antenna beam**  
   Predicts the best beam direction before the signal weakens.

3. **Flags high-interference-risk users**  
   Identifies users who are likely to experience or cause interference so the network can take protective action.

---

## Data Sources

- **Synthetic Data (Primary):**  
  Generated to realistically simulate user mobility, signal conditions (RSRP, RSRQ, SINR), speed, direction, Doppler shift, beam indices, and multi-cell interference.

- **Real-World Data (Validation):**  
  Public “Mobility Dataset from a 7.2 O-RAN deployment” containing real base-station measurements of RSRP, RSRQ, and SINR.  
  Source: https://data.mendeley.com/datasets/khxgr6m8wz/1

Using both data sources allows the system to be developed with rich features and then validated under real network conditions.

---

## Key Findings

- **SINR** and **RSRP** are the strongest predictors of high-interference risk.
- Signal quality clearly declines as users move away from base stations or travel between cells.
- **Random Forest** performed best among classification models for detecting high-risk users.
- Tree-based models outperformed simple Linear Regression when predicting the next antenna beam.
- **LSTM** shows promising capability for forecasting future signal strength.
- On real O-RAN data, the system achieved high precision (very few false alarms). This is valuable because unnecessary network changes are costly.
- A performance gap exists between synthetic and real data (sim-to-real gap). This is expected and can be reduced by further training on real measurements.
- A decision threshold around 0.40–0.45 provides a practical balance between catching high-risk users and limiting false alarms.

---

## Business Recommendations

1. **Flag high-risk users early**  
   Use the model to identify potential interference problems before customers experience degraded service.

2. **Prefer high-confidence alerts**  
   High precision is more valuable than catching every possible case, because false alarms waste network resources.

3. **Keep SINR and RSRP as core inputs**  
   These signal measures consistently provide the most useful information.

4. **Combine forecasting, beam prediction, and risk detection**  
   Integrating these capabilities into one pipeline will deliver greater operational value.

---

## Next Steps

- Fine-tune the best models on more real network data to reduce the sim-to-real gap
- Improve multi-step signal strength forecasting using LSTM
- Integrate all components (forecasting + beam prediction + risk detection) into a single operational pipeline
- Measure business impact using simple metrics such as reduction in interference events or improvement in average user speed

---

## Technical Overview

The full technical analysis is available in the Jupyter Notebook linked above. It includes:

- Data cleaning and exploratory analysis
- Feature engineering
- Multiple classification and regression models
- Cross-validation and Grid Search
- Real-world validation
- LSTM and ARIMA signal forecasting experiments

**Main models evaluated:**
- Classification: Random Forest, KNN, Logistic Regression, Decision Tree
- Regression: Linear Regression, Random Forest Regressor, Decision Tree Regressor
- Forecasting: ARIMA and LSTM

---

## How to Explore This Project

1. Read this README for the business summary and findings
2. Open `01_EDA_and_Baseline_Model.ipynb` for the complete technical analysis
3. Run the notebook cells in order
