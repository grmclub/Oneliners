# C++ Ultra-Fast String Splitting Cheatsheet (Zero-Copy & Zero Dependencies)

## 1. Fast `std::string_view` Split (In-Place Pointer Arithmetic)
```cpp
#include <vector>
#include <string_view>

// Splitting without allocating dynamic string buffers
inline void split_sv(std::string_view str, char delim, std::vector<std::string_view>& out) {
    out.clear();
    const char* ptr = str.data();
    const char* end = ptr + str.size();
    const char* start = ptr;

    while (ptr < end) {
        if (*ptr == delim) {
            out.emplace_back(start, ptr - start);
            start = ptr + 1;
        }
        ptr++;
    }
    out.emplace_back(start, end - start);
}
```

## 2. Fixed-Array Output (Zero Heap Allocation Split)
```cpp
#include <string_view>
#include <array>

// Best for rows with a known number of columns (N)
template <size_t N>
inline size_t split_fixed(std::string_view str, char delim, std::array<std::string_view, N>& out) {
    const char* ptr = str.data();
    const char* end = ptr + str.size();
    const char* start = ptr;
    size_t count = 0;

    while (ptr < end && count < N) {
        if (*ptr == delim) {
            out[count++] = std::string_view(start, ptr - start);
            start = ptr + 1;
        }
        ptr++;
    }
    if (count < N) {
        out[count++] = std::string_view(start, end - start);
    }
    return count; // Number of parsed columns
}
```

## 3. Fast In-Place CSV Quote-Aware Splitting
```cpp
#include <vector>
#include <string_view>

// Handles embedded commas inside quotes (e.g., "Doe, John",100)
inline void split_csv_quoted(std::string_view str, std::vector<std::string_view>& out) {
    out.clear();
    const char* ptr = str.data();
    const char* end = ptr + str.size();
    const char* start = ptr;
    bool in_quotes = false;

    while (ptr < end) {
        if (*ptr == '"') {
            in_quotes = !in_quotes;
        } else if (*ptr == ',' && !in_quotes) {
            out.emplace_back(start, ptr - start);
            start = ptr + 1;
        }
        ptr++;
    }
    out.emplace_back(start, end - start);
}
```

## 4. Single-Pass Iteration via Range-Like State Struct
```cpp
#include <string_view>

// Low-overhead state machine for manual forward-only iteration
struct SVSplitter {
    std::string_view data;
    char delim;
    size_t pos = 0;

    bool next(std::string_view& field) {
        if (pos > data.size()) return false;
        
        size_t next_delim = data.find(delim, pos);
        if (next_delim == std::string_view::npos) {
            field = data.substr(pos);
            pos = data.size() + 1; // Mark complete
        } else {
            field = data.substr(pos, next_delim - pos);
            pos = next_delim + 1;
        }
        return true;
    }
};
```

## 5. Branchless Custom Length & Trimming Splitter
```cpp
#include <string_view>
#include <vector>

// Strips carriage returns (`\r`) or spaces while splitting
inline void split_and_trim(std::string_view str, char delim, std::vector<std::string_view>& out) {
    out.clear();
    const char* ptr = str.data();
    const char* end = ptr + str.size();
    const char* start = ptr;

    while (ptr < end) {
        if (*ptr == delim) {
            const char* token_end = ptr;
            while (token_end > start && (token_end[-1] == '' || token_end[-1] == ' ')) token_end--;
            out.emplace_back(start, token_end - start);
            start = ptr + 1;
        }
        ptr++;
    }
    const char* token_end = end;
    while (token_end > start && (token_end[-1] == '' || token_end[-1] == ' ')) token_end--;
    out.emplace_back(start, token_end - start);
}
```
