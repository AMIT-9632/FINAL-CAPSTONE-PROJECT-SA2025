# FINAL-CAPSTONE-PROJECT-SA2025
**OVERVIEW**

**PROJECT FOCUS**

My task was to build a _real-time dynamic pricing system for urban parking spaces_ using streaming data and advanced pricing models. The project uses real-time data processing, geospatial analysis, and machine learning principles, implemented in Python with the Pathway streaming framework.

**KEY STEPS AND ACCOMPLISHMENTS**

- **Data Preparation:**  
  It processed 14 parking lots’ data, including occupancy, capacity, queue length, traffic conditions, special day flags, and vehicle types, and transformed it into a streamable format for real-time analytics.

- **Model 1 – Baseline Linear Model:**  
  I implemented a simple linear pricing model where the price increased with the occupancy rate. This served as a reference point for further development of a more advanced model.

- **Model 2 – Demand-Based Price Function:**  
  Then I developed a more sophisticated model that incorporates multiple features (occupancy, queue length, traffic, special days, vehicle type) into a demand score, which is then normalized and used to set dynamic prices. It was ensured that the price is bounded and smooth.

- **Streaming & Windowing:**  
  I used Pathway to process data in daily tumbling windows, aggregating features for application in the pricing models in real time.

- **Feature Engineering:**  
  I mapped categorical features (traffic, vehicle type) to numeric values for use in the demand function.

- **Error Handling & Robustness:**  
  I addressed and fixed several technical challenges, including:
    - Ensuring correct numeric types for aggregation
    - Broadcasting global min/max for normalization using `.ix_ref()` to avoid universe errors
    - Clipping prices using a helper function and `pw.apply_with_type` instead of pandas/numpy methods

- **Visualization:**  
  I set up interactive Bokeh/Panel dashboards to visualize daily price evolution for each model.

OUTCOME

I now have a robust, real-time pricing pipeline that:
- Processes live parking data streams
- Computes and normalizes demand for each day
- Dynamically adjusts prices using explainable, bounded logic
- Visualizes results interactively
- Is ready for further enhancements, such as competitor price integration or rerouting suggestions

This project demonstrates the ability to handle real-time data engineering, advanced pricing logic, and streaming analytics for smart urban mobility solutions.

**TECH STACK USED FOR THE COMPLETION OF THE PROJECT**

### 1. Programming Language

- **Python**
  - Core language for all data processing, modeling, and scripting.

### 2. Data Processing & Analysis

- **Pandas**
  - Data loading, cleaning, manipulation, and CSV export.
- **Numpy**
  - Numerical operations and array manipulations.

### 3. Real-Time Data Streaming & Windowing

- **Pathway**
  - Stream processing and real-time analytics.
  - Tumbling window aggregations for daily pricing.
  - Feature engineering and demand normalization in a streaming context.

### 4. Visualization

- **Bokeh**
  - Interactive plotting for time series price visualization.
- **Panel**
  - Dashboarding and serving interactive Bokeh plots as web apps.

### 5. Additional Python Standard Libraries

- **Datetime**
  - Date and time parsing, window definitions, and time-based aggregations.

**Overall Overview of the Techstack**

| Layer                  | Technology Used         | Purpose/Role                                      |
|------------------------|------------------------|---------------------------------------------------|
| Programming Language   | Python                 | All project logic and scripting                   |
| Data Analysis          | pandas, numpy          | Data wrangling, feature engineering, calculations |
| Streaming Framework    | Pathway                | Real-time data processing and windowing           |
| Visualization          | Bokeh, Panel           | Interactive dashboards and time series plots      |
| Date/Time Utilities    | datetime (Python std)  | Time parsing, windowing, aggregations             |

The above set of tech stack enabled robust, real-time, and explainable dynamic pricing for urban parking spaces, from raw data ingestion to interactive visualization.

**The Architecture Diagram following Model 1**
![Untitled diagram _ Mermaid Chart-2025-07-07-075946](https://github.com/user-attachments/assets/723ea145-3b9e-4db5-be5a-3893e0071c1d)
**The Architecture Diagram following Model 2**
![Untitled diagram _ Mermaid Chart-2025-07-07-080721](https://github.com/user-attachments/assets/86dc1481-792d-466c-aff1-a51373852e2c)

# DETAILED EXPLANATION OF THE PROJECT ARCHITECTURE AND WORKFLOW

This project implements a real-time, data-driven dynamic pricing system for urban parking spaces. The architecture is designed to ingest, process, and analyze live parking data streams, apply intelligent pricing logic, and visualize the results interactively. The system is built using Python, leveraging pandas and numpy for data handling, Pathway for streaming analytics, and Bokeh/Panel for visualization.

## 1. Data Ingestion and Preparation

### Data Sources

- **Raw Data:** CSV files containing records for 14 urban parking spaces over 73 days, sampled every 30 minutes.
- **Key Features:** Timestamp, Occupancy, Capacity, Queue Length, Traffic Condition, Special Day Indicator, Vehicle Type.

### Data Loading & Preprocessing

- **Tools:** pandas, numpy
- **Steps:**
  - Load CSV data into a pandas DataFrame.
  - Parse and combine date and time columns into a single timestamp.
  - Sort data chronologically for accurate streaming.
  - Export relevant columns for streaming.

## 2. Real-Time Streaming and Feature Engineering

### Streaming Data Ingestion

- **Framework:** Pathway
- **Process:**
  - Data is streamed into Pathway using a simulated real-time replay of the CSV.
  - Each row represents the state of a parking lot at a specific time.

### Feature Engineering

- **Numerical Mapping:** Categorical features (traffic condition, vehicle type) are mapped to numeric values using dictionaries and helper functions.
- **Type Safety:** All mapped columns are explicitly cast to `float` to ensure compatibility with downstream aggregations.

## 3. Windowed Aggregation and Model Logic

### Daily Tumbling Window Aggregation

- **Purpose:** Aggregate data by calendar day for each parking lot.
- **Method:** Pathway's tumbling window groups all records within a day, enabling day-level analytics.

### Feature Aggregation

- **Aggregated Features:** Sum and average of occupancy, capacity, queue length, traffic, special day, and vehicle type weight.
- **Logic:** Calculations are performed using Pathway’s reducers and column expressions.

## 4. Model Implementation

### Model 1: Baseline Linear Model

- **Logic:** Price increases linearly with occupancy rate.
- **Aggregation:** Daily average occupancy rate is computed, and the price is updated accordingly.

### Model 2: Demand-Based Price Function

- **Logic:** Price is a function of multiple features:
  - Occupancy rate
  - Queue length
  - Traffic level
  - Special day indicator
  - Vehicle type weight
- **Demand Calculation:** Combines daily averages using a weighted formula.
- **Normalization:** 
  - Global minimum and maximum of raw demand are computed using a single-row aggregate.
  - These are broadcast to all rows using Pathway’s `.ix_ref()` to avoid universe errors.
  - Demand is min-max normalized for smooth price scaling.
- **Price Calculation:** 
  - The normalized demand is used to compute the final price.
  - A custom clipping function ensures the price remains within specified bounds (0.5× to 2× the base price).

## 5. Output and Visualization

### Streaming Output Table

- **Contents:** For each day and parking lot, the table includes:
  - Aggregated features
  - Raw and normalized demand
  - Final computed price

### Visualization

- **Tools:** Bokeh, Panel
- **Process:**
  - Custom plotting functions display the daily price evolution for each model.
  - Interactive dashboards allow real-time monitoring and analysis.

## 6. Error Handling and Robustness

- **Type Safety:** All feature mappings and aggregations are explicitly typed to prevent reducer errors.
- **Universe Alignment:** Global aggregates (min/max) are broadcast using `.ix_ref()` to ensure compatibility with the main table.
- **Custom Operations:** Value clipping and other non-native operations are handled with helper functions and Pathway’s apply methods.

## 7. Workflow Summary

1. **Data Loading:** Raw CSV data is loaded, parsed, and preprocessed.
2. **Streaming:** Data is streamed into Pathway for real-time processing.
3. **Feature Engineering:** Categorical features are mapped to numeric values.
4. **Windowed Aggregation:** Data is grouped by day for each parking lot.
5. **Model Application:** Pricing models compute daily prices using aggregated features.
6. **Normalization & Clipping:** Demand is normalized globally, and prices are bounded.
7. **Output:** Results are streamed to an output table and visualized in real time.

This architecture and workflow ensured robust, scalable, and explainable dynamic pricing for urban parking, from raw data ingestion to interactive visualization and operational deployment.
