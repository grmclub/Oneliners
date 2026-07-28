# Python 3 Output & Formatting Cheatsheet

## 1. The `print()` Function
```python
# Basic Print
print("Hello", "World")                   # Default sep=' ', end='
'

# Custom Separator & Ending
print("2026", "07", "28", sep="-")        # 2026-07-28
print("Loading", end="...
")             # Loading...

# Print to Standard Error or File
import sys
print("Error message", file=sys.stderr)

with open("log.txt", "w") as f:
    print("Logged line", file=f)

# Unbuffered Output (Immediate print)
print("Processing...", flush=True)
```

## 2. F-String Formatting (Python 3.6+)
```python
name, score, ratio = "Alice", 95, 0.8756

# Basic Interpolation & Self-Documenting (Python 3.8+)
print(f"{name=}, {score=}")               # name='Alice', score=95

# Numbers & Floats
print(f"{ratio:.2f}")                     # 0.88 (2 decimal places)
print(f"{ratio:.1%}")                     # 87.6% (percentage)
print(f"{1000000_000:,}")                 # 1,000,000,000 (comma separator)
print(f"{1000000_000:_}")                 # 1_000_000_000 (underscore separator)

# Padding & Alignment
print(f"{'test':>10}")                    # '      test' (Right aligned)
print(f"{'test':<10}")                    # 'test      ' (Left aligned)
print(f"{'test':^10}")                    # '   test   ' (Centered)
print(f"{'test':*^10}")                   # '***test***' (Fill character)
print(f"{42:05d}")                        # '00042' (Zero padded)

# Base Conversions
n = 255
print(f"{n:b} | {n:o} | {n:x} | {n:X}")   # 11111111 | 377 | ff | FF
```

## 3. Date & Time Output Formatting
```python
from datetime import datetime
now = datetime.now()

print(f"{now:%Y-%m-%d %H:%M:%S}")         # 2026-07-28 23:08:16
print(f"{now:%B %d, %Y (%A)}")            # July 28, 2026 (Tuesday)
```

## 4. Pretty Printing & Inspection (`pprint`)
```python
from pprint import pprint

data = {"users": [{"id": 1, "roles": ["admin", "dev"]}, {"id": 2, "roles": ["user"]}]}
pprint(data, indent=2, width=40, depth=3)

# Data Inspection Output
import vars, dir
print(vars(obj))                           # Object __dict__ attribute
```

## 5. Structured Data Output (JSON & CSV)
```python
import json, csv, sys

# Formatted JSON Output
obj = {"status": "success", "code": 200, "data": [1, 2, 3]}
print(json.dumps(obj, indent=2, sort_keys=True))

# CSV Output to Console / stdout
writer = csv.writer(sys.stdout)
writer.writerow(["Name", "Role", "Score"])
writer.writerow(["Alice", "Admin", 95])
```

## 6. Standard Streams & ANSI Color Codes
```python
import sys

# Writing directly to STDOUT / STDERR
sys.stdout.write("Direct output
")
sys.stderr.write("Critical failure
")

# Terminal Color Codes (ANSI)
RED = " [91m"
GREEN = " [92m"
RESET = " [0m"

print(f"{RED}Error:{RESET} File not found!")
print(f"{GREEN}Success:{RESET} Operation complete.")
```
