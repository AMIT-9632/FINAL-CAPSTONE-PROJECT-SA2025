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
