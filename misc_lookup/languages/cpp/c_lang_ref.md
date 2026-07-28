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
