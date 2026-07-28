# C++ Master Cheatsheet & Modern Reference Guide

### 1. Program Structure & Essentials
```cpp
#include <iostream> // Preprocessor directive for standard input/output streams

int main() {        // Execution entry point
    std::cout << "Hello, World!" << std::endl; 
    return 0;       // Status code: 0 means success
}
```

### 2. Core Primitive Data Types & Modern Types

| Data Type | Memory Size | Format Specifier / Equivalent | Modern C++ Feature |
| :--- | :--- | :--- | :--- |
| **`char`** | 1 byte | Stream tracking (`std::cout`) | Use `char16_t` or `char32_t` for Unicode |
| **`int`** | 4 bytes | Stream tracking (`std::cout`) | Use `uint32_t` from `<cstdint>` for exact size |
| **`float`** | 4 bytes | Stream tracking (`std::cout`) | Standard single-precision float |
| **`double`** | 8 bytes | Stream tracking (`std::cout`) | Standard double-precision float |
| **`bool`** | 1 byte | Prints `1` or `0` (`std::boolalpha`) | Native true/false variable state |

*Modern Modifiers:* Use **`auto`** for compile-time type inference. Use **`decltype(expr)`** to extract the data type of an expression.

### 3. Strings & STL Containers (`std::string`, `std::vector`, `std::array`)
```cpp
#include <string>
#include <vector>
#include <array>

// 1. Strings: Dynamic, safe, and self-managed sequence of characters
std::string str = "Hello C++";
str += " 2026"; // Dynamic appending

// 2. Fixed-Size Array: Drop-in replacement for traditional C-arrays (no overhead)
std::array<int, 5> fixedArr = {10, 20, 30, 40, 50};

// 3. Dynamic Vector: Contiguous resizable sequence
std::vector<int> dynamicVec = {1, 2, 3};
dynamicVec.push_back(4); // Dynamically reallocates internal storage
```

### 4. Smart Pointers & Memory Management (`<memory>`)
Modern C++ avoids manual `new` and `delete` to prevent memory leaks.
```cpp
#include <memory>

class Resource { /* ... */ };

int main() {
    // 1. Unique Pointer: Sole owner of a piece of memory (cannot be copied, only moved)
    std::unique_ptr<Resource> ptr1 = std::make_unique<Resource>();
    
    // 2. Shared Pointer: Reference-counted ownership (can be copied to multiple references)
    std::shared_ptr<Resource> ptr2 = std::make_shared<Resource>();
    
    // 3. Weak Pointer: Non-owning observer to break shared cyclic dependencies
    std::weak_ptr<Resource> weakPtr = ptr2;
    
    return 0; // Memory is automatically freed when pointers drop out of scope
}
```

### 5. Control Flow Syntax & Range-Based Loops
```cpp
// Conditionals with Init-Statement (C++17)
if (int result = calculateStatus(); result > 0) {
    // 'result' is strictly scoped inside this block
}

// Modern Range-Based For Loop (Read-only or modification)
std::vector<int> items = {10, 20, 30};
for (const auto& item : items) { /* Read-only extraction */ }
for (auto& item : items) { item *= 2; } /* Direct modification */
```

### 6. Functions, Lambdas & Callables
```cpp
#include <algorithm>
#include <vector>

// 1. Standard Inline Lambda Expression
auto add = [](int a, int b) -> int { return a + b; };

// 2. Vector Mutation with Lambda Closures
std::vector<int> vec = {1, 2, 3};
std::for_each(vec.begin(), vec.end(), [](int &n) { n++; });
```

### 7. Classes, Inheritance & Object-Oriented Programming (OOP)
```cpp
class Animal {
public:
    virtual void makeValue() const = 0; // Pure virtual function (Interface boundary)
    virtual ~Animal() = default;         // Virtual destructor ensures proper resource release
};

class Dog : public Animal {
public:
    void makeValue() const override {   // Explicit compilation tracking with override
        std::cout << "Woof!" << std::endl;
    }
};
```

### 8. Bidirectional String Conversions Reference Guide

#### Type Conversions FROM Strings
These functions parse strings up to the first invalid character and throw explicit standard exceptions (`std::invalid_argument` or `std::out_of_range`) if the conversion fails.

* **`std::stoi()` / `std::stol()` / `std::stoll()`**  
  * **Syntax:** `int stoi(const std::string& str, size_t* pos = 0, int base = 10);`
  * **Use Case:** Converting numeric text into signed integers (base 10 by default, supports Hex/Binary bases). The optional `pos` pointer tracks exactly where parsing stopped.
* **`std::stof()` / `std::stod()` / `std::stold()`**  
  * **Syntax:** `double stod(const std::string& str, size_t* pos = 0);`
  * **Use Case:** Converting text entries into floating-point numbers (`float`, `double`, or `long double`).
* **`std::stringstream` (Stream Extraction)**  
  * **Syntax:** `std::stringstream ss(str); ss >> targetVar;`
  * **Use Case:** Safely unpacking mixed data schemas from single text strings into separate variables sequentially.

#### Type Conversions TO Strings
Modern C++ provides multiple paths to convert numeric primitives back into character strings depending on your performance and formatting requirements.

* **`std::to_string()`**  
  * **Header:** `<string>`
  * **Syntax:** `std::string str = std::to_string(value);`
  * **Use Case:** The easiest, most straightforward way to convert any standard primitive (`int`, `float`, `double`, etc.) into a modern `std::string`.
* **`std::format()` (C++20 Preferred)**  
  * **Header:** `<format>`
  * **Syntax:** `std::string str = std::format("Score: {} | Ratio: {:.2f}", 100, 0.756);`
  * **Use Case:** Type-safe, high-performance string interpolation and precision layout formatting. It completely replaces messy, verbose C++ stream manipulators and unsafe C-style `sprintf`.
* **`std::to_chars()` (C++17/C++20 Zero-Allocation)**  
  * **Header:** `<charconv>`
  * **Syntax:** `std::to_chars_result to_chars(char* first, char* last, T value);`
  * **Use Case:** High-performance, locale-independent formatting directly into a pre-allocated low-level text buffer. Designed for high-throughput network applications or writing custom JSON serializers without heap allocation overhead.

### 9. File System I/O Streams (`<fstream>` & `<filesystem>`)
```cpp
#include <fstream>
#include <filesystem>
namespace fs = std::filesystem;

// 1. Safe Writing Output Block
if (std::ofstream outFile{"output.txt"}) {
    outFile << "C++ Standard File Content\n";
} // Destructor auto-closes system file anchors

// 2. Directory Validation & Parsing
if (fs::exists("config/")) {
    for (const auto& entry : fs::directory_iterator("config/")) {
        std::cout << entry.path() << std::endl;
    }
}
```

### 10. Modern Modernizations (C++20 to C++26 Extensions)
* **Concepts (`<concepts>`)**: Constrains template parameters at compile-time for cleaner compiler error traces.
  ```cpp
  template<typename T> requires std::integral<T>
  T addIntegers(T a, T b) { return a + b; }
  ```
* **Ranges (`<ranges>`)**: Composable functional pipelines for modifying collections.
  ```cpp
  auto results = views | std::views::filter([](int n){ return n % 2 == 0; });
  ```
