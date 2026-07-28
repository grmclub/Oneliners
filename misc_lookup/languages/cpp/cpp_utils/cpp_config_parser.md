Here is the Quick Decision Matrix in Markdown:

## 1. Quick Decision Matrix

| Format | Best Modern Library | Header-Only? | Dependencies | Primary Use Case |
| --- | --- | --- | --- | --- |
| **JSON** | `nlohmann/json` | Yes | None | Web APIs, structured data, modern standards |
| **YAML** | `yaml-cpp` | No | CMake build | Human-readable configs, complex structures |
| **INI** | `mini-ini` / `inih` | Yes (mini-ini) | None | Simple key-value pairs, legacy/lightweight |
| **TOML** | `toml++` | Yes | None (C++17+) | User-friendly, type-safe app configs |

```
2. JSON: nlohmann/json
(Requires C++11 or higher)


#include <nlohmann/json.hpp>
#include <fstream>
#include <iostream>

using json = nlohmann::json;

void parse_json_config(const std::string& filepath) {
    std::ifstream file(filepath);
    json config = json::parse(file);

    // Reading values with defaults
    std::string host = config.value("host", "localhost");
    int port = config.value("port", 8080);
    bool debug = config["settings"].value("debug", false);

    // Accessing nested arrays
    for (const auto& server : config["backup_servers"]) {
        std::cout << server.get<std::string>() << "\n";
    }
}

3. TOML: toml++
(Requires C++17 or higher)


#include <toml++/toml.hpp>
#include <iostream>

void parse_toml_config(const std::string& filepath) {
    auto config = toml::parse_file(filepath);

    // Using optional access with defaults via value_or
    std::string_view title = config["title"].value_or("Untitled");
    std::string db_host   = config["database"]["ip"].value_or("127.0.0.1");
    int64_t db_port       = config["database"]["port"].value_or(5432);

    // Iterating over arrays
    if (auto ports = config["ports"].as_array()) {
        for (auto&& p : *ports) {
            std::cout << p.value_or(0) << "\n";
        }
    }
}

4. YAML: yaml-cpp

#include <yaml-cpp/yaml.h>
#include <iostream>

void parse_yaml_config(const std::string& filepath) {
    YAML::Node config = YAML::LoadFile(filepath);

    // Safe access with fallback checking
    std::string name = config["name"] ? config["name"].as<std::string>() : "default";
    int threads     = config["threads"] ? config["threads"].as<int>() : 4;

    // Reading nested maps
    if (config["server"] && config["server"]["port"]) {
        int port = config["server"]["port"].as<int>();
    }

    // Sequence iteration
    const YAML::Node& items = config["items"];
    for (std::size_t i = 0; i < items.size(); ++i) {
        std::cout << items[i].as<std::string>() << "\n";
    }
}

5. INI: Lightweight Parser (inih C++ Wrapper)

#include <INIReader.h>
#include <iostream>

void parse_ini_config(const std::string& filepath) {
    INIReader reader(filepath);

    if (reader.ParseError() < 0) {
        std::cerr << "Can't load INI file\n";
        return;
    }

    // Access syntax: Get(section, name, default_value)
    std::string host = reader.Get("protocol", "host", "localhost");
    int port        = reader.GetInteger("protocol", "port", 80);
    bool active     = reader.GetBoolean("user", "active", true);
}

6. Zero-Dependency Key=Value / INI Helper

Use when you cannot use third-party libraries.

#include <fstream>
#include <sstream>
#include <unordered_map>

std::unordered_map<std::string, std::string> parse_simple_env(const std::string& filepath) {
    std::unordered_map<std::string, std::string> config;
    std::ifstream file(filepath);
    std::string line;

    while (std::getline(file, line)) {
        // Trim leading spaces / ignore empty lines and comments
        auto first = line.find_first_not_of(" \t");
        if (first == std::string::npos || line[first] == '#' || line[first] == ';') 
            continue;

        std::istringstream is_line(line);
        std::string key, value;
        if (std::getline(is_line, key, '=') && std::getline(is_line, value)) {
            config[key] = value;
        }
    }
    return config;
}

```