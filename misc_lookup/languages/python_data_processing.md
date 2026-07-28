# Pure Python 3 Data Processing Cheatsheet

## 1. File I/O & CSV Parsing (`csv`, `json`)
```python
import csv, json

# Reading & Writing CSV with DictReader / DictWriter
with open("input.csv", mode="r", encoding="utf-8") as infile:
    reader = csv.DictReader(infile)
    rows = [row for row in reader if int(row["age"]) > 25]

with open("output.csv", mode="w", newline="", encoding="utf-8") as outfile:
    writer = csv.DictWriter(outfile, fieldnames=["id", "name", "age"])
    writer.writeheader()
    writer.writerows(rows)

# JSON Streaming / Reading
with open("data.json", "r", encoding="utf-8") as f:
    data = json.load(f)
```

## 2. Text Cleaning & Regex (`re`)
```python
import re

text = " Order #12345: $49.99 (Date: 2026-07-28) "

# Cleaning Whitespace & Lowercasing
clean_text = text.strip().lower()

# Regex Matching & Extraction
order_id = re.search(r"#(\d+)", text).group(1)            # '12345'
price = float(re.search(r"\$(\d+\.\d+)", text).group(1))  # 49.99

# Substitution & Tokenization
text_only = re.sub(r"[^a-zA-Z\s]", "", text)
tokens = [word for word in clean_text.split() if word.isalnum()]
```

## 3. Grouping, Aggregation & Counting (`collections`)
```python
from collections import defaultdict, Counter

# Counting Frequency
word_counts = Counter(["apple", "banana", "apple", "orange"])
top_two = word_counts.most_common(2)                      # [('apple', 2), ('banana', 1)]

# Grouping Data by Key
sales_by_dept = defaultdict(list)
data = [("HR", 100), ("IT", 150), ("HR", 200)]
for dept, amount in data:
    sales_by_dept[dept].append(amount)

# Aggregating Grouped Data
avg_by_dept = {k: sum(v) / len(v) for k, v in sales_by_dept.items()}
```

## 4. List & Dict Transformations (Comprehensions)
```python
raw = [{"name": "alice", "score": 85}, {"name": "bob", "score": 42}]

# Filtering & Mapping
passed = [user["name"].upper() for user in raw if user["score"] >= 50]

# Dict Building & Lookup Creation
score_lookup = {user["name"]: user["score"] for user in raw}

# Flattening Nested Lists
nested = [[1, 2], [3, 4], [5]]
flat = [item for sublist in nested for item in sublist]     # [1, 2, 3, 4, 5]
```

## 5. Iteration & Functional Tools (`itertools`, `functools`)
```python
import itertools, functools

# Chunking Data into Fixed Sizes
def chunk_data(data, size):
    it = iter(data)
    return iter(lambda: list(itertools.islice(it, size)), [])

# Cumulative Sums & Flattening
running_total = list(itertools.accumulate([10, 20, 30]))   # [10, 30, 60]
chained = list(itertools.chain([1, 2], [3, 4]))            # [1, 2, 3, 4]

# Deduplication (Preserving Order)
seen = set()
unique = [x for x in [1, 2, 2, 3, 1] if not (x in seen or seen.add(x))]
```

## 6. Sorting, Filtering & Summarizing
```python
records = [{"name": "A", "val": 30}, {"name": "B", "val": 10}]

# Multi-Key Sorting
sorted_recs = sorted(records, key=lambda x: x["val"], reverse=True)

# Min, Max, Sum, Zip
vals = [10, 20, 30]
total, avg = sum(vals), sum(vals) / len(vals)
zipped = list(zip(["a", "b"], [1, 2]))                     # [('a', 1), ('b', 2)]
```

=======================

# Data Processing in Python 3 Cheatsheet

## 1. Data Cleaning & Transformation (`pandas`)
```python
import pandas as pd
import numpy as np

df = pd.read_csv("data.csv")

# Filtering & Querying
df_filtered = df[(df["age"] > 25) & (df["status"] == "active")]

# Handling Missing Values
df.dropna(subset=["critical_col"], inplace=True)
df["val"] = df["val"].fillna(df["val"].median())

# String Operations & Type Conversion
df["email"] = df["email"].str.lower().str.strip()
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df["category"] = df["category"].astype("category")

# Creating & Applying Columns
df["total"] = df["price"] * df["quantity"]
df["log_val"] = np.log1p(df["value"])
df["tier"] = df["score"].apply(lambda x: "High" if x >= 80 else "Low")
```

## 2. Grouping, Aggregation & Pivoting
```python
# GroupBy Aggregation
summary = df.groupby("department").agg(
    avg_salary=("salary", "mean"),
    headcount=("emp_id", "count"),
    max_bonus=("bonus", "max")
).reset_index()

# Pivot Tables
pivot = df.pivot_table(
    index="region", 
    columns="year", 
    values="sales", 
    aggfunc="sum", 
    fill_value=0
)

# Window & Rolling Functions
df["rolling_avg"] = df.groupby("sensor_id")["temp"].transform(lambda x: x.rolling(7, min_periods=1).mean())
df["rank"] = df.groupby("category")["score"].rank(ascending=False, method="dense")
```

## 3. Merging & Reshaping
```python
# Joins (inner, left, right, outer)
merged = pd.merge(df1, df2, on="user_id", how="left")
concat = pd.concat([df1, df2], axis=0, ignore_index=True)

# Wide to Long (Melt) & Long to Wide (Pivot)
long_df = pd.melt(df, id_vars=["id"], value_vars=["q1", "q2"], var_name="quarter", value_name="sales")
```

## 4. Text & Regex Processing
```python
import re

text = "Order #12345 placed on 2026-07-28."

# Regex Search & Extraction
match = re.search(r"#(\d+)", text)
order_id = match.group(1) if match else None  # '12345'

# List Comprehension for Cleaning
clean_tokens = [w.lower() for w in text.split() if w.isalnum()]
```

## 5. Iterators & Functional Utilities (`itertools` / `functools`)
```python
import itertools, functools

# Flattening Lists
flat = list(itertools.chain.from_iterable([[1, 2], [3, 4]]))  # [1, 2, 3, 4]

# Chunking Data Iterative
def chunker(seq, size):
    return (seq[pos:pos + size] for pos in range(0, len(seq), size))

# Cumulative Aggregations
accumulated = list(itertools.accumulate([1, 2, 3, 4]))        # [1, 3, 6, 10]
```

## 6. Efficient File I/O
```python
# Memory-Efficient Batch Chunk Reading (CSV)
for chunk in pd.read_csv("large_dataset.csv", chunksize=10_000):
    process(chunk)

# Fast Parquet I/O
df.to_parquet("data.parquet", index=False)
df = pd.read_parquet("data.parquet")
```

=======================