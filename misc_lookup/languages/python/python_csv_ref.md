# Pure Python 3 CSV Data Processing Cheatsheet

## 1. Basic CSV Reading & Writing (`csv`)
```python
import csv

# Reading as Lists (Row by Row)
with open("input.csv", mode="r", encoding="utf-8") as f:
    reader = csv.reader(f)
    header = next(reader)                  # Extract header row
    data = [row for row in reader]         # List of list of strings

# Writing Lists to CSV
with open("output.csv", mode="w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(["id", "name", "val"]) # Write header
    writer.writerows(data)                 # Write multiple rows
```

## 2. Dictionary Reader & Writer (`DictReader` / `DictWriter`)
```python
import csv

# Reading CSV as Dictionaries
with open("data.csv", mode="r", encoding="utf-8") as f:
    reader = csv.DictReader(f)             # Header used as keys automatically
    rows = [row for row in reader if int(row["age"]) >= 18]

# Writing Dictionaries to CSV
fieldnames = ["id", "name", "age", "city"]
with open("filtered.csv", mode="w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()                   # Write fieldnames header
    writer.writerows(rows)
```

## 3. Data Cleaning, Type Conversion & Normalization
```python
import csv

cleaned_rows = []
with open("raw.csv", mode="r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        # Type Conversion & Stripping Whitespace
        row["id"] = int(row["id"].strip())
        row["price"] = float(row["price"].replace("$", "").strip())
        row["email"] = row["email"].lower().strip()
        
        # Handle Missing / Default Values
        row["status"] = row["status"].strip() or "PENDING"
        cleaned_rows.append(row)
```

## 4. Filtering, Sorting & Deduplication
```python
import csv

# Filtering
active_users = [r for r in rows if r["status"] == "ACTIVE"]

# Sorting by Column (Single or Multi-key)
sorted_by_price = sorted(rows, key=lambda x: x["price"], reverse=True)
sorted_multi = sorted(rows, key=lambda x: (x["category"], -x["price"]))

# Deduplication (Preserving Order by Unique ID)
seen_ids = set()
unique_rows = []
for r in rows:
    if r["id"] not in seen_ids:
        seen_ids.add(r["id"])
        unique_rows.append(r)
```

## 5. Grouping & Aggregations (`collections`)
```python
import csv
from collections import defaultdict, Counter

# Grouping Rows by Key
grouped = defaultdict(list)
for r in rows:
    grouped[r["category"]].append(r["price"])

# Calculating Aggregates (Sum, Avg, Min, Max)
summary = []
for cat, prices in grouped.items():
    summary.append({
        "category": cat,
        "count": len(prices),
        "total": sum(prices),
        "avg": round(sum(prices) / len(prices), 2),
        "max_price": max(prices)
    })

# Counting Frequencies
city_counts = Counter(r["city"] for r in rows)
top_3_cities = city_counts.most_common(3)
```

## 6. Dialects & Handling Special CSV Formats
```python
import csv

# Custom Delimiters (TSV or Pipe-separated)
with open("data.tsv", mode="r", encoding="utf-8") as f:
    tsv_reader = csv.DictReader(f, delimiter="	")

# Quoting & Escape Characters
with open("custom.csv", mode="w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f, delimiter="|", quoting=csv.QUOTE_MINIMAL)
    writer.writerow(["Text with | pipe", "Normal Text"])
```