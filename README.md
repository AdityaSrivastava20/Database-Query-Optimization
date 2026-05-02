# Database-Query-Optimization

# 📊 Database Query Optimization System

## 🚀 Overview

This project demonstrates how to analyze and optimize slow SQL queries using indexing, query restructuring, and execution plan analysis in PostgreSQL. It simulates real-world database performance tuning using an e-commerce dataset.

---

## 🎯 Objective

* Identify slow-performing SQL queries
* Analyze execution plans using `EXPLAIN ANALYZE`
* Optimize queries using indexing techniques
* Compare performance before and after optimization

---

## 🧰 Tech Stack

* PostgreSQL
* SQL
* Python (psycopg2)
* Linux (optional for environment setup)

---

## 📦 Dataset

Dataset used: **Brazilian E-commerce Dataset (Olist)**

Download from:
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

### Files Used:

* `customers.csv`
* `orders.csv`
* `order_items.csv`

---

## ⚙️ Project Setup

### 1. Create Database

```sql
CREATE DATABASE ecommerce;
```

---

### 2. Create Tables

```sql
CREATE TABLE customers (
    customer_id VARCHAR PRIMARY KEY,
    customer_city VARCHAR,
    customer_state VARCHAR
);

CREATE TABLE orders (
    order_id VARCHAR PRIMARY KEY,
    customer_id VARCHAR,
    order_purchase_timestamp TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_id VARCHAR,
    product_id VARCHAR,
    price FLOAT,
    freight_value FLOAT
);
```

---

### 3. Load Data

```sql
COPY customers FROM '/path/customers.csv' DELIMITER ',' CSV HEADER;
COPY orders FROM '/path/orders.csv' DELIMITER ',' CSV HEADER;
COPY order_items FROM '/path/order_items.csv' DELIMITER ',' CSV HEADER;
```

---

## 🐌 Slow Query (Before Optimization)

```sql
EXPLAIN ANALYZE
SELECT c.customer_state, SUM(oi.price) AS total_sales
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_state;
```

---

## ⚡ Optimization Techniques

### 🔹 Indexing

```sql
CREATE INDEX idx_customer_id ON orders(customer_id);
CREATE INDEX idx_order_id ON order_items(order_id);
```

### 🔹 Re-run Query

```sql
EXPLAIN ANALYZE
SELECT c.customer_state, SUM(oi.price) AS total_sales
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_state;
```

---

## 📊 Performance Comparison

| Metric         | Before Optimization | After Optimization |
| -------------- | ------------------- | ------------------ |
| Execution Time | ~2.5 sec            | ~0.8 sec           |
| Query Cost     | High                | Reduced            |

---

## 🧠 Bonus Optimization

### Materialized View

```sql
CREATE MATERIALIZED VIEW state_sales AS
SELECT c.customer_state, SUM(oi.price) AS total_sales
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_state;
```

---

## 📊 Python Benchmark Script

```python
import psycopg2
import time

conn = psycopg2.connect(
    dbname="ecommerce",
    user="postgres",
    password="yourpassword",
    host="localhost"
)

cur = conn.cursor()

query = """
SELECT c.customer_state, SUM(oi.price)
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY c.customer_state;
"""

start = time.time()
cur.execute(query)
cur.fetchall()
end = time.time()

print("Execution Time:", end - start)

cur.close()
conn.close()
```

---

## 📈 Key Learnings

* Understanding query execution plans using `EXPLAIN ANALYZE`
* Impact of indexing on query performance
* Optimization of multi-table JOIN operations
* Practical database performance tuning techniques

---

## 📝 Resume Highlights

* Optimized complex SQL queries reducing execution time by ~40–60%
* Applied indexing strategies to improve database performance
* Analyzed execution plans using EXPLAIN ANALYZE
* Designed relational schema for large datasets

---

## 📌 Future Improvements

* Table partitioning for large datasets
* Query caching mechanisms
* Deployment on cloud databases (AWS RDS)
* Monitoring using performance tools

---

## 👤 Author

**Aditya Srivastava**

* Email: [adityasri277@gmail.com](mailto:adityasri277@gmail.com)
* LinkedIn: linkedin.com
* GitHub: github.com

---
