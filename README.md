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
