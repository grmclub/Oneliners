# Modern C++ CSV Data Processing Cheatsheet (Standard Library)

## 1. Reading CSV Line-by-Line & Parsing
```cpp
#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <string>

std::vector<std::vector<std::string>> read_csv(const std::string& filename) {
    std::vector<std::vector<std::string>> data;
    std::ifstream file(filename);
    std::string line, cell;

    while (std::getline(file, line)) {
        std::vector<std::string> row;
        std::stringstream ss(line);
        while (std::getline(ss, cell, ',')) {
            row.push_back(cell);
        }
        data.push_back(row);
    }
    return data;
}
```

## 2. Writing CSV Files
```cpp
#include <fstream>
#include <vector>
#include <string>

void write_csv(const std::string& filename, const std::vector<std::vector<std::string>>& rows) {
    std::ofstream file(filename);
    for (const auto& row : rows) {
        for (size_t i = 0; i < row.size(); ++i) {
            file << row[i] << (i + 1 < row.size() ? "," : "");
        }
        file << "
";
    }
}
```

## 3. String Cleaning & Type Conversions
```cpp
#include <string>
#include <algorithm>
#include <cctype>

// Trim whitespace
std::string trim(const std::string& s) {
    auto start = s.find_first_not_of(" 	
");
    auto end = s.find_last_not_of(" 	
");
    return (start == std::string::npos) ? "" : s.substr(start, end - start + 1);
}

// Convert String to Numbers
int id = std::stoi(trim(" 101 "));
double price = std::stod(trim(" 49.99 "));

// Convert Number to String
std::string str_val = std::to_string(price);
```

## 4. Struct Mapping, Filtering & Sorting
```cpp
#include <vector>
#include <algorithm>
#include <string>

struct Record {
    int id;
    std::string category;
    double price;
};

// Filtering Records
std::vector<Record> filter_by_price(const std::vector<Record>& records, double min_price) {
    std::vector<Record> result;
    for (const auto& r : records) {
        if (r.price >= min_price) result.push_back(r);
    }
    return result;
}

// Sorting Records (Multi-Key)
void sort_records(std::vector<Record>& records) {
    std::sort(records.begin(), records.end(), [](const Record& a, const Record& b) {
        if (a.category != b.category) return a.category < b.category; // Primary: Category
        return a.price > b.price;                                    // Secondary: Price (Desc)
    });
}
```

## 5. Grouping & Aggregations (`std::unordered_map`)
```cpp
#include <unordered_map>
#include <vector>
#include <string>
#include <numeric>

struct Aggregation {
    int count = 0;
    double total = 0.0;
    double max_val = 0.0;
};

std::unordered_map<std::string, Aggregation> group_by_category(const std::vector<Record>& records) {
    std::unordered_map<std::string, Aggregation> grouped;
    for (const auto& r : records) {
        auto& agg = grouped[r.category];
        agg.count++;
        agg.total += r.price;
        agg.max_val = std::max(agg.max_val, r.price);
    }
    return grouped;
}
```

## 6. Handling Quotes & Escalated CSV Fields
```cpp
#include <string>
#include <vector>

// Parse a single line containing quoted commas (e.g. "Hello, World",10)
std::vector<std::string> parse_quoted_line(const std::string& line) {
    std::vector<std::string> row;
    std::string cell;
    bool in_quotes = false;

    for (char c : line) {
        if (c == '"') {
            in_quotes = !in_quotes; // Toggle quote state
        } else if (c == ',' && !in_quotes) {
            row.push_back(cell);
            cell.clear();
        } else {
            cell += c;
        }
    }
    row.push_back(cell);
    return row;
}
```