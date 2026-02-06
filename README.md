
# Project: Hyperliquid Trader Behavioral Analysis with Fear & Greed Index

## Overview
This project analyzes the behavior and performance of traders on the Hyperliquid platform in correlation with the Bitcoin Fear & Greed Index. It aims to identify how market sentiment influences trading decisions and profitability, segment traders based on their behavioral patterns, and propose actionable strategies.

## Setup and Execution Guide

### 1. Google Colab Environment
This notebook is designed to run in Google Colab. No local setup is required other than a Google account.

### 2. Data Files
Ensure you have the following CSV files uploaded to your Colab environment (e.g., in the `/content/` directory):
- `fear_greed_index.csv`: Contains historical Bitcoin Fear & Greed Index data.
- `historical_data.csv`: Contains historical trading data from Hyperliquid.

### 3. Step-by-Step Execution

#### A. Initial Setup and Data Loading
-  Imports the `pandas` library, essential for data manipulation.
-  Loads `fear_greed_index.csv` into `sentiment_df` and `historical_data.csv` into `trader_df`. A `DtypeWarning` for `Columns (12) have mixed types` is expected and handled later.

#### B. Data Exploration and Cleaning
-  Defines a utility function `analyze_data_structure` to inspect dataframes (rows/columns, missing values, duplicates).
-  Executes `analyze_data_structure` on both `sentiment_df` and `trader_df` to understand their initial state. Missing values in `trader_df` are noted.
-  Prints column names and data types for both dataframes, which is crucial for identifying columns needing conversion or alignment.

#### C. Data Alignment and Merging
-  Defines and executes `align_and_merge_datasets`.
  - Converts `date` in `sentiment_df` and `Timestamp` (float, assumed milliseconds) in `trader_df` to datetime objects, then normalizes them to daily dates.
  - Merges `trader_df` with `sentiment_df` on the normalized daily dates (`Date_Mapped`), keeping only records present in both (inner merge).
  - Renames `Date_Mapped` to `Trade_Date` in the `merged_df`.
- Defines and executes `verify_alignment`.
  - Checks for nulls in the merged sentiment columns (should be zero).
  - Provides a sample verification of timestamp conversion.
  - Reports data retention after merging.
  - Ensures each day has exactly one sentiment classification.

#### D. Key Metrics Calculation
-  Defines and executes `calculate_key_metrics`.
  - Calculates daily PnL per account, number of trades per day, win rate, average trade size, leverage distribution, and long/short ratio.
  - Handles dynamic identification of 'Buy'/'Sell' columns for long/short ratio calculation.
- ** Defines and executes `validate_metrics`.
  - Performs checks to ensure the aggregated metrics (e.g., total PnL) match the raw data sums and that win rates are within valid bounds.

#### E. Part B: Performance and Behavioral Analysis by Sentiment
 Defines and executes `analyze_performance_by_sentiment`.
  - **Task 1 (Performance by Sentiment)**: Calculates `total_pnl`, `avg_pnl_per_trade`, `win_rate`, `avg_loss`, `max_daily_drawdown`, and `trade_count` grouped by `classification`.
  - Normalizes PnL by the number of unique days in each sentiment category.
  - Generates bar plots for Win Rate and Average Loss by sentiment.
- Defines and executes `analyze_behavioral_shifts`.
  - **Task 2 (Behavioral Shifts by Sentiment)**: Calculates `total_trades`, `avg_position_size`, `cross_margin_ratio` (leverage proxy), and `long_bias` (directional bias) grouped by `classification`.
  - Normalizes trade frequency by the number of unique days in each sentiment category.
  - Generates bar plots for Daily Trade Frequency, Average Position Size, and Long Bias Ratio by sentiment.

#### F. Part B: Trader Segmentation
- Defines and executes `segment_traders`.
  - **Task 3 (Trader Segmentation)**: Aggregates trader data by `Account` to create `trader_profiles` with `total_trades`, `total_pnl`, `win_rate`, `high_leverage_ratio`, and `avg_size`.
  - Segments traders into `Frequent`/`Infrequent`, `High Leverage`/`Low Leverage`, and `Consistent Winner`/`Inconsistent` categories using median-based thresholds and logic.
  - Merges these segments back into the `aligned_data` to create `df_segmented`.
  - Prints segment distribution counts.

#### G. Part B, Task 4: Final Evidence Summary
- Defines and executes `generate_final_insights_table`.
  - Combines key performance and behavioral metrics by sentiment into a single summary table for final reporting.

#### H. Part C: Actionable Strategies
-  Defines and executes `propose_actionable_strategies`.
  - Proposes two concrete trading strategies based on the insights derived from the analysis, with clear rules and supporting evidence.
  - Quantifies the potential impact by identifying the number of accounts each strategy would affect.

#### I. Bonus Tasks: Advanced Analysis
-  Defines and executes `cluster_behavioral_archetypes`.
  - **Clustering: Uses K-Means clustering on scaled features (`total_trades`, `avg_size`, `win_rate`, `high_leverage_ratio`) to identify distinct trader archetypes.
  - Prints the characteristics of each cluster.
-  Defines and executes `train_profitability_predictor`.
  - **Predictive Modeling**: Engineers a `Target_Profit` feature (1 for profitable, 0 for loss) for the next day.
  - Trains a RandomForestClassifier using `sentiment_score`, `Size USD`, and `Crossed` as features.
  - Evaluates the model using a classification report.
- Saves the trained `profit_model` to a `profit_predictor.pkl` file using `pickle`.

#### J. Running the Streamlit Application
This section sets up and runs a simple Streamlit dashboard for the predictive model.
- **Cell `install_streamlit`**: Installs the `streamlit` library in the Colab environment.
- ** Provides a basic example of Streamlit integration within Colab, demonstrating simple UI elements.
-  Writes the `app.py` file, which contains the full Streamlit application code. This app loads the saved predictive model and provides an interactive interface for users to input trade parameters and get profitability predictions.
- Executes the commands to:
  1. Kill any existing Streamlit processes to avoid port conflicts (`!pkill streamlit`).
  2. Start the Streamlit application (`app.py`) on port `8501`, configuring it for Colab compatibility.
  3. Waits for the server to start.
  4. Serves the Streamlit application directly within the Colab output using an iframe, allowing interactive use.

To run the Streamlit app, simply execute all cells sequentially, especially the final few cells related to `app.py` and starting Streamlit.
```
