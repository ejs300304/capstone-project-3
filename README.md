# capstone-project-3

# Saudi Arabia Used Car Price Prediction

## Project Overview

This project develops a machine learning regression model to predict used car prices in Saudi Arabia (SAR) based on vehicle characteristics such as manufacturer, model, manufacturing age, engine size, mileage, transmission type, and vehicle options.

The objective is to help buyers, sellers, and automotive businesses better understand used car pricing and make more informed pricing decisions through data-driven predictions.

## Business Problem

Used car prices can vary significantly depending on vehicle condition, age, mileage, brand, and specifications. Determining a reasonable selling price can be challenging for both buyers and sellers.

This project aims to:
- Identify the key factors influencing used car prices.
- Develop and compare regression models to predict vehicle prices.
- Evaluate model performance and prediction errors across different price segments.
- Provide insights and recommendations for more informed pricing decisions.

## Dataset

**Dataset:** Saudi Arabia Used Cars Dataset  

The dataset contains 5,624 used car listings collected from Syarah.com.

### Features

| Feature | Description |
|---|---|
| Type | Vehicle model |
| Region | Region where the car is listed |
| Make | Vehicle manufacturer |
| Gear_Type | Transmission type |
| Origin | Vehicle origin |
| Options | Vehicle trim or options |
| Year | Manufacturing year |
| Engine_Size | Engine size |
| Mileage | Total mileage |
| Price | Listed car price in SAR (target variable) |

## Data Preprocessing

The following preprocessing steps were performed:

1. **Data Cleaning**
   - Inspected missing values and duplicate records.
   - Examined suspicious price values.
   - Excluded listings with a price of zero because they were marked as negotiable.

2. **Feature Engineering**
   - Created `Car_Age` using the formula:

     `Car_Age = 2026 - Year`

   - Excluded the original `Year` column after creating `Car_Age`.
   - Excluded `Negotiable` to avoid target leakage because it is derived from the target price.

3. **Categorical Encoding**
   - Binary Encoding: `Type`, `Region`, and `Make`.
   - One-Hot Encoding: `Gear_Type`, `Origin`, and `Options`.

4. **Feature Scaling**
   - Applied StandardScaler to numerical features: `Car_Age`, `Engine_Size`, and `Mileage`.

5. **Train-Test Split**
   - Training set: 80%
   - Testing set: 20%
   - Random state: 42

## Exploratory Data Analysis (EDA)

EDA was conducted to understand the dataset's characteristics and identify patterns related to used car prices.

Key areas of investigation included:
- Distribution of used car prices.
- Price differences across vehicle manufacturers and models.
- Relationship between car age, engine size, mileage, and price.
- Distribution of listings across regions.
- Price patterns across vehicle options and transmission types.
- Potential outliers and unusual price listings.

## Model Development

Five regression algorithms were trained and compared:

1. Linear Regression
2. Ridge Regression
3. Random Forest Regressor
4. Extra Trees Regressor
5. Gradient Boosting Regressor

A preprocessing and modeling Pipeline was used to ensure consistent transformations during training and prediction.

### Model Evaluation

The models were evaluated using:

- **MAE (Mean Absolute Error):** Measures the average absolute difference between actual and predicted prices.
- **RMSE (Root Mean Squared Error):** Penalizes larger prediction errors more heavily.
- **R² Score:** Measures how much variation in the target variable is explained by the model.

### Initial Model Comparison

| Model | MAE (SAR) | RMSE (SAR) | R² |
|---|---:|---:|---:|
| Extra Trees | 17,480.40 | 33,578.35 | 0.77 |
| Random Forest | 18,965.60 | 34,668.64 | 0.76 |
| Gradient Boosting | 22,054.91 | 36,107.87 | 0.74 |
| Ridge Regression | 35,545.96 | 58,540.35 | 0.31 |
| Linear Regression | 35,555.57 | 58,538.42 | 0.32 |

**Extra Trees Regressor** achieved the best initial performance among the five models.

## Hyperparameter Tuning

RandomizedSearchCV with 5-fold cross-validation was used to optimize the Extra Trees Regressor.

The hyperparameter search included:
- `n_estimators`
- `max_depth`
- `min_samples_split`
- `min_samples_leaf`
- `max_features`

### Best Parameters

```python
{
    'n_estimators': 300,
    'max_depth': None,
    'min_samples_split': 2,
    'min_samples_leaf': 1,
    'max_features': 1.0
}
```

### Tuned Model Performance

| Metric | Result |
|---|---:|
| MAE | 17,366.37 SAR |
| RMSE | 33,420.93 SAR |
| R² | 0.7767 |

The tuned Extra Trees model achieved a modest improvement over the initial model.

*Note: These results correspond to the earlier reported test set and should be updated if the final dataset or train-test split changes.*

## Feature Importance

Impurity-based feature importance (Mean Decrease in Impurity) was extracted from the trained Extra Trees model to identify influential features.

The leading individual encoded features included:

| Feature | Importance |
|---|---:|
| Car_Age | 0.229602 |
| Engine_Size | 0.119916 |
| Options_Full | 0.111014 |
| Mileage | 0.063043 |
| Make-related encoded features | Varies |

The results suggest that vehicle age, engine size, vehicle options, and mileage are important predictors of used car prices.

*Note: Binary-encoded categorical features are represented by multiple columns, so their individual importance values should not be interpreted as the total importance of the original categorical variable.*

## Business Recommendations

Based on the model development and evaluation:

1. **Consider Vehicle Age:** Car age is an important pricing factor. Businesses should account for depreciation when estimating resale values.
2. **Evaluate Vehicle Specifications:** Engine size, mileage, and trim options should be considered when comparing similar vehicles.
3. **Use Price Segmentation:** Prediction errors differ across price ranges. Separate pricing strategies or models may improve estimates for budget and luxury vehicles.
4. **Support Pricing Decisions:** The model can serve as a reference tool for initial price estimation, helping buyers and sellers establish more reasonable price expectations.
5. **Review Unusual Listings:** Extremely low or high predicted prices should be reviewed against comparable market listings before making pricing decisions.

## Limitations

- The dataset contains listings from a specific source and may not represent the entire Saudi Arabian used car market.
- Vehicle condition, accident history, service records, and ownership history are not included.
- Listing prices may differ from actual transaction prices.
- Some vehicle models and categories may have limited observations.
- Prediction errors are higher for certain price segments, particularly lower-priced and high-priced vehicles.
- The `Car_Age` feature uses 2026 as a fixed reference year and should be updated when the model is reused in future years.

## Future Improvements

- Incorporate vehicle condition, accident history, and service records.
- Collect more recent listings to reflect market price changes.
- Explore additional algorithms such as XGBoost and CatBoost.
- Develop separate models for different vehicle price segments.
- Deploy the trained model through a web application for interactive price estimation.

## Technologies Used

- **Programming:** Python
- **Data Manipulation:** Pandas, NumPy
- **Visualization:** Matplotlib, Seaborn
- **Machine Learning:** Scikit-learn
- **Categorical Encoding:** Category Encoders
- **Environment:** Jupyter Notebook / Google Colab

## Project Structure

```text
Saudi-Used-Car-Price-Prediction/
│
├── data/
│   └── Saudi_Used_Cars.csv
│
├── notebooks/
│   └── Saudi_Used_Car_Price_Prediction.ipynb
│
├── README.md
└── requirements.txt
```

*Adjust the folder and file names to match your actual GitHub repository.*

## Conclusion

This project demonstrates how machine learning can be applied to estimate used car prices in Saudi Arabia. Among the five regression algorithms evaluated, the tuned Extra Trees Regressor achieved the strongest reported performance, with an R² score of 0.7767.

The findings highlight the importance of vehicle age, engine size, mileage, and vehicle options in price prediction. While the model can support data-driven pricing decisions, its predictions should be treated as estimates rather than guaranteed market values.
