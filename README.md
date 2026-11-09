# Codveda Data Analytics Internship

Portfolio of the projects completed during my **Data Analytics internship at Codveda Technology**. The work is organised in three levels (Basic, Intermediate, Advanced) and covers the full workflow of a data project: cleaning, exploration, visualization, regression, time series, clustering, classification, NLP and business dashboards.

**Tools:** Python · pandas · NumPy · matplotlib · seaborn · scikit-learn · statsmodels · NLTK · TextBlob · WordCloud · Power BI

---

## Table of Contents

1. [Repository Structure](#repository-structure)
2. [Datasets](#datasets)
3. [Level 1: Basic](#level-1--basic)
4. [Level 2: Intermediate](#level-2--intermediate)
5. [Level 3: Advanced](#level-3--advanced)
6. [Skills Demonstrated](#skills-demonstrated)
7. [How to Run](#how-to-run)
8. [Notes and Limitations](#notes-and-limitations)
9. [Links](#links)

---

## Repository Structure

```
codveda-data-analytics-internship/
├── README.md
├── datasets/                  # CSV files used in the tasks
├── Level1_Basic/
│   ├── Task1_Data_Cleaning.ipynb
│   ├── Task2_EDA.ipynb
│   ├── Task3_Visualization.ipynb
│   └── plots/
├── Level2_Intermediate/
│   ├── Task1_Regression.ipynb
│   ├── Task2_Time_Series.ipynb
│   ├── Task3_Clustering.ipynb
│   └── plots/
└── Level3_Advanced/
    ├── Task1_Classification.ipynb
    ├── Task2_PowerBI_Dashboard/
    ├── Task3_NLP_Sentiment.ipynb
    └── plots/
```

Each level has its own folder, and each task has its own notebook, as required by the internship instructions.

---

## Datasets

| Dataset | Used in | Description |
|---|---|---|
| `Sentiment dataset.csv` | L1 Task 1, L3 Task 3 | 732 social media posts with text, user, platform, country, timestamp, likes and retweets |
| `iris.csv` | L1 Tasks 2-3, L2 Task 3 | 150 flowers, 4 numeric measurements, 3 species |
| `house Prediction Data Set.csv` | L2 Task 1 | Boston housing data (506 rows, 14 variables, no header) |
| `Stock Prices Data Set.csv` | L2 Task 2 | Daily prices for many companies (497,472 rows), 2014 onwards |
| `churn-bigml-80.csv` / `churn-bigml-20.csv` | L3 Tasks 1-2 | Telecom customers: 2,666 for training and 667 for testing, target is `Churn` |

---

## Level 1: Basic

### Task 1: Data Cleaning and Preprocessing
**Notebook:** `Level1_Basic/Task1_Data_Cleaning.ipynb`
**Dataset:** Sentiment dataset

**What I did**
- Loaded the raw CSV with pandas and inspected structure, data types, missing values and duplicates (`info()`, `head()`, `isna().sum()`, `duplicated().sum()`).
- Dropped redundant index columns left over from a previous export (`Unnamed: 0`, `Unnamed: 0.1`).
- Removed leading and trailing whitespace from all text columns using `.str.strip()`.
- Standardized categorical values (`Sentiment`, `Platform`, `Country`) with `.str.title()`.
- Converted `Timestamp` from text to a proper `datetime` type.
- Handled missing values: `Retweets` and `Likes` would be filled with 0, and rows missing `Text`, `Sentiment` or `Timestamp` would be dropped. This dataset had no missing values, so nothing changed, but the checks stay in the pipeline.
- Removed exact duplicate rows and duplicate posts (same text, user and timestamp).
- Reset the index and saved the result to `sentiment_data_cleaned.csv`.

**Result:** 732 rows reduced to **707 clean rows** across 13 columns.

### Task 2: Exploratory Data Analysis (EDA)
**Notebook:** `Level1_Basic/Task2_EDA.ipynb`
**Dataset:** Iris

**What I did**
- Calculated mean, median, mode and standard deviation for the four numeric features.
- Counted samples per species (50 each).
- Plotted histograms with density curves for every feature.
- Plotted boxplots by species to compare distributions and spot outliers.
- Plotted a petal length vs petal width scatter plot coloured by species.
- Computed the correlation matrix and visualized it as a heatmap.

**Key findings**
- Mean values: sepal length 5.84 cm, sepal width 3.05 cm, petal length 3.76 cm, petal width 1.20 cm.
- Petal length and petal width are very strongly correlated (**r = 0.96**). Sepal length is also strongly correlated with petal length (0.87).
- Sepal width is the weakest feature, with a moderate negative correlation to the petal measurements (about -0.4).
- *Setosa* is clearly separated from the other two species on petal measurements, while *versicolor* and *virginica* overlap partially.

### Task 3: Basic Data Visualization
**Notebook:** `Level1_Basic/Task3_Visualization.ipynb`
**Dataset:** Iris

**What I did**
- Bar plot of the average petal length per species (setosa 1.46 cm, versicolor 4.26 cm, virginica 5.55 cm).
- Line chart comparing sepal length and petal length across samples sorted by sepal length.
- Scatter plot of sepal length vs sepal width coloured by species.
- Customized titles, axis labels and legends, and exported every chart as a PNG with `plt.savefig()` for use in reports.

---

## Level 2: Intermediate

### Task 1: Regression Analysis
**Notebook:** `Level2_Intermediate/Task1_Regression.ipynb`
**Dataset:** Boston housing

**What I did**
- Loaded a whitespace-separated file without a header and assigned the standard column names.
- Built a simple linear regression predicting house value (`MEDV`, in $1000s) from the average number of rooms (`RM`).
- Split the data 80/20 into training (404 rows) and test (102 rows) sets with `train_test_split(random_state=42)`.
- Trained `LinearRegression` from scikit-learn and evaluated it on the unseen test set.

**Results**

| Metric | Value |
|---|---|
| Coefficient (RM) | 9.35 |
| Intercept | -36.25 |
| R-squared | 0.371 |
| Mean Squared Error | 46.14 |

**Interpretation:** each additional room is associated with a house value about $9,350 higher. The model explains about 37% of the variation in price, and the typical error is about $6,800 (square root of MSE). This is reasonable for a one-variable model, and it shows that other factors such as crime rate, taxes and distance to the city also matter.

### Task 2: Time Series Analysis
**Notebook:** `Level2_Intermediate/Task2_Time_Series.ipynb`
**Dataset:** Stock prices (Apple, symbol `AAPL`)

**What I did**
- Filtered the multi-company dataset to a single stock (1,007 trading days), converted dates to `datetime` and sorted chronologically.
- Plotted the closing price over time to identify the overall trend.
- Set the date as the index and decomposed the series into **trend, seasonality and residuals** with `statsmodels.seasonal_decompose` (additive model, period of 252 trading days, roughly one year).
- Calculated a **30-day moving average** with `rolling(window=30).mean()` and plotted it against the raw closing price to smooth short-term noise.

### Task 3: Clustering Analysis (K-Means)
**Notebook:** `Level2_Intermediate/Task3_Clustering.ipynb`
**Dataset:** Iris (the `species` label is not used for training)

**What I did**
- Standardized the four numeric features with `StandardScaler`.
- Ran K-Means for k = 1 to 10 and plotted the inertia (**elbow method**). The elbow appears at **k = 3**.
- Trained the final model with k = 3 and visualized the clusters in a 2D scatter plot.
- Compared the clusters with the real species using a cross-tabulation.

**Result**

| Species | Cluster 0 | Cluster 1 | Cluster 2 |
|---|---|---|---|
| setosa | 0 | 50 | 0 |
| versicolor | 39 | 0 | 11 |
| virginica | 14 | 0 | 36 |

Setosa is separated perfectly. Versicolor and virginica are partially mixed (25 of 100 flowers), which is consistent with the overlap seen in the EDA.

---

## Level 3: Advanced

### Task 1: Predictive Modeling (Classification)
**Notebook:** `Level3_Advanced/Task1_Classification.ipynb`
**Dataset:** Telecom customer churn (80% train file, 20% test file)

**Goal:** predict whether a customer will leave the company (`Churn`).

**Preprocessing**
- Converted `International plan` and `Voice mail plan` from Yes/No to 1/0, and `Churn` to 1/0.
- One-hot encoded `State` with `pd.get_dummies(drop_first=True)`.
- Aligned test columns to the training columns with `reindex` so both sets have exactly the same 69 features.
- Standardized features with `StandardScaler` (fitted on training data only) for Logistic Regression, which fixed a convergence warning.

**Models compared (on the test set)**

| Model | Accuracy | Precision | Recall | F1-score |
|---|---|---|---|---|
| Logistic Regression | 0.859 | 0.511 | 0.253 | 0.338 |
| Decision Tree | 0.928 | 0.742 | 0.758 | 0.750 |
| Random Forest | **0.946** | **0.984** | 0.632 | **0.769** |

**Hyperparameter tuning:** `GridSearchCV` (3-fold cross-validation, scored on F1) over `n_estimators`, `max_depth` and `min_samples_split` for Random Forest. Best parameters: `n_estimators=100`, `max_depth=None`, `min_samples_split=5`, with a cross-validated F1 of **0.727**.

**Insights**
- Only about 15% of customers churn (388 of 2,666), so **accuracy alone is misleading**. Logistic Regression looks decent at 86% accuracy but catches only 1 in 4 churners.
- Random Forest has the best overall balance and almost no false alarms (precision 98%).
- Decision Tree has the highest recall, so it is better if the priority is to catch as many at-risk customers as possible.

### Task 2: Interactive Dashboard (Power BI)
**Folder:** `Level3_Advanced/Task2_PowerBI_Dashboard/`
**Dataset:** Telecom customer churn

**What I built**
- Imported the dataset into Power BI Desktop and checked it in Power Query (data types and quality).
- **Pie chart** showing the share of customers who churn vs stay.
- **Column chart** of churn by international plan.
- **Column chart** of churn by number of customer service calls.
- **Slicers** for `State` and `Voice mail plan`, so every visual filters interactively.
- Added a title and arranged the layout into a single-page dashboard.

**Files:** the `.pbix` file and a PDF/screenshot of the dashboard.

### Task 3: NLP, Sentiment Analysis
**Notebook:** `Level3_Advanced/Task3_NLP_Sentiment.ipynb`
**Dataset:** cleaned Sentiment dataset (707 posts)

**What I did**
- **Text preprocessing** with NLTK: lowercasing, tokenization, removal of numbers and punctuation, stopword removal and lemmatization.
- **Sentiment scoring** with TextBlob polarity. Posts with polarity above 0.1 are labelled *Positive*, below -0.1 *Negative*, and the rest *Neutral*.
- Visualized the sentiment distribution as a bar chart.
- Generated a **word cloud** of the most frequent words.

**Result**

| Sentiment | Posts |
|---|---|
| Neutral | 384 |
| Positive | 240 |
| Negative | 83 |

The word cloud is dominated by words such as *joy*, *life*, *new*, *moment*, *friend* and *world*, which points to mostly positive, everyday-life themes.

---

## Skills Demonstrated

- **Data preparation:** cleaning, deduplication, type conversion, missing value handling, encoding, scaling
- **Analysis:** descriptive statistics, correlation analysis, outlier detection
- **Visualization:** matplotlib, seaborn, Power BI dashboards with slicers
- **Machine learning:** regression, clustering, classification, model evaluation, hyperparameter tuning
- **Time series:** decomposition and moving-average smoothing
- **NLP:** text preprocessing and lexicon-based sentiment analysis
- **Good practice:** train/test separation, no data leakage when scaling, reproducibility with fixed random seeds, metrics suited to imbalanced data

---
