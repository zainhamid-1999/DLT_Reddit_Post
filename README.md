# DLT_Reddit_Post
# Reddit Silver Layer Pipeline

This repository contains a Databricks notebook that processes Reddit data and stores it in the **Silver Layer** of a data lake, following the medallion architecture (Bronze, Silver, Gold). The pipeline includes data cleaning, transformation, and aggregation steps, and it leverages **Delta Lake** for data storage.

## Overview

The goal of this project is to take raw Reddit data from the **Bronze Layer**, clean and transform it, and store the cleaned data in the **Silver Layer** for further analysis and reporting.

## Project Structure

1. **Bronze Layer**:
   - Raw Reddit data is loaded into the system in the **Bronze Layer** from a Delta table.
   
2. **Silver Layer**:
   - Data is cleaned, enriched, and stored in the **Silver Layer** after applying transformations, including:
     - Dropping null values.
     - Removing duplicates based on `post_id`.
     - Filtering posts based on `score`.
     - Aggregating data by `author` and `subreddit`.
     - Extracting and aggregating data based on `created_at` for active hours and weekly trends.

3. **Gold Layer** (Not implemented in this code but can be added for further reporting and analytics):
   - Aggregated, business-ready data for final analysis and reporting.

## Data Flow

1. **Load Raw Data**: Raw Reddit data is loaded from the Bronze Layer stored in Delta format.
2. **Data Cleaning**: Null values are dropped, and duplicates are removed based on `post_id`.
3. **Transformation**: 
   - The `created_utc` field is converted to a `timestamp`.
   - Only posts with a `score` greater than 20 are kept.
   - Aggregations are performed on authors and subreddits, including:
     - Count of posts per author.
     - Average score per author and subreddit.
     - Identification of the top-performing posts by score.
   - Active hours of posting are extracted from the `created_at` field.
4. **Store Cleaned Data**: The cleaned data is saved in the **Silver Layer** in Delta format.

## Technologies Used

- **Apache Spark**: For distributed data processing.
- **Delta Lake**: For storing and managing the data in the **Bronze** and **Silver Layers**.
- **Databricks**: Cloud platform for running the notebook and processing data.

## Steps in the Notebook

1. **Create Spark Session**: Initializes the Spark session with Delta support.
2. **Load Data**: Loads raw Reddit data from the Bronze Layer.
3. **Data Cleaning**: Drops rows with null values and removes duplicates.
4. **Data Transformation**: 
   - Converts `created_utc` to `timestamp`.
   - Filters posts based on `score`.
   - Aggregates data by `author` and `subreddit`.
   - Extracts active posting hours and weekly trends.
5. **Write to Silver Layer**: Saves the cleaned and transformed data into the Silver Layer in Delta format.
6. **Create Table**: A Delta table is created in the Databricks catalog to store the Silver Layer data.

## Setup

1. **Install Databricks**: Ensure you have a Databricks workspace.
2. **Set up Delta Lake**: Ensure Delta Lake is enabled in your Databricks cluster.
3. **Mount Data**: Mount your raw data to DBFS (Databricks File System).
4. **Run the Notebook**: Follow the steps in the notebook to load, clean, and store data in the Silver Layer.

## Example SQL for Table Creation

```sql
CREATE TABLE silver_reddit_layer (
    post_id STRING,
    title STRING,
    description STRING,
    subreddit STRING,
    author STRING,
    score INT,
    created_at TIMESTAMP,
    url STRING,
    has_url BOOLEAN,
    created_utc TIMESTAMP,
    hour INT,
    week INT
)
USING delta;
