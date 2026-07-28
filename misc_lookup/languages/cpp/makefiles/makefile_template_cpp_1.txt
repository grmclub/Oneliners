Here is a modern, production-ready Makefile template tailored specifically for C++20/C++23 development. It includes auto-dependency generation (so changing a .hpp file automatically triggers a recompile of dependent modules), optimized directory handling, and standard safety flags. 

Save this file exactly as Makefile (with a capital M and no extension) in your project root.

# ==============================================================================
# MODERN C++20 / C++23 AUTOMATION MAKEFILE TEMPLATE
# ==============================================================================

# 1. Compiler and Standard Selection
CXX      := g++
# For C++20, use: -std=c++20 | For C++23, use: -std=c++23
CXXFLAGS := -std=c++23 -O2 -Wall -Wextra -Werror -pedantic

# Dependency Tracking Flags (Automatically detects changes in header .hpp files)
DEPFLAGS = -MT $@ -MMD -MP -MF $(OBJ_DIR)/$*.d

# 2. Project Directory Layout
SRC_DIR  := src
OBJ_DIR  := obj
BIN_DIR  := bin

# 3. Target Executable Name
TARGET   := $(BIN_DIR)/my_cpp_program

# 4. Source, Object, and Dependency File Mapping
SRCS     := $(wildcard $(SRC_DIR)/*.cpp)
OBJS     := $(SRCS:$(SRC_DIR)/%.cpp=$(OBJ_DIR)/%.o)
DEPS     := $(SRCS:$(SRC_DIR)/%.cpp=$(OBJ_DIR)/%.d)

# ==============================================================================
# Build Rules & Targets
# ==============================================================================

# Default Rule
all: $(TARGET)

# Link Rule: Combines object files into the final executable
$(TARGET): $(OBJS) | $(BIN_DIR)
	@echo "Linking binary target: $@"
	$(CXX) $(CXXFLAGS) $^ -o $@
	@echo "Build successful! Run your app via: ./$@"

# Compilation Rule: Transforms source files into object modules
$(OBJ_DIR)/%.o: $(SRC_DIR)/%.cpp $(OBJ_DIR)/%.d | $(OBJ_DIR)
	@echo "Compiling C++ source module: $<"
	$(CXX) $(CXXFLAGS) $(DEPFLAGS) -c $< -o $@

# Safe Directory Generation Target Rules
$(BIN_DIR) $(OBJ_DIR):
	@mkdir -p $@

# Workspace Clean Rule
clean:
	@echo "Sweeping away compiled artifacts and dependency trees..."
	rm -rf $(OBJ_DIR) $(BIN_DIR)
	@echo "Workspace cleaned."

# Execution Run Target Rule
run: $(TARGET)
	@./$(TARGET)

# Include automatically generated dependency tracking rules safely
$(DEPS):
-include $(wildcard $(DEPS))

# Phony Targets Declaration
.PHONY: all clean run

===================================================

📂 Recommended C++ Project Folder StructureTo use this Makefile right out of the box, layout your project repository like this:textmy_modern_cpp_project/

├── Makefile
└── src/
    ├── main.cpp
    ├── engine.cpp
    └── engine.hpp

===================================================
🛠️ Execution Commands
Open your system terminal inside your project's root folder and run these directives:
    • make: Compiles your modified source code files and builds the optimized binary inside the /bin folder.
    • make run: Compiles any fresh changes and executes your program in one step.
    • make clean: Completely deletes the tracking objects, binary output folders, and dependency charts for a fresh re-build.

===================================================






