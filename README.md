# GTC ML Project 1 – Hotel Booking Data Cleaning & Preprocessing

This project was completed as part of the Machine Learning Internship at Genius Technology Center (GTC).  
The main objective is to analyze hotel booking data and identify the factors that influence booking cancellations, ultimately helping hotels reduce revenue losses.

---

## Project Overview

Cancellations represent a significant loss for the hospitality industry. By analyzing historical booking data, we aim to **anticipate cancellations** and provide actionable insights for revenue management.

The dataset contains **119,390 bookings** from Resort Hotels and City Hotels, with features including:

- Lead time (`lead_time`)  
- Cancellation status (`is_canceled`)  
- Number of nights (weekend and weekday)  
- Guest nationality (`country`)  
- Meal type (`meal`)  
- Booking channel (`distribution_channel`)  
- Total number of guests (`total_guests`)  
- Average daily rate (`adr`)  

The goal is to transform this raw data into a clean, structured dataset ready for machine learning.

---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Loaded and inspected the dataset (.head(), .info(), .describe()).  
- Identified missing values and visualized them using heatmaps and missingno.  
- Detected outliers in numerical columns such as `adr`, `lead_time`, and `stays_in_weekend_nights` using boxplots and the IQR method.  

### 2. Data Cleaning
- **Missing Values:**  
  - `children` → filled with median  
  - `country` → filled with `"unknown"`  
  - `agent`, `company` → filled with `0`  
- **Duplicates:** Removed exact duplicate rows.  
- **Outliers:**  
  - Capped `adr` at 1000  
  - Removed negative values in numerical columns  
- **Data Types:**  
  - Converted `reservation_status_date` to datetime  
  - Converted categorical columns to `category` type for efficiency  

### 3. Feature Engineering & Preprocessing
- Created `total_guests = adults + children + babies`  
- Created `total_nights = stays_in_weekend_nights + stays_in_week_nights`  
- Created `is_family` flag based on presence of children or babies  
- Dropped **data leakage columns**: `reservation_status` and `reservation_status_date`  

### 4. Encoding Categorical Variables
- Low-cardinality features (`meal`, `market_segment`, `distribution_channel`) → One-Hot Encoding  
- High-cardinality feature (`country`) → Frequency Encoding  
- Converted `is_family` to numeric labels  

### 5. Train/Test Split
Prepared the dataset for machine learning:  

```python
from sklearn.model_selection import train_test_split

X = df1.drop('is_canceled', axis=1)
y = df1['is_canceled']

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
