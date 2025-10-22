# Hands-on L10: 🚖 Spark Structured Streaming + MLlib  

## 📘 Overview
This project demonstrates a **real-time analytics pipeline** for a ride-sharing platform using **Apache Spark Structured Streaming** and **Spark MLlib**.  

The system processes live ride data, performs on-the-fly predictions, and learns fare trends using regression models.  
It is divided into two main tasks:

- **Task 4:** Real-Time Fare Prediction using MLlib Regression  
- **Task 5:** Time-Based Fare Trend Prediction  

Both components leverage **Spark’s Structured Streaming API** and **MLlib’s Linear Regression** model.

---

## Repository Structure

```
HANDSON-10--SPARK-STREAMING-MLlib/
├── data_generator.py
├── task4.py
├── task5.py
├── training-dataset.csv
├── README.md
├── models/
│
├── Screenshots/
│   ├── Output-of-Task4.png
│   ├── Output-of-Task5.png
│   └── Running Datagen.py
```

---

## Setup Instructions

### Install Dependencies
```bash
pip install pyspark
```
```bash
pip install faker
```

---

### Step 1: Start the Data Generator
Run this in **Terminal 1**:
```bash
python data_generator.py
```

You should see output similar to:
```bash
Streaming data to localhost:9999...
Sent: {'trip_id': '2f922147-4dc9-4140-9265-84f4fdc0db0b', 'driver_id': 39, 'distance_km': 36.37, 'fare_amount': 115.48, 'timestamp': '2025-10-22 02:29:10'}
```
This indicates that the generator is streaming continuous JSON events to `localhost:9999`.

📸 * screenshot of console output:*  [Datgen.py](./Screenshots/Running Datagen.png)


---

## 🚀 Task 4: Real-Time Fare Prediction
**Goal:** Predict ride fares in real-time based on trip distance.

### Approach
1. Load historical data (`training-dataset.csv`).
2. Use `VectorAssembler` to convert `distance_km` → `features`.
3. Train a `LinearRegression` model (`fare_amount` as label).
4. Save trained model to `models/fare_model`.
5. In streaming mode:
   - Read live data from socket.
   - Load saved model and perform prediction.
   - Compute deviation = `|actual_fare - predicted_fare|`.
   - Display results in the console continuously.

### Run
In **Terminal 2**:
```bash
python task4.py
```

### Expected Output
```
+------------------------------------+---------+-----------+-----------+-----------------+-----------------+
|trip_id                             |driver_id|distance_km|fare_amount|predicted_fare   |deviation        |
+------------------------------------+---------+-----------+-----------+-----------------+-----------------+
|1ebb8399-1af9-48e8-a2b7-7f0ca6a9f134|28       |30.62      |41.14      |90.62262673805247|49.48262673805247|
|7dc893e0-7de1-48b4-9c55-6af2a717c3cc|42       |9.26       |58.77      |95.87321494167797|37.10321494167797|
+------------------------------------+---------+-----------+-----------+-----------------+-----------------+

```

📸 * screenshot of console output:*  [Task4 results](./Screenshots/Output-of-Task4.png)


---

## 📈 Task 5: Time-Based Fare Trend Prediction
**Goal:** Predict average fare trends over 5-minute intervals using time-based features.

### Approach
1. Group static data into 5-minute windows and compute average fare.  
2. Extract `hour_of_day` and `minute_of_hour` (as cyclical features).  
3. Train `LinearRegression` model on these features and save to `models/fare_trend_model_v2`.  
4. In streaming logic:
   - Aggregate incoming rides in 5-minute windows.
   - Generate same time features.
   - Load and apply the pre-trained model.
   - Output `window_start`, `window_end`, `actual_avg_fare`, and `predicted_next_avg_fare`.

### Run
In **Terminal 3**:
```bash
python task5.py
```

### Expected Output
```
+-------------------+-------------------+--------------+------------------------+
|window_start       |window_end         |actual_avg_fare|predicted_next_avg_fare|
+-------------------+-------------------+--------------+------------------------+
|2025-10-21 10:00:00|2025-10-21 10:05:00| 12.40        | 12.31                 |
|2025-10-21 10:05:00|2025-10-21 10:10:00| 13.10        | 13.00                 |
+-------------------+-------------------+--------------+------------------------+
```

📸 * screenshot of console output:*  [Task5 results](./Screenshots/Output-of-Task5.png)

---

## 🧠 Key Learnings 

1. **Structured Streaming Architecture** — Gained hands-on experience in building real-time data pipelines using Spark Structured Streaming, understanding micro-batch processing, state management, and fault tolerance.

2. **Model Training & Inference with MLlib** — Trained and deployed Linear Regression models using Spark MLlib, applying them for both static and live streaming data predictions.

3. **Feature Engineering & Vectorization** — Learned to prepare data using `VectorAssembler` and engineered cyclical time-based features (sin/cos) to improve temporal model performance.

4. **Windowed Aggregation & Time-Series Analysis** — Implemented 5-minute tumbling windows and computed average fares to identify temporal fare trends within streaming datasets.

5. **Model Persistence & Real-Time Prediction Pipeline** — Built a reusable ML pipeline that saves trained models, reloads them in streaming applications, and continuously outputs predictions with deviation metrics.

---

## Submission Checklist

- [✅] `data_generator.py` — Streams JSON ride data  
- [✅] Task 4 & Task 5 scripts implemented.  
- [✅] Model directories created (`models/fare_model`, `models/fare_trend_model_v2`).  
- [✅] Screenshots added under `/screenshots/`.  
- [✅ Final README with explanations and results.  
- [✅] Repository pushed to GitHub in correct structure.

---

# Handson-L10-Spark-Streaming-MLlib
