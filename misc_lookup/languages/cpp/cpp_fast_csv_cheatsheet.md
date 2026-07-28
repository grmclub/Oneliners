# High-Performance C++ CSV Parsing Cheatsheet (Zero Dependencies)

## 1. Memory-Mapped File I/O (`POSIX mmap` / Windows `MapViewOfFile`)
```cpp
#include <iostream>
#include <fcntl.h>
#include <sys/mman.h>
#include <sys/stat.h>
#include <unistd.h>

struct MmapFile {
    const char* data = nullptr;
    size_t size = 0;

    bool open(const char* filepath) {
        int fd = ::open(filepath, O_RDONLY);
        if (fd == -1) return false;
        struct stat sb;
        fstat(fd, &sb);
        size = sb.st_size;
        data = static_cast<const char*>(mmap(nullptr, size, PROT_READ, MAP_PRIVATE, fd, 0));
        close(fd);
        return data != MAP_FAILED;
    }

    ~MmapFile() { if (data && data != MAP_FAILED) munmap((void*)data, size); }
};
```

## 2. Zero-Allocation String Views (`std::string_view`)
```cpp
#include <vector>
#include <string_view>

// Parse row without allocating heap memory for cell strings
inline std::vector<std::string_view> parse_line_fast(const char* line_start, size_t length) {
    std::vector<std::string_view> row;
    row.reserve(16); // Pre-allocate estimated columns
    
    const char* ptr = line_start;
    const char* end = line_start + length;
    const char* cell_start = ptr;

    while (ptr < end) {
        if (*ptr == ',') {
            row.emplace_back(cell_start, ptr - cell_start);
            cell_start = ptr + 1;
        }
        ptr++;
    }
    row.emplace_back(cell_start, end - cell_start); // Final column
    return row;
}
```

## 3. Fast In-Place Number Parsing (`std::from_chars`)
```cpp
#include <charconv>
#include <string_view>

// High-speed integer parsing (Directly from memory buffer)
inline int fast_atoi(std::string_view sv) {
    int val = 0;
    std::from_chars(sv.data(), sv.data() + sv.size(), val);
    return val;
}

// High-speed double parsing (C++17)
inline double fast_atof(std::string_view sv) {
    double val = 0.0;
    std::from_chars(sv.data(), sv.data() + sv.size(), val);
    return val;
}
```

## 4. Full High-Speed File Iteration Engine
```cpp
#include <vector>
#include <string_view>

void process_csv_fast(const char* data, size_t size) {
    const char* ptr = data;
    const char* end = data + size;
    const char* line_start = ptr;

    while (ptr < end) {
        if (*ptr == '
') {
            size_t line_len = ptr - line_start;
            if (line_len > 0 && *(ptr - 1) == '') line_len--; // Strip ''

            std::vector<std::string_view> row = parse_line_fast(line_start, line_len);
            
            // Process row values instantly using std::from_chars
            // int id = fast_atoi(row[0]);
            // double val = fast_atof(row[1]);

            line_start = ptr + 1;
        }
        ptr++;
    }
}
```

## 5. Branchless Custom Integer Parser (Maximum Speed)
```cpp
#include <string_view>

// Bypasses locale & standard checks for maximum throughput
inline int parse_uint_branchless(std::string_view sv) {
    int result = 0;
    for (char c : sv) {
        result = result * 10 + (c - '0');
    }
    return result;
}
```

## 6. Pre-Allocated Fixed Buffer I/O (`std::fread`)
```cpp
#include <cstdio>
#include <vector>

void read_buffered(const char* filepath) {
    FILE* file = std::fopen(filepath, "rb");
    if (!file) return;

    constexpr size_t BUFFER_SIZE = 1 << 20; // 1 MB Buffer
    std::vector<char> buffer(BUFFER_SIZE);
    size_t bytes_read = 0;

    while ((bytes_read = std::fread(buffer.data(), 1, BUFFER_SIZE, file)) > 0) {
        // Process buffer chunks...
    }
    std::fclose(file);
}
```
