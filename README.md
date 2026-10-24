# UAV GPS Spoofing Detection Experiment

Python implementation for cleaning, benchmarking, and explaining machine learning models on UAV GNSS telemetry data.

## Dataset

The experiment telemetry and extracted feature dataset is hosted externally due to file size constraints:

- **File:** `GPS_Data_Simplified_2D_Feature_Map.xlsx` (~72 MB)
- **Download Link:** [Download Dataset from Mendelay Univeristy of North Dakota](https://data.mendeley.com/datasets/z7dj3yyzt8/3)

### Usage

1. Download the file from the link above.
2. Place it in the project root.

## Project Structure

```text
├── GPS_Data_Simplified_2D_Feature_Map.xlsx  # Raw dataset source
├── UAV_GPS_Spoofing_Experiment.ipynb        # Main experiment notebook
├── requirements.txt                         # Python dependencies
                                
```

## Features & Pipeline

1. **Ingestion & Caching:** Reads raw GNSS records and saves a local CSV cache for faster execution.
2. **Data Sanitization:** Cleans invalid/NaN values and removes 31,218 duplicate rows caused by receiver overpolling.
3. **Correlation & Leakage Analysis:**
   - Computes Pearson correlation across all 13 receiver telemetry features.
   - Detects collinear pairs (`DO`/`TCD`, `EC`/`LC`/`PC`).
   - Identifies temporal leakage between monotonic clock counters (`TOW`, `RX`) and target labels.
4. **Stratified Split & Scaling:** 80/20 train-test partition using `StandardScaler` fitted strictly on the training partition.
5. **Model Benchmarking:** Evaluates 7 classifiers (Logistic Regression, Linear SVM, KNN, Decision Tree, Random Forest, Gradient Boosting, XGBoost) across Accuracy, Precision, Recall, F1-Score, ROC-AUC, FPR, MCC, and latency ($\mu\text{s}$).
6. **Interpretability & Visualization:**
   - Plots comparative ROC curves and the top-model confusion matrix.
   - Computes SHAP values to evaluate feature importance before and after clock leakage ablation.

## Installation

```bash
pip install -r requirements.txt
```

### `requirements.txt`
```text
numpy>=1.24.0
pandas>=2.0.0
scipy>=1.10.0
scikit-learn>=1.3.0
xgboost>=2.0.0
shap>=0.44.0
matplotlib>=3.7.0
seaborn>=0.12.0
openpyxl>=3.1.0
```

## Running the Code

1. Ensure `GPS_Data_Simplified_2D_Feature_Map.xlsx` is in the project root directory.
2. Open and run all cells in `UAV_GPS_Spoofing_Experiment.ipynb`:
   ```bash
   jupyter notebook UAV_GPS_Spoofing_Experiment.ipynb
   ```
3. Generated tables (`.csv`) and figures (`.png`, `.pdf`) will be saved automatically to the working directory.