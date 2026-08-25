# Hospital Resource Demand Forecasting

A portfolio implementation of a hospital demand forecasting workflow for capacity planning.

## Problem

Hospital resource demand changes throughout the year, which makes it difficult to plan beds, staffing, and other operational resources using fixed rules alone.

This project explores how historical hospital operations data can be used to forecast short-term and mid-term demand and support better capacity planning.

## What I'm Building

The project follows the main stages of a forecasting workflow:

1. Clean and prepare historical hospital operations data
2. Explore trends, seasonality, and demand patterns
3. Build time-series features at different operational levels
4. Establish an ARIMA baseline
5. Train a Gradient Boosting model for comparison
6. Evaluate forecasts using MAPE and RMSE
7. Simulate demand scenarios such as seasonal surges
8. Expose forecast results through a simple API

## Tech Stack

- Python
- Pandas
- PySpark
- Scikit-learn
- ARIMA
- Azure Databricks
- MLflow
- Flask
- Docker

## Planned Repository Structure

```text
data/          Sample data used for development
notebooks/     Exploratory analysis and model experiments
src/           Data preparation, features, and modeling code
images/        Forecast plots and other results
