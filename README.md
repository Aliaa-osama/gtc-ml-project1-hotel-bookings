=======
# Hotel Booking Cancellations – Data Cleaning & Preprocessing Project

## 📌 Project Overview
This project is part of **GTC ML Project 1**, focusing on **data cleaning and preprocessing** for a hotel booking dataset (`hotel_bookings.csv`).  
The business objective is to prepare raw data for building a machine learning model that predicts **booking cancellations**, which directly affect hotel profitability.

> ⚠️ Note: The scope of this project is **data preparation only** – not building the predictive model.

---

## 🏨 Business Problem
The **Revenue Team** identified that **last-minute cancellations** significantly reduce profitability.  
By preparing a **clean and robust dataset**, future machine learning models can better predict cancellations, allowing the business to take proactive measures (e.g., overbooking strategies, deposit requirements).

---

## 📂 Dataset
- **Source**: Hotel Property Management System (PMS)  
- **File**: `hotel_bookings.csv`  
- **Size**: ~119,000 rows × 32 columns  
- **Target Variable (for future ML use)**: `is_canceled`

---

## ⚙️ Project Phases

### **Phase 1 – Exploratory Data Analysis (EDA) & Data Quality Report**
- Loaded dataset and generated summary statistics (`.describe()`, `.info()`).
- Identified missing values and visualized them (missingno matrix / heatmap).
- Detected outliers in numerical columns (`adr`, `lead_time`) using **boxplots** and **IQR method**.
- Documented key **data quality issues**.

---

### **Phase 2 – Data Cleaning**
- **Missing Values**  
  - `company`, `agent`: replaced with `"None"` or `0`  
  - `country`: imputed with **mode** or `"Unknown"`  
  - `children`: imputed with **median/mode**
- **Duplicates**: dropped exact duplicates.  
- **Outliers**: capped extreme `adr` values (above `1000` set to `1000`).  
- **Data Types**: ensured date columns were converted to proper `datetime`.

---

### **Phase 3 – Feature Engineering & Preprocessing**
- **New Features**  
  - `total_guests = adults + children + babies`  
  - `total_nights = stays_in_weekend_nights + stays_in_week_nights`  
  - `is_family = 1 if (children > 0 or babies > 0) else 0`
- **Categorical Encoding**  
  - One-Hot Encoding for **low-cardinality features** (`meal`, `market_segment`).  
  - Frequency encoding or "Other" grouping for **high-cardinality features** (`country`).  
- **Data Leakage Prevention**  
  - Dropped `reservation_status` and `reservation_status_date`.  
- **Final Dataset Split**  
  - Train-Test split (`test_size=0.2`, `random_state=42`).

---

## 🎯 Deliverables
1. **Cleaned Dataset** ready for ML pipeline.  
2. **EDA & Data Quality Report** documenting missing values, outliers, and fixes.  
3. **Feature-Engineered Dataset** with new derived features.  
4. **Preprocessing Pipeline** applied consistently for reproducibility.

---

## 🛠️ Tech Stack
- **Language**: Python 3.x  
- **Libraries**:  
  - `pandas`, `numpy` – Data manipulation  
  - `matplotlib`, `seaborn`, `missingno` – Visualization  
  - `scikit-learn` – Preprocessing, train-test split  

---

## 🚀 Next Steps
- Train ML models (Logistic Regression, Random Forest, Gradient Boosting).  
- Evaluate performance using **AUC, Precision, Recall, F1-score**.  
- Deploy the model into a dashboard for real-time cancellation predictions.

---



