# Machine Learning Pipeline – Mining Production & Shipping Optimization

## Project Overview

This repository contains the **Machine Learning (ML) pipeline** used to support an **Agentic AI system for mining production and shipping optimization**.

The ML component is responsible for:

* Generating **predictive outputs** from historical and forecast data
* Providing **structured, explainable inputs** to the Agentic AI layer
* Ensuring **reproducibility, consistency, and traceability** of model inference

**Important Note:**
This repository **does not handle conversational logic, memory, or user interaction**. Those responsibilities belong to the backend and Agentic AI layers.

---
## Value Proposition

This ML pipeline enables **data-driven mining value optimization** by transforming
uncertain environmental and operational signals into **actionable, structured predictions**.

Instead of reacting to delays and production shortfalls, the system supports:
- Proactive planning
- Risk-aware decision making
- Operational efficiency under uncertainty

---

## ML System Architecture (High Level)

```
Raw Data
   ↓
Preprocessing & Feature Engineering
   ↓
Trained ML Models (4 Models)
   ↓
Unified ML Output (ml_output_clean)
   ↓
Agentic AI (Decision & Recommendation Layer)
```


---

## Model Overview

The ML system consists of **four sequential models**, each serving a specific operational purpose.

### **Model 1 – Weather Forecasting (Regression)**

**Objective:**
Predict short-term environmental conditions affecting mining and shipping operations.

**Examples of Outputs:**

* Temperature
* Wind speed
* Rainfall
* Other weather-related variables

**Model Type:**

* XGBoost Regressor
* Time-series forecasting with **sliding window approach**

---

### **Model 2 – Weather Condition Classification**

**Objective:**
Classify forecasted weather into operational categories (e.g., safe, moderate risk, high risk).

**Inputs:**

* Output from Model 1

**Outputs:**

* Discrete weather condition class
* Used as a decision signal for downstream models

**Model Type:**

* XGBoost / Tree-based Classifier

---

### **Model 3 – Effective Capacity Prediction (Regression)**

**Objective:**
Estimate the **effective operational capacity** of mining and logistics systems under predicted weather conditions.

**Inputs:**

* Weather forecast (Model 1)
* Weather class (Model 2)
* Operational features

**Outputs:**

* Effective capacity value

---

### **Model 4 – Actual Production Prediction (Regression)**

**Objective:**
Predict **actual mine production output** given effective capacity and environmental constraints.

**Inputs:**

* Effective capacity (Model 3)
* Weather & operational signals

**Outputs:**

* Expected actual production
  
### Output Terminology

Although named `actual_output_ton`, this value represents:

> **Predicted Realized Production Output**

It estimates the *expected actual production* under current constraints,
not the planned or theoretical capacity.

---

## Model Architecture & Parameters

This section describes the **feature construction, input variables (X), target variables (y), and model configurations** used in each stage of the ML pipeline.

### **Model 1 – Weather Forecasting Architecture**

**Target Variables (y):**
- `rainfall_mm`
- `temperature_c`
- `humidity_pct`
- `wind_speed_kmh`

**Feature Engineering Strategy:**
- Time-based features:
  - `month`
  - `week`
  - `dayofyear`
- Lag features per mine:
  - Lag-1, Lag-3, Lag-7
- Rolling statistics per mine:
  - Rolling mean (3-day, 7-day)

**Model Configuration:**
- Algorithm: `XGBRegressor`
- Key parameters:
  - `n_estimators = 500`
  - `learning_rate = 0.05`
  - `max_depth = 6`
  - `subsample = 0.8`
  - `colsample_bytree = 0.8`
  - `objective = reg:squarederror`
- Training strategy:
  - Time-based train–test split (80% / 20%)
  - Early stopping (20 rounds)
  - Evaluation metric: RMSE

---

### **Model 2 – Weather Classification Architecture**

**Target Variable (y):**
- `weather_remark` (e.g., Cerah, Mendung, Hujan Ringan, Hujan Lebat)

**Input Features (X):**
- Numerical weather forecasts from Model 1:
  - `rainfall_mm`
  - `temperature_c`
  - `humidity_pct`
  - `wind_speed_kmh`

**Model Configuration:**
- Algorithm: `XGBClassifier`
- Objective: `multi:softmax`
- Hyperparameter tuning:
  - GridSearchCV (5-fold CV)
- Optimized parameters:
  - `max_depth = 4`
  - `learning_rate = 0.05`
  - `n_estimators = 150`
  - `subsample = 1`
  - `colsample_bytree = 0.8`
- Evaluation metric:
  - Accuracy, Precision, Recall, F1-score

---

### **Model 3 – Effective Capacity Prediction Architecture**

**Target Variable (y):**
- `effective_capacity_ton_day`

**Input Features (X):**
- `mine_id`
- `equipment_type`
- `road_condition`
- `weather_condition`
- `availability_pct`

**Model Configuration:**
- Algorithm: `RandomForestRegressor`
- Hyperparameter optimization:
  - Bayesian Optimization (`BayesSearchCV`)
- Key optimized parameters:
  - `n_estimators = 200`
  - `max_depth = 11`
  - `max_features = log2`
  - `min_samples_leaf = 4`
  - `min_samples_split = 10`
- Evaluation metrics:
  - MSE
  - R²
  - MAPE

---

### **Model 4 – Actual Production Prediction Architecture**

**Target Variable (y):**
- `actual_output_ton`

**Input Features (X):**
- `road_condition`
- `weather_condition`
- `availability_pct`
- `effective_capacity_ton_day`
- `planned_output_ton`

**Preprocessing Pipeline:**
- Categorical encoding:
  - One-Hot Encoding (`road_condition`, `weather_condition`)
- Numerical scaling:
  - StandardScaler

**Model Configuration:**
- Algorithm: `RandomForestRegressor`
- `n_estimators = 100`
- Integrated using `sklearn Pipeline`

---

## Evaluation & Results

This section summarizes the **quantitative performance** of each model on held-out test data.

### **Model 1 – Weather Forecasting Performance (MAE)**

| Variable | MAE |
|--------|-----|
| Rainfall (mm) | 0.271 |
| Temperature (°C) | 0.441 |
| Humidity (%) | 2.005 |
| Wind Speed (km/h) | 0.150 |

---

### **Model 2 – Weather Classification Performance**

- Accuracy: **~83.8%**
- Weighted F1-score: **~0.79**
- Strong performance on dominant operational classes (Mendung, Hujan Ringan, Hujan Lebat)
- Misclassification mainly occurs in minority classes (e.g., Cerah)

---

### **Model 3 – Effective Capacity Prediction Performance**

- R² Score: **0.91**
- MAPE: **~1.27%**
- Indicates strong explanatory power and low relative error for operational capacity estimation


**Visualization:**  
Actual vs Predicted Effective Capacity

<p align="center">
  <img 
    src="assets/Actual%20vs%20Predicted%20Values%20(Tuned%20Random%20Forest%20Regressor).png"
    width="600"
    alt="Actual vs Predicted Effective Capacity"
  />
</p>

---

### **Model 4 – Actual Production Prediction Performance**

- R² Score: **0.89**
- RMSE: **~11,667 tons**
- Demonstrates reliable estimation of realized production under operational constraints

**Visualization (Example):**
> Actual vs Predicted Production Output  
> *(Insert prediction comparison plot here)*

---

## Pipeline Dependency Flow

```
Model 1 (Weather Forecast)
        ↓
Model 2 (Weather Classification)
        ↓
Model 3 (Effective Capacity)
        ↓
Model 4 (Actual Production)
```

Each model output is **required** for the next stage.

---

## Preprocessing & Feature Engineering

All preprocessing steps are treated as **first-class artifacts** and must be preserved alongside trained models.

This includes:

* Feature scaling / normalization
* Encoding logic
* Sliding window configuration
* Feature ordering
* Handling of missing values
* Time alignment logic

**Important:**
**Models cannot be used independently without their corresponding preprocessing pipelines.**

---

## Model Artifacts

Each trained model is stored together with:

* Model binary (e.g., `.pkl`, `.json`)
* Preprocessing objects (scalers, encoders, window logic)
* Feature metadata

Example structure:

```
models/
├── weather_forecast/
│   ├── model.pkl
│   ├── preprocessor.pkl
│   └── metadata.json
├── weather_classification/
├── effective_capacity/
└── actual_production/
```

---

## Unified ML Output

All model outputs are merged into a single structured JSON object:

```json
{
  "weather_forecast": {...},
  "weather_classification": {...},
  "effective_capacity": {...},
  "actual_production": {...}
}
```

This object is referred to as:

```
ml_output_clean
```

### Purpose of `ml_output_clean`

* Single source of truth for downstream AI systems
* Direct input to the Agentic AI recommendation engine
* Serializable and API-ready

---

## Integration with Agentic AI

The ML pipeline **does not generate recommendations directly**.

Instead:

* ML produces **predictions**
* Agentic AI consumes predictions + operational constraints
* Agentic AI generates **optimized recommendations and justifications**

This separation ensures:

* Explainability
* Maintainability
* Clear ownership between ML and AI reasoning layers

---

## ML Responsibility Scope

✅ Model development & evaluation
✅ Feature engineering & preprocessing
✅ Model inference pipeline
✅ Structured output generation

❌ Conversational memory
❌ User interaction
❌ Chat logic
❌ Decision orchestration

---

## Reproducibility & Deployment

* All models are exportable and reloadable in inference environments (e.g., Google Colab)
* No retraining occurs during production inference
* Models are designed for **read-only inference usage**

---

## Assumptions & Limitations

* Predictions are dependent on input data quality
* Weather uncertainty propagates through downstream models
* ML outputs are **decision support**, not final operational decisions

---

## Notes for Backend & AI Teams

* Backend must handle:

  * API orchestration
  * Chat history memory
  * Data persistence
* Agentic AI must:

  * Consume ML outputs
  * Apply reasoning and optimization logic
  * Generate user-facing explanations

---
