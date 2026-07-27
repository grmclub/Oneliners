# Excel Formulas & Functions Cheat Sheet

---

### 1. Essential Basics & Math

| Formula | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`SUM`** | Adds a range of numbers. | `=SUM(A1:A10)` |
| **`AVERAGE`** | Calculates the arithmetic mean. | `=AVERAGE(A1:A10)` |
| **`COUNT`** | Counts cells containing **numbers**. | `=COUNT(A1:A10)` |
| **`COUNTA`** | Counts non-empty cells (text + numbers). | `=COUNTA(A1:A10)` |
| **`COUNTBLANK`** | Counts empty cells in a range. | `=COUNTBLANK(A1:A10)` |
| **`MIN` / `MAX`** | Finds the lowest or highest value. | `=MIN(A1:A10)` / `=MAX(A1:A10)` |
| **`ROUND`** | Rounds a number to specified digits. | `=ROUND(12.3456, 2)` $\rightarrow$ `12.35` |

---

### 2. Modern Lookups (Replacing VLOOKUP)

| Formula | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`XLOOKUP`** *(Modern Standard)* | Searches a range and returns a matching value from another column (works left or right). | `=XLOOKUP(lookup_val, lookup_range, return_range, [if_not_found])`<br>`=XLOOKUP(E2, A2:A100, C2:C100, "Not Found")` |
| **`INDEX` + `MATCH`** *(Classic Dynamic Lookup)* | Combines position matching with array lookup. | `=INDEX(return_range, MATCH(lookup_val, lookup_range, 0))`<br>`=INDEX(C2:C100, MATCH(E2, A2:A100, 0))` |
| **`VLOOKUP`** *(Legacy)* | Vertical lookup (requires lookup column to be on the far left). | `=VLOOKUP(lookup_val, table_range, col_index, [exact_match])`<br>`=VLOOKUP(E2, A2:C100, 3, FALSE)` |

---

### 3. Conditional Aggregations

| Formula | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`COUNTIF`** | Counts cells meeting a single condition. | `=COUNTIF(A1:A100, ">50")` |
| **`COUNTIFS`** | Counts cells meeting **multiple** conditions. | `=COUNTIFS(A1:A100, ">50", B1:B100, "Active")` |
| **`SUMIF`** | Sums values matching a single condition. | `=SUMIF(range, criteria, [sum_range])`<br>`=SUMIF(A1:A100, "East", B1:B100)` |
| **`SUMIFS`** | Sums values matching **multiple** conditions. | `=SUMIFS(sum_range, criteria_range1, criteria1, ...)`<br>`=SUMIFS(C1:C100, A1:A100, "East", B1:B100, ">2025")` |
| **`AVERAGEIFS`** | Calculates mean based on multiple conditions. | `=AVERAGEIFS(C1:C100, A1:A100, "East")` |

---

### 4. Logic & Error Handling

| Formula | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`IF`** | Tests a logical condition. | `=IF(A1>=70, "Pass", "Fail")` |
| **`IFS`** | Evaluates multiple logical conditions in sequence. | `=IFS(A1>=90, "A", A1>=80, "B", A1>=70, "C")` |
| **`AND` / `OR`** | Combines logical tests. | `=IF(AND(A1>50, B1="Yes"), "Approved", "Denied")` |
| **`IFERROR`** | Returns a fallback value if a formula errors out. | `=IFERROR(A1/B1, "Calculation Error")` |

---

### 5. Text Manipulation

| Formula | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`CONCAT` / `TEXTJOIN`** | Joins text strings together (`TEXTJOIN` handles delimiters). | `=TEXTJOIN(", ", TRUE, A1:A5)` |
| **`LEFT` / `RIGHT` / `MID`** | Extracts characters from text. | `=LEFT(A1, 3)` / `=MID(A1, 2, 4)` |
| **`LEN`** | Returns total character length of a cell. | `=LEN(A1)` |
| **`TRIM`** | Removes extra spaces (keeps single spaces between words). | `=TRIM(A1)` |
| **`UPPER` / `LOWER` / `PROPER`** | Changes text casing. | `=PROPER("john doe")` $\rightarrow$ `"John Doe"` |

---

### 6. Modern Dynamic Arrays (Excel 365 / 2021+)

| Formula | Description | Syntax / Example |
| :--- | :--- | :--- |
| **`UNIQUE`** | Extracts unique values from a range. | `=UNIQUE(A1:A100)` |
| **`SORT`** | Sorts a range dynamically. | `=SORT(A1:B100, 1, 1)` *(Sorts column 1 ascending)* |
| **`FILTER`** | Filters data array based on criteria. | `=FILTER(A1:C100, B1:B100="Completed", "No Data")` |

---

### 💡 Quick Reference Tips
* **Absolute Cell References:** Lock row/column references using `$` (`$A$1`) so formulas don't shift when copied.
* **Toggle Formulas Display:** Press `Ctrl + ~` to switch between showing values and showing raw formulas across the sheet.