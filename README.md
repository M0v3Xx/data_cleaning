# Automobile Data Cleaning & Price Prediction

## 📌 Overview
This project performs end-to-end data cleaning, exploratory data analysis (EDA), and predictive modeling on the Automobile Dataset. Using **Scikit-Learn** and **Pandas**, missing values across vehicle features (such as `bore`, `stroke`, `horsepower`, and `price`) were systematically imputed using Linear Regression models based on strongly correlated attributes.

## 🛠 Tech Stack
- **Language:** Python
- **Libraries:** Pandas, NumPy, Scikit-Learn, Matplotlib
- **Environment:** Jupyter Notebook

## 🔍 Key Highlights & Methodology
- **Exploratory Data Analysis:** Computed feature correlation matrix to identify key price predictors (`engine-size`, `curb-weight`, `horsepower`).
- **Regression Imputation:** Imputed missing values for continuous variables like `bore` and `horsepower` using Linear Regression models fitted on highly correlated features (e.g., `curb-weight` and `engine-size`).
- **Price Modeling:** Built a linear model predicting vehicle price based on `engine-size` ($R^2$ baseline evaluation).

## 🚀 How to Run
1. Clone the repository:
   ```bash
   git clone [https://github.com/M0v3Xx/automobile-price-analysis.git](https://github.com/M0v3Xx/automobile-price-analysis.git)
