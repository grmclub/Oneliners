# Python Quick Lookup Cheatsheet

## 1. Basic Syntax & Types
```python
# Variables & Printing
x, y = 10, "Hello"
print(f"Formatted: {x}, {y}")

# Core Types
i = 42                 # int
f = 3.14               # float
s = "Python"           # str
b = True               # bool
n = None               # NoneType

# Type Conversions
int("10")              # 10
str(100)               # "100"
float(5)               # 5.0
bool(1)                # True
```

## 2. Data Structures
```python
# Lists (Ordered, Mutable)
arr = [1, 2, 3]
arr.append(4)          # [1, 2, 3, 4]
arr.pop()              # Returns 4 -> [1, 2, 3]
first, rest = arr[0], arr[1:]

# Dictionaries (Key-Value)
d = {"name": "Alice", "age": 30}
d["role"] = "Dev"
age = d.get("age", 0)   # Safe lookup with default
keys, values = d.keys(), d.values()

# Sets (Unique, Unordered) & Tuples (Immutable)
s = {1, 2, 2, 3}       # {1, 2, 3}
t = (10, 20)           # Tuple
```

## 3. Control Flow & Loops
```python
# Conditionals
if x > 10:
    print("Greater")
elif x == 10:
    print("Equal")
else:
    print("Smaller")

# Ternary Operator
status = "Adult" if age >= 18 else "Minor"

# Loops
for i in range(5):                     # 0..4
    pass

for idx, item in enumerate(["a", "b"]): # Index + Item
    print(idx, item)

while count > 0:
    count -= 1
```

## 4. Comprehensions
```python
# List: [expr for item in iterable if cond]
evens = [x for x in range(10) if x % 2 == 0]

# Dict & Set
squares = {x: x**2 for x in range(5)}
unique_chars = {char for char in "hello"}
```

## 5. Functions & Lambdas
```python
# Def with Type Hints & Defaults
def greet(name: str, shout: bool = False) -> str:
    return name.upper() if shout else f"Hello, {name}"

# *args (Tuple), **kwargs (Dict)
def flex(*args, **kwargs):
    print(args, kwargs)

# Lambda
square = lambda x: x ** 2
```

## 6. Helpful Built-ins & Operations
```python
len([1, 2, 3])                 # 3
sum([1, 2, 3])                 # 6
min(a, b), max(a, b)           # Min/Max
sorted([3, 1, 2])              # [1, 2, 3]
list(zip([1, 2], ['a', 'b']))  # [(1, 'a'), (2, 'b')]
any([False, True]), all([1, 1]) # True, True
```

## 7. File I/O & Exception Handling
```python
# File I/O (Context Manager)
with open("file.txt", "w", encoding="utf-8") as f:
    f.write("Hello World\n")

with open("file.txt", "r") as f:
    text = f.read()

# Exception Handling
try:
    val = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
finally:
    print("Cleanup / Always executes")
```