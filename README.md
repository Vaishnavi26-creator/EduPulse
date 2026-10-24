# EduPulse: Machine Learning-Based Student Academic Risk Early-Warning System

**EduPulse** is an end-to-end educational data mining and decision-support prototype designed to identify students experiencing academic difficulty at an early stage. By leveraging machine learning models, EduPulse evaluates academic indicators and engagement metrics to classify student risk levels, enabling timely and supportive educational interventions.

## Key Features

* **Recall-First Machine Learning Pipeline**: Prioritizes capturing true positive cases (students needing assistance) over standard accuracy to ensure early warning flags are not missed.

* **Interactive Dashboards**:
  * **Student Dashboard**: Personalized views for risk indicators, assignment metrics, and academic progress tracking.
  * **Faculty Dashboard**: Cohort-level risk summaries, priority queues, and student filtering.

* **Explainability & What-If Simulator**: Feature-importance breakdowns and counterfactual/what-if analysis tools for educators to explore hypothetical scenario outcomes.

* **Model Disagreement Alerts**: Multi-model comparison cues to highlight borderline or uncertain prediction cases.

## System Architecture & Workflow

EduPulse processes student features using a pipeline designed to prevent data leakage during feature transformation and evaluation.

```
+---------------------------+      +-------------------------------+      +-------------------------+
| Student Data              | ---> | Preprocessing & Selection     | ---> | Model Training          |
| (10,000 Records, 25 Col)  |      | (Imputation, Scaling, Mutual  |      | (Stratified 5-Fold CV:  |
|                           |      |  Information Top-10 Selection)|      |  LR, DT, Random Forest) |
+---------------------------+      +-------------------------------+      +-------------------------+
                                                                                       |
                                                                                       v
+---------------------------+      +-------------------------------+      +-------------------------+
| Web Application           | <--- | Risk Score & Priority         | <--- | Model Selection         |
| (Streamlit Dashboard)     |      | Category Output               |      | (Prioritizing Recall)   |
+---------------------------+      +-------------------------------+      +-------------------------+
```

1. **Preprocessing & Feature Selection**:
   * Median imputation for missing continuous variables; standard scaling for normalized inputs.
   * Categorical encodings via One-Hot Encoding.
   * Top 10 feature selection performed via Mutual Information within cross-validation loops to avoid data leakage.

2. **Model Training**: Evaluated **Logistic Regression**, depth-limited **Decision Tree**, and **Random Forest** models with class-weight balancing on a 70:30 target distribution split.

## Dataset & Model Performance

The evaluation was conducted on an expanded dataset of 10,000 student records using an 80:20 train-test stratified split. Models were evaluated using 5-fold cross-validation, with **Recall** prioritized over F1-score due to the high operational cost of unflagged academic risk.
<img width="1275" height="192" alt="image" src="https://github.com/user-attachments/assets/ccab9c2e-0a5a-4d30-84c4-984718181cea" />
Model Selected: Logistic Regression

## Repository Structure

```
EduPulse/
├── data/
│   └── student_data.csv        # Dataset containing 10,000 student academic and engagement records
├── models/
│   └── pipeline.joblib         # Serialized preprocessing and Logistic Regression model
├── src/
│   ├── preprocessing.py        # Imputation, scaling, and mutual information selection pipelines
│   ├── train.py                # Model comparison and cross-validation execution
│   └── utils.py                # Helper functions for evaluations and visualizations
├── app.py                      # Streamlit web interface entry point
├── requirements.txt            # Dependency configuration
├── README.md                   # Project documentation
└── LICENSE                     # Open-source license file
```

## Quick Start & Installation

### Prerequisites

* Python 3.8 or higher
* `git` installed on your machine

### Setup Instructions

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-username/EduPulse.git
   cd EduPulse
   ```

2. **Create a Virtual Environment**:
   ```bash
   python -m venv venv
   # On Windows
   venv\Scripts\activate
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the Streamlit Application**:
   ```bash
   streamlit run app.py
   ```
   Open your browser and navigate to `http://localhost:8501`.

## Limitations & Ethical Considerations

* **Decision-Support Role**: EduPulse is intended solely as an advisory tool to facilitate academic support conversations. Risk predictions should **never** replace professional judgment or result in punitive action.
* **Dataset Scope**: While expanded to 10,000 records, the dataset represents a prototype benchmark. Independent longitudinal institutional validation remains necessary before operational campus deployment.
