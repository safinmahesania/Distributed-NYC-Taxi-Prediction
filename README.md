# Distributed NYC Taxi Prediction
Scalable Trip Duration Prediction for NYC Yellow Taxi: A Distributed Machine Learning System

This project predicts taxi trip durations using more than 53 million NYC Yellow Taxi trip records. The system uses a distributed data pipeline and machine learning workflow built with Apache Spark, Delta Lake, and Azure Databricks.

## Team Members
- Shrey Nileshkumar Hingu  
- Md Ariful Hossain  
- Kawshik Kumar Ghosh  
- Safin Mahesania  
- Irfan Maknojia  

## Overview
Taxi services depend on accurate trip duration predictions for dispatch and planning. The dataset is large, so distributed computing was required. This project includes:

- Large-scale data cleaning and feature engineering  
- Partitioned Delta Lake storage  
- Distributed model training  
- Model deployment and batch inference  

## Technology Stack
- Azure Databricks  
- Apache Spark  
- Delta Lake  
- MLflow Model Registry  
- Python / PySpark  

## System Architecture
**Azure Components:**
- Storage Account: taximldata2024 (Standard LRS)  
- Databricks Workspace: taxi-ml-workspace (Premium)  
- Cluster: production-cluster  
  - DBR 17.3 LTS  
  - Spark 4.0.0  
  - Autoscaling: 2 → 4 workers  
  - Standard_D2ads_v6 (8GB RAM, 2 cores)  

## Dataset
- 53.3M clean trips  
- NYC Yellow Taxi (Jan 2024 – Jun 2025)  
- 18 parquet files (1.05 GB)  
- 81.7% retention after cleaning  

### Key Features:
- trip_distance
- passenger_count
- fare_amount
- hour_of_day
- day_of_week
- is_weekend
- PU/DOLocationID  

## Data Pipeline
1. Load 65.3M raw records  
2. Clean and validate  
3. Feature engineering  
4. Partition by year and month (24 partitions)  
5. Train-test split (80/20)  

### Partitioning Example
```python
df.write.format("delta") \
    .partitionBy("year", "month") \
    .save()
```

### Performance Impact
- 5.11x faster queries
- Full scan: 2.41s
- Partitioned: 0.47s

### Delta Lake Features
- ACID-compliant transactions
- Schema enforcement
- Transaction logs
- Time travel
- Reliable concurrent reads/writes
These features helped maintain data integrity across reads, writes, and versioned updates.

## Distributed Machine Learning
Machine learning models were trained using Spark MLlib with data parallelism.
### Models Trained
- Linear Regression
- Random Forest
- Gradient Boosted Trees (best model)

### Dataset Scale
- Training: 42.5M samples
- Testing: 10.6M samples
- Horizontal Scaling Results

## Model Performance
### Best Model: Gradient Boosted Trees
| Metric | Score        |
| ------ | ------------ |
| RMSE   | 4.79 minutes |
| MAE    | 2.22 minutes |
| R²     | 0.8776       |

### Prediction Accuracy
- Excellent (<2 min error): 72.6%
- Good (2–5 min): 16.4%
- Fair (5–10 min): 6.2%
- Poor (>10 min): 4.8%

### Production Deployment
- Managed with MLflow Model Registry
- Best model auto-selected using RMSE
- Batch inference on 3.2M rows
- Predictions saved back to Delta Lake
- Results partitioned by accuracy level

## Key Takeaways
### Distributed Features
- Partitioning improved query performance significantly
- Delta Lake ensured strong ACID guarantees
- Distributed ML scaled effectively for large datasets

### Lessons Learned
- Partitioning is essential for analytical workloads
- ACID transactions prevent inconsistent writes
- ML training scalability varies by model
- Cloud platforms simplify pipelines but still require tuning


## License
- This project is licensed under the MIT License.

## Contributing
- Contributions are welcome. Feel free to open issues or submit pull requests.

## Happy farming & early disease detection!


