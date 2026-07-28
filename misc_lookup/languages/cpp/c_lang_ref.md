# C Language Quick Lookup Cheatsheet

### 1. Program Structure & Essentials
```c
#include <stdio.h> // Preprocessor directive for standard I/O

int main() {       // Execution entry point
    printf("Hello, World!\n"); 
    return 0;      // Status code: 0 means success
}
```

### 2. Core Primitive Data Types

| Data Type | Memory Size | Format Specifier | Typical Range / Description |
| :--- | :--- | :--- | :--- |
| **`char`** | 1 byte | `%c` | Single character or integer from -128 to 127 |
| **`int`** | 4 bytes | `%d` or `%i` | Whole numbers from -2.14B to 2.14B |
| **`float`** | 4 bytes | `%f` | Single-precision floating point (6 decimal places) |
| **`double`** | 8 bytes | `%lf` | Double-precision floating point (15 decimal places) |

*Modifiers:* Add `unsigned` (e.g., `unsigned int`) to remove negative ranges and double positive maximums. Formatted with `%u`.

### 3. Strings & Arrays
```c
// Arrays: Contiguous blocks of identical data types
int numbers[5] = {10, 20, 30, 40, 50}; // Fixed-size allocation
numbers[0] = 99;                       // Zero-indexed mutation

// Strings: Character arrays terminated by a null byte '\0'
char staticString[] = "Hello";         // Auto-appends '\0'
```

### 4. Directives, Pointers & Memory Address Mapping
```c
int num = 42;
int *ptr = &num; // Address operator (&): Fetches memory address location of num

// Dereference operator (*): Accesses value stored at the pointing address
printf("Address: %p\n", (void*)&num); // Output location index
printf("Pointer Value: %d\n", *ptr);  // Outputs: 42
```

### 5. Control Flow Syntax
```c
// Conditionals
if (x > 10) { 
    /* Code */ 
} else if (x == 10) { 
    /* Code */ 
} else { 
    /* Code */ 
}

// Switch Case
switch(grade) {
    case 'A': printf("Excellent"); break;
    case 'B': printf("Good"); break;
    default: printf("Invalid");
}

// Iterative Loops
while (condition) { /* ... */ }
for (int i = 0; i < 10; i++) { /* ... */ }
do { /* ... */ } while (condition);
```

### 6. Functions & Variable Prototypes
```c
// Prototype Statement (Declared above main)
int calculateSum(int a, int b);

// Definition Implementation (Declared below main)
int calculateSum(int a, int b) {
    return a + b; // Return matched type value
}
```

### 7. Custom Data Structuring
```c
// Struct: Collects different data types under one identifier name
struct User {
    int id;
    char grade;
};

struct User user1 = {101, 'A'};
user1.id = 102; // Access via dot operator (.)
```

### 8. Advanced String Handling Functions (`<string.h>`)
```c
#include <string.h>

char dest[20] = "Hello";
char src[] = " World";

// 1. Length Evaluation
size_t len = strlen(dest);       // Returns number of characters excluding '\0'

// 2. Safe Concatenation & Copying
strncat(dest, src, sizeof(dest) - strlen(dest) - 1); // Safely appends src to dest
strncpy(dest, "New Text", sizeof(dest) - 1);         // Safely copies string up to limit

// 3. String Comparison
int result = strcmp(dest, "Hello"); // Returns 0 if equal, <0 if dest < src, >0 if dest > src

// 4. Searching & Substrings
char *ptr = strchr(dest, 'W');   // Returns pointer to first occurrence of 'W'
char *sub = strstr(dest, "Text"); // Returns pointer to first occurrence of substring "Text"

// 5. Splitting String by Delimiter (Tokenization)
char csv[] = "John,25,Engineer";
char *token = strtok(csv, ",");
while (token != NULL) {
    printf("%s\n", token);       // Prints individual parts separate by commas
    token = strtok(NULL, ",");   // Pass NULL to continue tokenizing same string
}
```

### 9. Standard String Functions Reference Guide

#### Copying & Moving
* **`strcpy()`**: `char *strcpy(char *dest, const char *src);`
  * *Use Case:* Basic variable assignment when cloning text into a target buffer.
* **`strncpy()`**: `char *strncpy(char *dest, const char *src, size_t n);`
  * *Use Case:* Secure string copying to prevent buffer overflow exploits.
* **`memcpy()`**: `void *memcpy(void *dest, const void *src, size_t n);`
  * *Use Case:* High-performance byte cloning of structured binary structs or data packets.
* **`memmove()`**: `void *memmove(void *dest, const void *src, size_t n);`
  * *Use Case:* Safe copy operations handling overlapping memory buffers accurately.

#### Comparison
* **`strcmp()`**: `int strcmp(const char *s1, const char *s2);`
  * *Use Case:* Checking if user input matches an exact system text command.
* **`strncmp()`**: `int strncmp(const char *s1, const char *s2, size_t n);`
  * *Use Case:* Prefix filtering to verify if a text block begins with a token like `"https://"`.
* **`memcmp()`**: `int memcmp(const void *s1, const void *s2, size_t n);`
  * *Use Case:* Binary validation to check if two memory blocks match bit-for-bit.
* **`strcoll()`**: `int strcoll(const char *s1, const char *s2);`
  * *Use Case:* Sorting strings across localized dictionary rules rather than raw ASCII layout.

#### Searching & Utilities
* **`strrchr()`**: `char *strrchr(const char *s, int c);`
  * *Use Case:* Finding the last occurrence of a slash (`/`) to extract a raw filename from a system path.
* **`strspn()`**: `size_t strspn(const char *s, const char *accept);`
  * *Use Case:* Counting valid numeric characters at the start of an alphanumeric input string.
* **`strcspn()`**: `size_t strcspn(const char *s, const char *reject);`
  * *Use Case:* Finding the index location of a trailing `\n` character from a console input to strip it away.
* **`strpbrk()`**: `char *strpbrk(const char *s, const char *accept);`
  * *Use Case:* Sanitization check to see if any illegal characters exist inside a target string.
* **`memset()`**: `void *memset(void *s, int c, size_t n);`
  * *Use Case:* Zeroing out a sensitive input text data buffer holding keys or passwords right after use.
* **`strerror()`**: `char *strerror(int errnum);`
  * *Use Case:* Translating system error codes into human-readable messages during standard diagnostic logging.

### 10. Type Conversion Functions (`<stdlib.h>` & `<stdio.h>`)

#### String to Numeric Type (Fast / Basic)
* **`atoi()`**: `int atoi(const char *str);`
  * *Use Case:* Quick parsing of whole integer CLI variables (`argv`).
* **`atof()`**: `double atof(const char *str);`
  * *Use Case:* Fast conversion of decimal configuration entries (e.g., `"19.99"`).

#### String to Numeric Type (Robust with Error Checking)
* **`strtol()`**: `long strtol(const char *str, char **endptr, int base);`
  * *Use Case:* Secure string-to-integer parsing. Tracks where scanning stops via `endptr` to trap garbage data or trailing characters, and handles custom bases (like Hex or Binary).
* **`strtod()`**: `double strtod(const char *str, char **endptr);`
  * *Use Case:* Precision data translation for float arrays while ensuring parsing boundaries are checked safely.

#### Inline Buffer Parsing & Reverse Formatting
* **`sscanf()`**: `int sscanf(const char *str, const char *format, ...);`
  * *Use Case:* Extracting mixed formatting variables out of complex data lines (e.g., parsing `"ID:42 Val:88.2"` directly into integers and floats).
* **`snprintf()`**: `int snprintf(char *str, size_t size, const char *format, ...);`
  * *Use Case:* Converting numerical primitives securely into text string buffers without crashing array parameters.

### 11. Type Conversions From Strings Reference Guide

#### 1. Quick Primitive Parsing (Fast, no error verification)
* **`atoi()`**
  * **Syntax:** `int atoi(const char *str);`
  * **Header:** `<stdlib.h>`
  * **Use Case:** Converting numerical command-line arguments directly into working integer parameters (e.g., passing `"5"` via `argv[1]` to a loop counter).
* **`atof()`**
  * **Syntax:** `double atof(const char *str);`
  * **Header:** `<stdlib.h>`
  * **Use Case:** Fast conversion of geometric dimensions or flat configuration settings from file texts (e.g., parsing a scaling constant like `"0.75"`).
* **`atol()` / `atoll()`**
  * **Syntax:** `long atol(const char *str);` / `long long atoll(const char *str);`
  * **Header:** `<stdlib.h>`
  * **Use Case:** Converting larger integer metrics like timestamps or file sizes stored as basic plain text.

#### 2. Robust Parsing (With error trapping & pointer bounds tracking)
* **`strtol()` / `strtoull()`**
  * **Syntax:** `long strtol(const char *str, char **endptr, int base);`
  * **Header:** `<stdlib.h>`
  * **Use Case:** Secure validation of numeric fields. The user tracks where scanning breaks via `endptr` to detect trailing garbage characters (e.g., parsing `"42abc"` sets `endptr` to point to `'a'`). Supports custom bases like hex (`16`) or binary (`2`).
* **`strtod()` / `strtof()`**
  * **Syntax:** `double strtod(const char *str, char **endptr);` / `float strtof(const char *str, char **endptr);`
  * **Header:** `<stdlib.h>`
  * **Use Case:** Extracting precise float metrics from unknown streams or external APIs while guaranteeing validation metrics are verified.

#### 3. Structured Data Extraction (Multi-variable parsing)
* **`sscanf()`**
  * **Syntax:** `int sscanf(const char *str, const char *format, ...);`
  * **Header:** `<stdio.h>`
  * **Use Case:** Dissecting variables directly out of compound structural text arrays. You match text tokens and extract integers (`%d`), floats (`%f`), and single characters (`%c`) simultaneously into matching addresses in a single step (e.g., parsing log strings like `"TEMP: 23.5C ID: 104"`).


### 12. Dynamic Memory Management (`<stdlib.h>`)
```c
#include <stdlib.h>

// 1. Dynamic Allocation (Leaves memory uninitialized with random garbage data)
int *arr = (int *)malloc(5 * sizeof(int)); 
if (arr == NULL) { return 1; }            // ALWAYS check for allocation failure

// 2. Clear Allocation (Initializes all bits sequentially to zero)
int *clean_arr = (int *)calloc(5, sizeof(int));

// 3. Resize Allocation (Expands/contracts block, moving data if necessary)
int *expanded_arr = (int *)realloc(arr, 10 * sizeof(int));
if (expanded_arr != NULL) { arr = expanded_arr; } // Prevent leaks if realloc fails

// 4. Manual Deallocation (ALWAYS clear memory blocks to avoid memory leaks)
free(arr);
free(clean_arr);
arr = NULL; // Zero out pointer immediately to prevent dangerous dangling pointers
```

### 13. Safe Input Processing & Buffer Sanitization
```c
#include <stdio.h>
#include <string.h>

char buffer[32];

// NEVER USE gets() - it does not track boundaries and causes buffer overflows.
// USE fgets() instead to specify maximum boundary length constraints.
if (fgets(buffer, sizeof(buffer), stdin) != NULL) {
    
    // Crucial step: Strip off the trailing newline ('\n') appended by pressing enter
    buffer[strcspn(buffer, "\n")] = '\0';
    
    // Flush extra leftover bytes in stdin if the user input exceeded the buffer size
    if (strlen(buffer) == sizeof(buffer) - 1 && buffer[sizeof(buffer) - 2] != '\0') {
        int ch;
        while ((ch = getchar()) != '\n' && ch != EOF);
    }
}
```

### 14. File System I/O Streams (`<stdio.h>`)
```c
#include <stdio.h>

// 1. Streaming Open Actions
FILE *file = fopen("log.txt", "w"); // Modes: "r" (read), "w" (write/overwrite), "a" (append)
if (file == NULL) { return 1; }     // Fail-safe exit if file does not exist or lacks permissions

// 2. Text Content Operations
fprintf(file, "Status Code: %d\n", 200); // Format text output into file stream

// 3. Reading Line-by-Line Safely
FILE *readFile = fopen("config.txt", "r");
char line[128];
if (readFile != NULL) {
    while (fgets(line, sizeof(line), readFile) != NULL) {
        printf("Read: %s", line);
    }
    fclose(readFile); // ALWAYS close file streams to unlock resource anchors
}
fclose(file);
```

### 15. Bitwise Operators & Bit Manipulation

| Operator | Name | Operation Description | Real-World Use Case |
| :---: | :--- | :--- | :--- |
| **`&`** | **Bitwise AND** | Sets bit to `1` only if **both** corresponding bits are `1`. | **Masking Data:** Clearing unwanted bits or checking if a specific bit is set. |
| **`\|`** | **Bitwise OR** | Sets bit to `1` if **at least one** corresponding bit is `1`. | **Setting Flags:** Combining multiple configuration options or turning a specific bit on. |
| **`^`** | **Bitwise XOR** | Sets bit to `1` only if the bits are **different**. | **Toggling Bits:** Flipping state back and forth, or simple lightweight encryption. |
| **`~`** | **Bitwise NOT** | Inverts all bits (one's complement; `0` becomes `1`, `1` becomes `0`). | **Inverting Masks:** Generating an inverse pattern to clear specific flags. |
| **`<<`** | **Left Shift** | Shifts bits to the left, padding empty right slots with `0`. | **Fast Multiplication:** Shifting left by `n` multiplies an integer by $2^n$. |
| **`>>`** | **Right Shift** | Shifts bits to the right, discarding overflowing right bits. | **Fast Division:** Shifting right by `n` divides an integer by $2^n$. |

#### Essential Bit Manipulation Patterns
```c
#include <stdio.h>

// Define positions as bit masks using the Left Shift (<<) operator
#define FLAG_A (1 << 0)  // 00000001 (Decimal 1)
#define FLAG_B (1 << 1)  // 00000010 (Decimal 2)
#define FLAG_C (1 << 2)  // 00000100 (Decimal 4)

int main() {
    unsigned char settings = 0; // Start with all bits cleared (00000000)

    // 1. SET a bit (Turn on FLAG_A and FLAG_C using OR)
    settings |= FLAG_A; 
    settings |= FLAG_C;         // settings is now 00000101

    // 2. CHECK a bit (Verify if FLAG_A is active using AND)
    if (settings & FLAG_A) {
        printf("FLAG_A is active!\n");
    }

    // 3. TOGGLE a bit (Flip FLAG_B from 0 to 1, or 1 to 0 using XOR)
    settings ^= FLAG_B;         // settings is now 00000111

    // 4. CLEAR a bit (Turn off FLAG_A using AND along with an inverted NOT mask)
    settings &= ~FLAG_A;        // Inverts 00000001 to 11111110, forcing FLAG_A to 0.

    // 5. Check if an integer is EVEN or ODD instantly
    int num = 43;
    if (num & 1) {
        printf("%d is Odd\n", num); // If the lowest bit is 1, the number is odd.
    }
    
    return 0;
}
```
