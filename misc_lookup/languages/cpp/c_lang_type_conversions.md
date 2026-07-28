
==============================
### 1. Type Conversion Functions (`<stdlib.h>` & `<stdio.h>`)

#### 1. String to Numeric Type (Standard parsing)
* **`atoi()`**
  * **Syntax:** `int atoi(const char *str);`
  * **Description:** Converts an ASCII string representing a whole number directly into an integer.
  * **Use Case:** Converting numeric command-line arguments (`argv[1]`) into working loop parameters.
* **`atof()`**
  * **Syntax:** `double atof(const char *str);`
  * **Description:** Converts an ASCII string into a floating-point `double`.
  * **Use Case:** Parsing standard configurations or decimal inputs (e.g., pricing strings like `"19.99"`).

#### 2. Robust String to Numeric (With Error Checking)
* **`strtol()`**
  * **Syntax:** `long strtol(const char *str, char **endptr, int base);`
  * **Description:** Converts a string to a `long` integer, providing a pointer to where parsing stopped to detect trailing garbage text. Supports arbitrary bases (e.g., base 16 for hex, base 2 for binary).
  * **Use Case:** Secure validation of numeric forms or reading raw memory addresses from text strings.
* **`strtod()`**
  * **Syntax:** `double strtod(const char *str, char **endptr);`
  * **Description:** Converts a string to a double-precision float while exposing validation pointers for parsing errors.
  * **Use Case:** Safe financial calculations when processing text streams from unchecked files or external APIs.

#### 3. Numeric to String Type
* **`sprintf()` / `snprintf()`**
  * **Syntax:** `int snprintf(char *str, size_t size, const char *format, ...);`
  * **Description:** Writes formatted text (numbers, characters, strings) into a pre-allocated text buffer. `snprintf` is strictly preferred as it caps write limits.
  * **Use Case:** Transforming numerical variables (`int x = 42;`) into string text to render on a user UI or append into a string message.
  
==============================
### 2. String to Numeric Conversion Reference

#### 1. Basic Conversions (Fast, no error tracking)
* **`atoi()`**
  * **Syntax:** `int atoi(const char *str);`
  * **Header:** `<stdlib.h>`
  * **Description:** Parses an ASCII string and converts it directly into a standard `int`. Stops at the first non-numeric character.
  * **Use Case:** Converting simple numeric command-line arguments (e.g., `"10"` from `argv`) into a loop iteration counter.
* **`atof()`**
  * **Syntax:** `double atof(const char *str);`
  * **Header:** `<stdlib.h>`
  * **Description:** Parses an ASCII string into a floating-point `double`. Handles decimals and scientific notation (e.g., `"3.14"`, `"1e-3"`).
  * **Use Case:** Quick conversion of configuration file values like fractional weight multipliers or coordinate metrics.

#### 2. Advanced Conversions (Robust, with error checking)
* **`strtol()`**
  * **Syntax:** `long strtol(const char *str, char **endptr, int base);`
  * **Header:** `<stdlib.h>`
  * **Description:** Converts a string into a `long` integer. Sets `endptr` to point to the character where parsing failed, allowing you to catch trailing non-numeric garbage data. Supports custom bases (e.g., base 16 for hex, base 2 for binary).
  * **Use Case:** Validating clean user input fields to ensure string entries contain entirely valid digits without hidden characters.
* **`strtod()`**
  * **Syntax:** `double strtod(const char *str, char **endptr);`
  * **Header:** `<stdlib.h>`
  * **Description:** Converts a string into a double-precision float while logging residual text position within `endptr` to monitor conversion success or failure.
  * **Use Case:** Safely converting messy data files containing varying string formats into pure numeric arrays for data processing blocks.

#### 3. Inline Buffer Parsing
* **`sscanf()`**
  * **Syntax:** `int sscanf(const char *str, const char *format, ...);`
  * **Header:** `<stdio.h>`
  * **Description:** Reads formatted data directly from an existing string buffer, pulling integers (`%d`) and floats (`%f`) out matching a template schema. Returns the number of successfully matched variables.
  * **Use Case:** Deconstructing complex, structured lines of text (e.g., parsing a log file string like `"ID:402 Score:92.5"` straight into variables in one step).
 
 ==============================