# **CMake From Zero to Hero: Complete 20-Part Course**

## **PART-1: CMake Fundamentals & Your First CMake Project**

### **Topic: Introduction to CMake and Basic Project Setup**

### **What is CMake?**
CMake is a **build system generator** that creates native build files for your platform (Makefiles for Linux/macOS, Visual Studio projects for Windows, etc.). It's **not** a compiler or a build system itself, but rather a tool that generates files for build systems.

### **Why Use CMake?**
- **Cross-platform**: Write once, build anywhere
- **Manage complex projects**: Handle dependencies, libraries, and configurations
- **Modern build practices**: Support for testing, packaging, and installation
- **Widely adopted**: Used by major projects like KDE, Qt, and many open-source libraries

### **Installation**
#### **Linux (Ubuntu/Debian)**
```bash
sudo apt-get update
sudo apt-get install cmake build-essential
```

#### **macOS**
```bash
brew install cmake
```

#### **Windows**
1. Download installer from [cmake.org](https://cmake.org/download/)
2. Or use Chocolatey: `choco install cmake`
3. Add CMake to your PATH during installation

Verify installation:
```bash
cmake --version
```

### **CMake Basic Concepts**

#### **1. CMakeLists.txt**
This is the **configuration file** that tells CMake what to do. Every CMake project must have at least one `CMakeLists.txt` file in the root directory.

#### **2. Source and Build Separation**
**Best Practice**: Never build in your source directory. Always create a separate build directory:
```
project/
├── CMakeLists.txt    # Source directory
├── src/
└── build/           # Build directory (created by you)
```

### **Your First CMake Project**

Let's create a minimal working example:

**Step 1: Create Project Structure**
```bash
mkdir my_first_cmake_project
cd my_first_cmake_project
```

**Step 2: Create CMakeLists.txt**
```cmake
# CMakeLists.txt
cmake_minimum_required(VERSION 3.10)  # Minimum CMake version
project(MyFirstProject VERSION 1.0.0) # Project name and version

# Print a message (for debugging/learning)
message(STATUS "Configuring MyFirstProject version ${MyFirstProject_VERSION}")

# Add an executable target
add_executable(my_app main.cpp)
```

**Step 3: Create a Simple C++ Source File**
```cpp
// main.cpp
#include <iostream>

int main() {
    std::cout << "Hello, CMake World!" << std::endl;
    std::cout << "This is my first CMake project!" << std::endl;
    return 0;
}
```

**Step 4: Build the Project**
```bash
# Create and enter build directory
mkdir build
cd build

# Configure the project (generates build files)
cmake ..

# Build the project
cmake --build .

# On Linux/macOS, you can also use: make
# On Windows with Visual Studio: cmake --build . --config Release
```

**Step 5: Run the Executable**
```bash
# Linux/macOS
./my_app

# Windows
./Debug/my_app.exe  # or ./Release/my_app.exe
```

### **Understanding the CMakeLists.txt Line by Line**

```cmake
# 1. Minimum Version Requirement
cmake_minimum_required(VERSION 3.10)
# Always put this first. Specifies the minimum CMake version.
# Modern projects should use at least 3.10 for good feature support.

# 2. Project Definition
project(MyFirstProject        # Project name
        VERSION 1.0.0         # Project version (optional but recommended)
        DESCRIPTION "My first CMake project"  # Description (optional)
        LANGUAGES CXX)        # Programming languages (CXX=C++, C=C)

# This command creates several variables automatically:
# ${PROJECT_NAME} = "MyFirstProject"
# ${PROJECT_VERSION} = "1.0.0"

# 3. Set C++ Standard (Important!)
set(CMAKE_CXX_STANDARD 11)    # Use C++11 standard
set(CMAKE_CXX_STANDARD_REQUIRED ON)  # Require the standard
set(CMAKE_CXX_EXTENSIONS OFF) # Disable compiler-specific extensions

# 4. Add Executable
add_executable(my_app        # Target name
               main.cpp)     # Source files
```

### **Improved First Project with Best Practices**

Here's a better version with more complete practices:

```cmake
# CMakeLists.txt - Improved Version
cmake_minimum_required(VERSION 3.10)

# Project definition with all common fields
project(MyFirstProject
    VERSION 1.0.0
    DESCRIPTION "A simple CMake example project"
    HOMEPAGE_URL "https://github.com/yourusername/myproject"
    LANGUAGES CXX)

# Set policies to ensure modern behavior
cmake_policy(SET CMP0077 NEW)  # Prefer setting options via targets

# Configure project-wide settings
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Enable better warnings if using GCC or Clang
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Print configuration info
message(STATUS "Project: ${PROJECT_NAME}")
message(STATUS "Version: ${PROJECT_VERSION}")
message(STATUS "Source directory: ${CMAKE_SOURCE_DIR}")
message(STATUS "Build directory: ${CMAKE_BINARY_DIR}")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID}")

# Add the executable
add_executable(${PROJECT_NAME} 
    src/main.cpp
    src/helper.cpp  # Example: additional source file
)

# Set target properties
set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)
```

### **Project Structure with Multiple Files**

Create a more organized structure:

```
my_project/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── helper.cpp
├── include/
│   └── helper.h
└── build/          # You'll create this
```

**helper.h:**
```cpp
// include/helper.h
#ifndef HELPER_H
#define HELPER_H

#include <string>

void print_message(const std::string& msg);
int add_numbers(int a, int b);

#endif
```

**helper.cpp:**
```cpp
// src/helper.cpp
#include "helper.h"
#include <iostream>

void print_message(const std::string& msg) {
    std::cout << "Message: " << msg << std::endl;
}

int add_numbers(int a, int b) {
    return a + b;
}
```

**main.cpp (updated):**
```cpp
// src/main.cpp
#include "helper.h"
#include <iostream>

int main() {
    std::cout << "Hello from structured CMake project!" << std::endl;
    print_message("This is a helper function");
    std::cout << "2 + 3 = " << add_numbers(2, 3) << std::endl;
    return 0;
}
```

### **CMakeLists.txt for Structured Project**

```cmake
cmake_minimum_required(VERSION 3.10)
project(StructuredProject VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Add include directory
include_directories(${CMAKE_SOURCE_DIR}/include)

# Create executable with multiple source files
add_executable(${PROJECT_NAME}
    src/main.cpp
    src/helper.cpp
)

# Alternative: Set include directories per target (MODERN APPROACH)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_SOURCE_DIR}/include)
```

### **Build Commands Cheat Sheet**

```bash
# Basic build sequence
mkdir build
cd build
cmake ..                    # Configure
cmake --build .            # Build

# With specific generator
cmake -G "Unix Makefiles" ..           # Linux/macOS
cmake -G "Visual Studio 16 2019" ..    # Windows Visual Studio 2019
cmake -G "Ninja" ..                    # Ninja build system (faster)

# Build specific configuration (Windows)
cmake --build . --config Release
cmake --build . --config Debug

# Clean build
rm -rf build/*      # Linux/macOS
# OR from build directory:
cmake --build . --target clean

# See all targets
cmake --build . --target help

# Verbose build output
cmake --build . --verbose
# OR for Makefiles:
make VERBOSE=1
```

### **Common Issues and Solutions**

#### **Issue 1: "No CMAKE_CXX_COMPILER found"**
**Solution**: Install a C++ compiler
- Linux: `sudo apt-get install g++`
- macOS: Install Xcode Command Line Tools
- Windows: Install Visual Studio with C++ support

#### **Issue 2: Old CMake version**
**Solution**: Update CMake or specify older version
```cmake
cmake_minimum_required(VERSION 3.5)  # Lower if needed
```

#### **Issue 3: Source files not found**
**Solution**: Check paths in `add_executable()`. Use relative paths from CMakeLists.txt.

### **Beginner Tips**
1. **Always** create a separate build directory
2. Start with simple projects and gradually add complexity
3. Use `message()` for debugging
4. Comment your CMakeLists.txt files
5. Keep CMake version requirement as low as possible for compatibility

### **Advanced Tips (Preview)**
1. Use `target_include_directories()` instead of `include_directories()` (more on this in Part 3)
2. Consider using Ninja generator for faster builds: `cmake -G Ninja ..`
3. Use `CMAKE_BUILD_TYPE` to control build type: `cmake -DCMAKE_BUILD_TYPE=Debug ..`

### **Practice Exercise**
Create a project with:
1. Two source files in `src/` directory
2. One header file in `include/` directory
3. Use at least one function call between files
4. Build and run successfully

### **Next Steps**
In **PART-2**, we'll cover:
- Variables and cache variables in CMake
- Controlling build types (Debug/Release)
- Adding compiler flags and options
- Basic project configuration

**To continue, please reply with "Next" or "PART-2" to receive the next part of the course.**




# **PART-2: Variables, Build Types, and Compiler Options**

### **Topic: CMake Variables, Build Configurations, and Customizing the Build Process**

### **Understanding CMake Variables**

CMake has several types of variables, each with different scopes and lifetimes:

#### **1. Normal Variables**
Created using `set()` and exist only in the current scope (directory or function).

```cmake
# Basic variable assignment
set(MY_VARIABLE "Hello World")
set(NUMBER_VARIABLE 42)
set(LIST_VARIABLE item1 item2 item3)

# Using variables
message(STATUS "Value: ${MY_VARIABLE}")
message(STATUS "Number: ${NUMBER_VARIABLE}")
message(STATUS "List: ${LIST_VARIABLE}")  # Prints as: item1item2item3

# Proper way to print lists
foreach(item IN LISTS LIST_VARIABLE)
    message(STATUS "Item: ${item}")
endforeach()
```

#### **2. Environment Variables**
Access system environment variables:

```cmake
# Get environment variable
set(HOME_DIR $ENV{HOME})
message(STATUS "Home directory: ${HOME_DIR}")

# Set environment variable for CMake process
set(ENV{PATH} "$ENV{PATH}:${CMAKE_SOURCE_DIR}/bin")
```

#### **3. Cache Variables**
Persistent variables stored in CMakeCache.txt. Users can modify them via command line or GUI.

```cmake
# Syntax: set(<variable> <value> CACHE <type> <docstring> [FORCE])
set(MY_CACHE_VAR "default_value" CACHE STRING "Description of this variable")

# Common types: STRING, BOOL, FILEPATH, PATH
set(USE_FEATURE_X OFF CACHE BOOL "Enable feature X")
set(INSTALL_PREFIX "/usr/local" CACHE PATH "Installation directory")
```

**Modify cache variables from command line:**
```bash
cmake -DMY_CACHE_VAR="new_value" ..
cmake -DUSE_FEATURE_X=ON ..
cmake -DCMAKE_INSTALL_PREFIX="$HOME/.local" ..
```

### **Project Configuration with Variables**

Let's create a configurable project:

**Project Structure:**
```
configurable_project/
├── CMakeLists.txt
├── src/
│   └── main.cpp
└── build/
```

**CMakeLists.txt:**
```cmake
cmake_minimum_required(VERSION 3.10)
project(ConfigurableProject VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard with cache variable
set(CMAKE_CXX_STANDARD 11 CACHE STRING "C++ standard to use")
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Custom cache variables for project configuration
option(ENABLE_LOGGING "Enable detailed logging" ON)
option(ENABLE_TESTS "Build tests" OFF)
set(MAX_USERS 100 CACHE STRING "Maximum number of users")

# Boolean variables show up as checkboxes in cmake-gui
option(BUILD_SHARED_LIBS "Build shared libraries instead of static" OFF)

# Print configuration
message(STATUS "=== Project Configuration ===")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Enable Logging: ${ENABLE_LOGGING}")
message(STATUS "Enable Tests: ${ENABLE_TESTS}")
message(STATUS "Max Users: ${MAX_USERS}")
message(STATUS "Build Shared Libs: ${BUILD_SHARED_LIBS}")

# Add executable
add_executable(${PROJECT_NAME} src/main.cpp)

# Use variables to control compilation
if(ENABLE_LOGGING)
    target_compile_definitions(${PROJECT_NAME} PRIVATE ENABLE_LOGGING=1)
    message(STATUS "Logging enabled - adding define ENABLE_LOGGING")
endif()

# Add a definition based on the variable
target_compile_definitions(${PROJECT_NAME} PRIVATE "MAX_USERS=${MAX_USERS}")
```

**src/main.cpp:**
```cpp
#include <iostream>

#ifdef ENABLE_LOGGING
    #define LOG(msg) std::cout << "[LOG] " << msg << std::endl
#else
    #define LOG(msg) // Nothing
#endif

int main() {
    LOG("Application starting...");
    
    std::cout << "Configurable Project" << std::endl;
    std::cout << "Maximum users: " << MAX_USERS << std::endl;
    
    #ifdef ENABLE_LOGGING
    std::cout << "Logging is enabled" << std::endl;
    #endif
    
    LOG("Application exiting...");
    return 0;
}
```

### **Build Types (Debug, Release, etc.)**

CMake supports different build types that affect compiler optimizations and debugging information:

#### **Common Build Types:**
- **Debug**: No optimizations, full debug symbols (`-g`)
- **Release**: Full optimizations, no debug symbols (`-O3`)
- **RelWithDebInfo**: Optimizations with debug symbols (`-O2 -g`)
- **MinSizeRel**: Optimize for smallest binary size

#### **Setting Build Type:**

**Method 1: Command line** (Recommended)
```bash
# Configure with specific build type
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake -DCMAKE_BUILD_TYPE=Release ..
```

**Method 2: In CMakeLists.txt** (Only affects if not set on command line)
```cmake
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Build type" FORCE)
endif()
```

#### **Complete Example with Build Types:**

```cmake
cmake_minimum_required(VERSION 3.10)
project(BuildTypesDemo VERSION 1.0.0 LANGUAGES CXX)

# Set default build type if not specified
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING 
        "Choose the type of build (Debug, Release, RelWithDebInfo, MinSizeRel)"
        FORCE
    )
endif()

message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

# Different compiler flags for different build types
set(CMAKE_CXX_FLAGS_DEBUG "-g -O0 -Wall -Wextra -DDEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "-O2 -g -DNDEBUG")
set(CMAKE_CXX_FLAGS_MINSIZEREL "-Os -DNDEBUG")

# Modern approach: Use generator expressions (more on this later)
add_executable(${PROJECT_NAME} src/main.cpp)

# Add flags specific to build type using generator expressions
target_compile_options(${PROJECT_NAME} PRIVATE 
    "$<$<CONFIG:Debug>:-Og>"
    "$<$<CONFIG:Release>:-O3>"
)
```

### **Compiler Options and Flags**

#### **Setting Global Compiler Flags:**
```cmake
# Add flags for all targets (C and C++)
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")

# Or append safely
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pedantic")
```

#### **Modern Approach: Per-Target Flags** (Recommended)
```cmake
add_executable(myapp src/main.cpp)

# Add compile options to specific target
target_compile_options(myapp PRIVATE -Wall -Wextra)

# Add definitions (like -D flags)
target_compile_definitions(myapp PRIVATE 
    MY_DEFINITION=1
    "VERSION=\\\"${PROJECT_VERSION}\\\""
)

# Include directories for this target
target_include_directories(myapp PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)
```

### **Complete Practical Example: Math Library Project**

**Project Structure:**
```
math_project/
├── CMakeLists.txt
├── include/
│   └── math_utils.h
├── src/
│   ├── main.cpp
│   └── math_utils.cpp
└── build/
```

**include/math_utils.h:**
```cpp
#ifndef MATH_UTILS_H
#define MATH_UTILS_H

// Function declarations
double add(double a, double b);
double subtract(double a, double b);
double multiply(double a, double b);
double divide(double a, double b);
double power(double base, double exponent);

#endif
```

**src/math_utils.cpp:**
```cpp
#include "math_utils.h"
#include <cmath>
#include <stdexcept>

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    if (b == 0.0) {
        throw std::runtime_error("Division by zero");
    }
    return a / b;
}

double power(double base, double exponent) {
    return std::pow(base, exponent);
}
```

**src/main.cpp:**
```cpp
#include "math_utils.h"
#include <iostream>
#include <iomanip>

int main() {
    std::cout << std::fixed << std::setprecision(2);
    
    double a = 10.5;
    double b = 3.2;
    
    std::cout << "Math Operations Demo" << std::endl;
    std::cout << "a = " << a << ", b = " << b << std::endl << std::endl;
    
    std::cout << "Addition: " << a << " + " << b << " = " << add(a, b) << std::endl;
    std::cout << "Subtraction: " << a << " - " << b << " = " << subtract(a, b) << std::endl;
    std::cout << "Multiplication: " << a << " * " << b << " = " << multiply(a, b) << std::endl;
    
    try {
        std::cout << "Division: " << a << " / " << b << " = " << divide(a, b) << std::endl;
        std::cout << "Power: " << a << " ^ " << b << " = " << power(a, b) << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
    
    return 0;
}
```

**CMakeLists.txt:**
```cmake
cmake_minimum_required(VERSION 3.15)
project(MathProject VERSION 1.0.0 LANGUAGES CXX)

# Configuration options
option(ENABLE_EXTRA_MATH "Enable extra mathematical functions" ON)
option(USE_FAST_MATH "Use fast math optimizations" OFF)
set(MATH_PRECISION "double" CACHE STRING "Precision type (float, double, long double)")

# Build type configuration
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

message(STATUS "========================================")
message(STATUS "Math Project Configuration")
message(STATUS "Project: ${PROJECT_NAME}")
message(STATUS "Version: ${PROJECT_VERSION}")
message(STATUS "Build Type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Math Precision: ${MATH_PRECISION}")
message(STATUS "Extra Math: ${ENABLE_EXTRA_MATH}")
message(STATUS "Fast Math: ${USE_FAST_MATH}")
message(STATUS "========================================")

# C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Create executable
add_executable(${PROJECT_NAME}
    src/main.cpp
    src/math_utils.cpp
)

# Include directories
target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# Compiler definitions based on configuration
target_compile_definitions(${PROJECT_NAME} PRIVATE
    "MATH_PRECISION_${MATH_PRECISION}"
    $<$<BOOL:${ENABLE_EXTRA_MATH}>:ENABLE_EXTRA_MATH>
)

# Compiler options
target_compile_options(${PROJECT_NAME} PRIVATE
    -Wall
    -Wextra
    -Wpedantic
)

# Add fast math if enabled
if(USE_FAST_MATH)
    target_compile_options(${PROJECT_NAME} PRIVATE
        -ffast-math
    )
    message(STATUS "Fast math optimizations enabled")
endif()

# Different optimizations per build type using generator expressions
target_compile_options(${PROJECT_NAME} PRIVATE
    # Debug: minimal optimizations, all warnings
    "$<$<CONFIG:Debug>:-O0;-g3;-DDEBUG>"
    # Release: maximum optimizations
    "$<$<CONFIG:Release>:-O3;-DNDEBUG>"
    # Release with debug info
    "$<$<CONFIG:RelWithDebInfo>:-O2;-g;-DNDEBUG>"
    # Minimum size
    "$<$<CONFIG:MinSizeRel>:-Os;-DNDEBUG>"
)
```

### **Building and Testing the Project**

```bash
# Create build directory
mkdir build && cd build

# Configure with Debug build
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake --build .
./MathProject

# Clean and reconfigure with Release build
rm -rf *
cmake -DCMAKE_BUILD_TYPE=Release -DUSE_FAST_MATH=ON ..
cmake --build .
./MathProject

# Try different configurations
rm -rf *
cmake -DMATH_PRECISION="float" -DENABLE_EXTRA_MATH=OFF ..
cmake --build .
./MathProject
```

### **Useful CMake Variables Reference**

#### **System Information Variables:**
```cmake
CMAKE_SYSTEM_NAME          # Linux, Windows, Darwin (macOS)
CMAKE_SYSTEM_VERSION
CMAKE_SYSTEM_PROCESSOR    # x86_64, arm, etc.

CMAKE_CXX_COMPILER_ID     # GNU, Clang, MSVC, AppleClang
CMAKE_CXX_COMPILER_VERSION
CMAKE_CXX_COMPILER        # Full path to compiler
```

#### **Build System Variables:**
```cmake
CMAKE_BUILD_TYPE          # Debug, Release, etc.
CMAKE_CONFIGURATION_TYPES # Multi-config generators (VS, Xcode)

CMAKE_CXX_FLAGS           # C++ compiler flags
CMAKE_CXX_FLAGS_DEBUG     # Debug-specific flags
CMAKE_CXX_FLAGS_RELEASE   # Release-specific flags

CMAKE_EXECUTABLE_SUFFIX   # .exe on Windows, empty on Unix
CMAKE_SHARED_LIBRARY_PREFIX   # lib on Unix, empty on Windows
CMAKE_SHARED_LIBRARY_SUFFIX   # .so on Linux, .dylib on macOS, .dll on Windows
```

#### **Path Variables:**
```cmake
CMAKE_SOURCE_DIR          # Top-level source directory
CMAKE_BINARY_DIR          # Top-level build directory
CMAKE_CURRENT_SOURCE_DIR  # Current source directory
CMAKE_CURRENT_BINARY_DIR  # Current build directory
CMAKE_CURRENT_LIST_DIR    # Directory of current CMakeLists.txt
PROJECT_SOURCE_DIR        # Source dir of most recent project()
PROJECT_BINARY_DIR        # Build dir of most recent project()
```

### **Best Practices**

1. **Use target-specific commands** (`target_compile_options`, `target_include_directories`, `target_compile_definitions`) instead of global commands
2. **Set cache variables with sensible defaults**
3. **Use `option()` for boolean switches** - they show up nicely in CMake GUI
4. **Always check for undefined variables** before using them:
   ```cmake
   if(DEFINED MY_VARIABLE)
       # Use it
   endif()
   ```
5. **Use message() for debugging** configuration:
   ```cmake
   message(STATUS "Variable value: ${MY_VAR}")
   message(DEBUG "Debug info only shown with --log-level=DEBUG")
   ```

### **Common Pitfalls and Solutions**

**Pitfall 1:** Variables don't persist between runs
```cmake
# WRONG - normal variable won't persist
set(MY_SETTING "value")

# CORRECT - use cache variable
set(MY_SETTING "default" CACHE STRING "My setting")
```

**Pitfall 2:** Case sensitivity
```cmake
# CMake variables are CASE-SENSITIVE
set(MyVar "value")  # ${MyVar} works, ${MYVAR} doesn't
```

**Pitfall 3:** String vs list
```cmake
# Strings
set(MY_STR "a b c")        # One string: "a b c"

# Lists
set(MY_LIST a b c)         # Three items: a, b, c
set(MY_LIST "a;b;c")       # Also three items
```

### **Exercise: Create a Configurable Application**

Create a project that:
1. Has a cache variable for application name
2. Has an option to enable/disable verbose output
3. Uses different optimization levels for Debug and Release builds
4. Prints configuration summary at configure time
5. Sets compiler warnings appropriately for the compiler being used

### **Next Steps Preview**
In **PART-3**, we'll cover:
- Creating and using libraries (static and shared)
- Linking libraries to executables
- Modern CMake target-based approach vs old-style
- Interface libraries and usage requirements

**To continue, please reply with "Next" or "PART-3" to receive the next part of the course.**



# **PART-3: Creating and Using Libraries**

### **Topic: Building Static and Shared Libraries, Linking, and Modern Target-Based Approach**

### **Understanding Libraries in CMake**

Libraries allow you to organize code into reusable components. CMake supports:
- **Static Libraries** (`.a` on Linux/macOS, `.lib` on Windows): Linked at compile time, code becomes part of executable
- **Shared Libraries** (`.so` on Linux, `.dylib` on macOS, `.dll` on Windows): Linked at runtime, separate files

### **Project Structure for Library Example**

```
library_project/
├── CMakeLists.txt
├── app/
│   ├── CMakeLists.txt
│   └── main.cpp
├── mathlib/
│   ├── CMakeLists.txt
│   ├── include/
│   │   └── mathlib.h
│   └── src/
│       └── mathlib.cpp
└── stringlib/
    ├── CMakeLists.txt
    ├── include/
    │   └── stringlib.h
    └── src/
        └── stringlib.cpp
```

### **Creating a Static Library**

**mathlib/include/mathlib.h:**
```cpp
#ifndef MATHLIB_H
#define MATHLIB_H

namespace mathlib {
    // Basic operations
    int add(int a, int b);
    int subtract(int a, int b);
    int multiply(int a, int b);
    double divide(int a, int b);
    
    // Advanced operations
    double power(double base, double exp);
    double square_root(double value);
    
    // Statistics
    double average(const double* values, int count);
    double standard_deviation(const double* values, int count);
}

#endif
```

**mathlib/src/mathlib.cpp:**
```cpp
#include "mathlib.h"
#include <cmath>
#include <stdexcept>

namespace mathlib {
    int add(int a, int b) {
        return a + b;
    }
    
    int subtract(int a, int b) {
        return a - b;
    }
    
    int multiply(int a, int b) {
        return a * b;
    }
    
    double divide(int a, int b) {
        if (b == 0) {
            throw std::runtime_error("Division by zero");
        }
        return static_cast<double>(a) / b;
    }
    
    double power(double base, double exp) {
        return std::pow(base, exp);
    }
    
    double square_root(double value) {
        if (value < 0) {
            throw std::runtime_error("Cannot compute square root of negative number");
        }
        return std::sqrt(value);
    }
    
    double average(const double* values, int count) {
        if (count <= 0) return 0.0;
        
        double sum = 0.0;
        for (int i = 0; i < count; ++i) {
            sum += values[i];
        }
        return sum / count;
    }
    
    double standard_deviation(const double* values, int count) {
        if (count <= 1) return 0.0;
        
        double avg = average(values, count);
        double sum_squares = 0.0;
        
        for (int i = 0; i < count; ++i) {
            double diff = values[i] - avg;
            sum_squares += diff * diff;
        }
        
        return std::sqrt(sum_squares / (count - 1));
    }
}
```

**mathlib/CMakeLists.txt:**
```cmake
# Math Library CMakeLists.txt
cmake_minimum_required(VERSION 3.10)

# Create the math library
add_library(mathlib STATIC
    src/mathlib.cpp
)

# Set target properties
set_target_properties(mathlib PROPERTIES
    VERSION 1.0.0
    SOVERSION 1
    OUTPUT_NAME "math"  # Library will be named libmath.a (or math.lib on Windows)
)

# Include directories for this target
target_include_directories(mathlib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Compiler options for this library
target_compile_options(mathlib PRIVATE
    -Wall
    -Wextra
)

# Set C++ standard
set_target_properties(mathlib PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

# Export compile definitions if needed
target_compile_definitions(mathlib PUBLIC
    MATHLIB_EXPORTS
)

# Create an alias for easier usage
add_library(Math::math ALIAS mathlib)
```

### **Creating a Shared Library**

**stringlib/include/stringlib.h:**
```cpp
#ifndef STRINGLIB_H
#define STRINGLIB_H

#include <string>
#include <vector>

#ifdef _WIN32
    #ifdef STRINGLIB_EXPORTS
        #define STRINGLIB_API __declspec(dllexport)
    #else
        #define STRINGLIB_API __declspec(dllimport)
    #endif
#else
    #define STRINGLIB_API
#endif

namespace stringlib {
    STRINGLIB_API std::string to_upper(const std::string& str);
    STRINGLIB_API std::string to_lower(const std::string& str);
    STRINGLIB_API std::string reverse(const std::string& str);
    STRINGLIB_API bool starts_with(const std::string& str, const std::string& prefix);
    STRINGLIB_API bool ends_with(const std::string& str, const std::string& suffix);
    STRINGLIB_API std::vector<std::string> split(const std::string& str, char delimiter);
    STRINGLIB_API std::string join(const std::vector<std::string>& parts, const std::string& delimiter);
    STRINGLIB_API std::string trim(const std::string& str);
}

#endif
```

**stringlib/src/stringlib.cpp:**
```cpp
#include "stringlib.h"
#include <algorithm>
#include <cctype>
#include <sstream>

namespace stringlib {
    std::string to_upper(const std::string& str) {
        std::string result = str;
        std::transform(result.begin(), result.end(), result.begin(),
                      [](unsigned char c) { return std::toupper(c); });
        return result;
    }
    
    std::string to_lower(const std::string& str) {
        std::string result = str;
        std::transform(result.begin(), result.end(), result.begin(),
                      [](unsigned char c) { return std::tolower(c); });
        return result;
    }
    
    std::string reverse(const std::string& str) {
        std::string result = str;
        std::reverse(result.begin(), result.end());
        return result;
    }
    
    bool starts_with(const std::string& str, const std::string& prefix) {
        if (prefix.size() > str.size()) return false;
        return std::equal(prefix.begin(), prefix.end(), str.begin());
    }
    
    bool ends_with(const std::string& str, const std::string& suffix) {
        if (suffix.size() > str.size()) return false;
        return std::equal(suffix.rbegin(), suffix.rend(), str.rbegin());
    }
    
    std::vector<std::string> split(const std::string& str, char delimiter) {
        std::vector<std::string> tokens;
        std::stringstream ss(str);
        std::string token;
        
        while (std::getline(ss, token, delimiter)) {
            tokens.push_back(token);
        }
        
        return tokens;
    }
    
    std::string join(const std::vector<std::string>& parts, const std::string& delimiter) {
        std::string result;
        for (size_t i = 0; i < parts.size(); ++i) {
            if (i > 0) {
                result += delimiter;
            }
            result += parts[i];
        }
        return result;
    }
    
    std::string trim(const std::string& str) {
        auto start = str.begin();
        auto end = str.end();
        
        // Trim left
        while (start != end && std::isspace(static_cast<unsigned char>(*start))) {
            ++start;
        }
        
        // Trim right
        do {
            --end;
        } while (std::distance(start, end) > 0 && std::isspace(static_cast<unsigned char>(*end)));
        
        return std::string(start, end + 1);
    }
}
```

**stringlib/CMakeLists.txt:**
```cmake
# String Library CMakeLists.txt
cmake_minimum_required(VERSION 3.10)

# Create shared library
add_library(stringlib SHARED
    src/stringlib.cpp
)

# Set target properties
set_target_properties(stringlib PROPERTIES
    VERSION 1.0.0
    SOVERSION 1
    OUTPUT_NAME "string"  # libstring.so / string.dll
    # Windows specific: add .def file or use declspec
    WINDOWS_EXPORT_ALL_SYMBOLS ON  # Alternative to manual __declspec(dllexport)
)

# Include directories
target_include_directories(stringlib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Compiler definitions - important for shared libraries on Windows
target_compile_definitions(stringlib PRIVATE
    STRINGLIB_EXPORTS
)

# Compiler options
target_compile_options(stringlib PRIVATE
    -Wall
    -Wextra
    -fPIC  # Position Independent Code (important for shared libs on Unix)
)

# Set C++ standard
set_target_properties(stringlib PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

# Create alias
add_library(String::string ALIAS stringlib)
```

### **Main Application**

**app/main.cpp:**
```cpp
#include "mathlib.h"
#include "stringlib.h"
#include <iostream>
#include <vector>
#include <iomanip>

int main() {
    std::cout << "Library Demo Application\n" << std::endl;
    
    // Math library usage
    std::cout << "=== Math Library Examples ===" << std::endl;
    std::cout << "5 + 3 = " << mathlib::add(5, 3) << std::endl;
    std::cout << "10 - 4 = " << mathlib::subtract(10, 4) << std::endl;
    std::cout << "6 * 7 = " << mathlib::multiply(6, 7) << std::endl;
    std::cout << "15 / 4 = " << mathlib::divide(15, 4) << std::endl;
    std::cout << "2^8 = " << mathlib::power(2, 8) << std::endl;
    std::cout << "sqrt(25) = " << mathlib::square_root(25) << std::endl;
    
    double values[] = {1.5, 2.5, 3.5, 4.5, 5.5};
    std::cout << "Average: " << mathlib::average(values, 5) << std::endl;
    std::cout << "Std Dev: " << mathlib::standard_deviation(values, 5) << std::endl;
    
    // String library usage
    std::cout << "\n=== String Library Examples ===" << std::endl;
    std::string text = "Hello, World!";
    std::cout << "Original: " << text << std::endl;
    std::cout << "Uppercase: " << stringlib::to_upper(text) << std::endl;
    std::cout << "Lowercase: " << stringlib::to_lower(text) << std::endl;
    std::cout << "Reversed: " << stringlib::reverse(text) << std::endl;
    std::cout << "Starts with 'Hello': " 
              << (stringlib::starts_with(text, "Hello") ? "Yes" : "No") << std::endl;
    std::cout << "Ends with 'World!': " 
              << (stringlib::ends_with(text, "World!") ? "Yes" : "No") << std::endl;
    
    std::string csv = "apple,banana,cherry,date";
    auto fruits = stringlib::split(csv, ',');
    std::cout << "Split CSV: ";
    for (const auto& fruit : fruits) {
        std::cout << fruit << " ";
    }
    std::cout << std::endl;
    
    std::string joined = stringlib::join(fruits, " | ");
    std::cout << "Joined: " << joined << std::endl;
    
    std::string padded = "   Hello World!   ";
    std::cout << "Trimmed: '" << stringlib::trim(padded) << "'" << std::endl;
    
    return 0;
}
```

**app/CMakeLists.txt:**
```cmake
# Application CMakeLists.txt
cmake_minimum_required(VERSION 3.10)

# Create executable
add_executable(LibraryDemo main.cpp)

# Link with our libraries
target_link_libraries(LibraryDemo PRIVATE
    mathlib      # Static library
    stringlib    # Shared library
)

# Include directories from libraries
# Note: These are automatically propagated because we used PUBLIC
# in the library target_include_directories()

# Additional compiler options for executable
target_compile_options(LibraryDemo PRIVATE
    -Wall
    -Wextra
)

# Set C++ standard
set_target_properties(LibraryDemo PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)
```

### **Root CMakeLists.txt**

```cmake
# Root CMakeLists.txt
cmake_minimum_required(VERSION 3.10)
project(LibraryProject VERSION 1.0.0 LANGUAGES CXX)

# Set default build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

# C++ standard for the whole project
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Global compile options (applied to all targets)
add_compile_options(-Wall -Wextra)

# Platform-specific settings
if(UNIX AND NOT APPLE)
    # Linux-specific settings
    add_compile_options(-fPIC)
endif()

if(APPLE)
    # macOS-specific settings
    add_compile_options(-fPIC)
endif()

if(WIN32)
    # Windows-specific settings
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    
    # For shared libraries, ensure we export symbols
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

# Print configuration
message(STATUS "Project: ${PROJECT_NAME}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID}")

# Add subdirectories (order matters if there are dependencies)
add_subdirectory(mathlib)
add_subdirectory(stringlib)
add_subdirectory(app)
```

### **Building the Project**

```bash
# Create build directory
mkdir build && cd build

# Configure
cmake ..

# Build all targets
cmake --build .

# Run the application
./app/LibraryDemo  # On Linux/macOS
# Or on Windows: .\app\Debug\LibraryDemo.exe

# You can also build specific targets
cmake --build . --target mathlib
cmake --build . --target stringlib
cmake --build . --target LibraryDemo
```

### **Understanding Modern vs Old-Style CMake**

#### **Old-Style (Avoid this):**
```cmake
# NOT RECOMMENDED - Old style
include_directories(${PROJECT_SOURCE_DIR}/mathlib/include)
include_directories(${PROJECT_SOURCE_DIR}/stringlib/include)

add_library(mathlib STATIC mathlib/src/mathlib.cpp)
add_library(stringlib SHARED stringlib/src/stringlib.cpp)

add_executable(myapp app/main.cpp)
target_link_libraries(myapp mathlib stringlib)
```

#### **Modern Style (Use this):**
```cmake
# RECOMMENDED - Modern style
add_library(mathlib STATIC src/mathlib.cpp)
target_include_directories(mathlib PUBLIC include)

add_library(stringlib SHARED src/stringlib.cpp)
target_include_directories(stringlib PUBLIC include)

add_executable(myapp main.cpp)
target_link_libraries(myapp PRIVATE mathlib stringlib)
```

### **Library Types in Detail**

#### **1. Static Library**
```cmake
add_library(mylib STATIC src.cpp)
# Produces: libmylib.a (Linux/macOS) or mylib.lib (Windows)
```

**Pros:**
- No runtime dependencies
- Faster startup (code already in executable)
- No versioning issues

**Cons:**
- Larger executable size
- Cannot update without recompiling executable

#### **2. Shared Library**
```cmake
add_library(mylib SHARED src.cpp)
# Produces: libmylib.so (Linux), libmylib.dylib (macOS), mylib.dll (Windows)
```

**Pros:**
- Smaller executable size
- Can update library without recompiling executable
- Memory sharing between processes

**Cons:**
- Runtime dependencies (DLL hell)
- Slightly slower startup
- Version compatibility issues

#### **3. Object Library** (Intermediate)
```cmake
add_library(myobj OBJECT src.cpp)
# Creates object files that can be linked into other targets
```

#### **4. Interface Library** (Header-only)
```cmake
add_library(myinterface INTERFACE)
target_include_directories(myinterface INTERFACE include)
target_compile_definitions(myinterface INTERFACE USE_MY_LIBRARY=1)
```

### **Advanced Library Example: Choosing Library Type at Configure Time**

**CMakeLists.txt with option:**
```cmake
cmake_minimum_required(VERSION 3.10)
project(ConfigurableLibrary VERSION 1.0.0 LANGUAGES CXX)

# Option to choose library type
option(BUILD_SHARED_LIBS "Build shared libraries instead of static" OFF)

message(STATUS "Building ${PROJECT_NAME} as ${BUILD_SHARED_LIBS?shared:static} libraries")

# The library type will be determined by BUILD_SHARED_LIBS
add_library(configurable
    src/configurable.cpp
)

# Or explicitly control each library
if(BUILD_AS_SHARED)
    add_library(mylib SHARED src.cpp)
else()
    add_library(mylib STATIC src.cpp)
endif()

target_include_directories(configurable PUBLIC include)
```

### **Complete Example: Mixed Library Project**

Let's create a more complex example with both static and shared libraries that depend on each other:

**Project Structure:**
```
mixed_libs/
├── CMakeLists.txt
├── core/           # Static library
├── utils/          # Shared library (depends on core)
├── network/        # Shared library (depends on utils)
└── app/            # Executable (depends on all)
```

**core/CMakeLists.txt:**
```cmake
add_library(core STATIC core.cpp)
target_include_directories(core PUBLIC include)
target_compile_definitions(core PUBLIC CORE_LIBRARY)
```

**utils/CMakeLists.txt:**
```cmake
add_library(utils SHARED utils.cpp)
target_include_directories(utils PUBLIC include)
target_link_libraries(utils PUBLIC core)  # Link with core
```

**network/CMakeLists.txt:**
```cmake
add_library(network SHARED network.cpp)
target_include_directories(network PUBLIC include)
target_link_libraries(network PUBLIC utils)  # Links utils (and transitively core)
```

**app/CMakeLists.txt:**
```cmake
add_executable(myapp main.cpp)
target_link_libraries(myapp PRIVATE network)
# Automatically gets includes from core, utils, and network
```

### **Visualizing Dependencies**

CMake can generate dependency graphs:
```bash
# Install graphviz first
sudo apt-get install graphviz  # Ubuntu
brew install graphviz          # macOS

# Generate dependency graph
cd build
cmake --graphviz=dependencies.dot ..
dot -Tpng dependencies.dot -o dependencies.png
```

### **Library Best Practices**

1. **Always use `target_include_directories()` with `PUBLIC`/`PRIVATE`/`INTERFACE`**
   - `PUBLIC`: For headers needed by users of the library
   - `PRIVATE`: For internal headers only
   - `INTERFACE`: For header-only libraries

2. **Use target aliases for nicer names**
   ```cmake
   add_library(math::math ALIAS mathlib)
   ```

3. **Set library version properties**
   ```cmake
   set_target_properties(mylib PROPERTIES
       VERSION 1.2.3
       SOVERSION 1  # API compatibility version
   )
   ```

4. **Handle platform differences**
   ```cmake
   if(WIN32)
       target_compile_definitions(mylib PRIVATE MYLIB_EXPORTS)
   endif()
   ```

5. **Use generator expressions for conditional settings**
   ```cmake
   target_compile_options(mylib PRIVATE
       $<$<CXX_COMPILER_ID:GNU>:-Wall>
       $<$<CXX_COMPILER_ID:MSVC>:/W4>
   )
   ```

### **Debugging Library Issues**

**Common issues and solutions:**

1. **"Undefined reference" errors**
   ```bash
   # Check if library is being linked
   ldd ./myapp  # Linux - shows shared library dependencies
   otool -L ./myapp  # macOS
   dumpbin /dependents myapp.exe  # Windows
   ```

2. **Wrong library type being used**
   ```cmake
   # Force static linking on Linux
   set(CMAKE_FIND_LIBRARY_SUFFIXES ".a")
   ```

3. **RPATH issues (shared libraries not found at runtime)**
   ```cmake
   # Set RPATH so executable finds libraries in build directory
   set(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)
   set(CMAKE_INSTALL_RPATH "$ORIGIN")
   set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
   ```

### **Exercise: Create Your Own Library Project**

Create a project with:
1. One static library for data structures (linked list, stack, queue)
2. One shared library for algorithms (sorting, searching) that depends on the data structures library
3. An executable that uses both libraries
4. Options to build both libraries as static or shared
5. Proper include directory propagation

### **Next Steps Preview**
In **PART-4**, we'll cover:
- FindPackage and dependency management
- Using external libraries (Boost, OpenSSL, etc.)
- Writing your own Find modules
- Package managers with CMake (vcpkg, Conan)

**To continue, please reply with "Next" or "PART-4" to receive the next part of the course.**


# **PART-4: Dependency Management with FindPackage and External Libraries**

### **Topic: Finding and Using External Libraries, Writing Find Modules, and Package Managers**

### **Introduction to Dependency Management**

As projects grow, you'll need to use external libraries. CMake provides several ways to handle dependencies:

1. **FindPackage**: CMake's built-in module system
2. **Package Managers**: vcpkg, Conan, system packages
3. **FetchContent**: Download dependencies at configure time
4. **Submodules**: Git submodules for source dependencies

### **Project Structure for This Part**

```
dependency_project/
├── CMakeLists.txt
├── src/
│   └── main.cpp
├── cmake/          # Custom find modules
│   └── FindSomeLib.cmake
└── build/
```

### **Using CMake's FindPackage System**

CMake comes with many built-in Find modules for common libraries. These modules set variables that you can use to link against libraries.

#### **Basic FindPackage Usage**

**src/main.cpp (JSON example):**
```cpp
#include <iostream>
#include <string>

// We'll try to use JSON library (simulated)
#ifdef HAVE_JSON
    #include <nlohmann/json.hpp>
    using json = nlohmann::json;
#endif

int main() {
    std::cout << "Dependency Management Demo" << std::endl;
    
    #ifdef HAVE_JSON
        json j;
        j["name"] = "John";
        j["age"] = 30;
        j["city"] = "New York";
        
        std::cout << "JSON Example: " << j.dump(2) << std::endl;
    #else
        std::cout << "JSON library not available" << std::endl;
    #endif
    
    return 0;
}
```

**CMakeLists.txt with FindPackage:**
```cmake
cmake_minimum_required(VERSION 3.15)
project(DependencyDemo VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Try to find common libraries
find_package(Threads REQUIRED)  # Built-in CMake module for threads

# Look for optional dependencies
find_package(ZLIB)  # Try to find Zlib compression library

# For demonstration, we'll use a made-up JSON library
# In real projects, this would be nlohmann/json, RapidJSON, etc.
find_package(JSON QUIET)  # QUIET suppresses error if not found

message(STATUS "Threads found: ${Threads_FOUND}")
message(STATUS "ZLIB found: ${ZLIB_FOUND}")
message(STATUS "JSON found: ${JSON_FOUND}")

if(ZLIB_FOUND)
    message(STATUS "ZLIB version: ${ZLIB_VERSION}")
    message(STATUS "ZLIB include dirs: ${ZLIB_INCLUDE_DIRS}")
    message(STATUS "ZLIB libraries: ${ZLIB_LIBRARIES}")
endif()

# Add executable
add_executable(${PROJECT_NAME} src/main.cpp)

# Link with found libraries
target_link_libraries(${PROJECT_NAME} PRIVATE
    Threads::Threads  # Modern target-based approach
)

if(ZLIB_FOUND)
    target_link_libraries(${PROJECT_NAME} PRIVATE
        ${ZLIB_LIBRARIES}  # Old-style variable-based approach
    )
    target_include_directories(${PROJECT_NAME} PRIVATE
        ${ZLIB_INCLUDE_DIRS}
    )
endif()

if(JSON_FOUND)
    target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_JSON=1)
    # Modern libraries provide imported targets
    if(TARGET JSON::JSON)
        target_link_libraries(${PROJECT_NAME} PRIVATE JSON::JSON)
    else()
        target_link_libraries(${PROJECT_NAME} PRIVATE ${JSON_LIBRARIES})
        target_include_directories(${PROJECT_NAME} PRIVATE ${JSON_INCLUDE_DIRS})
    endif()
endif()
```

### **Common Built-in Find Modules**

#### **1. Threads Library**
```cmake
find_package(Threads REQUIRED)
# Provides: Threads::Threads target
target_link_libraries(myapp PRIVATE Threads::Threads)
```

#### **2. OpenSSL**
```cmake
find_package(OpenSSL REQUIRED)
# Provides: OpenSSL::SSL and OpenSSL::Crypto targets
target_link_libraries(myapp PRIVATE OpenSSL::SSL OpenSSL::Crypto)
```

#### **3. Boost**
```cmake
# Specify required components
find_package(Boost 1.70 REQUIRED COMPONENTS filesystem system)

# Use Boost targets
target_link_libraries(myapp PRIVATE Boost::filesystem Boost::system)
```

#### **4. Python**
```cmake
find_package(Python3 COMPONENTS Interpreter Development REQUIRED)
# Provides: Python3::Python target
target_link_libraries(myapp PRIVATE Python3::Python)
```

### **Complete Example: Using Real Libraries**

Let's create a project that uses multiple external libraries:

**src/secure_app.cpp:**
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <thread>
#include <chrono>

#ifdef HAVE_OPENSSL
#include <openssl/evp.h>
#include <openssl/sha.h>
#endif

#ifdef HAVE_ZLIB
#include <zlib.h>
#endif

void print_hash(const std::string& data) {
    #ifdef HAVE_OPENSSL
    unsigned char hash[SHA256_DIGEST_LENGTH];
    SHA256_CTX sha256;
    SHA256_Init(&sha256);
    SHA256_Update(&sha256, data.c_str(), data.size());
    SHA256_Final(hash, &sha256);
    
    std::cout << "SHA256: ";
    for(int i = 0; i < SHA256_DIGEST_LENGTH; i++) {
        printf("%02x", hash[i]);
    }
    std::cout << std::endl;
    #else
    std::cout << "OpenSSL not available for hashing" << std::endl;
    #endif
}

void compress_data(const std::string& data) {
    #ifdef HAVE_ZLIB
    uLongf compressed_size = compressBound(data.size());
    std::vector<Bytef> compressed(compressed_size);
    
    if(compress(compressed.data(), &compressed_size, 
                reinterpret_cast<const Bytef*>(data.c_str()), data.size()) == Z_OK) {
        std::cout << "Compressed " << data.size() << " bytes to " 
                  << compressed_size << " bytes" << std::endl;
        std::cout << "Compression ratio: " 
                  << (static_cast<float>(compressed_size) / data.size() * 100) 
                  << "%" << std::endl;
    }
    #else
    std::cout << "Zlib not available for compression" << std::endl;
    #endif
}

void thread_worker(int id) {
    std::this_thread::sleep_for(std::chrono::milliseconds(100));
    std::cout << "Thread " << id << " completed" << std::endl;
}

int main() {
    std::cout << "=== External Libraries Demo ===" << std::endl;
    
    // Thread example
    std::cout << "\n1. Threading Example:" << std::endl;
    std::vector<std::thread> threads;
    for(int i = 0; i < 3; ++i) {
        threads.emplace_back(thread_worker, i);
    }
    
    for(auto& t : threads) {
        t.join();
    }
    
    // OpenSSL example
    std::cout << "\n2. Cryptography Example:" << std::endl;
    std::string sensitive_data = "Hello, Secure World!";
    print_hash(sensitive_data);
    
    // Zlib example
    std::cout << "\n3. Compression Example:" << std::endl;
    std::string data_to_compress = std::string(1000, 'A') + std::string(1000, 'B');
    compress_data(data_to_compress);
    
    std::cout << "\nAll operations completed!" << std::endl;
    return 0;
}
```

**CMakeLists.txt:**
```cmake
cmake_minimum_required(VERSION 3.15)
project(SecureApp VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Required dependencies
find_package(Threads REQUIRED)

# Optional dependencies with version requirements
find_package(OpenSSL 1.1.0 QUIET)  # At least version 1.1.0
find_package(ZLIB QUIET)

# Print findings
message(STATUS "=== Dependency Status ===")
message(STATUS "Threads: Found")
message(STATUS "OpenSSL: ${OpenSSL_FOUND} (${OpenSSL_VERSION})")
message(STATUS "ZLIB: ${ZLIB_FOUND}")

# Add executable
add_executable(${PROJECT_NAME} src/secure_app.cpp)

# Always link with threads
target_link_libraries(${PROJECT_NAME} PRIVATE Threads::Threads)

# Conditional compilation based on found libraries
if(OpenSSL_FOUND)
    target_link_libraries(${PROJECT_NAME} PRIVATE OpenSSL::SSL OpenSSL::Crypto)
    target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_OPENSSL=1)
    message(STATUS "Linking with OpenSSL")
else()
    message(WARNING "OpenSSL not found - cryptographic features disabled")
endif()

if(ZLIB_FOUND)
    # Modern approach if available
    if(TARGET ZLIB::ZLIB)
        target_link_libraries(${PROJECT_NAME} PRIVATE ZLIB::ZLIB)
    else()
        # Fallback to old-style variables
        target_link_libraries(${PROJECT_NAME} PRIVATE ${ZLIB_LIBRARIES})
        target_include_directories(${PROJECT_NAME} PRIVATE ${ZLIB_INCLUDE_DIRS})
    endif()
    target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_ZLIB=1)
    message(STATUS "Linking with ZLIB")
else()
    message(WARNING "ZLIB not found - compression features disabled")
endif()

# Compiler warnings
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Wpedantic)
elseif(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE /W4)
endif()
```

### **Installing Required Libraries**

#### **Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install libssl-dev zlib1g-dev
```

#### **macOS:**
```bash
brew install openssl zlib
# For CMake to find Homebrew's openssl:
export PKG_CONFIG_PATH="/usr/local/opt/openssl/lib/pkgconfig"
```

#### **Windows (vcpkg):**
```powershell
vcpkg install openssl:x64-windows zlib:x64-windows
```

### **Writing Custom Find Modules**

When CMake doesn't have a built-in Find module for your library, you can write your own.

**cmake/FindMyCustomLib.cmake:**
```cmake
# FindMyCustomLib.cmake
# - Try to find MyCustomLib library
# Once done, this will define:
#  MyCustomLib_FOUND - System has MyCustomLib
#  MyCustomLib_INCLUDE_DIRS - The MyCustomLib include directories
#  MyCustomLib_LIBRARIES - The libraries needed to use MyCustomLib
#  MyCustomLib_VERSION - The version of MyCustomLib

find_path(MyCustomLib_INCLUDE_DIR
    NAMES mycustomlib.h
    PATHS
        /usr/include
        /usr/local/include
        /opt/local/include
        ${CMAKE_INSTALL_PREFIX}/include
    PATH_SUFFIXES mycustomlib
)

find_library(MyCustomLib_LIBRARY
    NAMES mycustomlib mycustom
    PATHS
        /usr/lib
        /usr/local/lib
        /opt/local/lib
        ${CMAKE_INSTALL_PREFIX}/lib
)

if(MyCustomLib_INCLUDE_DIR AND MyCustomLib_LIBRARY)
    set(MyCustomLib_FOUND TRUE)
    
    # Try to get version
    if(EXISTS "${MyCustomLib_INCLUDE_DIR}/mycustomlib_version.h")
        file(STRINGS "${MyCustomLib_INCLUDE_DIR}/mycustomlib_version.h" _version_line
             REGEX "^#define MYCUSTOMLIB_VERSION_[A-Z]+")
        string(REGEX REPLACE ".*VERSION_MAJOR ([0-9]+).*" "\\1" _major "${_version_line}")
        string(REGEX REPLACE ".*VERSION_MINOR ([0-9]+).*" "\\1" _minor "${_version_line}")
        string(REGEX REPLACE ".*VERSION_PATCH ([0-9]+).*" "\\1" _patch "${_version_line}")
        set(MyCustomLib_VERSION "${_major}.${_minor}.${_patch}")
    endif()
    
    # Set variables
    set(MyCustomLib_INCLUDE_DIRS ${MyCustomLib_INCLUDE_DIR})
    set(MyCustomLib_LIBRARIES ${MyCustomLib_LIBRARY})
    
    # Create imported target (modern CMake)
    if(NOT TARGET MyCustomLib::MyCustomLib)
        add_library(MyCustomLib::MyCustomLib UNKNOWN IMPORTED)
        set_target_properties(MyCustomLib::MyCustomLib PROPERTIES
            INTERFACE_INCLUDE_DIRECTORIES "${MyCustomLib_INCLUDE_DIR}"
            IMPORTED_LOCATION "${MyCustomLib_LIBRARY}"
        )
    endif()
endif()

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(MyCustomLib
    REQUIRED_VARS MyCustomLib_LIBRARY MyCustomLib_INCLUDE_DIR
    VERSION_VAR MyCustomLib_VERSION
)

mark_as_advanced(MyCustomLib_INCLUDE_DIR MyCustomLib_LIBRARY)
```

**Using the custom find module:**
```cmake
# Add custom module path
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Use custom find module
find_package(MyCustomLib REQUIRED)

# Link with custom library
target_link_libraries(myapp PRIVATE MyCustomLib::MyCustomLib)
```

### **Using Package Managers with CMake**

#### **1. vcpkg (Microsoft's C++ Library Manager)**

**CMakeLists.txt with vcpkg:**
```cmake
cmake_minimum_required(VERSION 3.15)
project(VcpkgDemo VERSION 1.0.0 LANGUAGES CXX)

# Find packages installed via vcpkg
find_package(OpenSSL REQUIRED)
find_package(ZLIB REQUIRED)
find_package(protobuf CONFIG REQUIRED)  # Note: Use CONFIG mode for vcpkg

add_executable(${PROJECT_NAME} src/main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE
    OpenSSL::SSL
    OpenSSL::Crypto
    ZLIB::ZLIB
    protobuf::libprotobuf
)
```

**Building with vcpkg:**
```bash
# Install vcpkg
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.sh  # or bootstrap-vcpkg.bat on Windows

# Install dependencies
./vcpkg install openssl zlib protobuf

# Configure CMake with vcpkg toolchain
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=/path/to/vcpkg/scripts/buildsystems/vcpkg.cmake
```

#### **2. Conan C/C++ Package Manager**

**conanfile.txt:**
```ini
[requires]
openssl/1.1.1w
zlib/1.3.1
boost/1.83.0

[generators]
cmake_find_package
# or cmake_paths for simpler integration

[options]
boost:shared=False
```

**CMakeLists.txt with Conan:**
```cmake
cmake_minimum_required(VERSION 3.15)
project(ConanDemo VERSION 1.0.0 LANGUAGES CXX)

# Run Conan before CMake configuration
# or run conan install manually
find_package(OpenSSL REQUIRED)
find_package(ZLIB REQUIRED)
find_package(Boost REQUIRED COMPONENTS filesystem system)

add_executable(${PROJECT_NAME} src/main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE
    OpenSSL::SSL
    ZLIB::ZLIB
    Boost::filesystem
    Boost::system
)
```

### **Modern Approach: FetchContent**

FetchContent downloads dependencies at configure time. Great for header-only libraries or when you want to vendor dependencies.

**CMakeLists.txt with FetchContent:**
```cmake
cmake_minimum_required(VERSION 3.14)  # FetchContent requires 3.14+
project(FetchContentDemo VERSION 1.0.0 LANGUAGES CXX)

include(FetchContent)

# Example: Fetch nlohmann/json (header-only library)
FetchContent_Declare(
    nlohmann_json
    GIT_REPOSITORY https://github.com/nlohmann/json.git
    GIT_TAG v3.11.2  # Always specify a version/tag
)

# Example: Fetch Google Test
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG release-1.12.1
)

# Populate (download) the dependencies
FetchContent_MakeAvailable(nlohmann_json googletest)

# Now you can use them
add_executable(${PROJECT_NAME} src/main.cpp)

# Header-only library
target_include_directories(${PROJECT_NAME} PRIVATE
    ${nlohmann_json_SOURCE_DIR}/include
)

# Or if the library provides CMake targets
# target_link_libraries(${PROJECT_NAME} PRIVATE nlohmann_json::nlohmann_json)
```

### **Complete Example: Multi-Dependency Project**

Let's create a project that uses various dependency methods:

**Project Structure:**
```
advanced_deps/
├── CMakeLists.txt
├── conanfile.txt
├── src/
│   └── main.cpp
├── thirdparty/          # For manually downloaded deps
└── cmake/
    └── FindCatch2.cmake
```

**src/main.cpp:**
```cpp
#include <iostream>
#include <vector>
#include <string>

// Conditional includes based on what's available
#ifdef HAVE_BOOST
#include <boost/algorithm/string.hpp>
#endif

#ifdef HAVE_JSON
#include <nlohmann/json.hpp>
#endif

int main() {
    std::cout << "Advanced Dependency Demo\n" << std::endl;
    
    // Boost example
    #ifdef HAVE_BOOST
    std::string text = "hello world from boost";
    boost::to_upper(text);
    std::cout << "Boost uppercase: " << text << std::endl;
    
    std::vector<std::string> words;
    boost::split(words, text, boost::is_any_of(" "));
    std::cout << "Split into " << words.size() << " words" << std::endl;
    #endif
    
    // JSON example
    #ifdef HAVE_JSON
    nlohmann::json j;
    j["project"] = "CMake Dependency Demo";
    j["version"] = "1.0.0";
    j["dependencies"] = {"Boost", "JSON", "Testing"};
    
    std::cout << "\nJSON output:\n" << j.dump(2) << std::endl;
    #endif
    
    std::cout << "\nAll dependencies working correctly!" << std::endl;
    return 0;
}
```

**CMakeLists.txt:**
```cmake
cmake_minimum_required(VERSION 3.18)
project(AdvancedDeps VERSION 1.0.0 LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Option to use system packages vs FetchContent
option(USE_SYSTEM_LIBS "Use system-installed libraries instead of FetchContent" OFF)
option(BUILD_TESTS "Build tests" ON)

# Method 1: System packages (traditional)
if(USE_SYSTEM_LIBS)
    message(STATUS "Using system-installed libraries")
    
    find_package(Boost 1.70 REQUIRED COMPONENTS algorithm)
    
    # JSON might not be available as system package
    find_package(nlohmann_json 3.10 QUIET)
    if(NOT nlohmann_json_FOUND)
        message(WARNING "System nlohmann/json not found, consider using FetchContent")
    endif()
    
else()
    # Method 2: FetchContent (modern)
    message(STATUS "Using FetchContent for dependencies")
    
    include(FetchContent)
    
    # Fetch nlohmann/json
    FetchContent_Declare(
        json
        GIT_REPOSITORY https://github.com/nlohmann/json.git
        GIT_TAG v3.11.2
        FIND_PACKAGE_ARGS NAMES nlohmann_json
    )
    
    # Fetch Boost (using a smaller subset or alternative)
    # Note: Full Boost is large, consider alternatives for real projects
    FetchContent_Declare(
        boost_algorithm
        URL https://github.com/boostorg/algorithm/archive/refs/tags/boost-1.83.0.tar.gz
        URL_HASH SHA256=...
    )
    
    FetchContent_MakeAvailable(json)
    
    # Mark dependencies as found for our code
    set(HAVE_JSON TRUE)
    set(HAVE_BOOST TRUE)  # Simplified for demo
endif()

# Testing framework
if(BUILD_TESTS)
    # Add custom module path for our FindCatch2.cmake
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
    
    find_package(Catch2 3.0 QUIET)
    if(NOT Catch2_FOUND)
        message(STATUS "Catch2 not found, downloading via FetchContent")
        
        FetchContent_Declare(
            Catch2
            GIT_REPOSITORY https://github.com/catchorg/Catch2.git
            GIT_TAG v3.4.0
        )
        FetchContent_MakeAvailable(Catch2)
    endif()
endif()

# Add main executable
add_executable(${PROJECT_NAME} src/main.cpp)

# Link with dependencies
if(USE_SYSTEM_LIBS)
    if(Boost_FOUND)
        target_link_libraries(${PROJECT_NAME} PRIVATE Boost::algorithm)
        target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_BOOST=1)
    endif()
    
    if(TARGET nlohmann_json::nlohmann_json)
        target_link_libraries(${PROJECT_NAME} PRIVATE nlohmann_json::nlohmann_json)
        target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_JSON=1)
    endif()
else()
    # Using FetchContent dependencies
    if(TARGET nlohmann_json)
        target_link_libraries(${PROJECT_NAME} PRIVATE nlohmann_json)
    else()
        target_include_directories(${PROJECT_NAME} PRIVATE
            ${json_SOURCE_DIR}/include
        )
    endif()
    target_compile_definitions(${PROJECT_NAME} PRIVATE HAVE_JSON=1 HAVE_BOOST=1)
endif()

# Add tests if Catch2 is available
if(BUILD_TESTS AND TARGET Catch2::Catch2WithMain)
    message(STATUS "Adding tests with Catch2")
    
    add_executable(tests tests/main.cpp tests/test_math.cpp)
    target_link_libraries(tests PRIVATE Catch2::Catch2WithMain)
    
    enable_testing()
    add_test(NAME tests COMMAND tests)
endif()

# Print configuration summary
message(STATUS "")
message(STATUS "=== Configuration Summary ===")
message(STATUS "Project: ${PROJECT_NAME} ${PROJECT_VERSION}")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Build tests: ${BUILD_TESTS}")
message(STATUS "Use system libs: ${USE_SYSTEM_LIBS}")
message(STATUS "Boost available: ${HAVE_BOOST}")
message(STATUS "JSON available: ${HAVE_JSON}")
message(STATUS "=============================")
```

### **Best Practices for Dependency Management**

1. **Always specify version requirements:**
   ```cmake
   find_package(Boost 1.70 REQUIRED)
   find_package(OpenSSL 1.1.0 REQUIRED)
   ```

2. **Handle optional dependencies gracefully:**
   ```cmake
   find_package(OptionalLib QUIET)
   if(OptionalLib_FOUND)
       target_link_libraries(myapp PRIVATE OptionalLib::OptionalLib)
       target_compile_definitions(myapp PRIVATE HAVE_OPTIONALLIB=1)
   else()
       message(STATUS "OptionalLib not found - feature disabled")
   endif()
   ```

3. **Use modern imported targets when available:**
   ```cmake
   # Good
   target_link_libraries(myapp PRIVATE OpenSSL::SSL)
   
   # Avoid (old style)
   target_link_libraries(myapp PRIVATE ${OPENSSL_LIBRARIES})
   target_include_directories(myapp PRIVATE ${OPENSSL_INCLUDE_DIRS})
   ```

4. **Provide clear error messages:**
   ```cmake
   if(NOT REQUIRED_LIB_FOUND)
       message(FATAL_ERROR "
       Required library not found!
       Please install it using:
         Ubuntu: sudo apt-get install librequired-dev
         macOS: brew install required
         Or download from: https://required-lib.org
       ")
   endif()
   ```

5. **Support multiple ways to find dependencies:**
   ```cmake
   # Try config mode first, then module mode
   find_package(SomeLib CONFIG QUIET)
   if(NOT SomeLib_FOUND)
       find_package(SomeLib MODULE REQUIRED)
   endif()
   ```

### **Debugging Dependency Issues**

**Common commands to diagnose issues:**

```bash
# See what CMake found
cmake -LAH ..  # List all variables with help

# Check include paths
echo | gcc -E -Wp,-v - 2>&1 | grep " /"  # Linux/macOS

# Check library paths
ldconfig -p | grep zlib  # Linux
otool -L /usr/lib/libz.dylib  # macOS
where zlib1.dll  # Windows

# Verbose output from find_package
cmake --find-package -DNAME=ZLIB -DCOMPILER_ID=GNU -DLANGUAGE=CXX -DMODE=COMPILE
```

### **Exercise: Create a Project with Mixed Dependencies**

Create a project that:
1. Uses at least 3 external libraries (one required, two optional)
2. Provides two ways to get dependencies (system packages and FetchContent)
3. Includes a custom Find module for a library
4. Has conditional compilation based on what's available
5. Prints a nice configuration summary

### **Next Steps Preview**
In **PART-5**, we'll cover:
- Advanced target properties and usage requirements
- Generator expressions for conditional compilation
- Interface libraries and header-only libraries
- Transitive dependencies and propagation

**To continue, please reply with "Next" or "PART-5" to receive the next part of the course.**



# **PART-5: Advanced Target Properties and Generator Expressions**

### **Topic: Mastering Target Properties, Usage Requirements, and Generator Expressions**

### **Understanding Target Properties**

In modern CMake, everything revolves around **targets** (executables, libraries). Each target has properties that control how it's built and used.

### **Project Structure for This Part**

```
target_properties/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── utils.cpp
│   └── utils.h
├── lib/
│   ├── interface_lib/
│   └── header_only/
└── tests/
```

### **Common Target Properties**

Let's explore the most important target properties:

**Basic CMakeLists.txt to start:**
```cmake
cmake_minimum_required(VERSION 3.18)
project(TargetPropertiesDemo VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Create a library
add_library(utils STATIC
    src/utils.cpp
)

# Create an executable
add_executable(${PROJECT_NAME}
    src/main.cpp
)

# Link them
target_link_libraries(${PROJECT_NAME} PRIVATE utils)
```

### **Setting and Getting Target Properties**

#### **Method 1: set_target_properties()**
```cmake
# Set multiple properties at once
set_target_properties(utils PROPERTIES
    # Output naming
    OUTPUT_NAME "myutils"
    PREFIX "lib"
    SUFFIX ".a"
    
    # Version information
    VERSION 1.2.3
    SOVERSION 1
    
    # C++ standard
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
    
    # Compiler features
    POSITION_INDEPENDENT_CODE ON
    
    # Visibility
    VISIBILITY_INLINES_HIDDEN ON
    
    # Windows specific
    WINDOWS_EXPORT_ALL_SYMBOLS ON
)

# For executable
set_target_properties(${PROJECT_NAME} PROPERTIES
    WIN32_EXECUTABLE ON  # Creates Windows GUI app (no console)
    MACOSX_BUNDLE ON     # Creates macOS .app bundle
    OUTPUT_NAME "app"
)
```

#### **Method 2: Individual Commands**
```cmake
# Set individual properties
set_property(TARGET utils PROPERTY CXX_VISIBILITY_PRESET hidden)
set_property(TARGET utils PROPERTY VISIBILITY_INLINES_HIDDEN ON)

# Get property values
get_target_property(utils_type utils TYPE)
get_target_property(utils_output_name utils OUTPUT_NAME)
message(STATUS "Utils type: ${utils_type}")
message(STATUS "Utils output name: ${utils_output_name}")
```

### **Complete Example: Library with Full Properties**

**src/utils.h:**
```cpp
#ifndef UTILS_H
#define UTILS_H

#include <string>
#include <vector>
#include <memory>

// API export macros
#if defined(_WIN32) && defined(UTILS_EXPORTS)
    #define UTILS_API __declspec(dllexport)
#elif defined(_WIN32)
    #define UTILS_API __declspec(dllimport)
#else
    #define UTILS_API __attribute__((visibility("default")))
#endif

namespace utils {
    
    UTILS_API std::string to_upper(const std::string& str);
    UTILS_API std::string to_lower(const std::string& str);
    
    UTILS_API std::vector<std::string> split(const std::string& str, char delimiter);
    UTILS_API std::string join(const std::vector<std::string>& parts, 
                               const std::string& delimiter);
    
    template<typename T>
    class UTILS_API CircularBuffer {
    private:
        std::unique_ptr<T[]> buffer;
        size_t capacity;
        size_t head = 0;
        size_t tail = 0;
        size_t count = 0;
        
    public:
        explicit CircularBuffer(size_t size);
        
        bool push(const T& item);
        bool pop(T& item);
        bool empty() const;
        bool full() const;
        size_t size() const;
        size_t get_capacity() const;
    };
    
} // namespace utils

#endif // UTILS_H
```

**src/utils.cpp:**
```cpp
#include "utils.h"
#include <algorithm>
#include <cctype>
#include <sstream>

namespace utils {
    
    std::string to_upper(const std::string& str) {
        std::string result = str;
        std::transform(result.begin(), result.end(), result.begin(),
                      [](unsigned char c) { return std::toupper(c); });
        return result;
    }
    
    std::string to_lower(const std::string& str) {
        std::string result = str;
        std::transform(result.begin(), result.end(), result.begin(),
                      [](unsigned char c) { return std::tolower(c); });
        return result;
    }
    
    std::vector<std::string> split(const std::string& str, char delimiter) {
        std::vector<std::string> tokens;
        std::stringstream ss(str);
        std::string token;
        
        while (std::getline(ss, token, delimiter)) {
            if (!token.empty()) {
                tokens.push_back(token);
            }
        }
        
        return tokens;
    }
    
    std::string join(const std::vector<std::string>& parts, 
                     const std::string& delimiter) {
        if (parts.empty()) return "";
        
        std::string result = parts[0];
        for (size_t i = 1; i < parts.size(); ++i) {
            result += delimiter + parts[i];
        }
        return result;
    }
    
    // CircularBuffer implementation
    template<typename T>
    CircularBuffer<T>::CircularBuffer(size_t size) 
        : capacity(size), buffer(std::make_unique<T[]>(size)) {}
    
    template<typename T>
    bool CircularBuffer<T>::push(const T& item) {
        if (full()) return false;
        
        buffer[tail] = item;
        tail = (tail + 1) % capacity;
        ++count;
        return true;
    }
    
    template<typename T>
    bool CircularBuffer<T>::pop(T& item) {
        if (empty()) return false;
        
        item = buffer[head];
        head = (head + 1) % capacity;
        --count;
        return true;
    }
    
    template<typename T>
    bool CircularBuffer<T>::empty() const {
        return count == 0;
    }
    
    template<typename T>
    bool CircularBuffer<T>::full() const {
        return count == capacity;
    }
    
    template<typename T>
    size_t CircularBuffer<T>::size() const {
        return count;
    }
    
    template<typename T>
    size_t CircularBuffer<T>::get_capacity() const {
        return capacity;
    }
    
    // Explicit template instantiations
    template class CircularBuffer<int>;
    template class CircularBuffer<double>;
    template class CircularBuffer<std::string>;
    
} // namespace utils
```

**src/main.cpp:**
```cpp
#include "utils.h"
#include <iostream>
#include <vector>

int main() {
    std::cout << "=== Advanced Target Properties Demo ===\n" << std::endl;
    
    // String utilities
    std::string text = "Hello, World!";
    std::cout << "Original: " << text << std::endl;
    std::cout << "Uppercase: " << utils::to_upper(text) << std::endl;
    std::cout << "Lowercase: " << utils::to_lower(text) << std::endl;
    
    // Split and join
    std::string csv = "apple,banana,cherry,date";
    auto fruits = utils::split(csv, ',');
    std::cout << "\nSplit CSV into " << fruits.size() << " items" << std::endl;
    std::cout << "Joined with ' | ': " << utils::join(fruits, " | ") << std::endl;
    
    // Circular buffer
    utils::CircularBuffer<int> buffer(5);
    std::cout << "\nCircular Buffer Demo:" << std::endl;
    std::cout << "Buffer capacity: " << buffer.get_capacity() << std::endl;
    
    // Push values
    for (int i = 1; i <= 7; ++i) {
        if (buffer.push(i * 10)) {
            std::cout << "Pushed: " << i * 10 << std::endl;
        } else {
            std::cout << "Buffer full! Cannot push: " << i * 10 << std::endl;
        }
    }
    
    // Pop values
    int value;
    while (!buffer.empty()) {
        buffer.pop(value);
        std::cout << "Popped: " << value << std::endl;
    }
    
    std::cout << "\nDemo completed successfully!" << std::endl;
    return 0;
}
```

### **Complete CMakeLists.txt with Advanced Properties**

```cmake
cmake_minimum_required(VERSION 3.18)
project(TargetPropertiesDemo VERSION 2.0.0 LANGUAGES CXX)

# Option to build as shared or static
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
option(ENABLE_LTO "Enable Link Time Optimization" OFF)

# Set C++ standard globally
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Platform-specific defaults
if(WIN32)
    # Windows: default to static for easier distribution
    if(NOT DEFINED BUILD_SHARED_LIBS)
        set(BUILD_SHARED_LIBS OFF CACHE BOOL "Build shared libraries" FORCE)
    endif()
endif()

message(STATUS "Building ${PROJECT_NAME} v${PROJECT_VERSION}")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Build shared libs: ${BUILD_SHARED_LIBS}")
message(STATUS "Enable LTO: ${ENABLE_LTO}")

# Create the utils library
add_library(utils
    src/utils.cpp
)

# Set extensive target properties for the library
set_target_properties(utils PROPERTIES
    # Output naming
    OUTPUT_NAME "utils"
    DEBUG_POSTFIX "_d"           # Adds _d suffix in Debug builds
    RELEASE_POSTFIX ""           # No suffix in Release
    MINSIZEREL_POSTFIX "_minsize"
    RELWITHDEBINFO_POSTFIX "_relwithdebinfo"
    
    # Version info (important for shared libraries)
    VERSION ${PROJECT_VERSION}
    SOVERSION 2                  # API version (increment on breaking changes)
    
    # C++ standard (overrides global)
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
    
    # Compiler feature control
    POSITION_INDEPENDENT_CODE ON  # Required for shared libs on most platforms
    
    # Visibility control (reduces binary size, improves loading time)
    CXX_VISIBILITY_PRESET hidden
    VISIBILITY_INLINES_HIDDEN ON
    
    # Linker optimization
    INTERPROCEDURAL_OPTIMIZATION ${ENABLE_LTO}
    
    # Windows specific
    WINDOWS_EXPORT_ALL_SYMBOLS ${BUILD_SHARED_LIBS}  # Auto-export on Windows
)

# Add include directories with proper scoping
target_include_directories(utils
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Add compile definitions
target_compile_definitions(utils
    PRIVATE
        $<$<BOOL:${BUILD_SHARED_LIBS}>:UTILS_EXPORTS>
    PUBLIC
        UTILS_API_EXPORTS  # Signal to users that this is our library
)

# Compiler options using generator expressions
target_compile_options(utils
    PRIVATE
        # Common warnings
        $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:
            -Wall
            -Wextra
            -Wpedantic
            -Wshadow
            -Wnon-virtual-dtor
            -Wold-style-cast
            -Wcast-align
            -Wunused
            -Woverloaded-virtual
            -Wconversion
            -Wsign-conversion
            -Wnull-dereference
            -Wdouble-promotion
            -Wformat=2
        >
        $<$<CXX_COMPILER_ID:MSVC>:
            /W4      # Warning level 4
            /WX      # Treat warnings as errors
            /permissive-  # Standards conformance
        >
        
        # Optimization level per build type
        $<$<CONFIG:Release>:
            $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-O3>
            $<$<CXX_COMPILER_ID:MSVC>:/O2>
        >
        $<$<CONFIG:Debug>:
            $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-O0 -g3>
            $<$<CXX_COMPILER_ID:MSVC>:/Od /Zi>
        >
        $<$<CONFIG:RelWithDebInfo>:
            $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-O2 -g>
            $<$<CXX_COMPILER_ID:MSVC>:/O2 /Zi>
        >
        
        # Link Time Optimization if enabled
        $<$<AND:$<BOOL:${ENABLE_LTO}>,$<CONFIG:Release,RelWithDebInfo>>:
            $<$<CXX_COMPILER_ID:GNU>:-flto>
            $<$<CXX_COMPILER_ID:Clang>:-flto=thin>
        >
)

# Create an alias for nicer usage
add_library(Utils::utils ALIAS utils)

# Create the main executable
add_executable(${PROJECT_NAME}
    src/main.cpp
)

# Set executable properties
set_target_properties(${PROJECT_NAME} PROPERTIES
    # Output naming
    OUTPUT_NAME "demo_app"
    DEBUG_POSTFIX "_debug"
    
    # Platform-specific
    WIN32_EXECUTABLE OFF  # Console app on Windows
    MACOSX_BUNDLE OFF     # Not a macOS app bundle
    
    # C++ standard
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)

# Link with our library
target_link_libraries(${PROJECT_NAME}
    PRIVATE
        Utils::utils
)

# Add compile options to executable
target_compile_options(${PROJECT_NAME}
    PRIVATE
        $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:
            -Wall
            -Wextra
        >
)

# Add definitions for executable
target_compile_definitions(${PROJECT_NAME}
    PRIVATE
        APP_NAME=\"${PROJECT_NAME}\"
        APP_VERSION=\"${PROJECT_VERSION}\"
)

# Print target information
message(STATUS "\n=== Target Information ===")
get_target_property(utils_type utils TYPE)
get_target_property(utils_output utils OUTPUT_NAME)
get_target_property(utils_version utils VERSION)
get_target_property(utils_soversion utils SOVERSION)

message(STATUS "Utils library type: ${utils_type}")
message(STATUS "Utils output name: ${utils_output}")
message(STATUS "Utils version: ${utils_version}")
message(STATUS "Utils soversion: ${utils_soversion}")
message(STATUS "===========================\n")
```

### **Understanding Generator Expressions**

Generator expressions are evaluated during build system generation. They're incredibly powerful for conditional settings.

#### **Basic Generator Expression Syntax**
- `$<condition:value>` - If condition is true, use value
- `$<IF:condition,true_value,false_value>` - Ternary operator
- `$<condition:value1:value2>` - Another form of ternary

#### **Common Generator Expression Conditions**

**1. Build Configuration:**
```cmake
$<CONFIG:Debug>          # True in Debug build
$<CONFIG:Release>        # True in Release build
$<CONFIG:Debug,Release>  # True in Debug OR Release
$<NOT:$<CONFIG:Debug>>   # True when NOT Debug
```

**2. Compiler Checks:**
```cmake
$<CXX_COMPILER_ID:GNU>           # GCC compiler
$<CXX_COMPILER_ID:Clang>         # Clang compiler
$<CXX_COMPILER_ID:MSVC>          # Microsoft Visual C++
$<CXX_COMPILER_ID:AppleClang>    # Apple's Clang
$<OR:$<CXX_COMPILER_ID:GNU>,$<CXX_COMPILER_ID:Clang>>  # GCC or Clang
```

**3. Platform Checks:**
```cmake
$<PLATFORM_ID:Linux>     # Linux
$<PLATFORM_ID:Windows>   # Windows
$<PLATFORM_ID:Darwin>    # macOS
$<STREQUAL:$<PLATFORM_ID>,Linux>  # Alternative syntax
```

**4. Target Properties:**
```cmake
$<TARGET_PROPERTY:target,property>  # Get property of another target
$<TARGET_EXISTS:target>             # Check if target exists
```

### **Practical Examples with Generator Expressions**

#### **Example 1: Different Compiler Flags per Compiler**
```cmake
target_compile_options(my_target PRIVATE
    # GCC/Clang flags
    $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:
        -Wall
        -Wextra
        -Werror
        -pedantic
    >
    
    # MSVC flags
    $<$<CXX_COMPILER_ID:MSVC>:
        /W4
        /WX
        /permissive-
    >
    
    # Debug-specific flags
    $<$<CONFIG:Debug>:
        $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-O0 -g3 -D_DEBUG>
        $<$<CXX_COMPILER_ID:MSVC>:/Od /Zi /DEBUG>
    >
    
    # Release-specific flags
    $<$<CONFIG:Release>:
        $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-O3 -DNDEBUG>
        $<$<CXX_COMPILER_ID:MSVC>:/O2 /DNDEBUG>
    >
)
```

#### **Example 2: Platform-Specific Libraries**
```cmake
target_link_libraries(my_target PRIVATE
    # Common libraries
    Threads::Threads
    
    # Linux-specific
    $<$<PLATFORM_ID:Linux>:pthread dl>
    
    # Windows-specific
    $<$<PLATFORM_ID:Windows>:
        ws2_32     # Winsock
        dbghelp    # Debug help
    >
    
    # macOS-specific
    $<$<PLATFORM_ID:Darwin>:
        "-framework Foundation"
        "-framework CoreServices"
    >
)
```

#### **Example 3: Conditional Feature Enabling**
```cmake
# Define based on option
option(USE_AVX "Use AVX instructions" ON)

target_compile_options(my_target PRIVATE
    # Only add AVX flags if option is ON AND compiler supports it
    $<$<AND:$<BOOL:${USE_AVX}>,$<CXX_COMPILER_ID:GNU,Clang,AppleClang>>:
        -mavx
        -mavx2
    >
    
    $<$<AND:$<BOOL:${USE_AVX}>,$<CXX_COMPILER_ID:MSVC>>:
        /arch:AVX2
    >
)

# Or with more complex conditions
target_compile_definitions(my_target PRIVATE
    $<$<AND:$<BOOL:${USE_AVX}>,$<CXX_COMPILER_ID:GNU,Clang>>:
        USE_AVX_INSTRUCTIONS=1
        ENABLE_SIMD=1
    >
)
```

### **Interface Libraries and Usage Requirements**

Interface libraries are targets that don't produce any output but can carry usage requirements.

#### **Creating an Interface Library**

```cmake
# Create an interface library (header-only library concept)
add_library(config INTERFACE)

# Add usage requirements
target_include_directories(config INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_compile_definitions(config INTERFACE
    CONFIG_NAMESPACE=myconfig
    CONFIG_VERSION=2024
)

target_compile_features(config INTERFACE cxx_std_17)

# Any target linking with config gets these requirements
target_link_libraries(my_target PRIVATE config)
```

#### **Complete Example: Header-Only Library with Interface**

**lib/header_only/math_functions.h:**
```cpp
#ifndef MATH_FUNCTIONS_H
#define MATH_FUNCTIONS_H

namespace math {
    
    template<typename T>
    constexpr T pow(T base, int exponent) {
        T result = 1;
        for (int i = 0; i < exponent; ++i) {
            result *= base;
        }
        return result;
    }
    
    template<typename T>
    constexpr T factorial(T n) {
        T result = 1;
        for (T i = 2; i <= n; ++i) {
            result *= i;
        }
        return result;
    }
    
    template<typename T>
    constexpr bool is_prime(T n) {
        if (n <= 1) return false;
        if (n <= 3) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        
        for (T i = 5; i * i <= n; i += 6) {
            if (n % i == 0 || n % (i + 2) == 0) return false;
        }
        return true;
    }
    
} // namespace math

#endif // MATH_FUNCTIONS_H
```

**CMakeLists.txt for header-only library:**
```cmake
# Create interface library for header-only code
add_library(math_functions INTERFACE)

# Add include directory
target_include_directories(math_functions INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/header_only
)

# Add compile definitions
target_compile_definitions(math_functions INTERFACE
    USE_MATH_FUNCTIONS=1
    MATH_NAMESPACE=math
)

# Set C++ standard requirement
target_compile_features(math_functions INTERFACE cxx_std_17)

# Add compiler warnings through interface
target_compile_options(math_functions INTERFACE
    $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:
        -Wall
        -Wextra
    >
)

# Create alias for nice usage
add_library(Math::Functions ALIAS math_functions)
```

**Using the interface library:**
```cmake
# In another CMakeLists.txt
add_executable(math_demo math_demo.cpp)
target_link_libraries(math_demo PRIVATE Math::Functions)

# Now math_demo automatically gets:
# - Include directory for math_functions.h
# - Compile definitions
# - C++17 requirement
# - Compiler warnings
```

### **Transitive Dependencies and Propagation**

Modern CMake automatically handles transitive dependencies through the `PUBLIC`, `PRIVATE`, and `INTERFACE` keywords.

#### **Understanding Propagation:**

```cmake
# Library A (base library)
add_library(A STATIC a.cpp)
target_include_directories(A PUBLIC include/A)      # Users of A need this
target_compile_definitions(A PRIVATE A_IMPLEMENTATION)  # Only A needs this

# Library B depends on A
add_library(B STATIC b.cpp)
target_include_directories(B PRIVATE include/B)     # Only B needs this
target_link_libraries(B PUBLIC A)                   # B's users need A too

# Executable uses B
add_executable(app main.cpp)
target_link_libraries(app PRIVATE B)  # Automatically gets A's public includes!

# Result:
# - app gets: include/A (from A, through B)
# - app does NOT get: include/B (B's includes are PRIVATE)
# - app does NOT get: A_IMPLEMENTATION (A's private definitions)
```

#### **Complete Transitive Dependency Example:**

**lib/interface_lib/CMakeLists.txt:**
```cmake
# Base interface library
add_library(base_interface INTERFACE)
target_compile_features(base_interface INTERFACE cxx_std_17)
target_compile_definitions(base_interface INTERFACE BASE_LIBRARY=1)

# Core library that implements the interface
add_library(core STATIC core.cpp)
target_include_directories(core PUBLIC include/core)
target_link_libraries(core PUBLIC base_interface)

# Extended library that uses core
add_library(extended STATIC extended.cpp)
target_include_directories(extended 
    PUBLIC include/extended      # Users need these headers
    PRIVATE src/extended/private # Internal only
)
target_link_libraries(extended PUBLIC core)

# Final executable
add_executable(final_app main.cpp)
target_link_libraries(final_app PRIVATE extended)

# final_app automatically gets:
# - base_interface requirements (C++17, BASE_LIBRARY definition)
# - core's public includes
# - extended's public includes
# But NOT extended's private includes
```

### **Advanced Generator Expression Patterns**

#### **1. Multi-level Conditions:**
```cmake
target_compile_options(my_target PRIVATE
    # Only on Linux, with GCC, in Release mode
    $<$<AND:$<PLATFORM_ID:Linux>,$<CXX_COMPILER_ID:GNU>,$<CONFIG:Release>>:
        -flto
        -fuse-linker-plugin
    >
)
```

#### **2. Target Property Queries:**
```cmake
# Use property from another target
target_include_directories(my_target PRIVATE
    $<TARGET_PROPERTY:other_target,INTERFACE_INCLUDE_DIRECTORIES>
)

# Conditional based on target type
target_compile_definitions(my_target PRIVATE
    $<$<STREQUAL:$<TARGET_PROPERTY:TYPE>,SHARED_LIBRARY>:BUILDING_SHARED=1>
    $<$<STREQUAL:$<TARGET_PROPERTY:TYPE>,EXECUTABLE>:IS_EXECUTABLE=1>
)
```

#### **3. File Operations in Generator Expressions:**
```cmake
# Get the output directory of a target
set(output_dir "$<TARGET_FILE_DIR:${PROJECT_NAME}>")

# Get the full path to the target file
set(exe_path "$<TARGET_FILE:${PROJECT_NAME}>")

# Get the target name without extension
set(exe_name "$<TARGET_FILE_NAME:${PROJECT_NAME}>")

# Use in custom commands
add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E echo 
            "Built: $<TARGET_FILE:${PROJECT_NAME}>"
    COMMAND ${CMAKE_COMMAND} -E copy
            "$<TARGET_FILE:${PROJECT_NAME}>"
            "${CMAKE_BINARY_DIR}/dist/"
)
```

### **Debugging Generator Expressions**

Generator expressions can be tricky to debug. Use these techniques:

```cmake
# Method 1: Use file(GENERATE) to see expanded values
file(GENERATE OUTPUT debug.txt
    CONTENT "Config: $<CONFIG>\nCompiler: $<CXX_COMPILER_ID>"
)

# Method 2: Use custom target to print values
add_custom_target(debug_genex
    COMMAND ${CMAKE_COMMAND} -E echo 
            "Debug build: $<CONFIG:Debug>"
    COMMAND ${CMAKE_COMMAND} -E echo 
            "GCC compiler: $<CXX_COMPILER_ID:GNU>"
    COMMAND ${CMAKE_COMMAND} -E echo 
            "Full expression: $<IF:$<CONFIG:Debug>,Debug,NotDebug>"
)

# Method 3: Use message() with CMAKE_CFG_INTDIR
# Note: message() evaluates generator expressions at configure time,
# but some expressions only make sense at build time
```

### **Best Practices**

1. **Use target-specific commands** instead of global commands whenever possible
2. **Prefer generator expressions** over conditional `if()` statements for build-time variations
3. **Understand propagation**: Use `PUBLIC` for what users need, `PRIVATE` for internal use, `INTERFACE` for header-only
4. **Set VERSION and SOVERSION** for shared libraries
5. **Use aliases** for nicer target names: `add_library(Foo::bar ALIAS bar)`
6. **Consider using `target_compile_features()`** instead of setting CXX_STANDARD
7. **Test on multiple platforms** to ensure generator expressions work everywhere

### **Exercise: Create a Complex Target Hierarchy**

Create a project with:
1. A base interface library defining C++ standard and common definitions
2. Two static libraries with different properties
3. One shared library that depends on both static libraries
4. An executable that uses the shared library
5. Use generator expressions for:
   - Different optimization levels per build type
   - Platform-specific libraries
   - Compiler-specific warnings
6. Print the complete dependency chain using target properties

### **Next Steps Preview**
In **PART-6**, we'll cover:
- Project organization and directory structure
- Managing large projects with multiple subdirectories
- Exporting and importing targets
- Creating config packages for distribution

**To continue, please reply with "Next" or "PART-6" to receive the next part of the course.**



# **PART-6: Project Organization and Multi-Directory Projects**

### **Topic: Structuring Large Projects, Exporting Targets, and Creating Distributable Packages**

### **Introduction to Large-Scale Project Organization**

As projects grow, organizing code into logical directories becomes essential. CMake provides tools to manage complex projects with multiple subdirectories, each with their own CMakeLists.txt files.

### **Project Structure for This Part**

```
enterprise_app/
├── CMakeLists.txt              # Root
├── README.md
├── LICENSE
├── .gitignore
├── cmake/                      # Project-wide CMake modules
│   ├── CodeCoverage.cmake
│   ├── CompilerWarnings.cmake
│   └── Sanitizers.cmake
├── external/                   # Third-party dependencies
│   └── CMakeLists.txt
├── src/                        # Main source code
│   ├── CMakeLists.txt
│   ├── core/                   # Core library
│   │   ├── CMakeLists.txt
│   │   ├── include/
│   │   └── src/
│   ├── modules/                # Feature modules
│   │   ├── math/
│   │   ├── network/
│   │   └── ui/
│   └── app/                    # Main application
│       ├── CMakeLists.txt
│       └── src/
├── tests/                      # Test suite
│   ├── CMakeLists.txt
│   ├── unit/
│   ├── integration/
│   └── performance/
├── benchmarks/                 # Benchmarks
│   └── CMakeLists.txt
├── examples/                   # Example programs
│   └── CMakeLists.txt
├── docs/                       # Documentation
├── scripts/                    # Build scripts
└── packaging/                  # Packaging scripts
```

### **Root CMakeLists.txt - The Entry Point**

**enterprise_app/CMakeLists.txt:**
```cmake
cmake_minimum_required(VERSION 3.18)
project(EnterpriseApp VERSION 2.5.0
    DESCRIPTION "Enterprise-grade application with modular architecture"
    HOMEPAGE_URL "https://github.com/yourcompany/enterprise_app"
    LANGUAGES CXX C
)

# Set policies for modern CMake behavior
cmake_policy(SET CMP0077 NEW)  # option() honors normal variables
cmake_policy(SET CMP0092 NEW)  # MSVC runtime library flags

# Set project-wide variables before any subdirectories
set(PROJECT_VENDOR "YourCompany Inc.")
set(PROJECT_COPYRIGHT "Copyright (c) 2024 YourCompany Inc.")

# Add custom module path for project-specific CMake modules
list(APPEND CMAKE_MODULE_PATH
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake
    ${CMAKE_CURRENT_SOURCE_DIR}/external/cmake
)

# Project configuration options
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(BUILD_TESTS "Build tests" ON)
option(BUILD_BENCHMARKS "Build benchmarks" OFF)
option(BUILD_EXAMPLES "Build examples" ON)
option(ENABLE_COVERAGE "Enable code coverage" OFF)
option(ENABLE_SANITIZERS "Enable sanitizers (ASAN, UBSAN)" OFF)
option(ENABLE_LTO "Enable Link Time Optimization" OFF)
option(ENABLE_PCH "Enable precompiled headers" ON)
option(WARNINGS_AS_ERRORS "Treat warnings as errors" ON)

# Build type configuration
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING
        "Choose the build type: Debug Release RelWithDebInfo MinSizeRel"
        FORCE
    )
endif()

# C++ standard configuration
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Set default output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Include project-wide CMake modules
include(CompilerWarnings)
if(ENABLE_COVERAGE)
    include(CodeCoverage)
endif()
if(ENABLE_SANITIZERS)
    include(Sanitizers)
endif()

# Print configuration summary
message(STATUS "")
message(STATUS "==============================================")
message(STATUS "EnterpriseApp Configuration Summary")
message(STATUS "==============================================")
message(STATUS "Project:        ${PROJECT_NAME} v${PROJECT_VERSION}")
message(STATUS "Build type:     ${CMAKE_BUILD_TYPE}")
message(STATUS "Compiler:       ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "C++ Standard:   ${CMAKE_CXX_STANDARD}")
message(STATUS "Shared libs:    ${BUILD_SHARED_LIBS}")
message(STATUS "Build tests:    ${BUILD_TESTS}")
message(STATUS "Build examples: ${BUILD_EXAMPLES}")
message(STATUS "Coverage:       ${ENABLE_COVERAGE}")
message(STATUS "Sanitizers:     ${ENABLE_SANITIZERS}")
message(STATUS "LTO:            ${ENABLE_LTO}")
message(STATUS "PCH:            ${ENABLE_PCH}")
message(STATUS "Warnings as errors: ${WARNINGS_AS_ERRORS}")
message(STATUS "==============================================")
message(STATUS "")

# Add subdirectories in dependency order
add_subdirectory(external)      # Third-party deps first

add_subdirectory(src/core)      # Core library - no dependencies
add_subdirectory(src/modules)   # Feature modules (depend on core)
add_subdirectory(src/app)       # Main app (depends on everything)

if(BUILD_TESTS)
    add_subdirectory(tests)     # Tests
endif()

if(BUILD_BENCHMARKS)
    add_subdirectory(benchmarks) # Benchmarks
endif()

if(BUILD_EXAMPLES)
    add_subdirectory(examples)   # Examples
endif()

# Installation configuration (we'll cover this in detail later)
include(GNUInstallDirs)  # Standard GNU installation directories

set(INSTALL_CONFIGDIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")
```

### **Custom CMake Modules**

**cmake/CompilerWarnings.cmake:**
```cmake
# CompilerWarnings.cmake - Configures compiler warnings

function(set_target_warnings target)
    set(MSVC_WARNINGS
        /W4     # Baseline reasonable warnings
        /w14242 # 'identifier': conversion from 'type1' to 'type1', possible loss of data
        /w14254 # 'operator': conversion from 'type1:field_bits' to 'type2:field_bits', possible loss of data
        /w14263 # 'function': member function does not override any base class virtual member function
        /w14265 # 'classname': class has virtual functions, but destructor is not virtual
        /w14287 # 'operator': unsigned/negative constant mismatch
        /we4289 # nonstandard extension used: 'variable': loop control variable declared in the for-loop is used outside the for-loop scope
        /w14296 # 'operator': expression is always 'boolean_value'
        /w14311 # 'variable': pointer truncation from 'type1' to 'type2'
        /w14545 # expression before comma evaluates to a function which is missing an argument list
        /w14546 # function call before comma missing argument list
        /w14547 # 'operator': operator before comma has no effect; expected operator with side-effect
        /w14549 # 'operator': operator before comma has no effect; did you intend 'operator'?
        /w14555 # expression has no effect; expected expression with side-effect
        /w14619 # pragma warning: there is no warning number 'number'
        /w14640 # Enable warning on thread un-safe static member initialization
        /w14826 # Conversion from 'type1' to 'type_2' is sign-extended. This may cause unexpected runtime behavior.
        /w14905 # wide string literal cast to 'LPSTR'
        /w14906 # string literal cast to 'LPWSTR'
        /w14928 # illegal copy-initialization; more than one user-defined conversion has been implicitly applied
    )

    set(CLANG_WARNINGS
        -Wall
        -Wextra
        -Wpedantic
        -Wshadow
        -Wnon-virtual-dtor
        -Wold-style-cast
        -Wcast-align
        -Wunused
        -Woverloaded-virtual
        -Wpedantic
        -Wconversion
        -Wsign-conversion
        -Wnull-dereference
        -Wdouble-promotion
        -Wformat=2
        -Wimplicit-fallthrough
        -Wmisleading-indentation
        -Wduplicated-cond
        -Wduplicated-branches
        -Wlogical-op
        -Wuseless-cast
    )

    set(GCC_WARNINGS
        ${CLANG_WARNINGS}
        -Wmisleading-indentation
        -Wduplicated-cond
        -Wduplicated-branches
        -Wlogical-op
        -Wuseless-cast
        -Wrestrict
    )

    if(WARNINGS_AS_ERRORS)
        set(CLANG_WARNINGS ${CLANG_WARNINGS} -Werror)
        set(GCC_WARNINGS ${GCC_WARNINGS} -Werror)
        set(MSVC_WARNINGS ${MSVC_WARNINGS} /WX)
    endif()

    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        set(PROJECT_WARNINGS ${MSVC_WARNINGS})
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        set(PROJECT_WARNINGS ${CLANG_WARNINGS})
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        set(PROJECT_WARNINGS ${GCC_WARNINGS})
    else()
        message(AUTHOR_WARNING "No compiler warnings set for '${CMAKE_CXX_COMPILER_ID}' compiler.")
        return()
    endif()

    target_compile_options(${target} PRIVATE ${PROJECT_WARNINGS})
endfunction()
```

### **Core Library Module**

**src/core/CMakeLists.txt:**
```cmake
# Core library - foundation of the project

# Create the core library
add_library(core)

# Source files grouped by functionality
set(CORE_SOURCES
    src/utils/string_utils.cpp
    src/utils/file_utils.cpp
    src/utils/date_time.cpp
    
    src/math/vector.cpp
    src/math/matrix.cpp
    src/math/statistics.cpp
    
    src/containers/circular_buffer.cpp
    src/containers/flat_map.cpp
    
    src/logging/logger.cpp
    src/logging/formatter.cpp
    
    src/config/configuration.cpp
    src/config/parser.cpp
)

# Add sources to the target
target_sources(core PRIVATE ${CORE_SOURCES})

# Include directories
target_include_directories(core
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Public interface (what users of core need)
target_compile_definitions(core PUBLIC
    CORE_LIBRARY=1
    PROJECT_VERSION=${PROJECT_VERSION}
)

# Private implementation details
target_compile_definitions(core PRIVATE
    CORE_IMPLEMENTATION=1
)

# Set target properties
set_target_properties(core PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    OUTPUT_NAME "core"
    DEBUG_POSTFIX "d"
    CXX_STANDARD ${CMAKE_CXX_STANDARD}
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

# Apply compiler warnings
if(COMMAND set_target_warnings)
    set_target_warnings(core)
endif()

# Enable Position Independent Code if building shared libs
if(BUILD_SHARED_LIBS)
    set_target_properties(core PROPERTIES
        POSITION_INDEPENDENT_CODE ON
    )
    target_compile_definitions(core PRIVATE CORE_BUILDING_SHARED=1)
endif()

# Create alias for nicer usage
add_library(EnterpriseApp::core ALIAS core)

# Export for other subdirectories to use
set(CORE_FOUND TRUE PARENT_SCOPE)
```

### **Module Directory with Multiple Submodules**

**src/modules/CMakeLists.txt:**
```cmake
# Modules directory - contains feature modules

# Optionally include each module
option(BUILD_MATH_MODULE "Build math module" ON)
option(BUILD_NETWORK_MODULE "Build network module" ON)
option(BUILD_UI_MODULE "Build UI module" ON)

message(STATUS "Module configuration:")
message(STATUS "  Math module:    ${BUILD_MATH_MODULE}")
message(STATUS "  Network module: ${BUILD_NETWORK_MODULE}")
message(STATUS "  UI module:      ${BUILD_UI_MODULE}")

# Add subdirectories conditionally
if(BUILD_MATH_MODULE)
    add_subdirectory(math)
endif()

if(BUILD_NETWORK_MODULE)
    add_subdirectory(network)
endif()

if(BUILD_UI_MODULE)
    add_subdirectory(ui)
endif()
```

**src/modules/math/CMakeLists.txt:**
```cmake
# Math module - depends on core

# Check if core is available
if(NOT TARGET core)
    message(FATAL_ERROR "Math module requires core library")
endif()

# Create math module library
add_library(math_module)

# Source files
target_sources(math_module PRIVATE
    src/algebra.cpp
    src/calculus.cpp
    src/statistics_advanced.cpp
    src/random.cpp
)

# Include directories
target_include_directories(math_module
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include/enterprise_app/math>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Dependencies
target_link_libraries(math_module PUBLIC core)

# Compile definitions
target_compile_definitions(math_module PUBLIC
    MATH_MODULE=1
    MATH_VERSION=1.0
)

# Set properties
set_target_properties(math_module PROPERTIES
    VERSION 1.0.0
    SOVERSION 1
    OUTPUT_NAME "math"
    DEBUG_POSTFIX "d"
)

# Apply warnings
if(COMMAND set_target_warnings)
    set_target_warnings(math_module)
endif()

# Create alias
add_library(EnterpriseApp::math ALIAS math_module)

# Export target for parent scope
set(MATH_MODULE_FOUND TRUE PARENT_SCOPE)
```

### **Main Application**

**src/app/CMakeLists.txt:**
```cmake
# Main application executable

# Find all available modules
set(AVAILABLE_MODULES)
if(TARGET math_module)
    list(APPEND AVAILABLE_MODULES math_module)
endif()
if(TARGET network_module)
    list(APPEND AVAILABLE_MODULES network_module)
endif()
if(TARGET ui_module)
    list(APPEND AVAILABLE_MODULES ui_module)
endif()

message(STATUS "Linking application with modules: ${AVAILABLE_MODULES}")

# Create the executable
add_executable(enterprise_app)

# Add sources (could use GLOB but explicit is safer for large projects)
set(APP_SOURCES
    src/main.cpp
    src/application.cpp
    src/command_line.cpp
    src/configuration_manager.cpp
)

target_sources(enterprise_app PRIVATE ${APP_SOURCES})

# Include directories
target_include_directories(enterprise_app PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Link with core and all available modules
target_link_libraries(enterprise_app PRIVATE core)
if(AVAILABLE_MODULES)
    target_link_libraries(enterprise_app PRIVATE ${AVAILABLE_MODULES})
endif()

# Application-specific definitions
target_compile_definitions(enterprise_app PRIVATE
    APPLICATION_NAME="${PROJECT_NAME}"
    APPLICATION_VERSION="${PROJECT_VERSION}"
)

# Set executable properties
set_target_properties(enterprise_app PROPERTIES
    OUTPUT_NAME "enterprise_app"
    DEBUG_POSTFIX "d"
    WIN32_EXECUTABLE ON  # Windows: GUI application
    MACOSX_BUNDLE ON     # macOS: Create .app bundle
)

# Apply warnings
if(COMMAND set_target_warnings)
    set_target_warnings(enterprise_app)
endif()

# Installation (we'll cover this in detail later)
install(TARGETS enterprise_app
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    BUNDLE DESTINATION ${CMAKE_INSTALL_BINDIR}
)
```

### **Tests Directory Structure**

**tests/CMakeLists.txt:**
```cmake
# Tests configuration

# Only build tests if requested
if(NOT BUILD_TESTS)
    return()
endif()

# Find testing framework
find_package(GTest 1.11.0 QUIET)
if(NOT GTest_FOUND)
    message(STATUS "GTest not found, fetching via FetchContent")
    
    include(FetchContent)
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG release-1.12.1
    )
    FetchContent_MakeAvailable(googletest)
endif()

enable_testing()

# Add test subdirectories
add_subdirectory(unit)
add_subdirectory(integration)

# Add a test target that runs all tests
add_custom_target(run_all_tests
    COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
    DEPENDS unit_tests integration_tests
    COMMENT "Running all tests"
)
```

**tests/unit/CMakeLists.txt:**
```cmake
# Unit tests

# Core library tests
add_executable(core_tests
    core/test_string_utils.cpp
    core/test_file_utils.cpp
    core/test_containers.cpp
)

target_link_libraries(core_tests PRIVATE
    core
    GTest::gtest_main
)

add_test(NAME core_tests COMMAND core_tests)

# Math module tests (if built)
if(TARGET math_module)
    add_executable(math_tests
        math/test_algebra.cpp
        math/test_calculus.cpp
    )
    
    target_link_libraries(math_tests PRIVATE
        math_module
        GTest::gtest_main
    )
    
    add_test(NAME math_tests COMMAND math_tests)
endif()

# Combine all unit tests into one target
add_custom_target(unit_tests
    DEPENDS core_tests
    COMMENT "Building unit tests"
)

if(TARGET math_tests)
    add_dependencies(unit_tests math_tests)
endif()
```

### **External Dependencies Management**

**external/CMakeLists.txt:**
```cmake
# External dependencies management

# Option to use system libraries or bundled versions
option(USE_SYSTEM_LIBS "Use system-installed libraries" OFF)

# Add custom find modules path
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

if(USE_SYSTEM_LIBS)
    # Try to find system libraries
    find_package(JSON 3.10 QUIET)
    find_package(fmt 8.0 QUIET)
    find_package(spdlog 1.10 QUIET)
else()
    # Use FetchContent for bundled dependencies
    include(FetchContent)
    
    # JSON library (header-only)
    FetchContent_Declare(
        nlohmann_json
        GIT_REPOSITORY https://github.com/nlohmann/json.git
        GIT_TAG v3.11.2
    )
    
    # fmt library (formatting)
    FetchContent_Declare(
        fmt
        GIT_REPOSITORY https://github.com/fmtlib/fmt.git
        GIT_TAG 9.1.0
    )
    
    # spdlog (logging)
    FetchContent_Declare(
        spdlog
        GIT_REPOSITORY https://github.com/gabime/spdlog.git
        GIT_TAG v1.12.0
    )
    
    # Make all available at once
    FetchContent_MakeAvailable(nlohmann_json fmt spdlog)
endif()

# Export variables for parent scope
set(JSON_AVAILABLE TRUE PARENT_SCOPE)
set(FMT_AVAILABLE TRUE PARENT_SCOPE)
set(SPDLOG_AVAILABLE TRUE PARENT_SCOPE)
```

### **Exporting and Installing Targets**

One of CMake's most powerful features is the ability to export targets so other projects can use them with `find_package()`.

**Creating an Export Configuration:**

Add to the root CMakeLists.txt:
```cmake
# At the end of the root CMakeLists.txt:

# Export configuration for install tree
install(TARGETS
    core
    math_module
    network_module
    ui_module
    enterprise_app
    EXPORT EnterpriseAppTargets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# Install headers
install(DIRECTORY src/core/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/enterprise_app/core
)

install(DIRECTORY src/modules/math/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/enterprise_app/math
)

# Export targets to build tree (for use without installation)
export(EXPORT EnterpriseAppTargets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/EnterpriseAppTargets.cmake"
)

# Generate and install the package configuration files
include(CMakePackageConfigHelpers)

# Generate the package configuration file
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/EnterpriseAppConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/EnterpriseAppConfig.cmake
    INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
)

# Generate the package version file
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/EnterpriseAppConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

# Install the export and configuration files
install(EXPORT EnterpriseAppTargets
    FILE EnterpriseAppTargets.cmake
    NAMESPACE EnterpriseApp::
    DESTINATION ${INSTALL_CONFIGDIR}
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/EnterpriseAppConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/EnterpriseAppConfigVersion.cmake
    DESTINATION ${INSTALL_CONFIGDIR}
)
```

**cmake/EnterpriseAppConfig.cmake.in:**
```cmake
@PACKAGE_INIT@

# Include the targets file
include("${CMAKE_CURRENT_LIST_DIR}/EnterpriseAppTargets.cmake")

# Provide find_dependency calls for our dependencies
include(CMakeFindDependencyMacro)

# Check for required dependencies
find_dependency(Threads)

# Set variables for use by dependent projects
set(EnterpriseApp_VERSION @PROJECT_VERSION@)
set(EnterpriseApp_INCLUDE_DIRS "@PACKAGE_INCLUDE_INSTALL_DIR@")
set(EnterpriseApp_LIBRARY_DIRS "@PACKAGE_LIB_INSTALL_DIR@")
set(EnterpriseApp_BINARY_DIRS "@PACKAGE_BIN_INSTALL_DIR@")

# Provide a helper function to easily use the package
function(enterprise_app_setup target)
    if(NOT TARGET ${target})
        message(FATAL_ERROR "enterprise_app_setup called with non-existent target: ${target}")
    endif()
    
    target_compile_features(${target} PRIVATE cxx_std_17)
    
    if(EnterpriseApp_USE_STATIC_LIBS)
        target_compile_definitions(${target} PRIVATE ENTERPRISE_APP_STATIC)
    endif()
    
    message(STATUS "Configured ${target} to use EnterpriseApp v${EnterpriseApp_VERSION}")
endfunction()
```

### **Using the Exported Package from Another Project**

```cmake
# Another project's CMakeLists.txt using our exported package

cmake_minimum_required(VERSION 3.18)
project(AnotherProject VERSION 1.0.0)

# Find our package
find_package(EnterpriseApp 2.5.0 REQUIRED)

# Create an executable
add_executable(another_app main.cpp)

# Link with our library using the namespace
target_link_libraries(another_app PRIVATE EnterpriseApp::core)

# Use the helper function
enterprise_app_setup(another_app)
```

### **Managing Build Variants**

For complex projects, you might need multiple build variants (e.g., with different feature sets).

**Creating Build Variants:**

```cmake
# Define build variants
set(BUILD_VARIANTS
    "minimal"     # Core only
    "standard"    # Core + essential modules
    "full"        # Everything
    "development" # Everything + tests + examples
)

set(DEFAULT_VARIANT "standard" CACHE STRING "Build variant")
set_property(CACHE DEFAULT_VARIANT PROPERTY STRINGS ${BUILD_VARIANTS})

# Configure based on variant
if(DEFAULT_VARIANT STREQUAL "minimal")
    set(BUILD_TESTS OFF)
    set(BUILD_EXAMPLES OFF)
    set(BUILD_MATH_MODULE OFF)
    set(BUILD_NETWORK_MODULE OFF)
    set(BUILD_UI_MODULE OFF)
    
elseif(DEFAULT_VARIANT STREQUAL "standard")
    set(BUILD_TESTS OFF)
    set(BUILD_EXAMPLES OFF)
    set(BUILD_MATH_MODULE ON)
    set(BUILD_NETWORK_MODULE ON)
    set(BUILD_UI_MODULE OFF)
    
elseif(DEFAULT_VARIANT STREQUAL "full")
    set(BUILD_TESTS OFF)
    set(BUILD_EXAMPLES ON)
    set(BUILD_MATH_MODULE ON)
    set(BUILD_NETWORK_MODULE ON)
    set(BUILD_UI_MODULE ON)
    
elseif(DEFAULT_VARIANT STREQUAL "development")
    set(BUILD_TESTS ON)
    set(BUILD_EXAMPLES ON)
    set(BUILD_MATH_MODULE ON)
    set(BUILD_NETWORK_MODULE ON)
    set(BUILD_UI_MODULE ON)
    set(ENABLE_COVERAGE ON)
    set(ENABLE_SANITIZERS ON)
endif()
```

### **Cross-Platform Considerations**

**Handling Platform Differences:**

```cmake
# Platform-specific configurations
if(WIN32)
    # Windows-specific settings
    add_definitions(-D_WIN32_WINNT=0x0A00)  # Windows 10
    add_definitions(-DNOMINMAX)              # Disable min/max macros
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    
    # Windows SDK version
    set(WINDOWS_TARGET_VERSION "10.0.19041.0" CACHE STRING "Windows Target Version")
    
elseif(APPLE)
    # macOS-specific settings
    set(CMAKE_MACOSX_RPATH ON)
    set(CMAKE_INSTALL_RPATH "@loader_path/../lib")
    
    # macOS version targeting
    set(CMAKE_OSX_DEPLOYMENT_TARGET "11.0" CACHE STRING "Minimum macOS version")
    
elseif(UNIX AND NOT APPLE)
    # Linux-specific settings
    set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib:$ORIGIN/../lib64")
    
    # Linux distribution specifics
    if(EXISTS "/etc/debian_version")
        # Debian/Ubuntu specific
        add_definitions(-DDEBIAN)
    elseif(EXISTS "/etc/redhat-release")
        # RedHat/Fedora specific
        add_definitions(-DREDHAT)
    endif()
endif()

# Architecture-specific optimizations
if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64|AMD64")
    # x86-64 specific optimizations
    option(ENABLE_AVX "Enable AVX instructions" ON)
    option(ENABLE_AVX2 "Enable AVX2 instructions" ON)
    
    if(ENABLE_AVX2)
        add_compile_options(-mavx2)
    elseif(ENABLE_AVX)
        add_compile_options(-mavx)
    endif()
    
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64|ARM64")
    # ARM64 specific optimizations
    option(ENABLE_NEON "Enable NEON instructions" ON)
    
    if(ENABLE_NEON)
        add_compile_options(-mfpu=neon)
    endif()
endif()
```

### **Build Scripts and Automation**

**scripts/build.sh (Linux/macOS):**
```bash
#!/bin/bash
set -e

# Configuration
BUILD_TYPE="RelWithDebInfo"
BUILD_DIR="build"
INSTALL_DIR="install"
NUM_JOBS=$(nproc)

# Parse command line arguments
while [[ $# -gt 0 ]]; do
    case $1 in
        --debug)
            BUILD_TYPE="Debug"
            shift
            ;;
        --release)
            BUILD_TYPE="Release"
            shift
            ;;
        --clean)
            echo "Cleaning build directory..."
            rm -rf ${BUILD_DIR}
            shift
            ;;
        --install)
            DO_INSTALL=1
            shift
            ;;
        --variant)
            VARIANT="$2"
            shift 2
            ;;
        *)
            echo "Unknown option: $1"
            echo "Usage: $0 [--debug|--release] [--clean] [--install] [--variant minimal|standard|full|development]"
            exit 1
            ;;
    esac
done

# Create build directory
mkdir -p ${BUILD_DIR}
cd ${BUILD_DIR}

# Configure
echo "Configuring with build type: ${BUILD_TYPE}"
cmake .. \
    -DCMAKE_BUILD_TYPE=${BUILD_TYPE} \
    -DCMAKE_INSTALL_PREFIX=../${INSTALL_DIR} \
    ${VARIANT:+-DDEFAULT_VARIANT=${VARIANT}} \
    -DBUILD_TESTS=ON \
    -DBUILD_EXAMPLES=ON

# Build
echo "Building with ${NUM_JOBS} jobs..."
cmake --build . --parallel ${NUM_JOBS}

# Run tests
echo "Running tests..."
ctest --output-on-failure

# Install if requested
if [[ ${DO_INSTALL} -eq 1 ]]; then
    echo "Installing to ${INSTALL_DIR}..."
    cmake --install .
fi

echo "Build completed successfully!"
```

### **Best Practices for Large Projects**

1. **Modular Design**: Split into logical modules with clear dependencies
2. **Consistent Naming**: Use consistent naming conventions for targets and variables
3. **Version Control**: Track CMakeLists.txt files and keep them minimal
4. **Documentation**: Document the project structure and build options
5. **Incremental Builds**: Ensure changes don't trigger unnecessary rebuilds
6. **Test Integration**: Make tests easy to run and maintain
7. **CI/CD Ready**: Structure for continuous integration
8. **Package Management**: Provide easy ways to use your project as a dependency

### **Exercise: Organize Your Own Project**

Take an existing project or create a new one with:

1. At least 3 logical modules with dependencies
2. Separate test directory with unit and integration tests
3. External dependencies managed via FetchContent
4. Build variants (minimal, full, development)
5. Export configuration for use by other projects
6. Platform-specific considerations
7. Build scripts for common tasks

### **Next Steps Preview**
In **PART-7**, we'll cover:
- Cross-compilation for different architectures
- Toolchain files and platform-specific builds
- Building for embedded systems
- Android/iOS builds with CMake
- Qt integration and GUI applications

**To continue, please reply with "Next" or "PART-7" to receive the next part of the course.**



# **PART-7: Cross-Compilation, Toolchains, and Multi-Platform Builds**

### **Topic: Building for Different Architectures, Embedded Systems, and Mobile Platforms**

### **Introduction to Cross-Compilation**

Cross-compilation is the process of building code on one system (the **host**) that will run on a different system (the **target**). This is essential for:
- Embedded systems (ARM, AVR, ESP32)
- Mobile platforms (Android, iOS)
- Different operating systems
- Specialized hardware

### **Understanding Toolchain Files**

Toolchain files tell CMake about the target system and how to compile for it. They set critical variables like the compiler, linker, and system information.

### **Project Structure for This Part**

```
cross_platform_project/
├── CMakeLists.txt
├── toolchains/
│   ├── arm-linux-gnueabihf.cmake
│   ├── android-arm64.cmake
│   ├── ios.cmake
│   └── wasm.cmake
├── src/
│   ├── common/
│   └── platform/
├── tests/
└── build/
```

### **Simple Cross-Compilation Example: ARM Linux**

**toolchains/arm-linux-gnueabihf.cmake:**
```cmake
# ARM Linux toolchain for Raspberry Pi, BeagleBone, etc.

# Target system
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

# Specify the cross compiler
set(CMAKE_C_COMPILER /usr/bin/arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER /usr/bin/arm-linux-gnueabihf-g++)

# Where to find target libraries and headers
set(CMAKE_FIND_ROOT_PATH /usr/arm-linux-gnueabihf)

# Adjust the default behavior of FIND_XXX() commands:
# Search for programs only in the build host directories
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

# Search for libraries and headers only in the target directories
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

# Target-specific compile flags
set(ARM_FLAGS "-march=armv7-a -mfpu=neon-vfpv4 -mfloat-abi=hard")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${ARM_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${ARM_FLAGS}")

# RPATH handling
set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib")
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

# Tell CMake we're cross-compiling
set(CMAKE_CROSSCOMPILING TRUE)

# System version (optional)
set(CMAKE_SYSTEM_VERSION 1)
```

**CMakeLists.txt for a cross-platform project:**
```cmake
cmake_minimum_required(VERSION 3.18)
project(CrossPlatformDemo VERSION 1.0.0 LANGUAGES C CXX)

# Detect if we're cross-compiling
if(CMAKE_CROSSCOMPILING)
    message(STATUS "Cross-compiling for ${CMAKE_SYSTEM_NAME} on ${CMAKE_SYSTEM_PROCESSOR}")
else()
    message(STATUS "Native compilation on ${CMAKE_SYSTEM_NAME}")
endif()

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Platform detection
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    message(STATUS "Target: Linux")
    set(PLATFORM_LINUX 1)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    message(STATUS "Target: Windows")
    set(PLATFORM_WINDOWS 1)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    message(STATUS "Target: macOS/iOS")
    set(PLATFORM_APPLE 1)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Android")
    message(STATUS "Target: Android")
    set(PLATFORM_ANDROID 1)
else()
    message(STATUS "Target: ${CMAKE_SYSTEM_NAME} (unspecified)")
endif()

# Architecture detection
if(CMAKE_SYSTEM_PROCESSOR MATCHES "arm" OR CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
    set(ARCH_ARM 1)
    message(STATUS "Architecture: ARM (${CMAKE_SYSTEM_PROCESSOR})")
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "x86" OR CMAKE_SYSTEM_PROCESSOR MATCHES "AMD64")
    set(ARCH_X86 1)
    message(STATUS "Architecture: x86 (${CMAKE_SYSTEM_PROCESSOR})")
else()
    message(STATUS "Architecture: ${CMAKE_SYSTEM_PROCESSOR}")
endif()

# Platform-specific configuration
if(PLATFORM_LINUX AND ARCH_ARM)
    message(STATUS "Building for ARM Linux (e.g., Raspberry Pi)")
    set(PLATFORM_ARM_LINUX 1)
    add_definitions(-DPLATFORM_ARM_LINUX)
endif()

# Create a simple library that works on all platforms
add_library(platform_utils STATIC
    src/common/utils.cpp
)

# Platform-specific source files
if(PLATFORM_LINUX)
    target_sources(platform_utils PRIVATE
        src/platform/linux/platform_linux.cpp
    )
    target_compile_definitions(platform_utils PRIVATE PLATFORM_LINUX)
    
elseif(PLATFORM_WINDOWS)
    target_sources(platform_utils PRIVATE
        src/platform/windows/platform_windows.cpp
    )
    target_compile_definitions(platform_utils PRIVATE PLATFORM_WINDOWS)
    
elseif(PLATFORM_ANDROID)
    target_sources(platform_utils PRIVATE
        src/platform/android/platform_android.cpp
    )
    target_compile_definitions(platform_utils PRIVATE PLATFORM_ANDROID)
    
elseif(PLATFORM_APPLE)
    target_sources(platform_utils PRIVATE
        src/platform/apple/platform_apple.cpp
    )
    target_compile_definitions(platform_utils PRIVATE PLATFORM_APPLE)
endif()

# Include directories
target_include_directories(platform_utils PUBLIC
    src/common
    src/platform
)

# Create an executable
add_executable(cross_platform_demo
    src/main.cpp
)

# Link with our platform utils
target_link_libraries(cross_platform_demo PRIVATE platform_utils)

# Platform-specific libraries
if(PLATFORM_LINUX)
    target_link_libraries(cross_platform_demo PRIVATE pthread dl)
    
elseif(PLATFORM_WINDOWS)
    target_link_libraries(cross_platform_demo PRIVATE ws2_32)
    
elseif(PLATFORM_ANDROID)
    target_link_libraries(cross_platform_demo PRIVATE log android)
    
elseif(PLATFORM_APPLE)
    # macOS/iOS frameworks if needed
    if(IOS)
        target_link_libraries(cross_platform_demo PRIVATE
            "-framework Foundation"
            "-framework UIKit"
        )
    else()
        target_link_libraries(cross_platform_demo PRIVATE
            "-framework Foundation"
            "-framework AppKit"
        )
    endif()
endif()
```

### **Complete Example Source Files**

**src/common/utils.h:**
```cpp
#ifndef UTILS_H
#define UTILS_H

#include <string>
#include <vector>

// Platform detection for header
#if defined(_WIN32) || defined(_WIN64)
    #define PLATFORM_WINDOWS 1
#elif defined(__linux__)
    #define PLATFORM_LINUX 1
#elif defined(__APPLE__) && defined(__MACH__)
    #include "TargetConditionals.h"
    #if TARGET_OS_IPHONE
        #define PLATFORM_IOS 1
    #else
        #define PLATFORM_MACOS 1
    #endif
#elif defined(__ANDROID__)
    #define PLATFORM_ANDROID 1
#endif

// Architecture detection
#if defined(__arm__) || defined(__aarch64__)
    #define ARCH_ARM 1
#elif defined(__i386__) || defined(__x86_64__)
    #define ARCH_X86 1
#endif

namespace platform_utils {
    
    // Cross-platform functions
    std::string get_platform_name();
    std::string get_architecture();
    std::string get_system_info();
    
    // Memory information (platform-specific implementation)
    size_t get_total_memory();
    size_t get_available_memory();
    
    // CPU information
    int get_cpu_count();
    std::vector<double> get_cpu_load(int samples = 3);
    
    // File system
    std::string get_home_directory();
    std::string get_temp_directory();
    
} // namespace platform_utils

#endif // UTILS_H
```

**src/common/utils.cpp:**
```cpp
#include "utils.h"
#include <sstream>
#include <iomanip>

namespace platform_utils {
    
    std::string get_platform_name() {
        #ifdef PLATFORM_WINDOWS
            return "Windows";
        #elif defined(PLATFORM_LINUX)
            return "Linux";
        #elif defined(PLATFORM_MACOS)
            return "macOS";
        #elif defined(PLATFORM_IOS)
            return "iOS";
        #elif defined(PLATFORM_ANDROID)
            return "Android";
        #else
            return "Unknown";
        #endif
    }
    
    std::string get_architecture() {
        #ifdef ARCH_ARM
            #ifdef __aarch64__
                return "ARM64";
            #else
                return "ARM32";
            #endif
        #elif defined(ARCH_X86)
            #ifdef __x86_64__
                return "x86-64";
            #else
                return "x86";
            #endif
        #else
            return "Unknown";
        #endif
    }
    
    std::string get_system_info() {
        std::stringstream ss;
        ss << get_platform_name() << " on " << get_architecture();
        return ss.str();
    }
    
    // Default implementations that will be overridden by platform-specific code
    size_t get_total_memory() {
        return 0; // Platform-specific
    }
    
    size_t get_available_memory() {
        return 0; // Platform-specific
    }
    
    int get_cpu_count() {
        return 1; // Platform-specific
    }
    
    std::vector<double> get_cpu_load(int samples) {
        return std::vector<double>(samples, 0.0); // Platform-specific
    }
    
    std::string get_home_directory() {
        return ""; // Platform-specific
    }
    
    std::string get_temp_directory() {
        return ""; // Platform-specific
    }
    
} // namespace platform_utils
```

**src/platform/linux/platform_linux.cpp:**
```cpp
#include "../utils.h"
#include <unistd.h>
#include <sys/sysinfo.h>
#include <pwd.h>
#include <cstdlib>
#include <fstream>
#include <sstream>

namespace platform_utils {
    
    size_t get_total_memory() {
        struct sysinfo info;
        if (sysinfo(&info) == 0) {
            return info.totalram * info.mem_unit;
        }
        return 0;
    }
    
    size_t get_available_memory() {
        struct sysinfo info;
        if (sysinfo(&info) == 0) {
            return info.freeram * info.mem_unit;
        }
        return 0;
    }
    
    int get_cpu_count() {
        return sysconf(_SC_NPROCESSORS_ONLN);
    }
    
    std::vector<double> get_cpu_load(int samples) {
        std::vector<double> loads;
        loads.reserve(samples);
        
        for (int i = 0; i < samples; ++i) {
            std::ifstream proc_stat("/proc/stat");
            std::string line;
            if (std::getline(proc_stat, line)) {
                std::istringstream iss(line);
                std::string cpu_label;
                long user, nice, system, idle, iowait, irq, softirq, steal, guest, guest_nice;
                
                iss >> cpu_label >> user >> nice >> system >> idle >> iowait 
                    >> irq >> softirq >> steal >> guest >> guest_nice;
                
                if (cpu_label == "cpu") {
                    long total = user + nice + system + idle + iowait + irq + softirq + steal;
                    long idle_total = idle + iowait;
                    double load = (total > 0) ? (1.0 - static_cast<double>(idle_total) / total) : 0.0;
                    loads.push_back(load * 100.0); // Convert to percentage
                }
            }
            
            if (i < samples - 1) {
                sleep(1); // Wait 1 second between samples
            }
        }
        
        return loads;
    }
    
    std::string get_home_directory() {
        const char* home = getenv("HOME");
        if (home) return home;
        
        // Fallback to password database
        struct passwd* pw = getpwuid(getuid());
        if (pw && pw->pw_dir) {
            return pw->pw_dir;
        }
        
        return "";
    }
    
    std::string get_temp_directory() {
        const char* tmpdir = getenv("TMPDIR");
        if (tmpdir) return tmpdir;
        
        return "/tmp";
    }
    
} // namespace platform_utils
```

**src/main.cpp:**
```cpp
#include "common/utils.h"
#include <iostream>
#include <iomanip>

int main() {
    using namespace platform_utils;
    
    std::cout << "=== Cross-Platform System Information ===" << std::endl;
    std::cout << std::endl;
    
    // Platform information
    std::cout << "Platform: " << get_platform_name() << std::endl;
    std::cout << "Architecture: " << get_architecture() << std::endl;
    std::cout << "Full System: " << get_system_info() << std::endl;
    std::cout << std::endl;
    
    // Memory information
    size_t total_mem = get_total_memory();
    size_t avail_mem = get_available_memory();
    
    if (total_mem > 0) {
        std::cout << "Total Memory: " 
                  << std::fixed << std::setprecision(2)
                  << (total_mem / (1024.0 * 1024.0 * 1024.0)) << " GB" << std::endl;
        
        std::cout << "Available Memory: " 
                  << (avail_mem / (1024.0 * 1024.0 * 1024.0)) << " GB" << std::endl;
        
        double used_percent = (total_mem - avail_mem) * 100.0 / total_mem;
        std::cout << "Memory Usage: " << used_percent << "%" << std::endl;
    } else {
        std::cout << "Memory information not available" << std::endl;
    }
    std::cout << std::endl;
    
    // CPU information
    int cpu_count = get_cpu_count();
    std::cout << "CPU Cores: " << cpu_count << std::endl;
    
    auto cpu_loads = get_cpu_load(3);
    if (!cpu_loads.empty()) {
        std::cout << "CPU Load (%): ";
        for (size_t i = 0; i < cpu_loads.size(); ++i) {
            if (i > 0) std::cout << ", ";
            std::cout << std::fixed << std::setprecision(1) << cpu_loads[i];
        }
        std::cout << std::endl;
    }
    std::cout << std::endl;
    
    // Directories
    std::cout << "Home Directory: " << get_home_directory() << std::endl;
    std::cout << "Temp Directory: " << get_temp_directory() << std::endl;
    
    std::cout << std::endl << "Program completed successfully!" << std::endl;
    
    return 0;
}
```

### **Building for Different Targets**

```bash
# Native build (on your current system)
mkdir build-native && cd build-native
cmake ..
cmake --build .

# Cross-compile for ARM Linux
mkdir build-arm && cd build-arm
cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchains/arm-linux-gnueabihf.cmake
cmake --build .

# Cross-compile for Windows from Linux (using Mingw)
mkdir build-windows && cd build-windows
cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchains/windows-mingw.cmake
cmake --build .
```

### **Advanced Toolchain: Android**

**toolchains/android-arm64.cmake:**
```cmake
# Android ARM64 toolchain

# Minimum CMake version
cmake_minimum_required(VERSION 3.18)

# Set Android NDK path (can be overridden from command line)
if(NOT ANDROID_NDK)
    set(ANDROID_NDK $ENV{ANDROID_NDK_HOME})
    if(NOT ANDROID_NDK)
        set(ANDROID_NDK $ENV{ANDROID_NDK_ROOT})
    endif()
    if(NOT ANDROID_NDK)
        message(FATAL_ERROR "ANDROID_NDK not set. Please set ANDROID_NDK or ANDROID_NDK_HOME environment variable.")
    endif()
endif()

# Target Android API level
if(NOT ANDROID_PLATFORM)
    set(ANDROID_PLATFORM "android-24")
endif()

# Architecture
set(ANDROID_ABI "arm64-v8a")
set(CMAKE_ANDROID_ARCH_ABI ${ANDROID_ABI})

# System
set(CMAKE_SYSTEM_NAME Android)
set(CMAKE_SYSTEM_VERSION ${ANDROID_PLATFORM})
set(CMAKE_ANDROID_NDK ${ANDROID_NDK})
set(CMAKE_ANDROID_STL_TYPE "c++_shared")

# Compiler
set(CMAKE_ANDROID_NDK_TOOLCHAIN_VERSION "clang")
set(toolchain "${ANDROID_NDK}/toolchains/llvm/prebuilt/linux-x86_64")

set(CMAKE_C_COMPILER "${toolchain}/bin/aarch64-linux-android${ANDROID_PLATFORM#android-}-clang")
set(CMAKE_CXX_COMPILER "${toolchain}/bin/aarch64-linux-android${ANDROID_PLATFORM#android-}-clang++")

# Compile flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIE -fPIC")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIE -fPIC -std=c++11")

# Linker flags
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -pie")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS}")

# Find paths
set(CMAKE_FIND_ROOT_PATH "${toolchain}/sysroot")
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

# Cross-compiling flag
set(CMAKE_CROSSCOMPILING TRUE)

# Android specific definitions
add_definitions(-DANDROID -D__ANDROID_API__=${ANDROID_PLATFORM#android-})
```

### **iOS and macOS Universal Binaries**

**toolchains/ios.cmake:**
```cmake
# iOS toolchain for cross-compilation

# This toolchain file works for iOS, tvOS, and watchOS
set(IOS_PLATFORM "OS" CACHE STRING "iOS platform: OS, SIMULATOR, TVOS, SIMULATOR_TVOS, WATCHOS, SIMULATOR_WATCHOS")

# Determine platform
if(IOS_PLATFORM STREQUAL "OS")
    set(IOS_PLATFORM_NAME "iphoneos")
    set(CMAKE_OSX_SYSROOT "iphoneos")
    set(CMAKE_OSX_ARCHITECTURES "arm64;armv7")
    set(IOS_DEPLOYMENT_TARGET "12.0")
    
elseif(IOS_PLATFORM STREQUAL "SIMULATOR")
    set(IOS_PLATFORM_NAME "iphonesimulator")
    set(CMAKE_OSX_SYSROOT "iphonesimulator")
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")
    set(IOS_DEPLOYMENT_TARGET "12.0")
    
elseif(IOS_PLATFORM STREQUAL "TVOS")
    set(IOS_PLATFORM_NAME "appletvos")
    set(CMAKE_OSX_SYSROOT "appletvos")
    set(CMAKE_OSX_ARCHITECTURES "arm64")
    set(IOS_DEPLOYMENT_TARGET "12.0")
    
elseif(IOS_PLATFORM STREQUAL "SIMULATOR_TVOS")
    set(IOS_PLATFORM_NAME "appletvsimulator")
    set(CMAKE_OSX_SYSROOT "appletvsimulator")
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")
    set(IOS_DEPLOYMENT_TARGET "12.0")
    
elseif(IOS_PLATFORM STREQUAL "WATCHOS")
    set(IOS_PLATFORM_NAME "watchos")
    set(CMAKE_OSX_SYSROOT "watchos")
    set(CMAKE_OSX_ARCHITECTURES "armv7k;arm64_32")
    set(IOS_DEPLOYMENT_TARGET "5.0")
    
elseif(IOS_PLATFORM STREQUAL "SIMULATOR_WATCHOS")
    set(IOS_PLATFORM_NAME "watchsimulator")
    set(CMAKE_OSX_SYSROOT "watchsimulator")
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64")
    set(IOS_DEPLOYMENT_TARGET "5.0")
endif()

# Set system variables
set(CMAKE_SYSTEM_NAME Darwin)
set(CMAKE_SYSTEM_VERSION 1)
set(UNIX TRUE)
set(APPLE TRUE)
set(IOS TRUE)

# Set compilers (Xcode)
set(CMAKE_C_COMPILER /usr/bin/clang)
set(CMAKE_CXX_COMPILER /usr/bin/clang++)

# Compile flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -arch ${CMAKE_OSX_ARCHITECTURES} -isysroot ${CMAKE_OSX_SYSROOT}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -arch ${CMAKE_OSX_ARCHITECTURES} -isysroot ${CMAKE_OSX_SYSROOT} -stdlib=libc++")

# Linker flags
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -arch ${CMAKE_OSX_ARCHITECTURES}")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -arch ${CMAKE_OSX_ARCHITECTURES}")

# Find behavior
set(CMAKE_FIND_ROOT_PATH ${CMAKE_OSX_SYSROOT})
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

# Deployment target
set(CMAKE_OSX_DEPLOYMENT_TARGET ${IOS_DEPLOYMENT_TARGET})

# Cross-compiling
set(CMAKE_CROSSCOMPILING TRUE)
```

### **WebAssembly with Emscripten**

**toolchains/wasm.cmake:**
```cmake
# WebAssembly toolchain using Emscripten

# Find Emscripten
if(NOT EMSCRIPTEN_ROOT_PATH)
    set(EMSCRIPTEN_ROOT_PATH $ENV{EMSCRIPTEN})
    if(NOT EMSCRIPTEN_ROOT_PATH)
        message(FATAL_ERROR "EMSCRIPTEN_ROOT_PATH not set. Please set EMSCRIPTEN environment variable or pass -DEMSCRIPTEN_ROOT_PATH")
    endif()
endif()

# System
set(CMAKE_SYSTEM_NAME Emscripten)
set(CMAKE_SYSTEM_VERSION 1)

# Compilers
set(CMAKE_C_COMPILER "${EMSCRIPTEN_ROOT_PATH}/emcc")
set(CMAKE_CXX_COMPILER "${EMSCRIPTEN_ROOT_PATH}/em++")
set(CMAKE_AR "${EMSCRIPTEN_ROOT_PATH}/emar")
set(CMAKE_RANLIB "${EMSCRIPTEN_ROOT_PATH}/emranlib")

# Compile flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -s WASM=1")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -s WASM=1 -std=c++11")

# Linker flags for different output types
option(EMSCRIPTEN_BUILD_EXECUTABLE "Build as executable (HTML)" ON)
option(EMSCRIPTEN_BUILD_LIBRARY "Build as library (JS)" OFF)

if(EMSCRIPTEN_BUILD_EXECUTABLE)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -s WASM=1 -s EXPORTED_RUNTIME_METHODS=['ccall','cwrap']")
elseif(EMSCRIPTEN_BUILD_LIBRARY)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -s WASM=1 -s SIDE_MODULE=1")
endif()

# Find behavior
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

# Cross-compiling
set(CMAKE_CROSSCOMPILING TRUE)

# Emscripten specific
set(EMSCRIPTEN 1)
add_definitions(-DEMSCRIPTEN)
```

### **Complete Cross-Platform CMakeLists.txt with Feature Detection**

```cmake
cmake_minimum_required(VERSION 3.18)
project(UniversalProject VERSION 1.0.0 LANGUAGES C CXX ASM)

# Option to enable platform-specific features
option(ENABLE_NEON "Enable ARM NEON optimizations" OFF)
option(ENABLE_AVX "Enable x86 AVX optimizations" OFF)
option(ENABLE_SIMD "Enable platform-specific SIMD" ON)
option(ENABLE_THREADS "Enable threading support" ON)
option(ENABLE_OPENMP "Enable OpenMP parallelization" OFF)

# Detect platform and architecture
message(STATUS "System: ${CMAKE_SYSTEM_NAME}")
message(STATUS "Processor: ${CMAKE_SYSTEM_PROCESSOR}")
message(STATUS "Cross-compiling: ${CMAKE_CROSSCOMPILING}")

# Set C++ standard with fallbacks
if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 11)
endif()
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Platform-specific configurations
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    # Linux-specific settings
    add_definitions(-DPLATFORM_LINUX=1)
    
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "arm")
        # ARM Linux (Raspberry Pi, etc.)
        add_definitions(-DPLATFORM_ARM_LINUX=1)
        
        if(ENABLE_NEON AND CMAKE_SYSTEM_PROCESSOR MATCHES "armv7")
            add_definitions(-DENABLE_NEON=1)
            add_compile_options(-mfpu=neon -mfloat-abi=hard)
        endif()
        
    elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
        # ARM64 Linux
        add_definitions(-DPLATFORM_ARM64_LINUX=1)
        
        if(ENABLE_NEON)
            add_definitions(-DENABLE_NEON=1)
        endif()
    endif()
    
elseif(CMAKE_SYSTEM_NAME STREQUAL "Android")
    # Android-specific settings
    add_definitions(-DPLATFORM_ANDROID=1 -DANDROID)
    
    # Android API level
    if(DEFINED ANDROID_PLATFORM)
        string(REPLACE "android-" "" ANDROID_API ${ANDROID_PLATFORM})
        add_definitions(-D__ANDROID_API__=${ANDROID_API})
    endif()
    
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    # Apple platforms
    add_definitions(-DPLATFORM_APPLE=1)
    
    # Check if iOS
    if(CMAKE_OSX_SYSROOT MATCHES "iphoneos|iphonesimulator|appletvos|watchos")
        add_definitions(-DPLATFORM_IOS=1)
        set(IOS TRUE)
    else()
        add_definitions(-DPLATFORM_MACOS=1)
    endif()
    
    # Universal binaries
    if(CMAKE_OSX_ARCHITECTURES)
        message(STATUS "Building universal binary for: ${CMAKE_OSX_ARCHITECTURES}")
    endif()
    
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    # Windows-specific settings
    add_definitions(-DPLATFORM_WINDOWS=1 -D_WINDOWS -D_CRT_SECURE_NO_WARNINGS)
    
    # Unicode
    add_definitions(-DUNICODE -D_UNICODE)
    
elseif(CMAKE_SYSTEM_NAME STREQUAL "Emscripten")
    # WebAssembly
    add_definitions(-DPLATFORM_WEBASSEMBLY=1 -DEMSCRIPTEN)
endif()

# SIMD optimizations
if(ENABLE_SIMD)
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86" AND ENABLE_AVX)
        add_definitions(-DENABLE_AVX=1)
        add_compile_options(-mavx -mavx2)
    endif()
endif()

# Threading
if(ENABLE_THREADS)
    add_definitions(-DENABLE_THREADS=1)
    find_package(Threads REQUIRED)
endif()

# OpenMP
if(ENABLE_OPENMP)
    find_package(OpenMP)
    if(OpenMP_FOUND)
        add_definitions(-DENABLE_OPENMP=1)
    else()
        message(WARNING "OpenMP not found, disabling")
        set(ENABLE_OPENMP OFF)
    endif()
endif()

# Create a library with platform-specific optimizations
add_library(universal_lib)

# Common sources
target_sources(universal_lib PRIVATE
    src/math/vector_operations.cpp
    src/image/processing.cpp
    src/audio/signal_processing.cpp
)

# Platform-specific assembly or optimized code
if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86" AND ENABLE_AVX)
    target_sources(universal_lib PRIVATE
        src/arch/x86/avx_optimized.cpp
    )
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "arm" AND ENABLE_NEON)
    target_sources(universal_lib PRIVATE
        src/arch/arm/neon_optimized.cpp
    )
    
    # ARM assembly
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "armv7")
        enable_language(ASM)
        target_sources(universal_lib PRIVATE
            src/arch/arm/neon_assembly.S
        )
    endif()
endif()

# Include directories
target_include_directories(universal_lib PUBLIC
    src
    src/arch
)

# Compile definitions
target_compile_definitions(universal_lib PUBLIC
    UNIVERSAL_LIB_VERSION=${PROJECT_VERSION}
)

# Link libraries
target_link_libraries(universal_lib PRIVATE
    ${CMAKE_THREAD_LIBS_INIT}
)

if(ENABLE_OPENMP AND OpenMP_FOUND)
    target_link_libraries(universal_lib PRIVATE OpenMP::OpenMP_CXX)
endif()

# Platform-specific libraries
if(PLATFORM_LINUX)
    target_link_libraries(universal_lib PRIVATE m dl)
elseif(PLATFORM_WINDOWS)
    # Windows libraries
endif()

# Create executable
add_executable(universal_demo src/main.cpp)
target_link_libraries(universal_demo PRIVATE universal_lib)

# Installation
install(TARGETS universal_lib
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(TARGETS universal_demo
    RUNTIME DESTINATION bin
)
```

### **Building for Embedded Systems (ARM Cortex-M)**

**toolchains/arm-cortex-m4.cmake:**
```cmake
# ARM Cortex-M4 toolchain for embedded systems

set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_PROCESSOR arm)

# Toolchain paths (adjust based on your setup)
set(TOOLCHAIN_PREFIX arm-none-eabi)
set(TOOLCHAIN_PATH /usr/bin)

# Compilers
set(CMAKE_C_COMPILER ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-gcc)
set(CMAKE_CXX_COMPILER ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-g++)
set(CMAKE_ASM_COMPILER ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-gcc)
set(CMAKE_AR ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-ar)
set(CMAKE_OBJCOPY ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-objcopy)
set(CMAKE_OBJDUMP ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-objdump)
set(CMAKE_SIZE ${TOOLCHAIN_PATH}/${TOOLCHAIN_PREFIX}-size)

# MCU specific flags
set(MCU_FLAGS "-mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16")
set(CMAKE_C_FLAGS "${MCU_FLAGS} -ffunction-sections -fdata-sections")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -fno-exceptions -fno-rtti")
set(CMAKE_ASM_FLAGS "${MCU_FLAGS}")

# Linker flags
set(CMAKE_EXE_LINKER_FLAGS "${MCU_FLAGS} -Wl,--gc-sections -specs=nano.specs -specs=nosys.specs")

# Find behavior
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

# Cross-compiling
set(CMAKE_CROSSCOMPILING TRUE)
set(EMBEDDED TRUE)

# Define for embedded code
add_definitions(-DUSE_STDPERIPH_DRIVER -DSTM32F4xx -DARM_MATH_CM4)

# Linker script (project specific)
set(LINKER_SCRIPT "${CMAKE_SOURCE_DIR}/linker/stm32f4.ld")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -T ${LINKER_SCRIPT}")
```

### **Qt Applications with CMake**

**CMakeLists.txt for Qt application:**
```cmake
cmake_minimum_required(VERSION 3.16)
project(QtCrossPlatform VERSION 1.0.0 LANGUAGES CXX)

# Find Qt
find_package(Qt6 COMPONENTS Core Gui Widgets REQUIRED)

# Set Qt specific options
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

# Platform-specific Qt configuration
if(ANDROID)
    find_package(Qt6 COMPONENTS AndroidExtras REQUIRED)
    set(QT_ANDROID TRUE)
elseif(IOS)
    set(QT_IOS TRUE)
endif()

# Create executable
add_executable(qt_app
    src/main.cpp
    src/mainwindow.cpp
    src/mainwindow.ui
    resources/resources.qrc
)

# Link with Qt
target_link_libraries(qt_app PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Widgets
)

# Platform-specific Qt modules
if(QT_ANDROID)
    target_link_libraries(qt_app PRIVATE Qt6::AndroidExtras)
endif()

# Platform-specific deployment
if(WIN32)
    # Copy Qt DLLs on Windows
    add_custom_command(TARGET qt_app POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
                "$<TARGET_FILE:Qt6::Core>" "$<TARGET_FILE_DIR:qt_app>"
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
                "$<TARGET_FILE:Qt6::Gui>" "$<TARGET_FILE_DIR:qt_app>"
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
                "$<TARGET_FILE:Qt6::Widgets>" "$<TARGET_FILE_DIR:qt_app>"
    )
endif()
```

### **Testing Cross-Compiled Code**

```cmake
# Cross-compilation test configuration
if(CMAKE_CROSSCOMPILING)
    # For cross-compiled tests, we might need to run them on the target
    # or use emulation
    
    if(EMSCRIPTEN)
        # Emscripten tests run in Node.js
        find_program(NODE_EXECUTABLE node)
        if(NODE_EXECUTABLE)
            add_test(NAME wasm_test
                COMMAND ${NODE_EXECUTABLE} $<TARGET_FILE:wasm_test>
            )
        endif()
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Android")
        # Android tests require ADB
        find_program(ADB_EXECUTABLE adb)
        if(ADB_EXECUTABLE)
            add_test(NAME android_test
                COMMAND ${ADB_EXECUTABLE} shell $<TARGET_FILE:android_test>
            )
        endif()
    endif()
    
else()
    # Native tests
    enable_testing()
    add_test(NAME native_test COMMAND native_test)
endif()
```

### **Building and Deployment Script**

**scripts/cross-build.sh:**
```bash
#!/bin/bash
set -e

# Configuration
PROJECT_DIR=$(cd "$(dirname "$0")/.." && pwd)
BUILD_DIR="${PROJECT_DIR}/build"
INSTALL_DIR="${PROJECT_DIR}/install"
TOOLCHAINS_DIR="${PROJECT_DIR}/toolchains"

# Target configurations
declare -A TARGETS=(
    ["linux-x64"]=""
    ["linux-arm"]="arm-linux-gnueabihf.cmake"
    ["linux-arm64"]="aarch64-linux-gnu.cmake"
    ["windows-x64"]="windows-mingw64.cmake"
    ["android-arm64"]="android-arm64.cmake"
)

# Build function
build_target() {
    local target=$1
    local toolchain=$2
    
    echo "Building for ${target}..."
    
    local build_dir="${BUILD_DIR}/${target}"
    local install_dir="${INSTALL_DIR}/${target}"
    
    mkdir -p "${build_dir}"
    cd "${build_dir}"
    
    # Configure
    if [ -n "${toolchain}" ]; then
        cmake "${PROJECT_DIR}" \
            -DCMAKE_TOOLCHAIN_FILE="${TOOLCHAINS_DIR}/${toolchain}" \
            -DCMAKE_INSTALL_PREFIX="${install_dir}" \
            -DCMAKE_BUILD_TYPE=Release
    else
        cmake "${PROJECT_DIR}" \
            -DCMAKE_INSTALL_PREFIX="${install_dir}" \
            -DCMAKE_BUILD_TYPE=Release
    fi
    
    # Build
    cmake --build . --parallel $(nproc)
    
    # Install
    cmake --install .
    
    echo "Build for ${target} completed: ${install_dir}"
}

# Main build loop
if [ $# -eq 0 ]; then
    # Build all targets
    for target in "${!TARGETS[@]}"; do
        build_target "${target}" "${TARGETS[$target]}"
    done
else
    # Build specific target
    target=$1
    if [ -n "${TARGETS[$target]}" ]; then
        build_target "${target}" "${TARGETS[$target]}"
    else
        echo "Unknown target: ${target}"
        echo "Available targets: ${!TARGETS[@]}"
        exit 1
    fi
fi

echo "All builds completed!"
```

### **Best Practices for Cross-Compilation**

1. **Isolate Platform-Specific Code**: Keep platform-specific code in separate files/directories
2. **Use CMake's Platform Detection**: Rely on `CMAKE_SYSTEM_NAME` and `CMAKE_SYSTEM_PROCESSOR`
3. **Test on Real Hardware**: Always test cross-compiled binaries on actual target hardware
4. **Version Your Toolchains**: Track toolchain versions along with your code
5. **Handle Endianness**: Be aware of big vs little endian architectures
6. **Memory Constraints**: Consider memory limitations for embedded targets
7. **Floating Point**: Be careful with floating-point on architectures without FPUs
8. **Alignment**: Pay attention to data alignment requirements

### **Common Pitfalls and Solutions**

**Pitfall 1: Mixing host and target libraries**
```cmake
# Solution: Use CMAKE_FIND_ROOT_PATH_MODE_* variables correctly
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```

**Pitfall 2: Hardcoded paths**
```cmake
# BAD
include_directories(/usr/include)

# GOOD
if(NOT CMAKE_CROSSCOMPILING)
    include_directories(/usr/include)
endif()
```

**Pitfall 3: Assuming word size**
```cpp
// BAD
int pointer_size = sizeof(void*);  // Might be 4 or 8

// GOOD
#include <cstdint>
#if INTPTR_MAX == INT64_MAX
    // 64-bit
#elif INTPTR_MAX == INT32_MAX
    // 32-bit
#endif
```

### **Exercise: Create a Multi-Platform Project**

Create a project that:
1. Can be built for at least 3 different architectures (x64, ARM, WebAssembly)
2. Has platform-specific optimizations
3. Includes a toolchain file for each target
4. Has a build script that can build all targets
5. Tests that verify the correct platform is detected
6. Properly handles platform-specific dependencies

### **Next Steps Preview**
In **PART-8**, we'll cover:
- Testing frameworks integration (Google Test, Catch2)
- Writing and running tests with CTest
- Code coverage analysis
- Performance testing and benchmarks
- Continuous integration with CMake

**To continue, please reply with "Next" or "PART-8" to receive the next part of the course.**


# **PART-8: Testing and Quality Assurance with CMake**

### **Topic: Comprehensive Testing Strategies, Test Frameworks, and Quality Metrics**

### **Introduction to Testing in CMake**

Testing is crucial for software quality. CMake provides built-in testing support through CTest and integrates with popular testing frameworks like Google Test, Catch2, and Boost.Test.

### **Project Structure for This Part**

```
testing_project/
├── CMakeLists.txt
├── src/
│   ├── math/
│   ├── utils/
│   └── app/
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── performance/
│   └── fixtures/
├── benchmarks/
├── cmake/
│   ├── AddTests.cmake
│   └── CodeCoverage.cmake
└── scripts/
    └── run_tests.sh
```

### **Root CMakeLists.txt with Testing Configuration**

```cmake
cmake_minimum_required(VERSION 3.18)
project(TestingDemo VERSION 1.5.0 LANGUAGES CXX C)

# Testing options
option(BUILD_TESTING "Build tests" ON)
option(ENABLE_UNIT_TESTS "Enable unit tests" ${BUILD_TESTING})
option(ENABLE_INTEGRATION_TESTS "Enable integration tests" OFF)
option(ENABLE_PERFORMANCE_TESTS "Enable performance tests" OFF)
option(ENABLE_BENCHMARKS "Enable benchmarks" OFF)
option(ENABLE_COVERAGE "Enable code coverage reporting" OFF)
option(ENABLE_MEMCHECK "Enable memory checking" OFF)
option(ENABLE_THREAD_SANITIZER "Enable thread sanitizer" OFF)
option(ENABLE_ADDRESS_SANITIZER "Enable address sanitizer" OFF)
option(ENABLE_UNDEFINED_SANITIZER "Enable undefined behavior sanitizer" OFF)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(TEST_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/tests)

# Add custom CMake modules
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

# Enable testing if requested
if(BUILD_TESTING)
    enable_testing()
    
    # Include testing modules
    if(ENABLE_COVERAGE)
        include(CodeCoverage)
    endif()
    
    # Setup sanitizers if requested
    if(ENABLE_ADDRESS_SANITIZER OR ENABLE_THREAD_SANITIZER OR ENABLE_UNDEFINED_SANITIZER)
        include(Sanitizers)
    endif()
endif()

# Print configuration
message(STATUS "")
message(STATUS "=== Testing Configuration ===")
message(STATUS "Unit Tests:         ${ENABLE_UNIT_TESTS}")
message(STATUS "Integration Tests:  ${ENABLE_INTEGRATION_TESTS}")
message(STATUS "Performance Tests:  ${ENABLE_PERFORMANCE_TESTS}")
message(STATUS "Benchmarks:         ${ENABLE_BENCHMARKS}")
message(STATUS "Code Coverage:      ${ENABLE_COVERAGE}")
message(STATUS "Address Sanitizer:  ${ENABLE_ADDRESS_SANITIZER}")
message(STATUS "Thread Sanitizer:   ${ENABLE_THREAD_SANITIZER}")
message(STATUS "Undefined Sanitizer: ${ENABLE_UNDEFINED_SANITIZER}")
message(STATUS "========================")
message(STATUS "")

# Add source directories
add_subdirectory(src)

# Add tests if enabled
if(BUILD_TESTING)
    add_subdirectory(tests)
endif()

if(ENABLE_BENCHMARKS)
    add_subdirectory(benchmarks)
endif()
```

### **Source Code Structure**

**src/math/vector.h:**
```cpp
#ifndef VECTOR_H
#define VECTOR_H

#include <vector>
#include <cmath>
#include <stdexcept>
#include <type_traits>
#include <numeric>
#include <algorithm>

namespace math {

template<typename T>
class Vector {
    static_assert(std::is_arithmetic<T>::value, "Vector requires arithmetic type");
    
private:
    std::vector<T> data;
    
public:
    // Constructors
    Vector() = default;
    explicit Vector(size_t size, T value = T()) : data(size, value) {}
    Vector(std::initializer_list<T> init) : data(init) {}
    
    // Accessors
    size_t size() const { return data.size(); }
    bool empty() const { return data.empty(); }
    
    T& operator[](size_t index) { return data[index]; }
    const T& operator[](size_t index) const { return data[index]; }
    
    T& at(size_t index) {
        if (index >= data.size()) {
            throw std::out_of_range("Vector index out of range");
        }
        return data[index];
    }
    
    const T& at(size_t index) const {
        if (index >= data.size()) {
            throw std::out_of_range("Vector index out of range");
        }
        return data[index];
    }
    
    // Mathematical operations
    Vector operator+(const Vector& other) const {
        check_size(other, "addition");
        Vector result(data.size());
        std::transform(data.begin(), data.end(), other.data.begin(),
                      result.data.begin(), std::plus<T>());
        return result;
    }
    
    Vector operator-(const Vector& other) const {
        check_size(other, "subtraction");
        Vector result(data.size());
        std::transform(data.begin(), data.end(), other.data.begin(),
                      result.data.begin(), std::minus<T>());
        return result;
    }
    
    Vector operator*(T scalar) const {
        Vector result(data.size());
        std::transform(data.begin(), data.end(), result.data.begin(),
                      [scalar](T val) { return val * scalar; });
        return result;
    }
    
    T dot(const Vector& other) const {
        check_size(other, "dot product");
        return std::inner_product(data.begin(), data.end(), other.data.begin(), T());
    }
    
    double magnitude() const {
        T sum = std::inner_product(data.begin(), data.end(), data.begin(), T());
        return std::sqrt(static_cast<double>(sum));
    }
    
    Vector normalized() const {
        double mag = magnitude();
        if (mag == 0.0) {
            throw std::runtime_error("Cannot normalize zero vector");
        }
        return (*this) * (1.0 / mag);
    }
    
    // Utility methods
    void resize(size_t new_size, T value = T()) {
        data.resize(new_size, value);
    }
    
    void push_back(T value) {
        data.push_back(value);
    }
    
    void clear() {
        data.clear();
    }
    
    typename std::vector<T>::iterator begin() { return data.begin(); }
    typename std::vector<T>::iterator end() { return data.end(); }
    typename std::vector<T>::const_iterator begin() const { return data.begin(); }
    typename std::vector<T>::const_iterator end() const { return data.end(); }
    
private:
    void check_size(const Vector& other, const std::string& operation) const {
        if (data.size() != other.data.size()) {
            throw std::invalid_argument(
                "Vector sizes mismatch for " + operation + 
                ": " + std::to_string(data.size()) + 
                " vs " + std::to_string(other.data.size()));
        }
    }
};

// Type aliases for common vector types
using VectorF = Vector<float>;
using VectorD = Vector<double>;
using VectorI = Vector<int>;

} // namespace math

#endif // VECTOR_H
```

**src/math/CMakeLists.txt:**
```cmake
# Math library

add_library(math)

# Source files
target_sources(math PRIVATE
    vector.cpp
    matrix.cpp
    statistics.cpp
)

# Header files (if any implementation in .cpp files)
target_sources(math PUBLIC
    FILE_SET HEADERS
    BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
    FILES
        vector.h
        matrix.h
        statistics.h
)

# Include directories
target_include_directories(math
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
)

# Compile definitions
target_compile_definitions(math PUBLIC
    MATH_LIBRARY=1
)

# Set properties
set_target_properties(math PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
)

# Create alias
add_library(Math::math ALIAS math)
```

### **Google Test Integration**

**tests/unit/CMakeLists.txt:**
```cmake
# Unit tests with Google Test

if(NOT ENABLE_UNIT_TESTS)
    return()
endif()

# Find or fetch Google Test
find_package(GTest 1.11.0 QUIET)

if(NOT GTest_FOUND)
    message(STATUS "GTest not found, fetching via FetchContent")
    
    include(FetchContent)
    
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG release-1.12.1
        OVERRIDE_FIND_PACKAGE  # Important for CMake 3.24+
    )
    
    # Prevent GoogleTest from overriding our compiler options
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    set(BUILD_GMOCK OFF CACHE BOOL "" FORCE)
    
    FetchContent_MakeAvailable(googletest)
endif()

# Test configuration
set(GTEST_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(GTEST_OUTPUT_DIR ${TEST_OUTPUT_DIRECTORY}/unit)

# Vector tests
add_executable(test_vector
    math/test_vector.cpp
    math/test_vector_operations.cpp
)

target_link_libraries(test_vector PRIVATE
    math
    GTest::gtest
    GTest::gtest_main
)

# Add compiler definitions for tests
target_compile_definitions(test_vector PRIVATE
    TESTING=1
    GTEST_DONT_DEFINE_FAIL=1
)

# Set test properties
set_target_properties(test_vector PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${GTEST_OUTPUT_DIR}
    ARCHIVE_OUTPUT_DIRECTORY ${GTEST_OUTPUT_DIR}
    LIBRARY_OUTPUT_DIRECTORY ${GTEST_OUTPUT_DIR}
)

# Add test with labels
add_test(
    NAME VectorTests
    COMMAND test_vector
    WORKING_DIRECTORY ${GTEST_OUTPUT_DIR}
)

# Set test properties for better reporting
set_tests_properties(VectorTests PROPERTIES
    LABELS "unit;math;vector"
    TIMEOUT 30
    WILL_FAIL FALSE
    PASS_REGULAR_EXPRESSION ".*OK.*"
    FAIL_REGULAR_EXPRESSION ".*FAILED.*"
)

# Matrix tests (similar pattern)
add_executable(test_matrix
    math/test_matrix.cpp
)

target_link_libraries(test_matrix PRIVATE
    math
    GTest::gtest
    GTest::gtest_main
)

add_test(
    NAME MatrixTests
    COMMAND test_matrix
    WORKING_DIRECTORY ${GTEST_OUTPUT_DIR}
)

set_tests_properties(MatrixTests PROPERTIES
    LABELS "unit;math;matrix"
    TIMEOUT 30
)

# Create a test suite that runs all unit tests
add_custom_target(run_unit_tests
    COMMAND ${CMAKE_CTEST_COMMAND}
        --output-on-failure
        --label-exclude "integration;performance"
    DEPENDS test_vector test_matrix
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Running unit tests..."
)

# Add dependency so tests are built with main project
add_dependencies(run_unit_tests test_vector test_matrix)
```

**tests/unit/math/test_vector.cpp:**
```cpp
#include "math/vector.h"
#include <gtest/gtest.h>
#include <vector>
#include <stdexcept>
#include <type_traits>

namespace math {
namespace test {

// Test fixture for Vector tests
class VectorTest : public ::testing::Test {
protected:
    void SetUp() override {
        // Common setup for all tests
        vec3 = {1.0, 2.0, 3.0};
        vec3_other = {4.0, 5.0, 6.0};
        vec_empty = {};
        vec_zero = {0.0, 0.0, 0.0};
    }
    
    void TearDown() override {
        // Cleanup after each test
    }
    
    // Test vectors
    VectorD vec3;
    VectorD vec3_other;
    VectorD vec_empty;
    VectorD vec_zero;
};

// Basic construction tests
TEST_F(VectorTest, DefaultConstructor) {
    VectorD v;
    EXPECT_TRUE(v.empty());
    EXPECT_EQ(v.size(), 0);
}

TEST_F(VectorTest, SizeConstructor) {
    VectorD v(5);
    EXPECT_EQ(v.size(), 5);
    for (size_t i = 0; i < 5; ++i) {
        EXPECT_DOUBLE_EQ(v[i], 0.0);
    }
}

TEST_F(VectorTest, InitializerListConstructor) {
    VectorD v = {1.0, 2.0, 3.0, 4.0};
    EXPECT_EQ(v.size(), 4);
    EXPECT_DOUBLE_EQ(v[0], 1.0);
    EXPECT_DOUBLE_EQ(v[1], 2.0);
    EXPECT_DOUBLE_EQ(v[2], 3.0);
    EXPECT_DOUBLE_EQ(v[3], 4.0);
}

// Accessor tests
TEST_F(VectorTest, ElementAccess) {
    EXPECT_DOUBLE_EQ(vec3[0], 1.0);
    EXPECT_DOUBLE_EQ(vec3[1], 2.0);
    EXPECT_DOUBLE_EQ(vec3[2], 3.0);
    
    // Modify through operator[]
    vec3[1] = 10.0;
    EXPECT_DOUBLE_EQ(vec3[1], 10.0);
}

TEST_F(VectorTest, AtMethod) {
    EXPECT_DOUBLE_EQ(vec3.at(0), 1.0);
    EXPECT_DOUBLE_EQ(vec3.at(1), 2.0);
    EXPECT_DOUBLE_EQ(vec3.at(2), 3.0);
    
    // Test bounds checking
    EXPECT_THROW(vec3.at(3), std::out_of_range);
    EXPECT_THROW(vec3.at(-1), std::out_of_range); // -1 becomes large unsigned
}

// Mathematical operation tests
TEST_F(VectorTest, VectorAddition) {
    VectorD result = vec3 + vec3_other;
    ASSERT_EQ(result.size(), 3);
    EXPECT_DOUBLE_EQ(result[0], 5.0);  // 1 + 4
    EXPECT_DOUBLE_EQ(result[1], 7.0);  // 2 + 5
    EXPECT_DOUBLE_EQ(result[2], 9.0);  // 3 + 6
}

TEST_F(VectorTest, VectorAdditionSizeMismatch) {
    VectorD v4 = {1.0, 2.0, 3.0, 4.0};
    EXPECT_THROW(vec3 + v4, std::invalid_argument);
}

TEST_F(VectorTest, VectorSubtraction) {
    VectorD result = vec3_other - vec3;
    ASSERT_EQ(result.size(), 3);
    EXPECT_DOUBLE_EQ(result[0], 3.0);  // 4 - 1
    EXPECT_DOUBLE_EQ(result[1], 3.0);  // 5 - 2
    EXPECT_DOUBLE_EQ(result[2], 3.0);  // 6 - 3
}

TEST_F(VectorTest, ScalarMultiplication) {
    VectorD result = vec3 * 2.0;
    ASSERT_EQ(result.size(), 3);
    EXPECT_DOUBLE_EQ(result[0], 2.0);  // 1 * 2
    EXPECT_DOUBLE_EQ(result[1], 4.0);  // 2 * 2
    EXPECT_DOUBLE_EQ(result[2], 6.0);  // 3 * 2
}

TEST_F(VectorTest, DotProduct) {
    double dot = vec3.dot(vec3_other);
    EXPECT_DOUBLE_EQ(dot, 32.0);  // 1*4 + 2*5 + 3*6 = 4 + 10 + 18 = 32
    
    // Dot product with self should give magnitude squared
    double self_dot = vec3.dot(vec3);
    EXPECT_DOUBLE_EQ(self_dot, 14.0);  // 1*1 + 2*2 + 3*3 = 1 + 4 + 9 = 14
}

TEST_F(VectorTest, Magnitude) {
    double mag = vec3.magnitude();
    EXPECT_DOUBLE_EQ(mag, std::sqrt(14.0));  // sqrt(1^2 + 2^2 + 3^2)
    
    // Zero vector should have zero magnitude
    EXPECT_DOUBLE_EQ(vec_zero.magnitude(), 0.0);
}

TEST_F(VectorTest, Normalization) {
    VectorD normalized = vec3.normalized();
    double mag = normalized.magnitude();
    
    // Normalized vector should have magnitude 1
    EXPECT_NEAR(mag, 1.0, 1e-10);
    
    // Check direction is preserved
    EXPECT_NEAR(normalized[0], 1.0 / std::sqrt(14.0), 1e-10);
    EXPECT_NEAR(normalized[1], 2.0 / std::sqrt(14.0), 1e-10);
    EXPECT_NEAR(normalized[2], 3.0 / std::sqrt(14.0), 1e-10);
}

TEST_F(VectorTest, NormalizeZeroVector) {
    EXPECT_THROW(vec_zero.normalized(), std::runtime_error);
}

// Type safety tests
TEST_F(VectorTest, IntegerVector) {
    VectorI int_vec = {1, 2, 3, 4, 5};
    VectorI int_vec2 = {5, 4, 3, 2, 1};
    
    VectorI sum = int_vec + int_vec2;
    ASSERT_EQ(sum.size(), 5);
    EXPECT_EQ(sum[0], 6);  // 1 + 5
    EXPECT_EQ(sum[1], 6);  // 2 + 4
    EXPECT_EQ(sum[2], 6);  // 3 + 3
    EXPECT_EQ(sum[3], 6);  // 4 + 2
    EXPECT_EQ(sum[4], 6);  // 5 + 1
    
    // Integer division in normalization test
    VectorI scaled = int_vec * 2;
    EXPECT_EQ(scaled[0], 2);
    EXPECT_EQ(scaled[1], 4);
    EXPECT_EQ(scaled[2], 6);
}

// Iterator tests
TEST_F(VectorTest, Iterators) {
    std::vector<double> expected = {1.0, 2.0, 3.0};
    std::vector<double> actual;
    
    for (auto val : vec3) {
        actual.push_back(val);
    }
    
    EXPECT_EQ(actual, expected);
    
    // Test const iterators
    const VectorD& const_vec = vec3;
    actual.clear();
    for (auto it = const_vec.begin(); it != const_vec.end(); ++it) {
        actual.push_back(*it);
    }
    EXPECT_EQ(actual, expected);
}

// Resize and capacity tests
TEST_F(VectorTest, Resize) {
    VectorD v = {1.0, 2.0, 3.0};
    v.resize(5, 10.0);
    
    EXPECT_EQ(v.size(), 5);
    EXPECT_DOUBLE_EQ(v[0], 1.0);
    EXPECT_DOUBLE_EQ(v[1], 2.0);
    EXPECT_DOUBLE_EQ(v[2], 3.0);
    EXPECT_DOUBLE_EQ(v[3], 10.0);
    EXPECT_DOUBLE_EQ(v[4], 10.0);
    
    v.resize(2);
    EXPECT_EQ(v.size(), 2);
    EXPECT_DOUBLE_EQ(v[0], 1.0);
    EXPECT_DOUBLE_EQ(v[1], 2.0);
}

TEST_F(VectorTest, PushBack) {
    VectorD v;
    v.push_back(1.0);
    v.push_back(2.0);
    v.push_back(3.0);
    
    EXPECT_EQ(v.size(), 3);
    EXPECT_DOUBLE_EQ(v[0], 1.0);
    EXPECT_DOUBLE_EQ(v[1], 2.0);
    EXPECT_DOUBLE_EQ(v[2], 3.0);
}

TEST_F(VectorTest, Clear) {
    VectorD v = {1.0, 2.0, 3.0};
    EXPECT_FALSE(v.empty());
    
    v.clear();
    EXPECT_TRUE(v.empty());
    EXPECT_EQ(v.size(), 0);
}

// Parameterized tests for different numeric types
template<typename T>
class TypedVectorTest : public ::testing::Test {
protected:
    using VectorType = math::Vector<T>;
};

using NumericTypes = ::testing::Types<int, float, double>;
TYPED_TEST_SUITE(TypedVectorTest, NumericTypes);

TYPED_TEST(TypedVectorTest, BasicOperations) {
    using VectorType = typename TestFixture::VectorType;
    
    VectorType v1 = {static_cast<TypeParam>(1), static_cast<TypeParam>(2)};
    VectorType v2 = {static_cast<TypeParam>(3), static_cast<TypeParam>(4)};
    
    VectorType sum = v1 + v2;
    EXPECT_EQ(sum.size(), 2);
    EXPECT_EQ(sum[0], static_cast<TypeParam>(4));
    EXPECT_EQ(sum[1], static_cast<TypeParam>(6));
    
    VectorType diff = v2 - v1;
    EXPECT_EQ(diff[0], static_cast<TypeParam>(2));
    EXPECT_EQ(diff[1], static_cast<TypeParam>(2));
}

} // namespace test
} // namespace math
```

### **Catch2 Integration**

**tests/integration/CMakeLists.txt:**
```cmake
# Integration tests with Catch2

if(NOT ENABLE_INTEGRATION_TESTS)
    return()
endif()

# Find or fetch Catch2
find_package(Catch2 3.0 QUIET CONFIG)

if(NOT Catch2_FOUND)
    message(STATUS "Catch2 not found, fetching via FetchContent")
    
    include(FetchContent)
    
    FetchContent_Declare(
        Catch2
        GIT_REPOSITORY https://github.com/catchorg/Catch2.git
        GIT_TAG v3.4.0
    )
    
    FetchContent_MakeAvailable(Catch2)
    
    # Modern Catch2 provides a CMake config
    list(APPEND CMAKE_MODULE_PATH ${catch2_SOURCE_DIR}/extras)
endif()

set(CATCH_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(CATCH_OUTPUT_DIR ${TEST_OUTPUT_DIRECTORY}/integration)

# Integration test for vector-matrix operations
add_executable(test_integration
    math/test_vector_matrix_integration.cpp
    utils/test_file_operations.cpp
)

target_link_libraries(test_integration PRIVATE
    math
    Catch2::Catch2WithMain
)

# Set output directory
set_target_properties(test_integration PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CATCH_OUTPUT_DIR}
)

# Add test with Catch2 discovery
catch_discover_tests(test_integration
    WORKING_DIRECTORY ${CATCH_OUTPUT_DIR}
    TEST_PREFIX "Integration."
    TEST_SPEC "[integration]"
)

# Create custom target for integration tests
add_custom_target(run_integration_tests
    COMMAND ${CMAKE_CTEST_COMMAND}
        --output-on-failure
        --label-regex "integration"
    DEPENDS test_integration
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Running integration tests..."
)
```

**tests/integration/math/test_vector_matrix_integration.cpp:**
```cpp
#define CATCH_CONFIG_MAIN
#include <catch2/catch_test_macros.hpp>
#include <catch2/matchers/catch_matchers_all.hpp>
#include <catch2/benchmark/catch_benchmark_all.hpp>

#include "math/vector.h"
#include "math/matrix.h"  // Assuming we have a Matrix class

TEST_CASE("Vector-Matrix Integration Tests", "[integration][math]") {
    
    SECTION("Matrix-Vector Multiplication") {
        math::MatrixD mat = {{1, 2, 3},
                             {4, 5, 6},
                             {7, 8, 9}};
        
        math::VectorD vec = {1, 2, 3};
        
        // This would test if we have matrix-vector multiplication
        // auto result = mat * vec;
        // REQUIRE(result.size() == 3);
        // CHECK(result[0] == 14);  // 1*1 + 2*2 + 3*3
        // CHECK(result[1] == 32);  // 4*1 + 5*2 + 6*3
        // CHECK(result[2] == 50);  // 7*1 + 8*2 + 9*3
    }
    
    SECTION("Linear System Solution") {
        // Test solving Ax = b
        math::MatrixD A = {{4, 1},
                           {1, 3}};
        math::VectorD b = {1, 2};
        
        // If we had linear algebra functionality:
        // auto x = solve_linear_system(A, b);
        // CHECK(x.size() == 2);
        // CHECK_THAT(x[0], Catch::Matchers::WithinRel(0.0909, 0.001));
        // CHECK_THAT(x[1], Catch::Matchers::WithinRel(0.6363, 0.001));
    }
}

TEST_CASE("Complex Vector Operations", "[integration][math][vector]") {
    
    math::VectorD v1 = {1.0, 2.0, 3.0, 4.0};
    math::VectorD v2 = {5.0, 6.0, 7.0, 8.0};
    
    SECTION("Multiple Operations Chain") {
        auto result = (v1 + v2) * 2.0 - v1;
        
        REQUIRE(result.size() == 4);
        CHECK(result[0] == Approx(11.0));  // (1+5)*2 - 1 = 11
        CHECK(result[1] == Approx(14.0));  // (2+6)*2 - 2 = 14
        CHECK(result[2] == Approx(17.0));  // (3+7)*2 - 3 = 17
        CHECK(result[3] == Approx(20.0));  // (4+8)*2 - 4 = 20
    }
    
    SECTION("Normalization Chain") {
        auto normalized = (v1 + v2).normalized();
        double mag = normalized.magnitude();
        
        CHECK(mag == Approx(1.0).margin(1e-10));
    }
}

SCENARIO("Vector operations in different scenarios", "[integration][scenario]") {
    
    GIVEN("Two vectors of equal size") {
        math::VectorD a = {1.0, 2.0, 3.0};
        math::VectorD b = {4.0, 5.0, 6.0};
        
        WHEN("they are added") {
            auto c = a + b;
            
            THEN("the result has correct values") {
                REQUIRE(c.size() == 3);
                CHECK(c[0] == 5.0);
                CHECK(c[1] == 7.0);
                CHECK(c[2] == 9.0);
            }
            
            AND_WHEN("the result is multiplied by a scalar") {
                auto d = c * 2.0;
                
                THEN("all elements are doubled") {
                    CHECK(d[0] == 10.0);
                    CHECK(d[1] == 14.0);
                    CHECK(d[2] == 18.0);
                }
            }
        }
    }
    
    GIVEN("A zero vector") {
        math::VectorD zero = {0.0, 0.0, 0.0};
        
        WHEN("we try to normalize it") {
            THEN("an exception is thrown") {
                REQUIRE_THROWS_AS(zero.normalized(), std::runtime_error);
                REQUIRE_THROWS_WITH(zero.normalized(), 
                    "Cannot normalize zero vector");
            }
        }
    }
}
```

### **Performance Tests and Benchmarks**

**benchmarks/CMakeLists.txt:**
```cmake
# Benchmarks with Google Benchmark

if(NOT ENABLE_BENCHMARKS)
    return()
endif()

# Find or fetch Google Benchmark
find_package(benchmark 1.6.0 QUIET)

if(NOT benchmark_FOUND)
    message(STATUS "Google Benchmark not found, fetching via FetchContent")
    
    include(FetchContent)
    
    FetchContent_Declare(
        googlebenchmark
        GIT_REPOSITORY https://github.com/google/benchmark.git
        GIT_TAG v1.8.0
    )
    
    # Set benchmark options
    set(BENCHMARK_ENABLE_TESTING OFF CACHE BOOL "" FORCE)
    set(BENCHMARK_ENABLE_INSTALL OFF CACHE BOOL "" FORCE)
    
    FetchContent_MakeAvailable(googlebenchmark)
endif()

set(BENCHMARK_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(BENCHMARK_OUTPUT_DIR ${TEST_OUTPUT_DIRECTORY}/benchmarks)

# Vector operations benchmark
add_executable(bench_vector
    math/bench_vector_operations.cpp
)

target_link_libraries(bench_vector PRIVATE
    math
    benchmark::benchmark
    benchmark::benchmark_main
)

# Set output directory
set_target_properties(bench_vector PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${BENCHMARK_OUTPUT_DIR}
)

# Add custom target to run benchmarks
add_custom_target(run_benchmarks
    COMMAND bench_vector
        --benchmark_format=console
        --benchmark_min_time=0.5s
    DEPENDS bench_vector
    WORKING_DIRECTORY ${BENCHMARK_OUTPUT_DIR}
    COMMENT "Running benchmarks..."
)

# Add installation for benchmark results
install(PROGRAMS ${BENCHMARK_OUTPUT_DIR}/bench_vector
    DESTINATION ${CMAKE_INSTALL_BINDIR}/benchmarks
)
```

**benchmarks/math/bench_vector_operations.cpp:**
```cpp
#include <benchmark/benchmark.h>
#include "math/vector.h"
#include <random>
#include <vector>

// Helper function to generate random vector
math::VectorD generate_random_vector(size_t size) {
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_real_distribution<> dis(-100.0, 100.0);
    
    math::VectorD vec(size);
    for (size_t i = 0; i < size; ++i) {
        vec[i] = dis(gen);
    }
    return vec;
}

// Benchmark vector addition
static void BM_VectorAddition(benchmark::State& state) {
    size_t size = state.range(0);
    auto a = generate_random_vector(size);
    auto b = generate_random_vector(size);
    
    for (auto _ : state) {
        auto result = a + b;
        benchmark::DoNotOptimize(result);
        benchmark::ClobberMemory();
    }
    
    state.SetComplexityN(size);
    state.SetBytesProcessed(state.iterations() * size * sizeof(double) * 2);
}

// Register the benchmark with different sizes
BENCHMARK(BM_VectorAddition)
    ->RangeMultiplier(2)
    ->Range(1 << 4, 1 << 14)  // From 16 to 16384 elements
    ->Complexity(benchmark::oN)
    ->Unit(benchmark::kMicrosecond);

// Benchmark vector dot product
static void BM_VectorDotProduct(benchmark::State& state) {
    size_t size = state.range(0);
    auto a = generate_random_vector(size);
    auto b = generate_random_vector(size);
    
    for (auto _ : state) {
        double result = a.dot(b);
        benchmark::DoNotOptimize(result);
    }
    
    state.SetComplexityN(size);
    state.SetBytesProcessed(state.iterations() * size * sizeof(double) * 2);
}

BENCHMARK(BM_VectorDotProduct)
    ->RangeMultiplier(2)
    ->Range(1 << 4, 1 << 14)
    ->Complexity(benchmark::oN)
    ->Unit(benchmark::kMicrosecond);

// Benchmark vector normalization
static void BM_VectorNormalization(benchmark::State& state) {
    size_t size = state.range(0);
    auto a = generate_random_vector(size);
    
    // Make sure vector is not zero
    a[0] += 100.0;
    
    for (auto _ : state) {
        auto result = a.normalized();
        benchmark::DoNotOptimize(result);
        benchmark::ClobberMemory();
    }
    
    state.SetComplexityN(size);
}

BENCHMARK(BM_VectorNormalization)
    ->RangeMultiplier(2)
    ->Range(1 << 4, 1 << 12)
    ->Complexity(benchmark::oN)
    ->Unit(benchmark::kMicrosecond);

// Benchmark scalar multiplication
static void BM_VectorScalarMultiply(benchmark::State& state) {
    size_t size = state.range(0);
    auto a = generate_random_vector(size);
    double scalar = 2.5;
    
    for (auto _ : state) {
        auto result = a * scalar;
        benchmark::DoNotOptimize(result);
        benchmark::ClobberMemory();
    }
    
    state.SetComplexityN(size);
    state.SetBytesProcessed(state.iterations() * size * sizeof(double));
}

BENCHMARK(BM_VectorScalarMultiply)
    ->RangeMultiplier(2)
    ->Range(1 << 4, 1 << 14)
    ->Complexity(benchmark::oN)
    ->Unit(benchmark::kMicrosecond);

// Custom benchmark for different implementations
template <typename VectorType>
static void BM_VectorAddition_Template(benchmark::State& state) {
    size_t size = state.range(0);
    VectorType a(size, 1.0);
    VectorType b(size, 2.0);
    
    for (auto _ : state) {
        VectorType result = a + b;
        benchmark::DoNotOptimize(result);
        benchmark::ClobberMemory();
    }
    
    state.SetComplexityN(size);
}

// Register template benchmark for different types
BENCHMARK_TEMPLATE(BM_VectorAddition_Template, math::Vector<float>)
    ->Range(1 << 4, 1 << 12)
    ->Unit(benchmark::kMicrosecond);

BENCHMARK_TEMPLATE(BM_VectorAddition_Template, math::Vector<double>)
    ->Range(1 << 4, 1 << 12)
    ->Unit(benchmark::kMicrosecond);

BENCHMARK_TEMPLATE(BM_VectorAddition_Template, math::Vector<int>)
    ->Range(1 << 4, 1 << 12)
    ->Unit(benchmark::kMicrosecond);

// Fixture for reusable setup
class VectorFixture : public benchmark::Fixture {
public:
    void SetUp(const benchmark::State& state) override {
        size = state.range(0);
        a = generate_random_vector(size);
        b = generate_random_vector(size);
    }
    
    void TearDown(const benchmark::State&) override {
        // Cleanup if needed
    }
    
protected:
    size_t size;
    math::VectorD a;
    math::VectorD b;
};

BENCHMARK_DEFINE_F(VectorFixture, BM_FixtureAddition)(benchmark::State& state) {
    for (auto _ : state) {
        auto result = a + b;
        benchmark::DoNotOptimize(result);
        benchmark::ClobberMemory();
    }
    
    state.SetComplexityN(size);
    state.SetBytesProcessed(state.iterations() * size * sizeof(double) * 2);
}

BENCHMARK_REGISTER_F(VectorFixture, BM_FixtureAddition)
    ->RangeMultiplier(2)
    ->Range(1 << 4, 1 << 14)
    ->Complexity(benchmark::oN)
    ->Unit(benchmark::kMicrosecond);

BENCHMARK_MAIN();
```

### **Code Coverage with gcov/lcov**

**cmake/CodeCoverage.cmake:**
```cmake
# CodeCoverage.cmake - Generate code coverage reports

function(enable_coverage)
    if(NOT CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        message(WARNING "Code coverage only supported for GCC and Clang")
        return()
    endif()
    
    # Add coverage flags
    add_compile_options(
        -g
        -O0
        --coverage
        -fprofile-arcs
        -ftest-coverage
    )
    
    add_link_options(--coverage)
    
    # Find gcov and lcov
    find_program(GCOV_PATH gcov)
    find_program(LCOV_PATH lcov)
    find_program(GENHTML_PATH genhtml)
    
    if(NOT GCOV_PATH)
        message(WARNING "gcov not found - code coverage disabled")
        return()
    endif()
    
    # Create coverage targets
    add_custom_target(coverage
        COMMAND ${LCOV_PATH} --directory . --zerocounters
        COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
        COMMAND ${LCOV_PATH} --directory . --capture --output-file coverage.info
        COMMAND ${LCOV_PATH} --remove coverage.info '*/tests/*' '*/benchmarks/*' '/usr/*' --output-file coverage.filtered.info
        COMMAND ${GENHTML_PATH} coverage.filtered.info --output-directory ${CMAKE_BINARY_DIR}/coverage_report
        COMMAND ${CMAKE_COMMAND} -E echo "Coverage report generated: file://${CMAKE_BINARY_DIR}/coverage_report/index.html"
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        DEPENDS run_unit_tests
        COMMENT "Generating code coverage report"
    )
    
    # Quick coverage check target
    add_custom_target(coverage-quick
        COMMAND ${LCOV_PATH} --directory . --capture --output-file coverage.quick.info
        COMMAND ${LCOV_PATH} --summary coverage.quick.info
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        COMMENT "Quick coverage summary"
    )
    
    # Clean coverage data
    add_custom_target(coverage-clean
        COMMAND ${LCOV_PATH} --directory . --zerocounters
        COMMAND ${CMAKE_COMMAND} -E remove -f coverage.info coverage.filtered.info coverage.quick.info
        COMMAND ${CMAKE_COMMAND} -E remove_directory ${CMAKE_BINARY_DIR}/coverage_report
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
        COMMENT "Cleaning coverage data"
    )
    
    message(STATUS "Code coverage enabled")
endfunction()

# Call the function if ENABLE_COVERAGE is ON
if(ENABLE_COVERAGE)
    enable_coverage()
endif()
```

### **Sanitizers for Memory and Thread Safety**

**cmake/Sanitizers.cmake:**
```cmake
# Sanitizers.cmake - Enable various sanitizers

function(enable_sanitizers)
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        set(SANITIZERS "")
        
        if(ENABLE_ADDRESS_SANITIZER)
            list(APPEND SANITIZERS "address")
        endif()
        
        if(ENABLE_THREAD_SANITIZER)
            if(ENABLE_ADDRESS_SANITIZER)
                message(WARNING "ThreadSanitizer is not compatible with AddressSanitizer")
            else()
                list(APPEND SANITIZERS "thread")
            endif()
        endif()
        
        if(ENABLE_UNDEFINED_SANITIZER)
            list(APPEND SANITIZERS "undefined")
        endif()
        
        if(SANITIZERS)
            list(JOIN SANITIZERS "," SANITIZER_LIST)
            add_compile_options(-fsanitize=${SANITIZER_LIST})
            add_link_options(-fsanitize=${SANITIZER_LIST})
            
            if(ENABLE_ADDRESS_SANITIZER)
                # Additional flags for better ASan reports
                add_compile_options(-fno-omit-frame-pointer -fno-optimize-sibling-calls)
            endif()
            
            message(STATUS "Enabled sanitizers: ${SANITIZER_LIST}")
        endif()
        
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        # MSVC sanitizers
        if(ENABLE_ADDRESS_SANITIZER)
            add_compile_options(/fsanitize=address)
            message(STATUS "Enabled AddressSanitizer for MSVC")
        endif()
    endif()
endfunction()

# Call the function if any sanitizer is enabled
if(ENABLE_ADDRESS_SANITIZER OR ENABLE_THREAD_SANITIZER OR ENABLE_UNDEFINED_SANITIZER)
    enable_sanitizers()
endif()
```

### **CTest Configuration**

**CTestConfig.cmake (at project root):**
```cmake
# CTest configuration file
set(CTEST_PROJECT_NAME "TestingDemo")
set(CTEST_NIGHTLY_START_TIME "01:00:00 UTC")
set(CTEST_DROP_METHOD "http")
set(CTEST_DROP_SITE "my.cdash.org")
set(CTEST_DROP_LOCATION "/submit.php?project=TestingDemo")
set(CTEST_DROP_SITE_CDASH TRUE)

# Test timeout
set(CTEST_TEST_TIMEOUT 300)

# Coverage
set(CTEST_COVERAGE_COMMAND "gcov")
set(CTEST_COVERAGE_EXTRA_FLAGS "-b")

# Memory check
set(CTEST_MEMORYCHECK_COMMAND "valgrind")
set(CTEST_MEMORYCHECK_COMMAND_OPTIONS "--tool=memcheck --leak-check=full --show-leak-kinds=all --track-origins=yes")
set(CTEST_MEMORYCHECK_SUPPRESSIONS_FILE "${CMAKE_SOURCE_DIR}/valgrind.supp")
```

### **Advanced Test Configuration**

**tests/CMakeLists.txt (main tests directory):**
```cmake
# Main tests directory

if(NOT BUILD_TESTING)
    return()
endif()

# Create test output directory
file(MAKE_DIRECTORY ${TEST_OUTPUT_DIRECTORY})

# Include test utilities
include(AddTests)

# Discover and add all test files
file(GLOB_RECURSE TEST_SOURCES CONFIGURE_DEPENDS
    "*.cpp"
)

# Group tests by directory
foreach(test_source ${TEST_SOURCES})
    # Get relative path
    file(RELATIVE_PATH rel_path ${CMAKE_CURRENT_SOURCE_DIR} ${test_source})
    
    # Get directory name for grouping
    get_filename_component(test_dir ${rel_path} DIRECTORY)
    
    # Create test executable
    get_filename_component(test_name ${test_source} NAME_WE)
    
    if(NOT TARGET test_${test_name})
        add_executable(test_${test_name} ${test_source})
        
        # Link with appropriate libraries
        target_link_libraries(test_${test_name} PRIVATE
            math
        )
        
        # Add test
        add_test(
            NAME ${test_dir}/${test_name}
            COMMAND test_${test_name}
            WORKING_DIRECTORY ${TEST_OUTPUT_DIRECTORY}
        )
        
        # Set test properties
        set_tests_properties(${test_dir}/${test_name} PROPERTIES
            LABELS "${test_dir}"
            TIMEOUT 60
        )
    endif()
endforeach()

# Custom test runners
add_custom_target(check
    COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
    DEPENDS ${ALL_TEST_TARGETS}
    COMMENT "Running all tests"
)

add_custom_target(test-memory
    COMMAND ${CMAKE_CTEST_COMMAND} -T memcheck --output-on-failure
    DEPENDS ${ALL_TEST_TARGETS}
    COMMENT "Running tests with memory checking"
)

# Test fixtures for complex setups
add_test(NAME DatabaseTests
    COMMAND ${CMAKE_COMMAND}
        -DEXECUTABLE=$<TARGET_FILE:test_database>
        -DFIXTURE_SETUP=${CMAKE_SOURCE_DIR}/tests/fixtures/database_setup.sql
        -DFIXTURE_TEARDOWN=${CMAKE_SOURCE_DIR}/tests/fixtures/database_teardown.sql
        -P ${CMAKE_SOURCE_DIR}/cmake/RunWithFixture.cmake
)

# Performance test threshold
add_test(NAME PerformanceThreshold
    COMMAND ${CMAKE_COMMAND}
        -DEXECUTABLE=$<TARGET_FILE:bench_vector>
        -DTHRESHOLD_MS=100
        -P ${CMAKE_SOURCE_DIR}/cmake/CheckPerformance.cmake
)
```

### **Test Utilities**

**cmake/AddTests.cmake:**
```cmake
# AddTests.cmake - Utilities for adding tests

macro(add_gtest TEST_NAME)
    set(multi_value_args SOURCES LIBRARIES DEFINITIONS)
    cmake_parse_arguments(ARG "" "" "${multi_value_args}" ${ARGN})
    
    add_executable(${TEST_NAME} ${ARG_SOURCES})
    
    target_link_libraries(${TEST_NAME} PRIVATE
        ${ARG_LIBRARIES}
        GTest::gtest
        GTest::gtest_main
    )
    
    target_compile_definitions(${TEST_NAME} PRIVATE ${ARG_DEFINITIONS})
    
    add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME})
    
    # Add to global test targets list
    list(APPEND ALL_TEST_TARGETS ${TEST_NAME})
    set(ALL_TEST_TARGETS ${ALL_TEST_TARGETS} PARENT_SCOPE)
endmacro()

macro(add_catch2_test TEST_NAME)
    set(multi_value_args SOURCES LIBRARIES DEFINITIONS)
    cmake_parse_arguments(ARG "" "" "${multi_value_args}" ${ARGN})
    
    add_executable(${TEST_NAME} ${ARG_SOURCES})
    
    target_link_libraries(${TEST_NAME} PRIVATE
        ${ARG_LIBRARIES}
        Catch2::Catch2WithMain
    )
    
    target_compile_definitions(${TEST_NAME} PRIVATE ${ARG_DEFINITIONS})
    
    catch_discover_tests(${TEST_NAME})
    
    list(APPEND ALL_TEST_TARGETS ${TEST_NAME})
    set(ALL_TEST_TARGETS ${ALL_TEST_TARGETS} PARENT_SCOPE)
endmacro()

function(setup_test_target TARGET_NAME)
    # Common setup for all test targets
    target_include_directories(${TARGET_NAME} PRIVATE
        ${CMAKE_SOURCE_DIR}/src
        ${CMAKE_SOURCE_DIR}/tests
    )
    
    if(ENABLE_COVERAGE)
        target_compile_options(${TARGET_NAME} PRIVATE --coverage)
        target_link_options(${TARGET_NAME} PRIVATE --coverage)
    endif()
    
    # Set output directory
    set_target_properties(${TARGET_NAME} PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY ${TEST_OUTPUT_DIRECTORY}
        ARCHIVE_OUTPUT_DIRECTORY ${TEST_OUTPUT_DIRECTORY}
        LIBRARY_OUTPUT_DIRECTORY ${TEST_OUTPUT_DIRECTORY}
    )
endfunction()
```

### **Running Tests and Generating Reports**

**scripts/run_tests.sh:**
```bash
#!/bin/bash
set -e

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Configuration
BUILD_DIR="build"
REPORT_DIR="${BUILD_DIR}/reports"
COVERAGE_DIR="${BUILD_DIR}/coverage_report"

# Create directories
mkdir -p ${REPORT_DIR}
mkdir -p ${COVERAGE_DIR}

echo -e "${BLUE}=== Test Runner ===${NC}"

# Build the project
echo -e "${BLUE}Building project...${NC}"
cmake --build ${BUILD_DIR} --config Debug --parallel

# Run unit tests
echo -e "\n${BLUE}Running unit tests...${NC}"
cd ${BUILD_DIR}
ctest --output-on-failure --label-regex "unit" 2>&1 | tee ${REPORT_DIR}/unit_tests.log

# Run integration tests
echo -e "\n${BLUE}Running integration tests...${NC}"
ctest --output-on-failure --label-regex "integration" 2>&1 | tee ${REPORT_DIR}/integration_tests.log

# Run performance tests
echo -e "\n${BLUE}Running performance tests...${NC}"
ctest --output-on-failure --label-regex "performance" 2>&1 | tee ${REPORT_DIR}/performance_tests.log

# Run benchmarks
if [[ -f "bin/bench_vector" ]]; then
    echo -e "\n${BLUE}Running benchmarks...${NC}"
    ./bin/bench_vector --benchmark_format=json 2>&1 | tee ${REPORT_DIR}/benchmarks.json
    ./bin/bench_vector --benchmark_format=console 2>&1 | tee ${REPORT_DIR}/benchmarks.log
fi

# Generate coverage report if enabled
if [[ -f "coverage.info" ]]; then
    echo -e "\n${BLUE}Generating coverage report...${NC}"
    genhtml coverage.info --output-directory ${COVERAGE_DIR}
    echo -e "${GREEN}Coverage report: file://$(pwd)/${COVERAGE_DIR}/index.html${NC}"
fi

# Run memory check with valgrind
if command -v valgrind &> /dev/null; then
    echo -e "\n${BLUE}Running memory checks...${NC}"
    ctest -T memcheck --output-on-failure 2>&1 | tee ${REPORT_DIR}/memory_check.log
fi

# Summary
echo -e "\n${BLUE}=== Test Summary ===${NC}"
echo -e "Reports saved in: ${REPORT_DIR}"
echo -e "Coverage report: ${COVERAGE_DIR}/index.html"

# Check if all tests passed
if ctest --output-on-failure 2>&1 | grep -q "100% tests passed"; then
    echo -e "${GREEN}✓ All tests passed!${NC}"
else
    echo -e "${RED}✗ Some tests failed!${NC}"
    exit 1
fi
```

### **Best Practices for Testing with CMake**

1. **Separate Test Targets**: Keep test executables separate from production code
2. **Use Test Fixtures**: For complex setup/teardown scenarios
3. **Label Tests**: Use labels to categorize tests (unit, integration, performance)
4. **Parallel Testing**: Use `ctest -j N` for parallel test execution
5. **Test Discovery**: Automatically discover and add tests
6. **Coverage Integration**: Generate coverage reports as part of CI
7. **Performance Baselines**: Establish and track performance baselines
8. **Memory Checking**: Integrate valgrind or sanitizers
9. **Test Data Management**: Handle test data and fixtures properly
10. **Continuous Integration**: Make tests CI-friendly

### **Common Testing Patterns**

**Mocking with Google Mock:**
```cmake
# In CMakeLists.txt
find_package(GMock 1.11.0 REQUIRED)

add_executable(test_with_mocks
    test_mocked_service.cpp
)

target_link_libraries(test_with_mocks PRIVATE
    GMock::gmock
    GMock::gmock_main
)
```

**Property-based Testing:**
```cpp
// Using RapidCheck with Catch2
TEST_CASE("Vector properties") {
    rc::prop("Addition is commutative",
        [](const std::vector<double>& a, const std::vector<double>& b) {
            math::VectorD v1(a);
            math::VectorD v2(b);
            RC_ASSERT((v1 + v2) == (v2 + v1));
        });
}
```

**Golden File Testing:**
```cmake
# Compare output with expected "golden" files
add_test(NAME GoldenTest
    COMMAND ${CMAKE_COMMAND}
        -DEXECUTABLE=$<TARGET_FILE:my_app>
        -DINPUT=${CMAKE_SOURCE_DIR}/tests/data/input.txt
        -DGOLDEN=${CMAKE_SOURCE_DIR}/tests/data/expected_output.txt
        -DOUTPUT=${TEST_OUTPUT_DIRECTORY}/actual_output.txt
        -P ${CMAKE_SOURCE_DIR}/cmake/CompareWithGolden.cmake
)
```

### **Exercise: Create a Comprehensive Test Suite**

Create a project with:

1. **Unit Tests**: Using Google Test for core functionality
2. **Integration Tests**: Using Catch2 for component interactions
3. **Performance Tests**: Using Google Benchmark
4. **Property-based Tests**: Using RapidCheck
5. **Coverage Reports**: With gcov/lcov
6. **Memory Checks**: With valgrind/sanitizers
7. **Golden Tests**: For output validation
8. **Test Fixtures**: For complex setup scenarios
9. **CI Integration**: Scripts for automated testing
10. **Test Reports**: HTML/XML reports for analysis

### **Next Steps Preview**
In **PART-9**, we'll cover:
- Installation and packaging
- Creating installers for different platforms
- CPack configuration
- Creating Debian/RPM packages
- Windows MSI installers
- macOS .app bundles and DMG files
- Cross-platform deployment

**To continue, please reply with "Next" or "PART-9" to receive the next part of the course.**



# **PART-9: Installation, Packaging, and Distribution**

### **Topic: Creating Installers, Packages, and Deployable Distributions**

### **Introduction to Installation and Packaging**

Once you've built your project, you need to distribute it. CMake provides comprehensive support for installation and packaging through `install()` commands and CPack.

### **Project Structure for This Part**

```
packaging_project/
├── CMakeLists.txt
├── CPackConfig.cmake
├── README.md
├── LICENSE
├── CHANGELOG.md
├── src/
│   ├── lib/
│   ├── app/
│   └── tools/
├── include/
├── data/
│   ├── config/
│   └── resources/
├── docs/
├── cmake/
│   └── InstallConfig.cmake
├── packaging/
│   ├── debian/
│   ├── rpm/
│   └── nsis/
└── scripts/
    └── create_release.sh
```

### **Complete CMakeLists.txt with Installation**

```cmake
cmake_minimum_required(VERSION 3.18)
project(PackagingDemo VERSION 2.3.1
    DESCRIPTION "A demonstration of CMake packaging and installation"
    HOMEPAGE_URL "https://github.com/yourusername/packagingdemo"
    LANGUAGES CXX C
)

# Include GNU standard installation directories
include(GNUInstallDirs)

# Set project-specific installation directories
set(PROJECT_INSTALL_BINDIR "${CMAKE_INSTALL_BINDIR}")
set(PROJECT_INSTALL_LIBDIR "${CMAKE_INSTALL_LIBDIR}")
set(PROJECT_INSTALL_INCLUDEDIR "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}")
set(PROJECT_INSTALL_DATADIR "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}")
set(PROJECT_INSTALL_DOCDIR "${CMAKE_INSTALL_DOCDIR}/${PROJECT_NAME}")
set(PROJECT_INSTALL_CMAKEDIR "${PROJECT_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")

# Installation options
option(INSTALL_DEVELOPMENT "Install development files (headers, CMake config)" ON)
option(INSTALL_EXAMPLES "Install example programs" ON)
option(INSTALL_DOCS "Install documentation" ON)
option(INSTALL_DATA "Install data files" ON)
option(ENABLE_RUNTIME_DEPLOYMENT "Copy runtime dependencies during installation" OFF)

# Build options
option(BUILD_SHARED_LIBS "Build shared libraries" ON)
option(BUILD_TESTING "Build tests" OFF)  # Tests typically not installed

# C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

message(STATUS "=== Packaging Configuration ===")
message(STATUS "Project: ${PROJECT_NAME} v${PROJECT_VERSION}")
message(STATUS "Install prefix: ${CMAKE_INSTALL_PREFIX}")
message(STATUS "Install development files: ${INSTALL_DEVELOPMENT}")
message(STATUS "Build shared libraries: ${BUILD_SHARED_LIBS}")

# Add source subdirectories
add_subdirectory(src/lib)
add_subdirectory(src/app)
add_subdirectory(src/tools)

if(BUILD_TESTING)
    add_subdirectory(tests)
endif()

# ============ INSTALLATION CONFIGURATION ============

# Installation of targets
install(TARGETS
    mathlib              # Library target
    stats                # Another library
    DESTINATION ${PROJECT_INSTALL_LIBDIR}
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION ${PROJECT_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${PROJECT_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${PROJECT_INSTALL_BINDIR}  # For DLLs on Windows
    INCLUDES DESTINATION ${PROJECT_INSTALL_INCLUDEDIR}
)

# Install executables
install(TARGETS
    calc_app             # Main application
    math_tool            # Command line tool
    DESTINATION ${PROJECT_INSTALL_BINDIR}
)

# Install headers (development files)
if(INSTALL_DEVELOPMENT)
    install(DIRECTORY include/
        DESTINATION ${PROJECT_INSTALL_INCLUDEDIR}
        FILE_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
        DIRECTORY_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
        FILES_MATCHING
            PATTERN "*.h"
            PATTERN "*.hpp"
            PATTERN "*.hxx"
            PATTERN "*.inl"
        PATTERN ".svn" EXCLUDE
        PATTERN ".git" EXCLUDE
    )
endif()

# Install documentation
if(INSTALL_DOCS)
    install(DIRECTORY docs/
        DESTINATION ${PROJECT_INSTALL_DOCDIR}
        FILE_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
        DIRECTORY_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
        PATTERN ".svn" EXCLUDE
        PATTERN ".git" EXCLUDE
    )
    
    # Install individual documentation files
    install(FILES
        README.md
        LICENSE
        CHANGELOG.md
        DESTINATION ${PROJECT_INSTALL_DOCDIR}
        PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
    )
endif()

# Install data files
if(INSTALL_DATA)
    install(DIRECTORY data/
        DESTINATION ${PROJECT_INSTALL_DATADIR}
        FILE_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
        DIRECTORY_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
        USE_SOURCE_PERMISSIONS
    )
endif()

# Install examples
if(INSTALL_EXAMPLES)
    install(DIRECTORY examples/
        DESTINATION ${PROJECT_INSTALL_DATADIR}/examples
        FILE_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ OWNER_WRITE
        DIRECTORY_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
        USE_SOURCE_PERMISSIONS
    )
endif()

# Install CMake configuration files for find_package() support
include(CMakePackageConfigHelpers)

# Create a version file
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

# Create a project configuration file
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION ${PROJECT_INSTALL_CMAKEDIR}
)

# Export targets for find_package()
install(EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${PROJECT_INSTALL_CMAKEDIR}
)

# Install configuration files
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    DESTINATION ${PROJECT_INSTALL_CMAKEDIR}
)

# Install pkg-config file
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/${PROJECT_NAME}.pc.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.pc
    @ONLY
)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

# Post-installation script (platform specific)
if(WIN32)
    # On Windows, create a Start Menu shortcut
    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/packaging/windows/create_shortcut.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/create_shortcut.cmake
        @ONLY
    )
    install(SCRIPT ${CMAKE_CURRENT_BINARY_DIR}/create_shortcut.cmake)
endif()

# Generate uninstall target
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
    @ONLY
)
add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake
)

# Include CPack for packaging
set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
set(CPACK_PROJECT_DESCRIPTION ${PROJECT_DESCRIPTION})
set(CPACK_PROJECT_HOMEPAGE_URL ${PROJECT_HOMEPAGE_URL})
include(CPack)
```

### **Source Code Structure**

**src/lib/math/CMakeLists.txt:**
```cmake
# Math library
add_library(mathlib)

# Sources
target_sources(mathlib PRIVATE
    algebra.cpp
    calculus.cpp
    statistics.cpp
)

# Public headers
target_sources(mathlib PUBLIC
    FILE_SET HEADERS
    BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
    FILES
        algebra.h
        calculus.h
        statistics.h
)

# Include directories
target_include_directories(mathlib
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${PROJECT_INSTALL_INCLUDEDIR}>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# Library dependencies
find_package(Threads REQUIRED)
target_link_libraries(mathlib PUBLIC Threads::Threads)

# Properties
set_target_properties(mathlib PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    OUTPUT_NAME "math"
    DEBUG_POSTFIX "d"
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)

# Create alias for nice usage
add_library(${PROJECT_NAME}::math ALIAS mathlib)
```

**src/app/calculator/CMakeLists.txt:**
```cmake
# Calculator application
add_executable(calc_app)

target_sources(calc_app PRIVATE
    main.cpp
    calculator.cpp
    ui.cpp
)

# Link with our library
target_link_libraries(calc_app PRIVATE
    ${PROJECT_NAME}::math
)

# Platform-specific resources
if(WIN32)
    # Windows resource file
    target_sources(calc_app PRIVATE
        resources.rc
    )
    
    # Windows manifest
    set_target_properties(calc_app PROPERTIES
        WIN32_EXECUTABLE TRUE
    )
elseif(APPLE)
    # macOS bundle properties
    set_target_properties(calc_app PROPERTIES
        MACOSX_BUNDLE TRUE
        MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/Info.plist
    )
endif()

# Install runtime dependencies on Windows
if(WIN32 AND ENABLE_RUNTIME_DEPLOYMENT)
    # This would copy required DLLs
    add_custom_command(TARGET calc_app POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
                "$<TARGET_RUNTIME_DLLS:calc_app>" "$<TARGET_FILE_DIR:calc_app>"
        COMMAND_EXPAND_LISTS
    )
endif()
```

### **Configuration Files**

**cmake/PackagingDemoConfig.cmake.in:**
```cmake
@PACKAGE_INIT@

# Include the export file
include("${CMAKE_CURRENT_LIST_DIR}/@PROJECT_NAME@Targets.cmake")

# Check and include dependencies
include(CMakeFindDependencyMacro)

# Find dependencies that our library needs
find_dependency(Threads REQUIRED)

# Set variables for backward compatibility
set(@PROJECT_NAME@_VERSION @PROJECT_VERSION@)
set(@PROJECT_NAME@_INCLUDE_DIRS "@PACKAGE_PROJECT_INSTALL_INCLUDEDIR@")
set(@PROJECT_NAME@_LIBRARY_DIRS "@PACKAGE_PROJECT_INSTALL_LIBDIR@")
set(@PROJECT_NAME@_BINARY_DIRS "@PACKAGE_PROJECT_INSTALL_BINDIR@")

# Provide a helper macro to easily use the package
macro(@PROJECT_NAME@_setup_target target)
    if(NOT TARGET ${target})
        message(FATAL_ERROR
            "@PROJECT_NAME@_setup_target called with non-existent target: ${target}"
        )
    endif()
    
    # Set C++ standard
    target_compile_features(${target} PRIVATE cxx_std_17)
    
    # Add definitions if needed
    if(@PROJECT_NAME@_USE_STATIC_LIBS)
        target_compile_definitions(${target} PRIVATE @PROJECT_NAME@_STATIC)
    endif()
    
    message(STATUS 
        "Configured ${target} to use @PROJECT_NAME@ v@PROJECT_VERSION@"
    )
endmacro()
```

**cmake/cmake_uninstall.cmake.in:**
```cmake
# Uninstall script generated by CMake

if(NOT EXISTS "@CMAKE_CURRENT_BINARY_DIR@/install_manifest.txt")
    message(FATAL_ERROR "Cannot find install manifest: @CMAKE_CURRENT_BINARY_DIR@/install_manifest.txt")
endif()

file(READ "@CMAKE_CURRENT_BINARY_DIR@/install_manifest.txt" files)
string(REGEX REPLACE "\n" ";" files "${files}")

foreach(file ${files})
    message(STATUS "Uninstalling: ${file}")
    
    if(EXISTS "${file}")
        # Check if it's a directory or file
        if(IS_DIRECTORY "${file}")
            file(REMOVE_RECURSE "${file}")
        else()
            file(REMOVE "${file}")
        endif()
        
        if(EXISTS "${file}")
            message(FATAL_ERROR "Failed to remove: ${file}")
        endif()
    else()
        message(STATUS "File does not exist: ${file}")
    endif()
endforeach()

message(STATUS "Uninstallation completed")
```

**cmake/PackagingDemo.pc.in:**
```cmake
prefix=@CMAKE_INSTALL_PREFIX@
exec_prefix=${prefix}
libdir=${exec_prefix}/@CMAKE_INSTALL_LIBDIR@
includedir=${prefix}/@CMAKE_INSTALL_INCLUDEDIR@/@PROJECT_NAME@

Name: @PROJECT_NAME@
Description: @PROJECT_DESCRIPTION@
Version: @PROJECT_VERSION@
URL: @PROJECT_HOMEPAGE_URL@
Libs: -L${libdir} -lmath -lstats
Cflags: -I${includedir}
Requires: 
```

### **CPack Configuration**

**CPackConfig.cmake:**
```cmake
# CPack configuration

# Basic CPack settings
set(CPACK_PACKAGE_NAME "${PROJECT_NAME}")
set(CPACK_PACKAGE_VENDOR "YourCompany")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "${PROJECT_DESCRIPTION}")
set(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_SOURCE_DIR}/README.md")
set(CPACK_PACKAGE_HOMEPAGE_URL "${PROJECT_HOMEPAGE_URL}")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/LICENSE")
set(CPACK_RESOURCE_FILE_README "${CMAKE_SOURCE_DIR}/README.md")
set(CPACK_RESOURCE_FILE_WELCOME "${CMAKE_SOURCE_DIR}/docs/WELCOME.txt")

# Package maintainer
set(CPACK_PACKAGE_CONTACT "support@yourcompany.com")
set(CPACK_MAINTAINER "YourCompany Team <support@yourcompany.com>")

# Package icon (platform specific)
if(WIN32)
    set(CPACK_PACKAGE_ICON "${CMAKE_SOURCE_DIR}/packaging/windows/icon.ico")
elseif(APPLE)
    set(CPACK_PACKAGE_ICON "${CMAKE_SOURCE_DIR}/packaging/macos/icon.icns")
endif()

# Compress package
set(CPACK_THREADS 0)  # Use all cores
set(CPACK_ARCHIVE_COMPONENT_INSTALL ON)

# ============ COMPONENT-BASED INSTALLATION ============

# Define installation components
set(CPACK_COMPONENTS_ALL
    Runtime          # Executables and runtime libraries
    Development      # Headers, CMake configs, static libraries
    Documentation    # Docs and examples
    Data             # Data files
)

# Component descriptions
set(CPACK_COMPONENT_RUNTIME_DISPLAY_NAME "Runtime")
set(CPACK_COMPONENT_RUNTIME_DESCRIPTION 
    "Executables and shared libraries required to run ${PROJECT_NAME}"
)
set(CPACK_COMPONENT_RUNTIME_REQUIRED TRUE)  # Always install

set(CPACK_COMPONENT_DEVELOPMENT_DISPLAY_NAME "Development")
set(CPACK_COMPONENT_DEVELOPMENT_DESCRIPTION
    "Header files, static libraries, and CMake configuration for development"
)

set(CPACK_COMPONENT_DOCUMENTATION_DISPLAY_NAME "Documentation")
set(CPACK_COMPONENT_DOCUMENTATION_DESCRIPTION
    "Documentation, examples, and API reference"
)

set(CPACK_COMPONENT_DATA_DISPLAY_NAME "Data Files")
set(CPACK_COMPONENT_DATA_DESCRIPTION
    "Data files, configurations, and resources"
)

# Component grouping
set(CPACK_COMPONENT_RUNTIME_GROUP "Runtime")
set(CPACK_COMPONENT_DEVELOPMENT_GROUP "Development")
set(CPACK_COMPONENT_DOCUMENTATION_GROUP "Documentation")
set(CPACK_COMPONENT_DATA_GROUP "Data")

set(CPACK_COMPONENT_GROUP_RUNTIME_DESCRIPTION
    "Components required to run ${PROJECT_NAME}"
)
set(CPACK_COMPONENT_GROUP_DEVELOPMENT_DESCRIPTION
    "Components for developing with ${PROJECT_NAME}"
)
set(CPACK_COMPONENT_GROUP_DOCUMENTATION_DESCRIPTION
    "Documentation and examples"
)
set(CPACK_COMPONENT_GROUP_DATA_DESCRIPTION
    "Data files and resources"
)

# ============ PLATFORM-SPECIFIC PACKAGING ============

if(WIN32)
    # ============ WINDOWS: NSIS ============
    set(CPACK_GENERATOR "NSIS")
    
    # NSIS specific settings
    set(CPACK_NSIS_MODIFY_PATH ON)  # Add to PATH
    set(CPACK_NSIS_ENABLE_UNINSTALL_BEFORE_INSTALL ON)
    set(CPACK_NSIS_MUI_ICON "${CMAKE_SOURCE_DIR}/packaging/windows/icon.ico")
    set(CPACK_NSIS_MUI_UNIICON "${CMAKE_SOURCE_DIR}/packaging/windows/icon.ico")
    set(CPACK_NSIS_INSTALLED_ICON_NAME "bin\\\\calc_app.exe")
    set(CPACK_NSIS_DISPLAY_NAME "${PROJECT_NAME}")
    set(CPACK_NSIS_PACKAGE_NAME "${PROJECT_NAME}")
    set(CPACK_NSIS_HELP_LINK "${PROJECT_HOMEPAGE_URL}")
    set(CPACK_NSIS_URL_INFO_ABOUT "${PROJECT_HOMEPAGE_URL}")
    set(CPACK_NSIS_CONTACT "${CPACK_PACKAGE_CONTACT}")
    
    # Create Start Menu shortcut
    set(CPACK_NSIS_CREATE_ICONS_EXTRA "
        CreateShortCut '$SMPROGRAMS\\\\$STARTMENU_FOLDER\\\\${PROJECT_NAME}.lnk' '$INSTDIR\\\\bin\\\\calc_app.exe'
    ")
    set(CPACK_NSIS_DELETE_ICONS_EXTRA "
        Delete '$SMPROGRAMS\\\\$STARTMENU_FOLDER\\\\${PROJECT_NAME}.lnk'
    ")
    
    # Component installation for NSIS
    set(CPACK_NSIS_COMPONENT_PAGES ON)
    set(CPACK_NSIS_MENU_LINKS
        "bin/calc_app.exe" "${PROJECT_NAME}"
        "docs/README.html" "Documentation"
    )
    
    # ============ WINDOWS: WIX ============
    # Alternative to NSIS
    set(CPACK_WIX_UPGRADE_GUID "YOUR-GUID-HERE-1234-5678-90AB-CDEF12345678")
    set(CPACK_WIX_PRODUCT_ICON "${CMAKE_SOURCE_DIR}/packaging/windows/icon.ico")
    set(CPACK_WIX_UI_BANNER "${CMAKE_SOURCE_DIR}/packaging/windows/banner.bmp")
    set(CPACK_WIX_UI_DIALOG "${CMAKE_SOURCE_DIR}/packaging/windows/dialog.bmp")
    set(CPACK_WIX_PATCH_FILE "${CMAKE_SOURCE_DIR}/packaging/windows/patch.xml")
    
    # Add WIX to generators
    list(APPEND CPACK_GENERATOR "WIX")
    
elseif(APPLE)
    # ============ macOS: Bundle and PackageMaker ============
    set(CPACK_GENERATOR "Bundle;DragNDrop")
    
    # Bundle settings
    set(CPACK_BUNDLE_NAME "${PROJECT_NAME}")
    set(CPACK_BUNDLE_PLIST "${CMAKE_BINARY_DIR}/Info.plist")
    set(CPACK_BUNDLE_ICON "${CMAKE_SOURCE_DIR}/packaging/macos/icon.icns")
    set(CPACK_BUNDLE_STARTUP_COMMAND "${CMAKE_INSTALL_PREFIX}/bin/calc_app")
    
    # DMG settings
    set(CPACK_DMG_VOLUME_NAME "${PROJECT_NAME} ${PROJECT_VERSION}")
    set(CPACK_DMG_FORMAT "UDBZ")  # Compressed
    set(CPACK_DMG_DS_STORE "${CMAKE_SOURCE_DIR}/packaging/macos/DS_Store")
    set(CPACK_DMG_BACKGROUND_IMAGE "${CMAKE_SOURCE_DIR}/packaging/macos/background.png")
    
    # PackageMaker settings (legacy)
    set(CPACK_OSX_PACKAGE_VERSION "${CMAKE_OSX_DEPLOYMENT_TARGET}")
    
elseif(UNIX AND NOT APPLE)
    # ============ LINUX: DEB and RPM ============
    set(CPACK_GENERATOR "DEB;RPM;TGZ")
    
    # DEB package settings
    set(CPACK_DEBIAN_PACKAGE_NAME "${CMAKE_PROJECT_NAME}")
    set(CPACK_DEBIAN_PACKAGE_VERSION "${PROJECT_VERSION}")
    set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "amd64")
    set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CPACK_MAINTAINER}")
    set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "${PROJECT_DESCRIPTION}")
    set(CPACK_DEBIAN_PACKAGE_SECTION "devel")
    set(CPACK_DEBIAN_PACKAGE_PRIORITY "optional")
    set(CPACK_DEBIAN_PACKAGE_RECOMMENDS "libc6 (>= 2.31)")
    set(CPACK_DEBIAN_PACKAGE_SUGGESTS "cmake, build-essential")
    set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "${PROJECT_HOMEPAGE_URL}")
    set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
    
    # DEB component packaging
    set(CPACK_DEB_COMPONENT_INSTALL ON)
    set(CPACK_DEBIAN_RUNTIME_PACKAGE_NAME "${CMAKE_PROJECT_NAME}")
    set(CPACK_DEBIAN_DEVELOPMENT_PACKAGE_NAME "${CMAKE_PROJECT_NAME}-dev")
    set(CPACK_DEBIAN_DOCUMENTATION_PACKAGE_NAME "${CMAKE_PROJECT_NAME}-doc")
    set(CPACK_DEBIAN_DATA_PACKAGE_NAME "${CMAKE_PROJECT_NAME}-data")
    
    # RPM package settings
    set(CPACK_RPM_PACKAGE_NAME "${CMAKE_PROJECT_NAME}")
    set(CPACK_RPM_PACKAGE_VERSION "${PROJECT_VERSION}")
    set(CPACK_RPM_PACKAGE_ARCHITECTURE "x86_64")
    set(CPACK_RPM_PACKAGE_VENDOR "YourCompany")
    set(CPACK_RPM_PACKAGE_LICENSE "MIT")
    set(CPACK_RPM_PACKAGE_GROUP "Development/Tools")
    set(CPACK_RPM_PACKAGE_URL "${PROJECT_HOMEPAGE_URL}")
    set(CPACK_RPM_PACKAGE_DESCRIPTION "${PROJECT_DESCRIPTION}")
    set(CPACK_RPM_PACKAGE_REQUIRES "glibc >= 2.31")
    set(CPACK_RPM_FILE_NAME RPM-DEFAULT)
    
    # RPM component packaging
    set(CPACK_RPM_COMPONENT_INSTALL ON)
    set(CPACK_RPM_RUNTIME_PACKAGE_NAME "${CMAKE_PROJECT_NAME}")
    set(CPACK_RPM_DEVELOPMENT_PACKAGE_NAME "${CMAKE_PROJECT_NAME}-devel")
    set(CPACK_RPM_DOCUMENTATION_PACKAGE_NAME "${CMAKE_PROJECT_NAME}-doc")
    set(CPACK_RPM_DATA_PACKAGE_NAME "${CMAKE_PROJECT_NAME}-data")
    
    # Create post-install and pre-uninstall scripts
    set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA
        "${CMAKE_SOURCE_DIR}/packaging/debian/postinst"
        "${CMAKE_SOURCE_DIR}/packaging/debian/prerm"
    )
    
    set(CPACK_RPM_POST_INSTALL_SCRIPT_FILE
        "${CMAKE_SOURCE_DIR}/packaging/rpm/postinst"
    )
    set(CPACK_RPM_PRE_UNINSTALL_SCRIPT_FILE
        "${CMAKE_SOURCE_DIR}/packaging/rpm/prerm"
    )
endif()

# ============ SOURCE PACKAGE ============
set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")
set(CPACK_SOURCE_IGNORE_FILES
    "/build/"
    "/\\.git/"
    "/\\.svn/"
    "/\\.vs/"
    "/\\.vscode/"
    "\\\\.swp$"
    "\\\\.swo$"
    "\\\\.user$"
    "\\\\.suo$"
    "CMakeLists.txt.user"
    "cmake_install.cmake"
    "CMakeCache.txt"
    "CPackConfig.cmake"
    "CPackSourceConfig.cmake"
    "install_manifest.txt"
    "\\\\.gz$"
    "\\\\.zip$"
    "/\\\\.DS_Store$"
)
set(CPACK_SOURCE_PACKAGE_FILE_NAME
    "${PROJECT_NAME}-${PROJECT_VERSION}-src"
)

# ============ COMMON SETTINGS ============
set(CPACK_PACKAGE_DIRECTORY "${CMAKE_BINARY_DIR}/packages")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "${PROJECT_NAME}")

# Set file permissions for installed files
set(CMAKE_INSTALL_DEFAULT_DIRECTORY_PERMISSIONS
    OWNER_READ OWNER_WRITE OWNER_EXECUTE
    GROUP_READ GROUP_EXECUTE
    WORLD_READ WORLD_EXECUTE
)

# Configure file for component installation
configure_file(
    "${CMAKE_SOURCE_DIR}/cmake/InstallRequiredSystemLibraries.cmake.in"
    "${CMAKE_BINARY_DIR}/InstallRequiredSystemLibraries.cmake"
    @ONLY
)

# Include CPack module (MUST BE LAST)
include(CPack)
```

### **Platform-Specific Packaging Scripts**

**packaging/debian/postinst:**
```bash
#!/bin/bash
# Post-installation script for Debian/Ubuntu

set -e

case "$1" in
    configure)
        # Update system cache of shared libraries
        ldconfig || true
        
        # Create symlinks or perform other setup
        if [ ! -L /usr/local/bin/calc ]; then
            ln -sf /usr/bin/calc_app /usr/local/bin/calc
        fi
        
        # Update desktop database if we installed .desktop files
        if [ -d /usr/share/applications ]; then
            update-desktop-database /usr/share/applications || true
        fi
        
        echo "${PROJECT_NAME} v${VERSION} installed successfully."
        ;;
        
    abort-upgrade|abort-remove|abort-deconfigure)
        # Handle upgrade failures
        ;;
        
    *)
        echo "postinst called with unknown argument \`$1'" >&2
        exit 1
        ;;
esac

exit 0
```

**packaging/debian/prerm:**
```bash
#!/bin/bash
# Pre-removal script for Debian/Ubuntu

set -e

case "$1" in
    remove|upgrade)
        # Remove symlinks
        if [ -L /usr/local/bin/calc ]; then
            rm -f /usr/local/bin/calc
        fi
        
        # Clean up other resources
        ;;
        
    failed-upgrade)
        # Handle failed upgrade
        ;;
        
    *)
        echo "prerm called with unknown argument \`$1'" >&2
        exit 1
        ;;
esac

exit 0
```

**packaging/windows/create_shortcut.cmake.in:**
```cmake
# Windows shortcut creation script

# Create Start Menu shortcut
set(START_MENU_DIR "$STARTMENU_FOLDER\\@PROJECT_NAME@")
set(SHORTCUT_PATH "${START_MENU_DIR}\\@PROJECT_NAME@.lnk")
set(TARGET_PATH "$INSTDIR\\bin\\calc_app.exe")

# Create directory if it doesn't exist
execute_process(
    COMMAND cmd.exe /c "if not exist \"${START_MENU_DIR}\" mkdir \"${START_MENU_DIR}\""
)

# Create shortcut using Windows Script Host
file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/create_shortcut.vbs
"Set oWS = WScript.CreateObject(\"WScript.Shell\")
sLinkFile = \"${SHORTCUT_PATH}\"
Set oLink = oWS.CreateShortcut(sLinkFile)
oLink.TargetPath = \"${TARGET_PATH}\"
oLink.WorkingDirectory = \"$INSTDIR\\bin\"
oLink.Description = \"@PROJECT_NAME@\"
oLink.Save"
)

execute_process(
    COMMAND cscript.exe //NoLogo ${CMAKE_CURRENT_BINARY_DIR}/create_shortcut.vbs
)

# Clean up
file(REMOVE ${CMAKE_CURRENT_BINARY_DIR}/create_shortcut.vbs)

message(STATUS "Created shortcut: ${SHORTCUT_PATH}")
```

### **Installation Commands and Usage**

**Basic Installation:**
```bash
# Configure with install prefix
cmake -B build -S . -DCMAKE_INSTALL_PREFIX=/usr/local

# Build
cmake --build build --config Release

# Install to prefix
cmake --install build

# Install to custom location
cmake --install build --prefix ~/myapp

# Install specific components
cmake --install build --component Runtime
cmake --install build --component Development

# Create a staging directory for packaging
cmake --install build --prefix /tmp/staging
```

**Creating Packages:**
```bash
# Build the project first
cmake --build build --config Release

# Create packages (all generators)
cd build
cpack

# Create specific package types
cpack -G DEB    # Debian package
cpack -G RPM    # RPM package
cpack -G NSIS   # Windows installer
cpack -G Bundle # macOS bundle

# Create source package
cpack --config CPackSourceConfig.cmake

# Verbose output
cpack --verbose

# Override CPack variables
cpack -D CPACK_PACKAGE_VERSION="2.0.0" -D CPACK_PACKAGE_NAME="MyApp"
```

### **Testing Installation**

**test_installation.cmake:**
```cmake
# Script to test installation
cmake_minimum_required(VERSION 3.18)

# Test find_package() after installation
find_package(PackagingDemo 2.3 REQUIRED)

message(STATUS "Found PackagingDemo: ${PackagingDemo_VERSION}")
message(STATUS "Include dirs: ${PackagingDemo_INCLUDE_DIRS}")
message(STATUS "Library dirs: ${PackagingDemo_LIBRARY_DIRS}")

# Create a test project that uses the installed package
project(TestInstallation)
add_executable(test_install test.cpp)

# Use the installed package
target_link_libraries(test_install PRIVATE PackagingDemo::math)

# Configure and build
if(CMAKE_CROSSCOMPILING)
    message(STATUS "Cross-compiling, skipping build test")
else()
    # Try to build the test
    try_compile(COMPILE_RESULT
        ${CMAKE_CURRENT_BINARY_DIR}
        SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/test.cpp
        LINK_LIBRARIES PackagingDemo::math
        CMAKE_FLAGS "-DCMAKE_PREFIX_PATH=${CMAKE_INSTALL_PREFIX}"
    )
    
    if(COMPILE_RESULT)
        message(STATUS "Installation test PASSED")
    else()
        message(FATAL_ERROR "Installation test FAILED")
    endif()
endif()
```

### **Continuous Integration Packaging**

**.github/workflows/packages.yml:**
```yaml
name: Build Packages

on:
  release:
    types: [published]
  workflow_dispatch:

jobs:
  build-packages:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        include:
          - os: ubuntu-latest
            cpack_generator: "DEB"
            artifact_name: linux-deb
          - os: windows-latest
            cpack_generator: "NSIS;ZIP"
            artifact_name: windows
          - os: macos-latest
            cpack_generator: "Bundle;DragNDrop"
            artifact_name: macos

    steps:
    - uses: actions/checkout@v3
    
    - name: Install dependencies (Linux)
      if: runner.os == 'Linux'
      run: |
        sudo apt-get update
        sudo apt-get install -y build-essential cmake rpm
        
    - name: Configure CMake
      run: cmake -B build -DCMAKE_BUILD_TYPE=Release
      
    - name: Build
      run: cmake --build build --config Release --parallel
      
    - name: Create Packages
      run: |
        cd build
        cpack -G "${{ matrix.cpack_generator }}"
        
    - name: Upload Artifacts
      uses: actions/upload-artifact@v3
      with:
        name: ${{ matrix.artifact_name }}
        path: build/*.deb build/*.rpm build/*.exe build/*.msi build/*.dmg build/*.zip
```

### **Advanced Packaging Features**

**Component-based installation with dependencies:**
```cmake
# Define component dependencies
set(CPACK_DEBIAN_RUNTIME_PACKAGE_DEPENDS "libc6 (>= 2.31), libstdc++6 (>= 9)")
set(CPACK_DEBIAN_DEVELOPMENT_PACKAGE_DEPENDS 
    "${CPACK_DEBIAN_RUNTIME_PACKAGE_NAME} (>= ${PROJECT_VERSION}), cmake, pkg-config"
)

# Component-specific installation scripts
install(CODE "
    if(CMAKE_INSTALL_COMPONENT STREQUAL \"Runtime\")
        message(STATUS \"Installing runtime component...\")
        # Additional runtime installation steps
    endif()
")

# Platform-specific component rules
if(WIN32)
    set(CPACK_NSIS_COMPONENT_SECTIONS
        "Runtime"
        "Development"
        "Documentation"
    )
endif()
```

**Creating Relocatable Packages:**
```cmake
# Make packages relocatable
set(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT TRUE)

# Use relative paths in configuration files
set(CMAKE_INSTALL_RELATIVE_CMAKECONFIGDIR "../lib/cmake/${PROJECT_NAME}")

# Handle RPATH for relocatable executables
set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib")
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
```

**Versioning and Upgrade Support:**
```cmake
# Windows upgrade GUID (must be unique per major version)
set(CPACK_WIX_UPGRADE_GUID "12345678-1234-1234-1234-123456789012")

# RPM versioning
set(CPACK_RPM_PACKAGE_RELEASE "1")
set(CPACK_RPM_PACKAGE_EPOCH "0")  # For version resets

# DEB versioning with revision
set(CPACK_DEBIAN_PACKAGE_RELEASE "1ubuntu1")

# Handle file conflicts during upgrades
set(CPACK_RPM_EXCLUDE_FROM_AUTO_FILELIST
    /usr/local
    /usr/local/bin
)
```

### **Complete Example: Calculator Application**

**src/app/calculator/main.cpp:**
```cpp
#include "math/algebra.h"
#include "math/statistics.h"
#include <iostream>
#include <vector>
#include <string>

void print_help() {
    std::cout << "Calculator Application v2.3.1\n";
    std::cout << "Usage:\n";
    std::cout << "  calc <operation> <values...>\n";
    std::cout << "Operations:\n";
    std::cout << "  add <a> <b>          - Add two numbers\n";
    std::cout << "  mean <values...>     - Calculate mean\n";
    std::cout << "  stats <values...>    - Calculate statistics\n";
}

int main(int argc, char* argv[]) {
    if (argc < 2) {
        print_help();
        return 1;
    }
    
    std::string operation = argv[1];
    
    if (operation == "add" && argc == 4) {
        double a = std::stod(argv[2]);
        double b = std::stod(argv[3]);
        double result = math::add(a, b);
        std::cout << "Result: " << result << std::endl;
        
    } else if (operation == "mean" && argc > 2) {
        std::vector<double> values;
        for (int i = 2; i < argc; ++i) {
            values.push_back(std::stod(argv[i]));
        }
        double mean = math::mean(values.data(), values.size());
        std::cout << "Mean: " << mean << std::endl;
        
    } else if (operation == "stats" && argc > 2) {
        std::vector<double> values;
        for (int i = 2; i < argc; ++i) {
            values.push_back(std::stod(argv[i]));
        }
        
        auto stats = math::calculate_statistics(values.data(), values.size());
        std::cout << "Statistics:\n";
        std::cout << "  Mean:     " << stats.mean << "\n";
        std::cout << "  Median:   " << stats.median << "\n";
        std::cout << "  Std Dev:  " << stats.std_dev << "\n";
        std::cout << "  Min:      " << stats.min << "\n";
        std::cout << "  Max:      " << stats.max << "\n";
        
    } else {
        print_help();
        return 1;
    }
    
    return 0;
}
```

### **Packaging Script**

**scripts/create_release.sh:**
```bash
#!/bin/bash
set -e

# Release packaging script

VERSION=$(git describe --tags --abbrev=0)
BUILD_DIR="build-release"
PACKAGE_DIR="packages"

echo "Creating release packages for version: ${VERSION}"

# Clean previous builds
rm -rf ${BUILD_DIR} ${PACKAGE_DIR}
mkdir -p ${PACKAGE_DIR}

# Detect platform and set appropriate generators
if [[ "$OSTYPE" == "linux-gnu"* ]]; then
    GENERATORS="DEB;RPM;TGZ"
    EXTRA_FLAGS=""
elif [[ "$OSTYPE" == "darwin"* ]]; then
    GENERATORS="Bundle;DragNDrop;TGZ"
    EXTRA_FLAGS="-DCMAKE_OSX_DEPLOYMENT_TARGET=11.0"
elif [[ "$OSTYPE" == "msys" || "$OSTYPE" == "win32" ]]; then
    GENERATORS="NSIS;ZIP"
    EXTRA_FLAGS=""
else
    echo "Unknown OS: $OSTYPE"
    exit 1
fi

# Configure
cmake -B ${BUILD_DIR} -S . \
    -DCMAKE_BUILD_TYPE=Release \
    -DCPACK_PACKAGE_VERSION=${VERSION} \
    -DCPACK_GENERATOR="${GENERATORS}" \
    ${EXTRA_FLAGS}

# Build
cmake --build ${BUILD_DIR} --config Release --parallel

# Create packages
cd ${BUILD_DIR}
cpack --verbose

# Move packages to package directory
mv *.deb *.rpm *.tar.gz *.zip *.exe *.msi *.dmg ../${PACKAGE_DIR}/ 2>/dev/null || true

# Create source package
cpack --config CPackSourceConfig.cmake
mv *.tar.gz *.zip ../${PACKAGE_DIR}/ 2>/dev/null || true

cd ..

echo ""
echo "Packages created in ${PACKAGE_DIR}/:"
ls -la ${PACKAGE_DIR}/

# Generate checksums
cd ${PACKAGE_DIR}
sha256sum * > SHA256SUMS
cd ..

echo "Release packaging complete!"
```

### **Best Practices for Packaging**

1. **Version Everything**: Use semantic versioning for packages
2. **Component-based Installation**: Allow users to install only what they need
3. **Relocatable Packages**: Make packages work regardless of install location
4. **Dependency Management**: Clearly specify package dependencies
5. **Upgrade Support**: Handle upgrades and uninstalls cleanly
6. **Testing**: Test packages on clean systems
7. **Documentation**: Include installation instructions
8. **Signing**: Sign packages for security
9. **Reproducible Builds**: Ensure consistent package creation
10. **Multi-platform Support**: Support all target platforms

### **Common Packaging Issues and Solutions**

**Issue 1: File conflicts during upgrades**
```cmake
# Solution: Use versioned filenames
set(CPACK_PACKAGE_FILE_NAME "${PROJECT_NAME}-${PROJECT_VERSION}-${CMAKE_SYSTEM_PROCESSOR}")
```

**Issue 2: Missing dependencies**
```cmake
# Solution: Use CMake's find_dependency in config files
find_dependency(Threads REQUIRED)
find_dependency(ZLIB 1.2.8 REQUIRED)
```

**Issue 3: RPATH issues on Linux/macOS**
```cmake
# Solution: Set proper RPATH settings
set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib")
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
```

**Issue 4: Windows DLL deployment**
```cmake
# Solution: Bundle required DLLs
install(FILES
    $<TARGET_RUNTIME_DLLS:your_target>
    DESTINATION bin
    COMPONENT Runtime
)
```

### **Exercise: Create a Complete Packaging Project**

Create a project with:

1. **Library**: A useful library with public headers
2. **Application**: Command-line tool using the library
3. **Components**: Separate Runtime, Development, Documentation components
4. **Packages**: Create DEB, RPM, NSIS, and macOS packages
5. **Installation**: Proper install() commands for all files
6. **find_package() support**: Config files for other projects
7. **Testing**: Installation test scripts
8. **CI Integration**: Automated package building
9. **Release Script**: One-command release creation
10. **Documentation**: Installation and usage guides

### **Next Steps Preview**
In **PART-10**, we'll cover:
- Advanced CMake scripting and modules
- Writing custom CMake functions and macros
- File manipulation and generation
- Process execution and external commands
- Creating reusable CMake modules
- Debugging complex CMake projects

**To continue, please reply with "Next" or "PART-10" to receive the next part of the course.**


# **PART-10: Advanced CMake Scripting and Modules**

### **Topic: Mastering CMake Scripting, Custom Functions, and Module Creation**

### **Introduction to Advanced CMake Scripting**

CMake is not just a build system generator - it's a full scripting language. Mastering CMake scripting allows you to create powerful, reusable build systems and automate complex tasks.

### **Project Structure for This Part**

```
advanced_scripting/
├── CMakeLists.txt
├── cmake/
│   ├── AdvancedFunctions.cmake
│   ├── CodeGeneration.cmake
│   ├── ProjectTemplates.cmake
│   ├── PlatformChecks.cmake
│   └── UtilityFunctions.cmake
├── src/
├── generated/
├── scripts/
│   └── custom_build_steps.cmake
└── templates/
    └── class_template.h.in
```

### **Root CMakeLists.txt with Advanced Features**

```cmake
cmake_minimum_required(VERSION 3.20)
project(AdvancedScriptingDemo VERSION 2.0.0 LANGUAGES CXX C)

# Add custom module path
list(APPEND CMAKE_MODULE_PATH
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake
    ${CMAKE_CURRENT_BINARY_DIR}/cmake
)

# Include custom modules
include(AdvancedFunctions)
include(CodeGeneration)
include(PlatformChecks)
include(UtilityFunctions)

# Enable advanced features
cmake_policy(SET CMP0077 NEW)  # option() honors normal variables
cmake_policy(SET CMP0092 NEW)  # MSVC runtime library flags
cmake_policy(SET CMP0115 NEW)  # source file extensions

# Set output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Create a generated directory for dynamically created files
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/generated)

# ==================== ADVANCED SCRIPTING DEMOS ====================

# Example 1: Complex variable manipulation
set(COMPLEX_VAR "item1;item2;item3")
list(TRANSFORM COMPLEX_VAR PREPEND "prefix_")
list(TRANSFORM COMPLEX_VAR APPEND "_suffix")
message(STATUS "Transformed list: ${COMPLEX_VAR}")

# Example 2: Generator expressions in scripts
set(DEBUG_FLAGS "$<IF:$<CONFIG:Debug>,-DDEBUG=1,-DNDEBUG=1>")
message(STATUS "Debug flags expression: ${DEBUG_FLAGS}")

# Example 3: JSON-like structure with lists
set(PROJECT_CONFIG
    NAME "${PROJECT_NAME}"
    VERSION "${PROJECT_VERSION}"
    SOURCES "src/main.cpp;src/utils.cpp"
    INCLUDES "include;src"
)
message(STATUS "Project config: ${PROJECT_CONFIG}")

# Process the configuration
list(FIND PROJECT_CONFIG "SOURCES" SOURCES_INDEX)
math(EXPR SOURCES_START_INDEX "${SOURCES_INDEX} + 1")
list(SUBLIST PROJECT_CONFIG ${SOURCES_START_INDEX} -1 SOURCE_FILES)

# Add subdirectories
add_subdirectory(src)
```

### **Custom CMake Functions and Macros**

**cmake/UtilityFunctions.cmake:**
```cmake
# UtilityFunctions.cmake - Collection of reusable CMake utilities

# ==================== STRING FUNCTIONS ====================

# Convert string to uppercase
function(string_to_upper var str)
    string(TOUPPER "${str}" result)
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# Convert string to lowercase
function(string_to_lower var str)
    string(TOLOWER "${str}" result)
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# Check if string contains substring
function(string_contains var str substring)
    string(FIND "${str}" "${substring}" position)
    if(position GREATER_EQUAL 0)
        set(${var} TRUE PARENT_SCOPE)
    else()
        set(${var} FALSE PARENT_SCOPE)
    endif()
endfunction()

# Split string by delimiter
function(string_split var str delimiter)
    string(REPLACE "${delimiter}" ";" result "${str}")
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# Join list into string with delimiter
function(string_join var list delimiter)
    list(LENGTH list list_length)
    if(list_length EQUAL 0)
        set(${var} "" PARENT_SCOPE)
        return()
    endif()
    
    set(result "")
    foreach(item IN LISTS list)
        if(result STREQUAL "")
            set(result "${item}")
        else()
            set(result "${result}${delimiter}${item}")
        endif()
    endforeach()
    
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# ==================== FILE FUNCTIONS ====================

# Recursively find files with pattern
function(find_files_recursive var start_dir pattern)
    set(result "")
    file(GLOB_RECURSE files RELATIVE "${start_dir}" "${start_dir}/${pattern}")
    foreach(file IN LISTS files)
        list(APPEND result "${file}")
    endforeach()
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# Read file into variable
function(read_file var filename)
    if(NOT EXISTS "${filename}")
        message(WARNING "File not found: ${filename}")
        set(${var} "" PARENT_SCOPE)
        return()
    endif()
    
    file(READ "${filename}" content)
    set(${var} "${content}" PARENT_SCOPE)
endfunction()

# Write file with backup
function(write_file_with_backup filename content)
    if(EXISTS "${filename}")
        get_filename_component(dir "${filename}" DIRECTORY)
        get_filename_component(name "${filename}" NAME)
        set(backup "${dir}/${name}.backup")
        file(RENAME "${filename}" "${backup}")
        message(STATUS "Backed up ${filename} to ${backup}")
    endif()
    
    file(WRITE "${filename}" "${content}")
endfunction()

# ==================== LIST FUNCTIONS ====================

# Remove duplicates preserving order
function(list_remove_duplicates var)
    set(result "")
    set(seen "")
    foreach(item IN LISTS ${var})
        list(FIND seen "${item}" found)
        if(found EQUAL -1)
            list(APPEND result "${item}")
            list(APPEND seen "${item}")
        endif()
    endforeach()
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# Filter list with regex
function(list_filter var list regex)
    set(result "")
    foreach(item IN LISTS list)
        if(item MATCHES "${regex}")
            list(APPEND result "${item}")
        endif()
    endforeach()
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# Map function over list
function(list_map var list func)
    set(result "")
    foreach(item IN LISTS list)
        # Apply function (this is a simple example)
        # In real usage, you'd need to handle different functions
        if(func STREQUAL "TOUPPER")
            string(TOUPPER "${item}" mapped_item)
        elseif(func STREQUAL "TOLOWER")
            string(TOLOWER "${item}" mapped_item)
        else()
            set(mapped_item "${item}")
        endif()
        list(APPEND result "${mapped_item}")
    endforeach()
    set(${var} "${result}" PARENT_SCOPE)
endfunction()

# ==================== MATHEMATICAL FUNCTIONS ====================

# Calculate average of list of numbers
function(list_average var list)
    if(NOT list)
        set(${var} 0 PARENT_SCOPE)
        return()
    endif()
    
    set(total 0)
    set(count 0)
    foreach(item IN LISTS list)
        if(item MATCHES "^[0-9]+(\\.[0-9]+)?$")
            math(EXPR total "${total} + ${item}")
            math(EXPR count "${count} + 1")
        endif()
    endforeach()
    
    if(count GREATER 0)
        math(EXPR avg "${total} / ${count}")
        set(${var} ${avg} PARENT_SCOPE)
    else()
        set(${var} 0 PARENT_SCOPE)
    endif()
endfunction()

# Find minimum value in list
function(list_min var list)
    if(NOT list)
        set(${var} "" PARENT_SCOPE)
        return()
    endif()
    
    list(GET list 0 min_val)
    foreach(item IN LISTS list)
        if(item LESS min_val)
            set(min_val ${item})
        endif()
    endforeach()
    set(${var} ${min_val} PARENT_SCOPE)
endfunction()

# Find maximum value in list
function(list_max var list)
    if(NOT list)
        set(${var} "" PARENT_SCOPE)
        return()
    endif()
    
    list(GET list 0 max_val)
    foreach(item IN LISTS list)
        if(item GREATER max_val)
            set(max_val ${item})
        endif()
    endforeach()
    set(${var} ${max_val} PARENT_SCOPE)
endfunction()

# ==================== CONDITIONAL FUNCTIONS ====================

# Check if all items in list satisfy condition
function(list_all var list condition)
    set(all_true TRUE)
    foreach(item IN LISTS list)
        # Simple condition checking - can be expanded
        if(condition STREQUAL "EXISTS")
            if(NOT EXISTS "${item}")
                set(all_true FALSE)
                break()
            endif()
        elseif(condition STREQUAL "DIRECTORY")
            if(NOT IS_DIRECTORY "${item}")
                set(all_true FALSE)
                break()
            endif()
        endif()
    endforeach()
    set(${var} ${all_true} PARENT_SCOPE)
endfunction()

# Check if any item in list satisfies condition
function(list_any var list condition)
    set(any_true FALSE)
    foreach(item IN LISTS list)
        if(condition STREQUAL "EXISTS" AND EXISTS "${item}")
            set(any_true TRUE)
            break()
        elseif(condition STREQUAL "DIRECTORY" AND IS_DIRECTORY "${item}")
            set(any_true TRUE)
            break()
        endif()
    endforeach()
    set(${var} ${any_true} PARENT_SCOPE)
endfunction()

# ==================== DEBUGGING FUNCTIONS ====================

# Print variable with type information
function(print_var var)
    if(DEFINED ${var})
        set(type "")
        
        # Try to determine type
        if(${var} MATCHES "^[0-9]+$")
            set(type " (integer)")
        elseif(${var} MATCHES "^[0-9]+\\.[0-9]+$")
            set(type " (float)")
        elseif(${var} MATCHES ";")
            list(LENGTH ${var} list_length)
            set(type " (list of ${list_length} items)")
        endif()
        
        message(STATUS "${var}${type} = ${${var}}")
    else()
        message(STATUS "${var} is not defined")
    endif()
endfunction()

# Print all variables matching pattern
function(print_vars_matching pattern)
    get_cmake_property(vars VARIABLES)
    foreach(var IN LISTS vars)
        if(var MATCHES "${pattern}")
            print_var(${var})
        endif()
    endforeach()
endfunction()

# ==================== PROJECT MANAGEMENT FUNCTIONS ====================

# Create a standardized target with common properties
function(create_standard_target target type)
    set(options SHARED STATIC EXECUTABLE INTERFACE)
    set(one_value_args)
    set(multi_value_args SOURCES INCLUDES LIBRARIES DEFINITIONS)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    # Create target based on type
    if(type STREQUAL "LIBRARY")
        if(ARG_SHARED)
            add_library(${target} SHARED ${ARG_SOURCES})
        else()
            add_library(${target} STATIC ${ARG_SOURCES})
        endif()
    elseif(type STREQUAL "EXECUTABLE")
        add_executable(${target} ${ARG_SOURCES})
    elseif(type STREQUAL "INTERFACE")
        add_library(${target} INTERFACE)
    else()
        message(FATAL_ERROR "Unknown target type: ${type}")
    endif()
    
    # Set common properties
    if(ARG_INCLUDES)
        target_include_directories(${target} PUBLIC ${ARG_INCLUDES})
    endif()
    
    if(ARG_LIBRARIES)
        target_link_libraries(${target} PUBLIC ${ARG_LIBRARIES})
    endif()
    
    if(ARG_DEFINITIONS)
        target_compile_definitions(${target} PUBLIC ${ARG_DEFINITIONS})
    endif()
    
    # Set standard compile features
    target_compile_features(${target} PUBLIC cxx_std_17)
    
    # Add debug postfix
    set_target_properties(${target} PROPERTIES DEBUG_POSTFIX "d")
endfunction()

# End of UtilityFunctions.cmake
```

### **Advanced CMake Functions with Complex Logic**

**cmake/AdvancedFunctions.cmake:**
```cmake
# AdvancedFunctions.cmake - Complex CMake functions and macros

# ==================== MACROS VS FUNCTIONS ====================

# Macro example - expands in place
macro(setup_target_common target)
    # This modifies the parent scope directly
    target_compile_features(${target} PUBLIC cxx_std_17)
    target_compile_options(${target} PRIVATE
        $<$<CXX_COMPILER_ID:GNU,Clang>:-Wall -Wextra>
        $<$<CXX_COMPILER_ID:MSVC>:/W4>
    )
    
    # Set output directories
    set_target_properties(${target} PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    )
endmacro()

# Function example - has its own scope
function(configure_target target)
    # Arguments are passed by value, need PARENT_SCOPE to modify
    message(STATUS "Configuring target: ${target}")
    
    # Get target properties
    get_target_property(target_type ${target} TYPE)
    get_target_property(target_sources ${target} SOURCES)
    
    message(STATUS "  Type: ${target_type}")
    message(STATUS "  Source count: $<LENGTH:${target_sources}>")
    
    # This only affects function scope
    set(local_var "function_scope")
    
    # To affect parent scope
    set(${target}_CONFIGURED TRUE PARENT_SCOPE)
endfunction()

# ==================== GENERATOR EXPRESSION FUNCTIONS ====================

# Create conditional compile definitions using generator expressions
function(add_conditional_definitions target)
    set(options DEBUG RELEASE RELWITHDEBINFO MINSIZEREL)
    cmake_parse_arguments(ARG "${options}" "" "" ${ARGN})
    
    set(definitions "")
    
    if(ARG_DEBUG)
        list(APPEND definitions "$<$<CONFIG:Debug>:DEBUG_BUILD=1>")
    endif()
    
    if(ARG_RELEASE)
        list(APPEND definitions "$<$<CONFIG:Release>:RELEASE_BUILD=1>")
    endif()
    
    if(ARG_RELWITHDEBINFO)
        list(APPEND definitions "$<$<CONFIG:RelWithDebInfo>:RELWITHDEBINFO_BUILD=1>")
    endif()
    
    if(ARG_MINSIZEREL)
        list(APPEND definitions "$<$<CONFIG:MinSizeRel>:MINSIZEREL_BUILD=1>")
    endif()
    
    if(definitions)
        target_compile_definitions(${target} PRIVATE ${definitions})
    endif()
endfunction()

# Add platform-specific compile options
function(add_platform_options target)
    set(platform_options "")
    
    # Linux-specific
    list(APPEND platform_options 
        "$<$<PLATFORM_ID:Linux>:-DPLATFORM_LINUX>"
        "$<$<PLATFORM_ID:Linux>:-pthread>"
    )
    
    # Windows-specific
    list(APPEND platform_options
        "$<$<PLATFORM_ID:Windows>:-DPLATFORM_WINDOWS>"
        "$<$<PLATFORM_ID:Windows>:-D_WIN32_WINNT=0x0A00>"
    )
    
    # macOS-specific
    list(APPEND platform_options
        "$<$<PLATFORM_ID:Darwin>:-DPLATFORM_MACOS>"
    )
    
    target_compile_options(${target} PRIVATE ${platform_options})
endfunction()

# ==================== DYNAMIC TARGET CREATION ====================

# Create multiple similar targets
function(create_component_targets prefix count)
    foreach(i RANGE 1 ${count})
        set(target_name "${prefix}_${i}")
        add_library(${target_name} STATIC)
        target_sources(${target_name} PRIVATE
            ${CMAKE_CURRENT_SOURCE_DIR}/src/component_${i}.cpp
        )
        
        # Configure each target
        configure_target(${target_name})
    endforeach()
endfunction()

# Create interface for plugin system
function(create_plugin_interface name)
    add_library(${name}_interface INTERFACE)
    
    target_compile_definitions(${name}_interface INTERFACE
        PLUGIN_INTERFACE=1
        PLUGIN_NAME="${name}"
    )
    
    target_include_directories(${name}_interface INTERFACE
        ${CMAKE_CURRENT_SOURCE_DIR}/include
    )
    
    # Create a macro that sets up plugin targets
    macro(setup_plugin_target target)
        add_library(${target} MODULE)
        target_link_libraries(${target} PRIVATE ${name}_interface)
        
        # Platform-specific naming
        if(WIN32)
            set_target_properties(${target} PROPERTIES
                PREFIX ""
                SUFFIX ".dll"
            )
        else()
            set_target_properties(${target} PROPERTIES
                PREFIX "lib"
                SUFFIX ".so"
            )
        endif()
    endmacro()
    
    # Export the macro to parent scope
    set(setup_plugin_target ${setup_plugin_target} PARENT_SCOPE)
endfunction()

# ==================== FILE GENERATION FUNCTIONS ====================

# Generate version header file
function(generate_version_header)
    set(options PUBLIC INTERNAL)
    set(one_value_args NAMESPACE FILENAME)
    set(multi_value_args)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    if(NOT ARG_FILENAME)
        set(ARG_FILENAME "version.h")
    endif()
    
    if(NOT ARG_NAMESPACE)
        set(ARG_NAMESPACE "project")
    endif()
    
    string(TOUPPER "${ARG_NAMESPACE}" NAMESPACE_UPPER)
    string(TOLOWER "${ARG_NAMESPACE}" NAMESPACE_LOWER)
    
    set(VERSION_CONTENT
        "// Generated by CMake - DO NOT EDIT\n"
        "#ifndef ${NAMESPACE_UPPER}_VERSION_H\n"
        "#define ${NAMESPACE_UPPER}_VERSION_H\n\n"
        "namespace ${NAMESPACE_LOWER} {\n\n"
        "constexpr int VERSION_MAJOR = ${PROJECT_VERSION_MAJOR};\n"
        "constexpr int VERSION_MINOR = ${PROJECT_VERSION_MINOR};\n"
        "constexpr int VERSION_PATCH = ${PROJECT_VERSION_PATCH};\n"
        "constexpr const char* VERSION_STRING = \"${PROJECT_VERSION}\";\n\n"
    )
    
    if(ARG_PUBLIC)
        string(APPEND VERSION_CONTENT
            "constexpr const char* PROJECT_NAME = \"${PROJECT_NAME}\";\n"
            "constexpr const char* PROJECT_DESCRIPTION = \"${PROJECT_DESCRIPTION}\";\n"
        )
    endif()
    
    string(APPEND VERSION_CONTENT
        "} // namespace ${NAMESPACE_LOWER}\n\n"
        "#endif // ${NAMESPACE_UPPER}_VERSION_H\n"
    )
    
    # Write to file
    file(WRITE "${CMAKE_BINARY_DIR}/generated/${ARG_FILENAME}" "${VERSION_CONTENT}")
    
    # Return the generated file path
    set(generated_file "${CMAKE_BINARY_DIR}/generated/${ARG_FILENAME}" PARENT_SCOPE)
endfunction()

# ==================== CONFIGURATION MANAGEMENT ====================

# Store and retrieve configuration in cache
function(set_config key value description)
    set(${key} "${value}" CACHE STRING "${description}" FORCE)
    set_property(CACHE ${key} PROPERTY ADVANCED 1)
endfunction()

function(get_config var key)
    if(DEFINED ${key})
        set(${var} "${${key}}" PARENT_SCOPE)
    else()
        set(${var} "" PARENT_SCOPE)
    endif()
endfunction()

# Save configuration to file
function(save_configuration filename)
    get_cmake_property(vars CACHE_VARIABLES)
    
    set(config_content "# CMake Configuration Save\n")
    set(config_content "${config_content}# Generated on: $ENV{USER}@$ENV{HOSTNAME}\n")
    set(config_content "${config_content}# Timestamp: ${CMAKE_DATE} ${CMAKE_TIME}\n\n")
    
    foreach(var IN LISTS vars)
        get_property(help CACHE ${var} PROPERTY HELPSTRING)
        get_property(type CACHE ${var} PROPERTY TYPE)
        get_property(value CACHE ${var} PROPERTY VALUE)
        
        string(APPEND config_content "# ${help}\n")
        string(APPEND config_content "set(${var} \"${value}\" CACHE ${type} \"\" FORCE)\n\n")
    endforeach()
    
    file(WRITE "${filename}" "${config_content}")
    message(STATUS "Configuration saved to: ${filename}")
endfunction()

# ==================== COMPLEX CONDITIONAL LOGIC ====================

# Check compiler features with fallbacks
function(check_compiler_feature var feature)
    set(CMAKE_REQUIRED_FLAGS)
    
    # Try C++17 feature
    check_cxx_compiler_flag("-std=c++17" HAS_CXX17)
    
    # Try specific feature
    include(CheckCXXSourceCompiles)
    set(CMAKE_REQUIRED_FLAGS "-std=c++17")
    
    if(feature STREQUAL "filesystem")
        set(test_code [[
            #include <filesystem>
            int main() {
                std::filesystem::path p = "test";
                return 0;
            }
        ]])
    elseif(feature STREQUAL "optional")
        set(test_code [[
            #include <optional>
            int main() {
                std::optional<int> opt = 42;
                return opt.has_value() ? 0 : 1;
            }
        ]])
    else()
        set(${var} FALSE PARENT_SCOPE)
        return()
    endif()
    
    check_cxx_source_compiles("${test_code}" HAS_FEATURE)
    set(${var} ${HAS_FEATURE} PARENT_SCOPE)
endfunction()

# Platform-aware feature detection
function(detect_platform_features)
    set(features "")
    
    # CPU features
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86|x86_64|AMD64")
        list(APPEND features "X86")
        
        # Check for SSE/AVX
        include(CheckCXXSourceCompiles)
        
        # SSE2 check
        set(CMAKE_REQUIRED_FLAGS "-msse2")
        check_cxx_source_compiles("
            #include <emmintrin.h>
            int main() {
                __m128i a = _mm_setzero_si128();
                return 0;
            }
        " HAS_SSE2)
        
        if(HAS_SSE2)
            list(APPEND features "SSE2")
        endif()
        
        # Reset flags
        set(CMAKE_REQUIRED_FLAGS)
    elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "arm|aarch64")
        list(APPEND features "ARM")
    endif()
    
    # OS features
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
        list(APPEND features "LINUX" "POSIX")
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
        list(APPEND features "WINDOWS")
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
        list(APPEND features "MACOS" "BSD")
    endif()
    
    set(PLATFORM_FEATURES "${features}" PARENT_SCOPE)
endfunction()

# ==================== TEMPLATE PROCESSING ====================

# Process template file with variables
function(process_template template output)
    # Read template
    file(READ "${template}" template_content)
    
    # Get all variables in current scope
    get_cmake_property(vars VARIABLES)
    
    # Replace variables in template
    foreach(var IN LISTS vars)
        if(var MATCHES "^[A-Za-z_][A-Za-z0-9_]*$" AND NOT var MATCHES "^CMAKE_" AND NOT var MATCHES "^_")
            string(REPLACE "@${var}@" "${${var}}" template_content "${template_content}")
        endif()
    endforeach()
    
    # Write output
    file(WRITE "${output}" "${template_content}")
    message(STATUS "Generated: ${output} from ${template}")
endfunction()

# ==================== ADVANCED ERROR HANDLING ====================

# Assert with custom message
macro(assert test message)
    if(NOT ${test})
        message(FATAL_ERROR "Assertion failed: ${message}")
    endif()
endmacro()

# Check required variables
function(check_required_variables)
    foreach(var IN LISTS ARGN)
        if(NOT DEFINED ${var})
            message(FATAL_ERROR "Required variable not set: ${var}")
        endif()
    endforeach()
endfunction()

# Validate target exists
function(validate_target target)
    if(NOT TARGET ${target})
        message(FATAL_ERROR "Target does not exist: ${target}")
    endif()
endfunction()

# ==================== PERFORMANCE UTILITIES ====================

# Measure execution time of CMake code
macro(time_execution description)
    string(TIMESTAMP start_time "%s.%f")
    # Execute the code block
    ${ARGN}
    string(TIMESTAMP end_time "%s.%f")
    
    math(EXPR elapsed_time "${end_time} - ${start_time}" OUTPUT_FORMAT "%.6f")
    message(STATUS "${description}: ${elapsed_time} seconds")
endmacro()

# Profile variable operations
function(profile_variable_operations count)
    message(STATUS "Profiling variable operations with ${count} iterations")
    
    # List operations
    string(TIMESTAMP list_start "%s.%f")
    set(test_list "")
    foreach(i RANGE 1 ${count})
        list(APPEND test_list "item_${i}")
    endforeach()
    string(TIMESTAMP list_end "%s.%f")
    math(EXPR list_time "${list_end} - ${list_start}" OUTPUT_FORMAT "%.6f")
    
    # String operations
    string(TIMESTAMP string_start "%s.%f")
    set(test_string "")
    foreach(i RANGE 1 ${count})
        string(APPEND test_string "item_${i};")
    endforeach()
    string(TIMESTAMP string_end "%s.%f")
    math(EXPR string_time "${string_end} - ${string_start}" OUTPUT_FORMAT "%.6f")
    
    message(STATUS "  List append: ${list_time}s")
    message(STATUS "  String append: ${string_time}s")
endfunction()

# End of AdvancedFunctions.cmake
```

### **Code Generation Module**

**cmake/CodeGeneration.cmake:**
```cmake
# CodeGeneration.cmake - Advanced code generation utilities

# ==================== SOURCE CODE GENERATION ====================

# Generate C++ class from template
function(generate_cpp_class class_name)
    set(options HEADER_ONLY)
    set(one_value_args NAMESPACE BASE_DIR)
    set(multi_value_args BASES METHODS)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    if(NOT ARG_BASE_DIR)
        set(ARG_BASE_DIR "${CMAKE_BINARY_DIR}/generated")
    endif()
    
    # Prepare class name variations
    string(TOUPPER "${class_name}" CLASS_NAME_UPPER)
    string(TOLOWER "${class_name}" CLASS_NAME_LOWER)
    
    # Generate header guard
    set(header_guard "${CLASS_NAME_UPPER}_H")
    
    # Generate header content
    set(header_content
        "// Generated by CMake - DO NOT EDIT\n"
        "#ifndef ${header_guard}\n"
        "#define ${header_guard}\n\n"
    )
    
    # Add namespace if specified
    if(ARG_NAMESPACE)
        string(APPEND header_content "namespace ${ARG_NAMESPACE} {\n\n")
    endif()
    
    # Class declaration
    string(APPEND header_content "class ${class_name}")
    
    # Add base classes
    if(ARG_BASES)
        string(APPEND header_content " : ")
        set(first TRUE)
        foreach(base IN LISTS ARG_BASES)
            if(NOT first)
                string(APPEND header_content ", ")
            endif()
            string(APPEND header_content "public ${base}")
            set(first FALSE)
        endforeach()
    endif()
    
    string(APPEND header_content "\n{\n")
    string(APPEND header_content "public:\n")
    
    # Constructor and destructor
    string(APPEND header_content "    ${class_name}();\n")
    string(APPEND header_content "    virtual ~${class_name}();\n\n")
    
    # Copy and move operations
    string(APPEND header_content "    ${class_name}(const ${class_name}&) = default;\n")
    string(APPEND header_content "    ${class_name}(${class_name}&&) = default;\n")
    string(APPEND header_content "    ${class_name}& operator=(const ${class_name}&) = default;\n")
    string(APPEND header_content "    ${class_name}& operator=(${class_name}&&) = default;\n\n")
    
    # Generated methods
    foreach(method IN LISTS ARG_METHODS)
        string(APPEND header_content "    virtual void ${method}();\n")
    endforeach()
    
    string(APPEND header_content "\nprivate:\n")
    string(APPEND header_content "    // Implementation details\n")
    string(APPEND header_content "};\n\n")
    
    # Close namespace
    if(ARG_NAMESPACE)
        string(APPEND header_content "} // namespace ${ARG_NAMESPACE}\n\n")
    endif()
    
    string(APPEND header_content "#endif // ${header_guard}\n")
    
    # Write header file
    set(header_file "${ARG_BASE_DIR}/${class_name}.h")
    file(WRITE "${header_file}" "${header_content}")
    
    # Generate source file if not header-only
    if(NOT ARG_HEADER_ONLY)
        set(source_content
            "// Generated by CMake - DO NOT EDIT\n"
            "#include \"${class_name}.h\"\n\n"
        )
        
        # Add namespace if specified
        if(ARG_NAMESPACE)
            string(APPEND source_content "namespace ${ARG_NAMESPACE} {\n\n")
        endif()
        
        # Constructor implementation
        string(APPEND source_content "${class_name}::${class_name}()\n")
        string(APPEND source_content "{\n")
        string(APPEND source_content "    // Constructor implementation\n")
        string(APPEND source_content "}\n\n")
        
        # Destructor implementation
        string(APPEND source_content "${class_name}::~${class_name}()\n")
        string(APPEND source_content "{\n")
        string(APPEND source_content "    // Destructor implementation\n")
        string(APPEND source_content "}\n\n")
        
        # Method implementations
        foreach(method IN LISTS ARG_METHODS)
            string(APPEND source_content "void ${class_name}::${method}()\n")
            string(APPEND source_content "{\n")
            string(APPEND source_content "    // ${method} implementation\n")
            string(APPEND source_content "}\n\n")
        endforeach()
        
        # Close namespace
        if(ARG_NAMESPACE)
            string(APPEND source_content "} // namespace ${ARG_NAMESPACE}\n")
        endif()
        
        # Write source file
        set(source_file "${ARG_BASE_DIR}/${class_name}.cpp")
        file(WRITE "${source_file}" "${source_content}")
        
        # Return both files
        set(generated_files "${header_file};${source_file}" PARENT_SCOPE)
    else()
        # Return only header file
        set(generated_files "${header_file}" PARENT_SCOPE)
    endif()
    
    message(STATUS "Generated C++ class: ${class_name}")
endfunction()

# Generate enumeration with string conversion
function(generate_enum enum_name)
    set(options FLAGS)
    set(one_value_args NAMESPACE BASE_DIR)
    set(multi_value_args VALUES)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    if(NOT ARG_BASE_DIR)
        set(ARG_BASE_DIR "${CMAKE_BINARY_DIR}/generated")
    endif()
    
    # Prepare names
    string(TOUPPER "${enum_name}" ENUM_NAME_UPPER)
    string(TOLOWER "${enum_name}" ENUM_NAME_LOWER)
    
    # Generate header content
    set(header_content
        "// Generated by CMake - DO NOT EDIT\n"
        "#ifndef ${ENUM_NAME_UPPER}_H\n"
        "#define ${ENUM_NAME_UPPER}_H\n\n"
        "#include <string>\n"
        "#include <vector>\n\n"
    )
    
    if(ARG_NAMESPACE)
        string(APPEND header_content "namespace ${ARG_NAMESPACE} {\n\n")
    endif()
    
    # Enum declaration
    if(ARG_FLAGS)
        string(APPEND header_content "enum ${enum_name} : unsigned int\n")
    else()
        string(APPEND header_content "enum class ${enum_name}\n")
    endif()
    
    string(APPEND header_content "{\n")
    
    # Enum values
    set(value_index 0)
    set(string_values "")
    foreach(value IN LISTS ARG_VALUES)
        if(ARG_FLAGS)
            string(APPEND header_content "    ${value} = 1 << ${value_index},\n")
        else()
            string(APPEND header_content "    ${value},\n")
        endif()
        
        list(APPEND string_values "\"${value}\"")
        math(EXPR value_index "${value_index} + 1")
    endforeach()
    
    string(APPEND header_content "};\n\n")
    
    # String conversion functions
    string(APPEND header_content "// String conversion functions\n")
    string(APPEND header_content "std::string to_string(${enum_name} value);\n")
    
    if(ARG_FLAGS)
        string(APPEND header_content "${enum_name} from_string(const std::string& str);\n")
        string(APPEND header_content "std::vector<${enum_name}> get_all_${ENUM_NAME_LOWER}_values();\n")
    else()
        string(APPEND header_content "bool from_string(const std::string& str, ${enum_name}& out_value);\n")
    endif()
    
    string(APPEND header_content "\n")
    
    # Close namespace
    if(ARG_NAMESPACE)
        string(APPEND header_content "} // namespace ${ARG_NAMESPACE}\n\n")
    endif()
    
    string(APPEND header_content "#endif // ${ENUM_NAME_UPPER}_H\n")
    
    # Write header file
    set(header_file "${ARG_BASE_DIR}/${enum_name}.h")
    file(WRITE "${header_file}" "${header_content}")
    
    # Generate source content
    set(source_content
        "// Generated by CMake - DO NOT EDIT\n"
        "#include \"${enum_name}.h\"\n\n"
        "#include <algorithm>\n"
        "#include <stdexcept>\n\n"
    )
    
    if(ARG_NAMESPACE)
        string(APPEND source_content "namespace ${ARG_NAMESPACE} {\n\n")
    endif()
    
    # String array for conversion
    string(JOIN ", " string_array "${string_values}")
    
    if(ARG_FLAGS)
        string(APPEND source_content "static const char* ${ENUM_NAME_LOWER}_strings[] = {\n    ${string_array}\n};\n\n")
        
        string(APPEND source_content "std::string to_string(${enum_name} value)\n{\n")
        string(APPEND source_content "    std::string result;\n")
        string(APPEND source_content "    for (int i = 0; i < ${value_index}; ++i) {\n")
        string(APPEND source_content "        if (value & (1 << i)) {\n")
        string(APPEND source_content "            if (!result.empty()) result += \"|\";\n")
        string(APPEND source_content "            result += ${ENUM_NAME_LOWER}_strings[i];\n")
        string(APPEND sourceContent "        }\n    }\n")
        string(APPEND source_content "    return result.empty() ? \"NONE\" : result;\n}\n\n")
        
        string(APPEND source_content "${enum_name} from_string(const std::string& str)\n{\n")
        string(APPEND source_content "    ${enum_name} result = static_cast<${enum_name}>(0);\n")
        string(APPEND source_content "    if (str.empty() || str == \"NONE\") return result;\n\n")
        string(APPEND source_content "    size_t start = 0;\n")
        string(APPEND source_content "    size_t end = str.find('|');\n")
        string(APPEND source_content "    while (true) {\n")
        string(APPEND source_content "        std::string part = str.substr(start, end - start);\n")
        string(APPEND source_content "        for (int i = 0; i < ${value_index}; ++i) {\n")
        string(APPEND source_content "            if (part == ${ENUM_NAME_LOWER}_strings[i]) {\n")
        string(APPEND source_content "                result = static_cast<${enum_name}>(result | (1 << i));\n")
        string(APPEND source_content "                break;\n")
        string(APPEND source_content "            }\n        }\n")
        string(APPEND source_content "        if (end == std::string::npos) break;\n")
        string(APPEND source_content "        start = end + 1;\n")
        string(APPEND source_content "        end = str.find('|', start);\n")
        string(APPEND source_content "    }\n")
        string(APPEND source_content "    return result;\n}\n\n")
        
        string(APPEND source_content "std::vector<${enum_name}> get_all_${ENUM_NAME_LOWER}_values()\n{\n")
        string(APPEND source_content "    std::vector<${enum_name}> result;\n")
        string(APPEND source_content "    for (int i = 0; i < ${value_index}; ++i) {\n")
        string(APPEND source_content "        result.push_back(static_cast<${enum_name}>(1 << i));\n")
        string(APPEND source_content "    }\n")
        string(APPEND source_content "    return result;\n}\n")
        
    else()
        string(APPEND source_content "static const char* ${ENUM_NAME_LOWER}_strings[] = {\n    ${string_array}\n};\n\n")
        
        string(APPEND source_content "std::string to_string(${enum_name} value)\n{\n")
        string(APPEND source_content "    int index = static_cast<int>(value);\n")
        string(APPEND source_content "    if (index < 0 || index >= ${value_index}) {\n")
        string(APPEND source_content "        return \"INVALID\";\n    }\n")
        string(APPEND source_content "    return ${ENUM_NAME_LOWER}_strings[index];\n}\n\n")
        
        string(APPEND source_content "bool from_string(const std::string& str, ${enum_name}& out_value)\n{\n")
        string(APPEND source_content "    for (int i = 0; i < ${value_index}; ++i) {\n")
        string(APPEND source_content "        if (str == ${ENUM_NAME_LOWER}_strings[i]) {\n")
        string(APPEND source_content "            out_value = static_cast<${enum_name}>(i);\n")
        string(APPEND source_content "            return true;\n        }\n    }\n")
        string(APPEND source_content "    return false;\n}\n")
    endif()
    
    # Close namespace
    if(ARG_NAMESPACE)
        string(APPEND source_content "\n} // namespace ${ARG_NAMESPACE}\n")
    endif()
    
    # Write source file
    set(source_file "${ARG_BASE_DIR}/${enum_name}.cpp")
    file(WRITE "${source_file}" "${source_content}")
    
    set(generated_files "${header_file};${source_file}" PARENT_SCOPE)
    message(STATUS "Generated enum: ${enum_name} with ${value_index} values")
endfunction()

# ==================== CONFIGURATION FILE GENERATION ====================

# Generate configuration header from CMake variables
function(generate_config_header)
    set(options)
    set(one_value_args FILENAME NAMESPACE)
    set(multi_value_args VARIABLES)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    if(NOT ARG_FILENAME)
        set(ARG_FILENAME "config.h")
    endif()
    
    if(NOT ARG_NAMESPACE)
        set(ARG_NAMESPACE "config")
    endif()
    
    set(header_content
        "// Generated by CMake - DO NOT EDIT\n"
        "#ifndef CONFIG_H\n"
        "#define CONFIG_H\n\n"
        "#include <string>\n\n"
    )
    
    if(ARG_NAMESPACE)
        string(APPEND header_content "namespace ${ARG_NAMESPACE} {\n\n")
    endif()
    
    # Add build information
    string(APPEND header_content "// Build information\n")
    string(APPEND header_content "constexpr const char* BUILD_TYPE = \"${CMAKE_BUILD_TYPE}\";\n")
    string(APPEND header_content "constexpr const char* COMPILER = \"${CMAKE_CXX_COMPILER_ID}\";\n")
    string(APPEND header_content "constexpr int COMPILER_VERSION = ${CMAKE_CXX_COMPILER_VERSION};\n")
    string(APPEND header_content "constexpr const char* SYSTEM = \"${CMAKE_SYSTEM_NAME}\";\n")
    string(APPEND headerContent "constexpr const char* PROCESSOR = \"${CMAKE_SYSTEM_PROCESSOR}\";\n\n")
    
    # Add project variables
    if(ARG_VARIABLES)
        string(APPEND header_content "// Project configuration\n")
        foreach(var IN LISTS ARG_VARIABLES)
            if(DEFINED ${var})
                if(${var} MATCHES "^[0-9]+$")
                    # Integer
                    string(APPEND header_content "constexpr int ${var} = ${${var}};\n")
                elseif(${var} MATCHES "^(true|false|TRUE|FALSE|ON|OFF|YES|NO)$")
                    # Boolean
                    if(${var})
                        string(APPEND header_content "constexpr bool ${var} = true;\n")
                    else()
                        string(APPEND header_content "constexpr bool ${var} = false;\n")
                    endif()
                else()
                    # String
                    string(APPEND header_content "constexpr const char* ${var} = \"${${var}}\";\n")
                endif()
            endif()
        endforeach()
    endif()
    
    # Add helper functions
    string(APPEND header_content "\n// Helper functions\n")
    string(APPEND header_content "inline std::string get_build_info() {\n")
    string(APPEND header_content "    return std::string(\"Build: \") + BUILD_TYPE + \", Compiler: \" + COMPILER;\n")
    string(APPEND header_content "}\n")
    
    # Close namespace
    if(ARG_NAMESPACE)
        string(APPEND header_content "\n} // namespace ${ARG_NAMESPACE}\n\n")
    endif()
    
    string(APPEND header_content "#endif // CONFIG_H\n")
    
    # Write file
    file(WRITE "${CMAKE_BINARY_DIR}/generated/${ARG_FILENAME}" "${header_content}")
    message(STATUS "Generated config header: ${ARG_FILENAME}")
endfunction()

# ==================== DYNAMIC BUILD CONFIGURATION ====================

# Generate build configuration based on features
function(generate_feature_config)
    set(feature_definitions "")
    
    # Check compiler features
    check_compiler_feature(HAS_FILESYSTEM "filesystem")
    if(HAS_FILESYSTEM)
        list(APPEND feature_definitions "HAS_STD_FILESYSTEM")
    endif()
    
    check_compiler_feature(HAS_OPTIONAL "optional")
    if(HAS_OPTIONAL)
        list(APPEND feature_definitions "HAS_STD_OPTIONAL")
    endif()
    
    # Platform features
    detect_platform_features()
    list(APPEND feature_definitions ${PLATFORM_FEATURES})
    
    # Write feature configuration
    set(feature_content
        "// Feature configuration\n"
        "#pragma once\n\n"
    )
    
    foreach(def IN LISTS feature_definitions)
        string(APPEND feature_content "#define ${def} 1\n")
    endforeach()
    
    file(WRITE "${CMAKE_BINARY_DIR}/generated/features.h" "${feature_content}")
    message(STATUS "Generated feature configuration")
endfunction()

# End of CodeGeneration.cmake
```

### **Platform Checks Module**

**cmake/PlatformChecks.cmake:**
```cmake
# PlatformChecks.cmake - Comprehensive platform detection and checks

# ==================== PLATFORM DETECTION ====================

# Detect operating system in detail
function(detect_operating_system)
    set(os_info "")
    
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
        list(APPEND os_info "Linux")
        
        # Try to detect distribution
        if(EXISTS "/etc/os-release")
            file(STRINGS "/etc/os-release" os_release)
            foreach(line IN LISTS os_release)
                if(line MATCHES "^NAME=\"(.*)\"$")
                    set(distro_name "${CMAKE_MATCH_1}")
                    list(APPEND os_info "${distro_name}")
                elseif(line MATCHES "^VERSION_ID=\"(.*)\"$")
                    set(distro_version "${CMAKE_MATCH_1}")
                    list(APPEND os_info "v${distro_version}")
                endif()
            endforeach()
        elseif(EXISTS "/etc/redhat-release")
            list(APPEND os_info "RedHat")
        elseif(EXISTS "/etc/debian_version")
            list(APPEND os_info "Debian")
        endif()
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
        list(APPEND os_info "Windows")
        
        # Detect Windows version
        if(CMAKE_SYSTEM_VERSION MATCHES "10")
            list(APPEND os_info "10")
        elseif(CMAKE_SYSTEM_VERSION MATCHES "6.3")
            list(APPEND os_info "8.1")
        elseif(CMAKE_SYSTEM_VERSION MATCHES "6.2")
            list(APPEND os_info "8")
        elseif(CMAKE_SYSTEM_VERSION MATCHES "6.1")
            list(APPEND os_info "7")
        endif()
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
        list(APPEND os_info "macOS")
        
        # Get macOS version
        execute_process(
            COMMAND sw_vers -productVersion
            OUTPUT_VARIABLE macos_version
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        list(APPEND os_info "${macos_version}")
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Android")
        list(APPEND os_info "Android")
    else()
        list(APPEND os_info "${CMAKE_SYSTEM_NAME}")
    endif()
    
    set(OS_DETAILED_INFO "${os_info}" PARENT_SCOPE)
endfunction()

# Detect architecture features
function(detect_architecture_features)
    set(arch_features "")
    
    # CPU architecture
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64|AMD64")
        list(APPEND arch_features "X86_64")
        list(APPEND arch_features "64BIT")
    elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "i[3456]86")
        list(APPEND arch_features "X86")
        list(APPEND arch_features "32BIT")
    elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "arm|ARM")
        list(APPEND arch_features "ARM")
        if(CMAKE_SYSTEM_PROCESSOR MATCHES "armv7")
            list(APPEND arch_features "ARMv7")
        elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64|ARM64")
            list(APPEND arch_features "ARM64")
            list(APPEND arch_features "64BIT")
        endif()
    elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "ppc|powerpc")
        list(APPEND arch_features "POWERPC")
    endif()
    
    # Endianness check
    include(TestBigEndian)
    test_big_endian(IS_BIG_ENDIAN)
    if(IS_BIG_ENDIAN)
        list(APPEND arch_features "BIG_ENDIAN")
    else()
        list(APPEND arch_features "LITTLE_ENDIAN")
    endif()
    
    set(ARCH_FEATURES "${arch_features}" PARENT_SCOPE)
endfunction()

# ==================== COMPILER FEATURE DETECTION ====================

# Comprehensive compiler detection
function(detect_compiler_features)
    set(compiler_info "")
    
    # Compiler family
    if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
        list(APPEND compiler_info "GCC")
        list(APPEND compiler_info "Version: ${CMAKE_CXX_COMPILER_VERSION}")
        
        # GCC-specific features
        if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL "7.0")
            list(APPEND compiler_info "C++17_FULL")
        endif()
        
    elseif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
        list(APPEND compiler_info "Clang")
        list(APPEND compiler_info "Version: ${CMAKE_CXX_COMPILER_VERSION}")
        
        # Apple Clang vs regular Clang
        if(CMAKE_CXX_COMPILER_ID STREQUAL "AppleClang")
            list(APPEND compiler_info "Apple")
        endif()
        
    elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
        list(APPEND compiler_info "MSVC")
        list(APPEND compiler_info "Version: ${CMAKE_CXX_COMPILER_VERSION}")
        
        # MSVC version mapping
        if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL "19.28")
            list(APPEND compiler_info "VS2019")
        elseif(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL "19.0")
            list(APPEND compiler_info "VS2017")
        endif()
    endif()
    
    # Check C++ standard support
    set(CMAKE_REQUIRED_FLAGS "-std=c++17")
    include(CheckCXXSourceCompiles)
    
    # Check various C++17 features
    check_cxx_source_compiles("
        #include <optional>
        #include <string_view>
        int main() {
            std::optional<int> opt = 42;
            std::string_view sv = \"test\";
            return 0;
        }
    " HAS_CXX17_FEATURES)
    
    if(HAS_CXX17_FEATURES)
        list(APPEND compiler_info "C++17")
    endif()
    
    # Check threading support
    check_cxx_source_compiles("
        #include <thread>
        #include <mutex>
        int main() {
            std::thread t([](){});
            t.join();
            return 0;
        }
    " HAS_THREADS)
    
    if(HAS_THREADS)
        list(APPEND compiler_info "THREADS")
    endif()
    
    # Check filesystem support
    check_cxx_source_compiles("
        #include <filesystem>
        int main() {
            std::filesystem::path p = \".\";
            return 0;
        }
    " HAS_FILESYSTEM)
    
    if(HAS_FILESYSTEM)
        list(APPEND compiler_info "FILESYSTEM")
    endif()
    
    set(COMPILER_FEATURES "${compiler_info}" PARENT_SCOPE)
endfunction()

# ==================== SYSTEM CAPABILITIES ====================

# Check system memory
function(check_system_memory)
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
        # Linux memory check
        file(STRINGS "/proc/meminfo" meminfo)
        foreach(line IN LISTS meminfo)
            if(line MATCHES "^MemTotal:\\s+([0-9]+)")
                math(EXPR total_mem_kb "${CMAKE_MATCH_1}")
                math(EXPR total_mem_mb "${total_mem_kb} / 1024")
                set(TOTAL_MEMORY_MB ${total_mem_mb} PARENT_SCOPE)
                break()
            endif()
        endforeach()
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
        # macOS memory check
        execute_process(
            COMMAND sysctl hw.memsize
            OUTPUT_VARIABLE memsize_output
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        if(memsize_output MATCHES "hw.memsize: ([0-9]+)")
            math(EXPR total_mem_bytes "${CMAKE_MATCH_1}")
            math(EXPR total_mem_mb "${total_mem_bytes} / 1048576")
            set(TOTAL_MEMORY_MB ${total_mem_mb} PARENT_SCOPE)
        endif()
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
        # Windows memory check (approximate)
        set(TOTAL_MEMORY_MB 4096 PARENT_SCOPE) # Default assumption
    endif()
endfunction()

# Check CPU cores
function(check_cpu_cores)
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux" OR CMAKE_SYSTEM_NAME STREQUAL "Darwin")
        # Linux and macOS
        execute_process(
            COMMAND getconf _NPROCESSORS_ONLN
            OUTPUT_VARIABLE cpu_count
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        set(CPU_CORES ${cpu_count} PARENT_SCOPE)
        
    elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
        # Windows
        set(CPU_CORES $ENV{NUMBER_OF_PROCESSORS} PARENT_SCOPE)
    endif()
endfunction()

# ==================== LIBRARY AVAILABILITY CHECKS ====================

# Check for library with multiple names and versions
function(check_library_availability library_name)
    set(options REQUIRED)
    set(one_value_args VERSION)
    set(multi_value_args NAMES)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    if(NOT ARG_NAMES)
        set(ARG_NAMES "${library_name}")
    endif()
    
    set(found FALSE)
    set(found_version "")
    set(found_library "")
    
    # Try each library name
    foreach(lib_name IN LISTS ARG_NAMES)
        find_library(${library_name}_LIBRARY
            NAMES ${lib_name}
            PATHS /usr/lib /usr/local/lib /opt/local/lib /sw/lib
        )
        
        if(${library_name}_LIBRARY)
            # Found library, check version if requested
            if(ARG_VERSION)
                # Try to extract version (simplified)
                get_filename_component(lib_path ${${library_name}_LIBRARY} DIRECTORY)
                find_file(${library_name}_VERSION_FILE
                    NAMES "${lib_name}.pc" "version.txt"
                    PATHS ${lib_path}/../include ${lib_path}/..
                )
                
                if(${library_name}_VERSION_FILE)
                    file(READ ${${library_name}_VERSION_FILE} version_content)
                    # Simple version extraction - would need to be customized per library
                    if(version_content MATCHES "Version: ([0-9.]+)")
                        set(found_version "${CMAKE_MATCH_1}")
                    endif()
                endif()
                
                if(found_version AND found_version VERSION_GREATER_EQUAL ARG_VERSION)
                    set(found TRUE)
                    set(found_library "${${library_name}_LIBRARY}")
                    break()
                endif()
            else()
                set(found TRUE)
                set(found_library "${${library_name}_LIBRARY}")
                break()
            endif()
        endif()
    endforeach()
    
    if(found)
        set(${library_name}_FOUND TRUE PARENT_SCOPE)
        set(${library_name}_LIBRARY ${found_library} PARENT_SCOPE)
        if(found_version)
            set(${library_name}_VERSION ${found_version} PARENT_SCOPE)
        endif()
        
        message(STATUS "Found ${library_name}: ${found_library}")
        if(found_version)
            message(STATUS "  Version: ${found_version}")
        endif()
    else()
        if(ARG_REQUIRED)
            message(FATAL_ERROR "Could not find required library: ${library_name}")
        else()
            message(STATUS "Library not found: ${library_name}")
            set(${library_name}_FOUND FALSE PARENT_SCOPE)
        endif()
    endif()
endfunction()

# ==================== PLATFORM-SPECIFIC CHECKS ====================

# Check Windows SDK
function(check_windows_sdk)
    if(WIN32)
        # Look for Windows SDK
        if(CMAKE_VS_WINDOWS_TARGET_PLATFORM_VERSION)
            set(WINDOWS_SDK_VERSION ${CMAKE_VS_WINDOWS_TARGET_PLATFORM_VERSION} PARENT_SCOPE)
            message(STATUS "Windows SDK: ${WINDOWS_SDK_VERSION}")
        else()
            # Try to detect from registry
            execute_process(
                COMMAND reg query "HKLM\\SOFTWARE\\Microsoft\\Windows Kits\\Installed Roots"
                OUTPUT_VARIABLE sdk_reg_output
                ERROR_QUIET
            )
            
            if(sdk_reg_output MATCHES "KitsRoot10")
                set(WINDOWS_SDK_VERSION "10" PARENT_SCOPE)
                message(STATUS "Windows SDK: 10")
            endif()
        endif()
    endif()
endfunction()

# Check macOS deployment target
function(check_macos_deployment)
    if(APPLE)
        if(CMAKE_OSX_DEPLOYMENT_TARGET)
            set(MACOS_DEPLOYMENT_TARGET ${CMAKE_OSX_DEPLOYMENT_TARGET} PARENT_SCOPE)
            message(STATUS "macOS Deployment Target: ${CMAKE_OSX_DEPLOYMENT_TARGET}")
        else()
            # Get current macOS version
            execute_process(
                COMMAND sw_vers -productVersion
                OUTPUT_VARIABLE current_version
                OUTPUT_STRIP_TRAILING_WHITESPACE
            )
            set(MACOS_DEPLOYMENT_TARGET ${current_version} PARENT_SCOPE)
            message(STATUS "Current macOS: ${current_version}")
        endif()
    endif()
endfunction()

# Check Linux distribution
function(check_linux_distribution)
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
        set(distro_info "")
        
        if(EXISTS "/etc/os-release")
            file(STRINGS "/etc/os-release" os_release)
            foreach(line IN LISTS os_release)
                if(line MATCHES "^ID=\"?(.*)\"?$")
                    list(APPEND distro_info "ID=${CMAKE_MATCH_1}")
                elseif(line MATCHES "^VERSION_ID=\"?(.*)\"?$")
                    list(APPEND distro_info "VERSION=${CMAKE_MATCH_1}")
                elseif(line MATCHES "^PRETTY_NAME=\"?(.*)\"?$")
                    list(APPEND distro_info "NAME=${CMAKE_MATCH_1}")
                endif()
            endforeach()
        endif()
        
        set(LINUX_DISTRO_INFO "${distro_info}" PARENT_SCOPE)
    endif()
endfunction()

# ==================== COMPREHENSIVE SYSTEM REPORT ====================

# Generate complete system report
function(generate_system_report)
    message(STATUS "")
    message(STATUS "=== SYSTEM REPORT ===")
    
    # Operating System
    detect_operating_system()
    string(REPLACE ";" " " os_display "${OS_DETAILED_INFO}")
    message(STATUS "OS: ${os_display}")
    
    # Architecture
    detect_architecture_features()
    string(REPLACE ";" " " arch_display "${ARCH_FEATURES}")
    message(STATUS "Architecture: ${arch_display}")
    
    # Compiler
    detect_compiler_features()
    string(REPLACE ";" " " compiler_display "${COMPILER_FEATURES}")
    message(STATUS "Compiler: ${compiler_display}")
    
    # System capabilities
    check_system_memory()
    check_cpu_cores()
    message(STATUS "Memory: ${TOTAL_MEMORY_MB} MB")
    message(STATUS "CPU Cores: ${CPU_CORES}")
    
    # Platform-specific info
    check_windows_sdk()
    check_macos_deployment()
    check_linux_distribution()
    
    if(WINDOWS_SDK_VERSION)
        message(STATUS "Windows SDK: ${WINDOWS_SDK_VERSION}")
    endif()
    
    if(MACOS_DEPLOYMENT_TARGET)
        message(STATUS "macOS Deployment Target: ${MACOS_DEPLOYMENT_TARGET}")
    endif()
    
    if(LINUX_DISTRO_INFO)
        string(REPLACE ";" ", " linux_display "${LINUX_DISTRO_INFO}")
        message(STATUS "Linux: ${linux_display}")
    endif()
    
    message(STATUS "==========================")
    message(STATUS "")
endfunction()

# End of PlatformChecks.cmake
```

### **Using Advanced Scripting in Practice**

**scripts/custom_build_steps.cmake:**
```cmake
# Custom build steps demonstrating advanced CMake scripting

# Custom target that generates code before build
add_custom_target(generate_sources ALL
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_SOURCE_DIR}/scripts/generate_code.cmake
    COMMENT "Generating source code..."
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

# Custom command to process files
add_custom_command(
    OUTPUT ${CMAKE_BINARY_DIR}/processed/data.bin
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_SOURCE_DIR}/scripts/process_data.cmake
    DEPENDS ${CMAKE_SOURCE_DIR}/data/raw.dat
    COMMENT "Processing data files..."
)

# Complex custom target with multiple steps
add_custom_target(build_all
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target generate_sources
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --config Release
    COMMAND ${CMAKE_COMMAND} -E echo "Build completed successfully"
    COMMENT "Full build process"
)
```

### **Example Usage in Source Directory**

**src/CMakeLists.txt:**
```cmake
# Demonstrate advanced scripting in practice

# Generate version header
generate_version_header(PUBLIC NAMESPACE demo)

# Generate configuration header
generate_config_header(
    NAMESPACE config
    VARIABLES
        PROJECT_NAME
        PROJECT_VERSION
        CMAKE_BUILD_TYPE
        CMAKE_CXX_COMPILER_ID
)

# Generate a C++ class
generate_cpp_class(
    GeneratedClass
    NAMESPACE generated
    METHODS
        initialize
        process
        cleanup
)

# Generate an enum
generate_enum(
    ErrorCode
    NAMESPACE generated
    VALUES
        SUCCESS
        FAILURE
        TIMEOUT
        INVALID_INPUT
)

# Create a target using our generated files
add_library(generated_code STATIC
    ${CMAKE_BINARY_DIR}/generated/GeneratedClass.cpp
    ${CMAKE_BINARY_DIR}/generated/ErrorCode.cpp
)

target_include_directories(generated_code PUBLIC
    ${CMAKE_BINARY_DIR}/generated
)

# Add conditional definitions based on platform
add_conditional_definitions(generated_code
    DEBUG
    RELEASE
    RELWITHDEBINFO
)

# Add platform-specific options
add_platform_options(generated_code)

# Setup common target properties
setup_target_common(generated_code)

# Configure the target
configure_target(generated_code)

# Create multiple component targets
create_component_targets(component 5)

# Generate system report
generate_system_report()

# Create a test executable using generated code
add_executable(test_generated
    test_main.cpp
)

target_link_libraries(test_generated PRIVATE generated_code)

# Add generated headers to IDE
source_group("Generated" FILES
    ${CMAKE_BINARY_DIR}/generated/version.h
    ${CMAKE_BINARY_DIR}/generated/config.h
    ${CMAKE_BINARY_DIR}/generated/GeneratedClass.h
    ${CMAKE_BINARY_DIR}/generated/ErrorCode.h
)
```

### **Best Practices for Advanced CMake Scripting**

1. **Modular Design**: Split complex scripts into reusable modules
2. **Function Over Macros**: Prefer functions for better scoping
3. **Parameter Validation**: Always validate function arguments
4. **Error Handling**: Provide clear error messages
5. **Documentation**: Document complex functions thoroughly
6. **Testing**: Test CMake modules independently
7. **Performance**: Avoid expensive operations in scripts
8. **Compatibility**: Support older CMake versions when possible
9. **Consistency**: Follow consistent naming conventions
10. **Debugging**: Add debugging capabilities to complex scripts

### **Common Advanced Patterns**

**Pattern 1: Factory Pattern for Target Creation**
```cmake
# Target factory that creates standardized targets
function(create_target_factory type name)
    if(type STREQUAL "EXECUTABLE")
        add_executable(${name})
    elseif(type STREQUAL "STATIC_LIB")
        add_library(${name} STATIC)
    elseif(type STREQUAL "SHARED_LIB")
        add_library(${name} SHARED)
    endif()
    
    # Apply standard configuration
    set_target_properties(${name} PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
    )
endfunction()
```

**Pattern 2: Dependency Injection for Build Configuration**
```cmake
# Configurable build strategy
function(configure_build_strategy target strategy)
    if(strategy STREQUAL "PERFORMANCE")
        target_compile_options(${target} PRIVATE -O3 -march=native)
    elseif(strategy STREQUAL "DEBUG")
        target_compile_options(${target} PRIVATE -O0 -g3)
    elseif(strategy STREQUAL "SIZE")
        target_compile_options(${target} PRIVATE -Os)
    endif()
endfunction()
```

**Pattern 3: Plugin System with Registration**
```cmake
# Plugin registration system
set(PLUGIN_REGISTRY "" CACHE INTERNAL "Plugin registry")

function(register_plugin plugin_name)
    list(APPEND PLUGIN_REGISTRY ${plugin_name})
    set(PLUGIN_REGISTRY ${PLUGIN_REGISTRY} CACHE INTERNAL "Plugin registry")
    
    # Create plugin target
    add_library(${plugin_name} MODULE)
endfunction()

function(get_registered_plugins var)
    set(${var} ${PLUGIN_REGISTRY} PARENT_SCOPE)
endfunction()
```

### **Exercise: Create a Comprehensive CMake Module System**

Create a project that demonstrates:

1. **Custom Functions**: String, list, file, and mathematical operations
2. **Code Generation**: Dynamic generation of C++ classes and enums
3. **Platform Detection**: Comprehensive system feature detection
4. **Template Processing**: Advanced template file generation
5. **Configuration Management**: Save/load build configurations
6. **Factory Patterns**: Dynamic target creation
7. **Plugin Systems**: Extensible architecture
8. **Performance Profiling**: Measure CMake script performance
9. **Error Handling**: Comprehensive error reporting
10. **Documentation Generation**: Auto-document CMake modules

### **Next Steps Preview**
In **PART-11**, we'll cover:
- CMake with IDEs and development tools
- Integration with Visual Studio, CLion, VS Code
- Debugging CMake projects
- Profiling build performance
- Build caching (ccache, sccache)
- Distributed builds
- IDE-specific configurations and workarounds

**To continue, please reply with "Next" or "PART-11" to receive the next part of the course.**




# **PART-11: IDE Integration, Debugging, and Build Optimization**

### **Topic: Working with Development Tools, Performance Tuning, and Debug Techniques**

### **Introduction to IDE Integration**

Modern IDEs provide excellent CMake support, but each has its quirks. Understanding how to configure CMake for different IDEs improves developer productivity and debugging capabilities.

### **Project Structure for This Part**

```
ide_integration/
├── CMakeLists.txt
├── .vscode/
│   ├── settings.json
│   ├── tasks.json
│   └── launch.json
├── .clion/
│   └── CMakeSettings.txt
├── .vs/
│   └── ProjectSettings.json
├── src/
├── tests/
└── cmake/
    └── IDEConfig.cmake
```

### **Root CMakeLists.txt with IDE Support**

```cmake
cmake_minimum_required(VERSION 3.20)
project(IDEDemo VERSION 1.0.0
    DESCRIPTION "Demonstration of IDE integration and build optimization"
    LANGUAGES CXX C
)

# Set policies for better IDE support
cmake_policy(SET CMP0076 NEW)  # target_sources() with relative paths
cmake_policy(SET CMP0115 NEW)  # source file extensions
cmake_policy(SET CMP0135 NEW)  # CMAKE_MSVC_DEBUG_INFORMATION_FORMAT

# Detect IDE environment
if(DEFINED ENV{VISUALSTUDIOVERSION})
    set(IDE_VISUAL_STUDIO TRUE)
    message(STATUS "Building in Visual Studio")
elseif(DEFINED ENV{JETBRAINS_IDE})
    set(IDE_CLION TRUE)
    message(STATUS "Building in CLion")
elseif(DEFINED ENV{VSCODE_PID})
    set(IDE_VSCODE TRUE)
    message(STATUS "Building in VS Code")
elseif(CMAKE_GENERATOR MATCHES "Xcode")
    set(IDE_XCODE TRUE)
    message(STATUS "Building in Xcode")
elseif(CMAKE_GENERATOR MATCHES "Ninja")
    set(IDE_NINJA TRUE)
    message(STATUS "Using Ninja generator")
endif()

# IDE-specific configurations
include(cmake/IDEConfig.cmake)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Build configurations
set(CMAKE_CONFIGURATION_TYPES "Debug;Release;RelWithDebInfo;MinSizeRel" CACHE STRING "" FORCE)

# Source groups for IDE organization
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# Add subdirectories
add_subdirectory(src)
add_subdirectory(tests)

# Generate compile_commands.json for clang-tidy, clangd, etc.
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Generate IDE project files helper target
add_custom_target(generate_ide_files
    COMMAND ${CMAKE_COMMAND} -E echo "IDE files are ready"
    COMMENT "IDE project files generated"
)
```

### **IDE Configuration Module**

**cmake/IDEConfig.cmake:**
```cmake
# IDEConfig.cmake - IDE-specific configurations and workarounds

# ==================== VISUAL STUDIO CONFIGURATION ====================

if(IDE_VISUAL_STUDIO OR CMAKE_GENERATOR MATCHES "Visual Studio")
    message(STATUS "Configuring for Visual Studio")
    
    # Visual Studio version detection
    if(CMAKE_VS_PLATFORM_TOOLSET)
        message(STATUS "  Toolset: ${CMAKE_VS_PLATFORM_TOOLSET}")
    endif()
    
    if(CMAKE_VS_WINDOWS_TARGET_PLATFORM_VERSION)
        message(STATUS "  Windows SDK: ${CMAKE_VS_WINDOWS_TARGET_PLATFORM_VERSION}")
    endif()
    
    # Set debug information format
    if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.25)
        set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>")
    endif()
    
    # Visual Studio folder organization
    macro(vs_set_source_group sources group)
        source_group(${group} FILES ${sources})
    endmacro()
    
    # Precompiled headers setup for VS
    function(vs_setup_precompiled_header target header source)
        if(MSVC)
            set_target_properties(${target} PROPERTIES
                COMPILE_FLAGS "/Yu\"${header}\""
            )
            set_source_files_properties(${source} PROPERTIES
                COMPILE_FLAGS "/Yc\"${header}\""
            )
        endif()
    endfunction()
    
    # Solution folder organization
    set_property(GLOBAL PROPERTY USE_FOLDERS ON)
    
    # Set Visual Studio startup project
    if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.27)
        set_property(DIRECTORY ${CMAKE_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT "app_main")
    endif()
    
    # Enable AddressSanitizer for Visual Studio 2019+
    option(ENABLE_VS_ASAN "Enable AddressSanitizer in Visual Studio" OFF)
    if(ENABLE_VS_ASAN AND MSVC_VERSION GREATER_EQUAL 1920)
        add_compile_options(/fsanitize=address)
        add_link_options(/fsanitize=address)
    endif()
    
    # Visual Studio specific warnings
    if(MSVC)
        add_compile_options(
            /W4          # Warning level 4
            /w14242      # Conversion warning
            /w14287      # Unsigned/negative mismatch
            /w14296      # Expression always bool
            /permissive- # Standards conformance
        )
        
        if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.24)
            # Treat warnings as errors in CI
            if(DEFINED ENV{CI})
                add_compile_options(/WX)
            endif()
        endif()
    endif()
endif()

# ==================== CLION CONFIGURATION ====================

if(IDE_CLION OR CMAKE_GENERATOR MATCHES "Ninja")
    message(STATUS "Configuring for CLion/Ninja")
    
    # CLion works best with Ninja generator
    if(NOT CMAKE_GENERATOR MATCHES "Ninja")
        message(STATUS "  Consider using -G Ninja for better CLion integration")
    endif()
    
    # Enable Clang-Tidy if available
    find_program(CLANG_TIDY_EXE NAMES clang-tidy clang-tidy-14 clang-tidy-13)
    if(CLANG_TIDY_EXE)
        set(CMAKE_CXX_CLANG_TIDY ${CLANG_TIDY_EXE})
        message(STATUS "  Clang-Tidy enabled: ${CLANG_TIDY_EXE}")
    endif()
    
    # Enable include-what-you-use if available
    find_program(IWYU_EXE NAMES include-what-you-use iwyu)
    if(IWYU_EXE)
        set(CMAKE_CXX_INCLUDE_WHAT_YOU_USE ${IWYU_EXE})
        message(STATUS "  Include-What-You-Use enabled")
    endif()
    
    # CLion debug configuration helper
    function(clion_setup_debug target executable_args)
        # Create a debug configuration file for CLion
        set(debug_config "${CMAKE_BINARY_DIR}/clion_debug_${target}.json")
        string(JSON debug_json
            SET "{}"
            name "\"Debug ${target}\""
            type "\"cppdbg\""
            request "\"launch\""
            program "\"$<TARGET_FILE:${target}>\""
            args "${executable_args}"
            stopAtEntry false
            cwd "\"${CMAKE_BINARY_DIR}\""
            environment "[]"
            externalConsole true
            MIMode "\"gdb\""
            setupCommands "[{\"description\":\"Enable pretty-printing for gdb\",\"text\":\"-enable-pretty-printing\",\"ignoreFailures\":true}]"
        )
        
        file(WRITE ${debug_config} ${debug_json})
        message(STATUS "  Created CLion debug config: ${debug_config}")
    endfunction()
endif()

# ==================== VS CODE CONFIGURATION ====================

if(IDE_VSCODE)
    message(STATUS "Configuring for VS Code")
    
    # Create .vscode directory if it doesn't exist
    if(NOT EXISTS "${CMAKE_SOURCE_DIR}/.vscode")
        file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/.vscode")
    endif()
    
    # Generate VS Code settings
    function(vscode_generate_settings)
        set(settings_file "${CMAKE_SOURCE_DIR}/.vscode/settings.json")
        
        # Default VS Code settings for CMake projects
        set(vscode_settings "{
  \"cmake.configureSettings\": {
    \"CMAKE_BUILD_TYPE\": \"Debug\"
  },
  \"cmake.buildDirectory\": \"${CMAKE_BINARY_DIR}\",
  \"cmake.generator\": \"${CMAKE_GENERATOR}\",
  \"cmake.buildBeforeRun\": true,
  \"C_Cpp.default.configurationProvider\": \"ms-vscode.cmake-tools\",
  \"files.associations\": {
    \"*.cpp\": \"cpp\",
    \"*.h\": \"cpp\",
    \"*.hpp\": \"cpp\"
  },
  \"editor.formatOnSave\": true,
  \"C_Cpp.intelliSenseEngine\": \"default\"
}")
        
        file(WRITE ${settings_file} ${vscode_settings})
        message(STATUS "  Generated VS Code settings")
    endfunction()
    
    # Generate VS Code tasks
    function(vscode_generate_tasks)
        set(tasks_file "${CMAKE_SOURCE_DIR}/.vscode/tasks.json")
        
        set(vscode_tasks "{
  \"version\": \"2.0.0\",
  \"tasks\": [
    {
      \"label\": \"CMake: Configure\",
      \"type\": \"shell\",
      \"command\": \"cmake\",
      \"args\": [
        \"-B\", \"${CMAKE_BINARY_DIR}\",
        \"-S\", \"${CMAKE_SOURCE_DIR}\",
        \"-DCMAKE_BUILD_TYPE=Debug\"
      ],
      \"group\": \"build\",
      \"problemMatcher\": []
    },
    {
      \"label\": \"CMake: Build\",
      \"type\": \"shell\",
      \"command\": \"cmake\",
      \"args\": [
        \"--build\", \"${CMAKE_BINARY_DIR}\",
        \"--config\", \"Debug\"
      ],
      \"group\": {
        \"kind\": \"build\",
        \"isDefault\": true
      },
      \"problemMatcher\": [
        \"\$gcc\"
      ]
    },
    {
      \"label\": \"Run Tests\",
      \"type\": \"shell\",
      \"command\": \"${CMAKE_CTEST_COMMAND}\",
      \"args\": [
        \"--test-dir\", \"${CMAKE_BINARY_DIR}\",
        \"--output-on-failure\"
      ],
      \"group\": \"test\"
    }
  ]
}")
        
        file(WRITE ${tasks_file} ${vscode_tasks})
        message(STATUS "  Generated VS Code tasks")
    endfunction()
    
    # Generate VS Code launch configurations
    function(vscode_generate_launch)
        set(launch_file "${CMAKE_SOURCE_DIR}/.vscode/launch.json")
        
        set(vscode_launch "{
  \"version\": \"0.2.0\",
  \"configurations\": [
    {
      \"name\": \"Debug Application\",
      \"type\": \"cppdbg\",
      \"request\": \"launch\",
      \"program\": \"\${command:cmake.launchTargetPath}\",
      \"args\": [],
      \"stopAtEntry\": false,
      \"cwd\": \"\${workspaceFolder}\",
      \"environment\": [],
      \"externalConsole\": false,
      \"MIMode\": \"gdb\",
      \"setupCommands\": [
        {
          \"description\": \"Enable pretty-printing for gdb\",
          \"text\": \"-enable-pretty-printing\",
          \"ignoreFailures\": true
        }
      ],
      \"preLaunchTask\": \"CMake: Build\"
    },
    {
      \"name\": \"Run Tests\",
      \"type\": \"cppdbg\",
      \"request\": \"launch\",
      \"program\": \"${CMAKE_BINARY_DIR}/tests/test_runner\",
      \"args\": [],
      \"stopAtEntry\": false,
      \"cwd\": \"\${workspaceFolder}\",
      \"environment\": [],
      \"externalConsole\": false,
      \"MIMode\": \"gdb\"
    }
  ]
}")
        
        file(WRITE ${launch_file} ${vscode_launch})
        message(STATUS "  Generated VS Code launch configurations")
    endfunction()
    
    # Generate all VS Code configuration files
    vscode_generate_settings()
    vscode_generate_tasks()
    vscode_generate_launch()
endif()

# ==================== XCODE CONFIGURATION ====================

if(IDE_XCODE OR CMAKE_GENERATOR MATCHES "Xcode")
    message(STATUS "Configuring for Xcode")
    
    # Xcode specific settings
    set(CMAKE_XCODE_GENERATE_SCHEME TRUE)
    set(CMAKE_XCODE_SCHEME_ENVIRONMENT
        "DYLD_LIBRARY_PATH=${CMAKE_BINARY_DIR}/lib:\$DYLD_LIBRARY_PATH"
    )
    
    # Xcode scheme configuration
    function(xcode_configure_scheme target)
        set(scheme_file "${CMAKE_BINARY_DIR}/${target}.xcscheme")
        
        # Create custom scheme configuration
        # Note: This is a simplified example
        message(STATUS "  Configured Xcode scheme for ${target}")
    endfunction()
    
    # macOS framework support
    if(APPLE)
        # Enable @rpath
        set(CMAKE_MACOSX_RPATH ON)
        set(CMAKE_INSTALL_RPATH "@loader_path/../lib")
        
        # Universal binaries support
        if(NOT CMAKE_OSX_ARCHITECTURES)
            set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64" CACHE STRING "Build architectures")
        endif()
        
        # macOS deployment target
        if(NOT CMAKE_OSX_DEPLOYMENT_TARGET)
            set(CMAKE_OSX_DEPLOYMENT_TARGET "11.0" CACHE STRING "Minimum macOS version")
        endif()
    endif()
endif()

# ==================== GENERIC IDE FEATURES ====================

# Source code grouping for all IDEs
function(organize_source_files target)
    get_target_property(target_sources ${target} SOURCES)
    
    foreach(source IN LISTS target_sources)
        get_filename_component(source_path "${source}" PATH)
        
        # Convert path to IDE folder format
        string(REPLACE "/" "\\\\" folder_path "${source_path}")
        if(folder_path STREQUAL "")
            set(folder_path "Root")
        endif()
        
        source_group("${folder_path}" FILES "${source}")
    endforeach()
endfunction()

# Create IDE-friendly test targets
function(add_ide_test target)
    add_executable(${target} ${ARGN})
    
    # Mark as test executable
    set_target_properties(${target} PROPERTIES
        FOLDER "Tests"
        EXCLUDE_FROM_DEFAULT_BUILD FALSE
    )
    
    # Add to CTest
    add_test(NAME ${target} COMMAND ${target})
    
    # IDE-specific test configurations
    if(IDE_VISUAL_STUDIO)
        set_target_properties(${target} PROPERTIES
            VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_BINARY_DIR}/bin/$<CONFIG>"
        )
    endif()
    
    if(IDE_CLION)
        clion_setup_debug(${target} "")
    endif()
    
    if(IDE_XCODE)
        xcode_configure_scheme(${target})
    endif()
endfunction()

# Generate IDE project files helper
function(generate_ide_helper)
    # Create a README with IDE-specific instructions
    set(readme_content "# IDE Configuration Guide\n\n")
    
    if(IDE_VISUAL_STUDIO)
        string(APPEND readme_content "## Visual Studio\n")
        string(APPEND readme_content "- Open ${CMAKE_BINARY_DIR}/${PROJECT_NAME}.sln\n")
        string(APPEND readme_content "- Set startup project: ${PROJECT_NAME}\n")
        string(APPEND readme_content "- Build: Ctrl+Shift+B\n")
        string(APPEND readme_content "- Debug: F5\n\n")
    endif()
    
    if(IDE_CLION)
        string(APPEND readme_content "## CLion\n")
        string(APPEND readme_content "- Open folder: ${CMAKE_SOURCE_DIR}\n")
        string(APPEND readme_content "- Build: Ctrl+F9\n")
        string(APPEND readme_content "- Run: Shift+F10\n")
        string(APPEND readme_content "- Debug: Shift+F9\n\n")
    endif()
    
    if(IDE_VSCODE)
        string(APPEND readme_content "## VS Code\n")
        string(APPEND readme_content "- Install CMake Tools extension\n")
        string(APPEND readme_content "- Configure: F1 → CMake: Configure\n")
        string(APPEND readme_content "- Build: F7\n")
        string(APPEND readme_content "- Debug: F5\n\n")
    endif()
    
    if(IDE_XCODE)
        string(APPEND readme_content "## Xcode\n")
        string(APPEND readme_content "- Open ${CMAKE_BINARY_DIR}/${PROJECT_NAME}.xcodeproj\n")
        string(APPEND readme_content "- Build: Cmd+B\n")
        string(APPEND readme_content "- Run: Cmd+R\n")
        string(APPEND readme_content "- Debug: Cmd+Y\n\n")
    endif()
    
    file(WRITE "${CMAKE_BINARY_DIR}/IDE_README.md" "${readme_content}")
    message(STATUS "Generated IDE README: ${CMAKE_BINARY_DIR}/IDE_README.md")
endfunction()

# ==================== BUILD CACHE CONFIGURATION ====================

# Configure ccache for faster builds
option(ENABLE_CCACHE "Enable ccache for faster builds" ON)

if(ENABLE_CCACHE)
    find_program(CCACHE_PROGRAM ccache)
    if(CCACHE_PROGRAM)
        # Set ccache as compiler launcher
        set(CMAKE_CXX_COMPILER_LAUNCHER ${CCACHE_PROGRAM})
        set(CMAKE_C_COMPILER_LAUNCHER ${CCACHE_PROGRAM})
        
        # Configure ccache settings
        execute_process(
            COMMAND ${CCACHE_PROGRAM} --set-config=cache_dir=${CMAKE_BINARY_DIR}/.ccache
            COMMAND ${CCACHE_PROGRAM} --set-config=max_size=5G
            COMMAND ${CCACHE_PROGRAM} --set-config=sloppiness=file_macro,locale,time_macros
        )
        
        message(STATUS "ccache enabled: ${CCACHE_PROGRAM}")
        
        # Show ccache statistics
        add_custom_target(ccache-stats
            COMMAND ${CCACHE_PROGRAM} --show-stats
            COMMENT "Showing ccache statistics"
        )
    else()
        message(STATUS "ccache not found, building without cache")
    endif()
endif()

# Configure sccache (alternative to ccache)
option(ENABLE_SCCACHE "Enable sccache for distributed builds" OFF)

if(ENABLE_SCCACHE)
    find_program(SCCACHE_PROGRAM sccache)
    if(SCCACHE_PROGRAM)
        set(CMAKE_CXX_COMPILER_LAUNCHER ${SCCACHE_PROGRAM})
        set(CMAKE_C_COMPILER_LAUNCHER ${SCCACHE_PROGRAM})
        message(STATUS "sccache enabled: ${SCCACHE_PROGRAM}")
    endif()
endif()

# ==================== PERFORMANCE PROFILING ====================

# Add build timing
option(ENABLE_BUILD_TIMING "Enable build timing" ON)

if(ENABLE_BUILD_TIMING)
    # Ninja build timing
    if(CMAKE_GENERATOR MATCHES "Ninja")
        set(CMAKE_NINJA_OUTPUT_PATH_PREFIX "${CMAKE_BINARY_DIR}/ninja_timing/")
        message(STATUS "Ninja build timing enabled")
    endif()
    
    # Custom target to show build times
    add_custom_target(show-build-times
        COMMAND ${CMAKE_COMMAND} -E echo "Build timing not available for ${CMAKE_GENERATOR}"
        COMMENT "Build timing information"
    )
endif()

# End of IDEConfig.cmake
```

### **Source Code with IDE-Friendly Features**

**src/CMakeLists.txt:**
```cmake
# Source directory with IDE-specific configurations

# Create a library with IDE-friendly organization
add_library(core_lib STATIC
    math/vector.cpp
    math/matrix.cpp
    utils/string_utils.cpp
    utils/file_utils.cpp
    algorithms/sort.cpp
    algorithms/search.cpp
)

# Organize source files in IDE
organize_source_files(core_lib)

# Set IDE-friendly properties
set_target_properties(core_lib PROPERTIES
    FOLDER "Libraries"
    VERSION 1.0.0
    SOVERSION 1
)

# Include directories with IDE support
target_include_directories(core_lib
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

# IDE-specific compiler flags
if(MSVC)
    target_compile_options(core_lib PRIVATE
        /MP  # Multi-processor compilation
        /bigobj  # Larger object files for templates
    )
endif()

# Create main application
add_executable(app_main
    main.cpp
    ui/main_window.cpp
    ui/controls.cpp
)

# Organize application source files
organize_source_files(app_main)

set_target_properties(app_main PROPERTIES
    FOLDER "Applications"
    WIN32_EXECUTABLE TRUE
    MACOSX_BUNDLE TRUE
)

# Link with core library
target_link_libraries(app_main PRIVATE core_lib)

# IDE debug configuration
if(IDE_VISUAL_STUDIO)
    set_target_properties(app_main PROPERTIES
        VS_DEBUGGER_COMMAND_ARGUMENTS "--verbose --log-level=debug"
        VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        VS_DEBUGGER_ENVIRONMENT "PATH=${CMAKE_BINARY_DIR}/bin/%CONFIG%;%PATH%"
    )
endif()

if(IDE_CLION)
    clion_setup_debug(app_main "--verbose --log-level=debug")
endif()

# Create a test target using IDE-friendly function
add_ide_test(test_core
    tests/test_vector.cpp
    tests/test_matrix.cpp
    tests/test_algorithms.cpp
)

target_link_libraries(test_core PRIVATE core_lib)

# Add a benchmark target
add_executable(bench_core
    benchmarks/vector_bench.cpp
    benchmarks/matrix_bench.cpp
)

set_target_properties(bench_core PROPERTIES
    FOLDER "Benchmarks"
)

target_link_libraries(bench_core PRIVATE core_lib)

# Generate IDE helper files
generate_ide_helper()
```

### **Debugging CMake Projects**

**scripts/debug_cmake.cmake:**
```cmake
# CMake debugging utilities

# ==================== DEBUGGING MACROS ====================

# Print variable with context
macro(debug_var var)
    message("DEBUG: ${var} = ${${var}}")
    message("  Defined: ${DEFINED ${var}}")
    message("  Type: ${TYPE ${var}}")
endmacro()

# Print all variables matching pattern
macro(debug_vars pattern)
    get_cmake_property(vars VARIABLES)
    foreach(var IN LISTS vars)
        if(var MATCHES "${pattern}")
            debug_var(${var})
        endif()
    endforeach()
endmacro()

# Print target properties
macro(debug_target target)
    if(TARGET ${target})
        message("DEBUG TARGET: ${target}")
        
        get_target_property(target_type ${target} TYPE)
        get_target_property(target_sources ${target} SOURCES)
        get_target_property(target_includes ${target} INCLUDE_DIRECTORIES)
        get_target_property(target_defs ${target} COMPILE_DEFINITIONS)
        get_target_property(target_options ${target} COMPILE_OPTIONS)
        
        message("  Type: ${target_type}")
        message("  Sources: ${target_sources}")
        message("  Includes: ${target_includes}")
        message("  Definitions: ${target_defs}")
        message("  Options: ${target_options}")
    else()
        message("ERROR: Target ${target} not found")
    endif()
endmacro()

# ==================== TRACE EXECUTION ====================

# Trace function calls
macro(trace_function function_name)
    message(STATUS "TRACE: Entering ${function_name}")
    # Store current variables
    set(TRACE_DEPTH_${function_name} 0)
endmacro()

macro(trace_return function_name)
    message(STATUS "TRACE: Leaving ${function_name}")
endmacro()

# ==================== PERFORMANCE PROFILING ====================

# Profile a CMake code block
macro(profile_section section_name)
    string(TIMESTAMP start_time "%s.%f")
    set(_profile_section_name ${section_name})
    set(_profile_start_time ${start_time})
endmacro()

macro(end_profile_section)
    string(TIMESTAMP end_time "%s.%f")
    math(EXPR elapsed "${end_time} - ${_profile_start_time}" OUTPUT_FORMAT "%.6f")
    message(STATUS "PROFILE: ${_profile_section_name} took ${elapsed} seconds")
endmacro()

# ==================== DEPENDENCY GRAPH ====================

# Generate dependency graph
function(generate_dependency_graph)
    set(graph_file "${CMAKE_BINARY_DIR}/dependencies.dot")
    
    file(WRITE ${graph_file} "digraph dependencies {\n")
    file(APPEND ${graph_file} "  rankdir=LR;\n  node [shape=box];\n\n")
    
    get_property(targets GLOBAL PROPERTY REGISTERED_TARGETS)
    
    foreach(target IN LISTS targets)
        if(TARGET ${target})
            get_target_property(deps ${target} LINK_LIBRARIES)
            if(deps)
                foreach(dep IN LISTS deps)
                    if(TARGET ${dep})
                        file(APPEND ${graph_file} "  \"${target}\" -> \"${dep}\";\n")
                    endif()
                endforeach()
            else()
                file(APPEND ${graph_file} "  \"${target}\";\n")
            endif()
        endif()
    endforeach()
    
    file(APPEND ${graph_file} "}\n")
    
    message(STATUS "Dependency graph generated: ${graph_file}")
    
    # Convert to PNG if graphviz is available
    find_program(DOT_EXECUTABLE dot)
    if(DOT_EXECUTABLE)
        execute_process(
            COMMAND ${DOT_EXECUTABLE} -Tpng ${graph_file} -o ${CMAKE_BINARY_DIR}/dependencies.png
            OUTPUT_QUIET
            ERROR_QUIET
        )
        message(STATUS "Dependency graph image: ${CMAKE_BINARY_DIR}/dependencies.png")
    endif()
endfunction()

# ==================== DEBUG TARGETS ====================

# Create a debug target that shows all CMake variables
add_custom_target(debug-vars
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_SOURCE_DIR}/scripts/debug_cmake.cmake
    COMMENT "Showing all CMake variables"
)

# Target to show target information
add_custom_target(debug-targets
    COMMAND ${CMAKE_COMMAND} -E echo "=== TARGETS ==="
    COMMAND ${CMAKE_COMMAND} -E echo "Targets:"
    COMMAND ${CMAKE_COMMAND} -E echo "$<TARGETS>"
    COMMENT "Showing all targets"
)

# Target to generate build report
add_custom_target(build-report
    COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target help
    COMMAND ${CMAKE_COMMAND} -E echo "=== BUILD CONFIGURATION ==="
    COMMAND ${CMAKE_COMMAND} -E echo "Generator: ${CMAKE_GENERATOR}"
    COMMAND ${CMAKE_COMMAND} -E echo "Build type: ${CMAKE_BUILD_TYPE}"
    COMMAND ${CMAKE_COMMAND} -E echo "Compiler: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}"
    COMMENT "Generating build report"
)

# ==================== MEMORY CHECK ====================

# Check for memory issues in CMake scripts
macro(check_memory_usage)
    if(CMAKE_HOST_UNIX)
        execute_process(
            COMMAND ps -o rss= -p ${CMAKE_PARENT_PROCESS_ID}
            OUTPUT_VARIABLE memory_usage
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        if(memory_usage)
            math(EXPR memory_mb "${memory_usage} / 1024")
            message(STATUS "Memory usage: ${memory_mb} MB")
        endif()
    endif()
endmacro()

# ==================== ERROR ANALYSIS ====================

# Analyze CMake errors
function(analyze_error error_message)
    message("ERROR ANALYSIS:")
    message("  Message: ${error_message}")
    
    # Check common issues
    if(error_message MATCHES "not found")
        message("  Suggestion: Check if package is installed or path is correct")
    elseif(error_message MATCHES "undefined reference")
        message("  Suggestion: Check library linking order and dependencies")
    elseif(error_message MATCHES "no rule to make target")
        message("  Suggestion: Check if source files exist and are added to target")
    elseif(error_message MATCHES "cannot find")
        message("  Suggestion: Check file paths and existence")
    endif()
    
    # Show relevant variables
    debug_vars("CMAKE.*PATH")
    debug_vars(".*LIBRARY")
    debug_vars(".*INCLUDE")
endfunction()

# ==================== INTERACTIVE DEBUGGING ====================

# Create interactive debugging target
add_custom_target(interactive-debug
    COMMAND ${CMAKE_COMMAND} -i
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    COMMENT "Starting interactive CMake debugging"
)
```

### **Build Performance Optimization**

**cmake/BuildOptimization.cmake:**
```cmake
# BuildOptimization.cmake - Performance optimization for builds

# ==================== PARALLEL BUILD OPTIMIZATION ====================

# Detect number of CPU cores
function(detect_cpu_cores)
    if(CMAKE_HOST_UNIX)
        execute_process(
            COMMAND nproc
            OUTPUT_VARIABLE cpu_cores
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
    elseif(WIN32)
        execute_process(
            COMMAND cmd /C "echo %NUMBER_OF_PROCESSORS%"
            OUTPUT_VARIABLE cpu_cores
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
    else()
        set(cpu_cores 4)  # Default fallback
    endif()
    
    set(NUM_CPU_CORES ${cpu_cores} PARENT_SCOPE)
    message(STATUS "Detected CPU cores: ${cpu_cores}")
endfunction()

detect_cpu_cores()

# Set parallel build options
if(NUM_CPU_CORES)
    set(CMAKE_BUILD_PARALLEL_LEVEL ${NUM_CPU_CORES} CACHE STRING "Number of parallel jobs")
    
    # For Makefile generators
    if(CMAKE_GENERATOR MATCHES "Make")
        set(CMAKE_MAKE_PROGRAM "${CMAKE_MAKE_PROGRAM} -j${NUM_CPU_CORES}")
    endif()
    
    # For Ninja generator
    if(CMAKE_GENERATOR MATCHES "Ninja")
        set(CMAKE_JOB_POOLS "compile_job_pool=${NUM_CPU_CORES}")
        set(CMAKE_NINJA_JOBS ${NUM_CPU_CORES})
    endif()
endif()

# ==================== UNITY BUILD SUPPORT ====================

option(ENABLE_UNITY_BUILD "Enable unity builds (faster compilation)" OFF)

if(ENABLE_UNITY_BUILD)
    message(STATUS "Unity build enabled")
    
    # Unity build function
    function(enable_unity_build target batch_size)
        if(NOT batch_size)
            set(batch_size 10)
        endif()
        
        # Get target sources
        get_target_property(target_sources ${target} SOURCES)
        
        # Group sources into batches
        set(batch_index 0)
        set(current_batch "")
        set(unity_files "")
        
        foreach(source IN LISTS target_sources)
            list(APPEND current_batch ${source})
            
            if(${current_batch} LENGTH EQUAL ${batch_size})
                # Create unity file for this batch
                set(unity_file "${CMAKE_BINARY_DIR}/unity_${target}_${batch_index}.cpp")
                
                # Generate unity file content
                set(unity_content "// Unity build file for ${target}\n")
                foreach(batch_source IN LISTS current_batch)
                    get_filename_component(abs_source "${batch_source}" ABSOLUTE)
                    string(APPEND unity_content "#include \"${abs_source}\"\n")
                endforeach()
                
                file(WRITE ${unity_file} ${unity_content})
                list(APPEND unity_files ${unity_file})
                
                # Reset for next batch
                set(current_batch "")
                math(EXPR batch_index "${batch_index} + 1")
            endif()
        endforeach()
        
        # Handle remaining sources
        if(current_batch)
            set(unity_file "${CMAKE_BINARY_DIR}/unity_${target}_${batch_index}.cpp")
            set(unity_content "// Unity build file for ${target}\n")
            foreach(batch_source IN LISTS current_batch)
                get_filename_component(abs_source "${batch_source}" ABSOLUTE)
                string(APPEND unity_content "#include \"${abs_source}\"\n")
            endforeach()
            
            file(WRITE ${unity_file} ${unity_content})
            list(APPEND unity_files ${unity_file})
        endif()
        
        # Replace target sources with unity files
        set_target_properties(${target} PROPERTIES SOURCES "${unity_files}")
        
        message(STATUS "  ${target}: Created ${batch_index} unity files")
    endfunction()
endif()

# ==================== PRECOMPILED HEADERS ====================

option(ENABLE_PRECOMPILED_HEADERS "Enable precompiled headers" ON)

if(ENABLE_PRECOMPILED_HEADERS)
    # Detect compiler support for precompiled headers
    if(MSVC)
        set(PCH_SUPPORTED TRUE)
        set(PCH_EXTENSION ".pch")
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        set(PCH_SUPPORTED TRUE)
        set(PCH_EXTENSION ".gch")
    else()
        set(PCH_SUPPORTED FALSE)
    endif()
    
    if(PCH_SUPPORTED)
        message(STATUS "Precompiled headers enabled")
        
        function(add_precompiled_header target header)
            if(MSVC)
                # MSVC precompiled headers
                set_source_files_properties(${header} PROPERTIES
                    COMPILE_FLAGS "/Yc\"${header}\""
                )
                
                get_target_property(target_sources ${target} SOURCES)
                foreach(source IN LISTS target_sources)
                    if(NOT source STREQUAL ${header})
                        set_source_files_properties(${source} PROPERTIES
                            COMPILE_FLAGS "/Yu\"${header}\""
                        )
                    endif()
                endforeach()
                
            elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
                # GCC/Clang precompiled headers
                set(pch_output "${CMAKE_BINARY_DIR}/${target}.gch")
                
                add_custom_command(
                    OUTPUT ${pch_output}
                    COMMAND ${CMAKE_CXX_COMPILER}
                        -x c++-header
                        -o ${pch_output}
                        ${header}
                    DEPENDS ${header}
                    COMMENT "Generating precompiled header for ${target}"
                )
                
                add_custom_target(${target}_pch DEPENDS ${pch_output})
                add_dependencies(${target} ${target}_pch)
                
                target_compile_options(${target} PRIVATE
                    -Winvalid-pch
                    -include ${header}
                )
            endif()
        endfunction()
    endif()
endif()

# ==================== DISTRIBUTED BUILDS ====================

option(ENABLE_DISTRIBUTED_BUILD "Enable distributed build support" OFF)

if(ENABLE_DISTRIBUTED_BUILD)
    # Check for distcc or icecc
    find_program(DISTCC_EXECUTABLE distcc)
    find_program(ICECC_EXECUTABLE icecc)
    
    if(DISTCC_EXECUTABLE)
        message(STATUS "Distcc found: ${DISTCC_EXECUTABLE}")
        set(CMAKE_CXX_COMPILER_LAUNCHER ${DISTCC_EXECUTABLE})
        set(CMAKE_C_COMPILER_LAUNCHER ${DISTCC_EXECUTABLE})
    elseif(ICECC_EXECUTABLE)
        message(STATUS "Icecc found: ${ICECC_EXECUTABLE}")
        set(CMAKE_CXX_COMPILER_LAUNCHER ${ICECC_EXECUTABLE})
        set(CMAKE_C_COMPILER_LAUNCHER ${ICECC_EXECUTABLE})
    else()
        message(STATUS "No distributed build tool found")
    endif()
endif()

# ==================== INCREMENTAL BUILD OPTIMIZATION ====================

# Optimize incremental builds
if(MSVC)
    # MSVC incremental linking
    set(CMAKE_MSVC_INCREMENTAL_DEFAULT ON)
    
    # Link-time code generation (LTCG)
    option(ENABLE_LTCG "Enable Link Time Code Generation" OFF)
    if(ENABLE_LTCG)
        add_compile_options(/GL)
        add_link_options(/LTCG)
    endif()
endif()

# GCC/Clang link-time optimization
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    option(ENABLE_LTO "Enable Link Time Optimization" OFF)
    if(ENABLE_LTO)
        if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
            add_compile_options(-flto)
            add_link_options(-flto)
        elseif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
            add_compile_options(-flto=thin)
            add_link_options(-flto=thin)
        endif()
    endif()
endif()

# ==================== CACHE OPTIMIZATION ====================

# Optimize build cache usage
function(configure_build_cache)
    # Set cache directory
    if(NOT DEFINED CMAKE_CACHE_DIR)
        set(CMAKE_CACHE_DIR "${CMAKE_BINARY_DIR}/.cmake_cache" CACHE PATH "CMake cache directory")
    endif()
    
    # Create cache directory
    file(MAKE_DIRECTORY ${CMAKE_CACHE_DIR})
    
    # Configure cache policies
    set(CMAKE_CACHE_MAY_BE_USED_TO_LOAD TRUE CACHE BOOL "Enable CMake cache loading")
    set(CMAKE_CACHE_MAY_BE_USED_TO_SAVE TRUE CACHE BOOL "Enable CMake cache saving")
    
    message(STATUS "Build cache configured: ${CMAKE_CACHE_DIR}")
endfunction()

configure_build_cache()

# ==================== PROFILE GUIDED OPTIMIZATION ====================

option(ENABLE_PGO "Enable Profile Guided Optimization" OFF)

if(ENABLE_PGO)
    message(STATUS "Profile Guided Optimization enabled")
    
    # PGO setup for different compilers
    if(MSVC)
        set(PGO_TRAINING_DIR "${CMAKE_BINARY_DIR}/pgo_training")
        file(MAKE_DIRECTORY ${PGO_TRAINING_DIR})
        
        add_compile_options(/GL)
        add_link_options(/LTCG:PGI /GENPROFILE /FASTGENPROFILE)
        
    elseif(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
        set(PGO_TRAINING_DIR "${CMAKE_BINARY_DIR}/pgo_training")
        file(MAKE_DIRECTORY ${PGO_TRAINING_DIR})
        
        add_compile_options(-fprofile-generate=${PGO_TRAINING_DIR})
        add_link_options(-fprofile-generate=${PGO_TRAINING_DIR})
        
    elseif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
        set(PGO_TRAINING_DIR "${CMAKE_BINARY_DIR}/pgo_training")
        file(MAKE_DIRECTORY ${PGO_TRAINING_DIR})
        
        add_compile_options(-fprofile-generate=${PGO_TRAINING_DIR})
        add_link_options(-fprofile-generate=${PGO_TRAINING_DIR})
    endif()
endif()

# ==================== BUILD MONITORING ====================

# Add build monitoring
option(ENABLE_BUILD_MONITORING "Enable build monitoring" OFF)

if(ENABLE_BUILD_MONITORING)
    # Create build monitoring target
    add_custom_target(monitor-build
        COMMAND ${CMAKE_COMMAND} -E echo "Build monitoring not implemented"
        COMMENT "Monitoring build progress"
    )
    
    # Build progress tracking
    function(track_build_progress target stage)
        string(TIMESTAMP current_time "%Y-%m-%d %H:%M:%S")
        file(APPEND ${CMAKE_BINARY_DIR}/build.log
            "[${current_time}] ${stage}: ${target}\n"
        )
    endfunction()
endif()

# ==================== OPTIMIZATION PRESETS ====================

# Create optimization presets
set(OPTIMIZATION_PRESETS
    "Debug"      # No optimization, full debug info
    "Release"    # Maximum optimization
    "Size"       # Optimize for size
    "Speed"      # Optimize for speed
    "Balanced"   # Balanced optimization
    CACHE STRING "Optimization preset"
)

# Apply optimization preset
function(apply_optimization_preset target preset)
    message(STATUS "Applying ${preset} optimization to ${target}")
    
    if(preset STREQUAL "Debug")
        target_compile_options(${target} PRIVATE
            $<$<CXX_COMPILER_ID:GNU,Clang>:-O0 -g3>
            $<$<CXX_COMPILER_ID:MSVC>:/Od /Zi>
        )
        
    elseif(preset STREQUAL "Release")
        target_compile_options(${target} PRIVATE
            $<$<CXX_COMPILER_ID:GNU,Clang>:-O3 -DNDEBUG>
            $<$<CXX_COMPILER_ID:MSVC>:/O2 /DNDEBUG>
        )
        
    elseif(preset STREQUAL "Size")
        target_compile_options(${target} PRIVATE
            $<$<CXX_COMPILER_ID:GNU,Clang>:-Os -DNDEBUG>
            $<$<CXX_COMPILER_ID:MSVC>:/O1 /DNDEBUG>
        )
        
    elseif(preset STREQUAL "Speed")
        target_compile_options(${target} PRIVATE
            $<$<CXX_COMPILER_ID:GNU,Clang>:-O3 -march=native -DNDEBUG>
            $<$<CXX_COMPILER_ID:MSVC>:/O2 /arch:AVX2 /DNDEBUG>
        )
        
    elseif(preset STREQUAL "Balanced")
        target_compile_options(${target} PRIVATE
            $<$<CXX_COMPILER_ID:GNU,Clang>:-O2 -DNDEBUG>
            $<$<CXX_COMPILER_ID:MSVC>:/O2 /DNDEBUG>
        )
    endif()
endfunction()

# End of BuildOptimization.cmake
```

### **VS Code Configuration Files**

**.vscode/settings.json:**
```json
{
    "cmake.configureSettings": {
        "CMAKE_BUILD_TYPE": "Debug",
        "CMAKE_EXPORT_COMPILE_COMMANDS": true,
        "ENABLE_CCACHE": true,
        "ENABLE_UNITY_BUILD": false
    },
    "cmake.buildDirectory": "${workspaceFolder}/build",
    "cmake.generator": "Ninja",
    "cmake.buildBeforeRun": true,
    "cmake.options.verbose": true,
    "C_Cpp.default.configurationProvider": "ms-vscode.cmake-tools",
    "C_Cpp.default.intelliSenseMode": "clang-x64",
    "C_Cpp.default.compilerPath": "clang++",
    "C_Cpp.default.cppStandard": "c++17",
    "files.associations": {
        "*.cpp": "cpp",
        "*.h": "cpp",
        "*.hpp": "cpp",
        "*.inl": "cpp",
        "*.tpp": "cpp",
        "CMakeLists.txt": "cmake",
        "*.cmake": "cmake"
    },
    "editor.formatOnSave": true,
    "clangd.path": "clangd",
    "clangd.arguments": [
        "--background-index",
        "--compile-commands-dir=${workspaceFolder}/build",
        "--completion-style=detailed"
    ],
    "search.exclude": {
        "build/**": true,
        "**/.git": true,
        "**/node_modules": true
    },
    "files.watcherExclude": {
        "build/**": true,
        "**/.git": true
    }
}
```

**.vscode/launch.json:**
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Application",
            "type": "cppdbg",
            "request": "launch",
            "program": "${command:cmake.launchTargetPath}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "CMake: Build",
            "logging": {
                "moduleLoad": false,
                "trace": false
            }
        },
        {
            "name": "Run Tests",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/build/tests/test_runner",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        },
        {
            "name": "Debug CMake Script",
            "type": "cppdbg",
            "request": "launch",
            "program": "cmake",
            "args": [
                "-S",
                "${workspaceFolder}",
                "-B",
                "${workspaceFolder}/build",
                "-DCMAKE_BUILD_TYPE=Debug"
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true
        }
    ]
}
```

### **CLion Configuration**

**.idea/cmake.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project version="4">
  <component name="CMakeSettings">
    <configurations>
      <configuration
        name="Debug"
        type="CMakeConfiguration"
        GENERATION_DIR="$PROJECT_DIR$/build"
        BUILD_DIR="$PROJECT_DIR$/build"
        CONFIG_NAME="Debug"
        OPTIONS="-DCMAKE_BUILD_TYPE=Debug -DENABLE_CCACHE=ON"
        VERSION="3.20">
      </configuration>
      <configuration
        name="Release"
        type="CMakeConfiguration"
        GENERATION_DIR="$PROJECT_DIR$/build-release"
        BUILD_DIR="$PROJECT_DIR$/build-release"
        CONFIG_NAME="Release"
        OPTIONS="-DCMAKE_BUILD_TYPE=Release -DENABLE_CCACHE=ON"
        VERSION="3.20">
      </configuration>
    </configurations>
  </component>
</project>
```

### **Visual Studio Configuration**

**.vs/ProjectSettings.json:**
```json
{
  "CurrentProjectSetting": "Debug|x64",
  "Configurations": [
    {
      "Name": "Debug|x64",
      "Generator": "Visual Studio 16 2019",
      "ConfigurationType": "Debug",
      "BuildRoot": "${projectDir}\\build",
      "InstallRoot": "${projectDir}\\install",
      "CMakeExecutable": "C:\\Program Files\\CMake\\bin\\cmake.exe",
      "BuildCommandArgs": "-m",
      "CTestCommandArgs": "",
      "Variables": [
        {
          "name": "CMAKE_BUILD_TYPE",
          "value": "Debug"
        },
        {
          "name": "CMAKE_INSTALL_PREFIX",
          "value": "${projectDir}\\install"
        }
      ]
    },
    {
      "Name": "Release|x64",
      "Generator": "Visual Studio 16 2019",
      "ConfigurationType": "Release",
      "BuildRoot": "${projectDir}\\build-release",
      "InstallRoot": "${projectDir}\\install-release",
      "CMakeExecutable": "C:\\Program Files\\CMake\\bin\\cmake.exe",
      "BuildCommandArgs": "-m",
      "CTestCommandArgs": "",
      "Variables": [
        {
          "name": "CMAKE_BUILD_TYPE",
          "value": "Release"
        },
        {
          "name": "CMAKE_INSTALL_PREFIX",
          "value": "${projectDir}\\install-release"
        }
      ]
    }
  ]
}
```

### **Debugging Techniques**

**scripts/debug_techniques.cmake:**
```cmake
# Advanced debugging techniques for CMake projects

# ==================== MEMORY LEAK DETECTION ====================

# Configure memory debugging
option(ENABLE_MEMORY_DEBUG "Enable memory debugging tools" OFF)

if(ENABLE_MEMORY_DEBUG)
    # Find Valgrind
    find_program(VALGRIND_EXECUTABLE valgrind)
    if(VALGRIND_EXECUTABLE)
        message(STATUS "Valgrind found: ${VALGRIND_EXECUTABLE}")
        
        # Create Valgrind test target
        add_custom_target(memory-check
            COMMAND ${VALGRIND_EXECUTABLE}
                --leak-check=full
                --show-leak-kinds=all
                --track-origins=yes
                --error-exitcode=1
                ${CMAKE_BINARY_DIR}/bin/test_runner
            DEPENDS test_runner
            COMMENT "Running memory check with Valgrind"
        )
    endif()
    
    # AddressSanitizer
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        option(ENABLE_ASAN "Enable AddressSanitizer" OFF)
        if(ENABLE_ASAN)
            add_compile_options(-fsanitize=address -fno-omit-frame-pointer)
            add_link_options(-fsanitize=address)
            message(STATUS "AddressSanitizer enabled")
        endif()
        
        # UndefinedBehaviorSanitizer
        option(ENABLE_UBSAN "Enable UndefinedBehaviorSanitizer" OFF)
        if(ENABLE_UBSAN)
            add_compile_options(-fsanitize=undefined)
            add_link_options(-fsanitize=undefined)
            message(STATUS "UndefinedBehaviorSanitizer enabled")
        endif()
        
        # ThreadSanitizer
        option(ENABLE_TSAN "Enable ThreadSanitizer" OFF)
        if(ENABLE_TSAN)
            add_compile_options(-fsanitize=thread)
            add_link_options(-fsanitize=thread)
            message(STATUS "ThreadSanitizer enabled")
        endif()
    endif()
endif()

# ==================== CODE COVERAGE ====================

# Configure code coverage
option(ENABLE_CODE_COVERAGE "Enable code coverage reporting" OFF)

if(ENABLE_CODE_COVERAGE)
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        # GCC/Clang coverage
        add_compile_options(--coverage)
        add_link_options(--coverage)
        
        # Find lcov and genhtml
        find_program(LCOV_EXECUTABLE lcov)
        find_program(GENHTML_EXECUTABLE genhtml)
        
        if(LCOV_EXECUTABLE AND GENHTML_EXECUTABLE)
            # Create coverage target
            add_custom_target(coverage
                COMMAND ${LCOV_EXECUTABLE} --directory . --zerocounters
                COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
                COMMAND ${LCOV_EXECUTABLE} --directory . --capture --output-file coverage.info
                COMMAND ${LCOV_EXECUTABLE} --remove coverage.info '*/tests/*' '*/usr/*' --output-file coverage.filtered.info
                COMMAND ${GENHTML_EXECUTABLE} coverage.filtered.info --output-directory ${CMAKE_BINARY_DIR}/coverage_report
                COMMAND ${CMAKE_COMMAND} -E echo "Coverage report: file://${CMAKE_BINARY_DIR}/coverage_report/index.html"
                WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
                DEPENDS test_runner
                COMMENT "Generating code coverage report"
            )
        endif()
    endif()
endif()

# ==================== STATIC ANALYSIS ====================

# Configure static analysis
option(ENABLE_STATIC_ANALYSIS "Enable static analysis tools" OFF)

if(ENABLE_STATIC_ANALYSIS)
    # Clang-Tidy
    find_program(CLANG_TIDY_EXECUTABLE clang-tidy)
    if(CLANG_TIDY_EXECUTABLE)
        set(CMAKE_CXX_CLANG_TIDY ${CLANG_TIDY_EXECUTABLE})
        message(STATUS "Clang-Tidy enabled: ${CLANG_TIDY_EXECUTABLE}")
    endif()
    
    # Cppcheck
    find_program(CPPCHECK_EXECUTABLE cppcheck)
    if(CPPCHECK_EXECUTABLE)
        # Create cppcheck target
        add_custom_target(static-analysis
            COMMAND ${CPPCHECK_EXECUTABLE}
                --enable=all
                --suppress=missingIncludeSystem
                --inline-suppr
                --force
                ${CMAKE_SOURCE_DIR}/src
            COMMENT "Running static analysis with Cppcheck"
        )
    endif()
    
    # Include-what-you-use
    find_program(IWYU_EXECUTABLE include-what-you-use)
    if(IWYU_EXECUTABLE)
        set(CMAKE_CXX_INCLUDE_WHAT_YOU_USE ${IWYU_EXECUTABLE})
        message(STATUS "Include-what-you-use enabled")
    endif()
endif()

# ==================== PERFORMANCE PROFILING ====================

# Configure performance profiling
option(ENABLE_PERFORMANCE_PROFILING "Enable performance profiling" OFF)

if(ENABLE_PERFORMANCE_PROFILING)
    # Find profiling tools
    find_program(PERF_EXECUTABLE perf)
    find_program(GPROF_EXECUTABLE gprof)
    find_program(VALGRIND_CALLGRIND_EXECUTABLE valgrind)
    
    if(PERF_EXECUTABLE)
        add_custom_target(profile-perf
            COMMAND ${PERF_EXECUTABLE} record -g ${CMAKE_BINARY_DIR}/bin/benchmark_runner
            COMMAND ${PERF_EXECUTABLE} report
            DEPENDS benchmark_runner
            COMMENT "Profiling with perf"
        )
    endif()
    
    if(VALGRIND_CALLGRIND_EXECUTABLE)
        add_custom_target(profile-callgrind
            COMMAND ${VALGRIND_CALLGRIND_EXECUTABLE} --tool=callgrind ${CMAKE_BINARY_DIR}/bin/benchmark_runner
            DEPENDS benchmark_runner
            COMMENT "Profiling with Callgrind"
        )
    endif()
endif()

# ==================== DEBUG SYMBOLS AND FORMAT ====================

# Configure debug symbols
function(configure_debug_symbols target)
    if(MSVC)
        # MSVC debug information
        set_target_properties(${target} PROPERTIES
            MSVC_RUNTIME_LIBRARY "MultiThreadedDebugDLL"
        )
        
        if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.25)
            set_target_properties(${target} PROPERTIES
                MSVC_DEBUG_INFORMATION_FORMAT "$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>"
            )
        endif()
        
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        # GCC/Clang debug information
        target_compile_options(${target} PRIVATE
            $<$<CONFIG:Debug>:-g3 -ggdb>
            $<$<CONFIG:RelWithDebInfo>:-g2>
        )
    endif()
endfunction()

# ==================== REMOTE DEBUGGING ====================

# Configure remote debugging
option(ENABLE_REMOTE_DEBUGGING "Enable remote debugging configuration" OFF)

if(ENABLE_REMOTE_DEBUGGING)
    # Generate remote debugging configurations
    function(configure_remote_debugging target)
        # Generate gdbserver configuration
        set(gdb_config "${CMAKE_BINARY_DIR}/gdb_${target}.init")
        file(WRITE ${gdb_config}
            "target remote localhost:1234\n"
            "file ${CMAKE_BINARY_DIR}/bin/${target}\n"
            "b main\n"
        )
        
        message(STATUS "Generated remote debugging config: ${gdb_config}")
    endfunction()
endif()
```

### **Best Practices for IDE Integration**

1. **Use Folder Organization**: Organize targets and sources with `set_property(GLOBAL PROPERTY USE_FOLDERS ON)`
2. **Generate compile_commands.json**: Essential for clangd, clang-tidy, and other tools
3. **IDE-Specific Configurations**: Customize settings for each IDE
4. **Debug Configuration**: Set up debuggers with proper working directories and arguments
5. **Build Cache**: Enable ccache/sccache for faster incremental builds
6. **Parallel Builds**: Configure parallel compilation based on CPU cores
7. **Precompiled Headers**: Use PCH for faster compilation in large projects
8. **Static Analysis**: Integrate clang-tidy and cppcheck
9. **Memory Checking**: Enable sanitizers and valgrind integration
10. **Performance Profiling**: Set up profiling tools for performance analysis

### **Common IDE Issues and Solutions**

**Issue 1: VS Code can't find CMake**
```json
// .vscode/settings.json
{
    "cmake.cmakePath": "C:/Program Files/CMake/bin/cmake.exe"
}
```

**Issue 2: CLion not detecting CMake profiles**
```xml
<!-- .idea/cmake.xml -->
<configuration>
  <option name="GENERATION_DIR" value="$PROJECT_DIR$/build" />
  <option name="BUILD_DIR" value="$PROJECT_DIR$/build" />
</configuration>
```

**Issue 3: Visual Studio CMake settings not applying**
```cmake
# Set in CMakeLists.txt
set(CMAKE_VS_PLATFORM_TOOLSET "v142")
set(CMAKE_VS_WINDOWS_TARGET_PLATFORM_VERSION "10.0")
```

**Issue 4: Xcode scheme not working**
```cmake
set(CMAKE_XCODE_GENERATE_SCHEME TRUE)
set(CMAKE_XCODE_SCHEME_ENVIRONMENT "DYLD_LIBRARY_PATH=${CMAKE_BINARY_DIR}/lib:$ENV{DYLD_LIBRARY_PATH}")
```

### **Exercise: Create an IDE-Friendly Project**

Create a project with:

1. **Multi-IDE Support**: Configurations for VS Code, CLion, Visual Studio, Xcode
2. **Build Optimization**: ccache, unity builds, precompiled headers
3. **Debugging Setup**: Memory sanitizers, code coverage, static analysis
4. **Performance Profiling**: Integration with perf, callgrind, gprof
5. **Remote Debugging**: Configuration for gdbserver
6. **Custom Tasks**: Build, test, clean, format targets
7. **Source Organization**: Folder-based organization in IDEs
8. **Documentation**: IDE-specific README files
9. **CI Integration**: Automated testing with different IDEs
10. **Developer Experience**: One-command setup for all IDEs

### **Next Steps Preview**
In **PART-12**, we'll cover:
- CMake with modern C++ features (modules, concepts, coroutines)
- C++20/23 support in CMake
- Module-based projects
- Concepts and constraints
- Coroutines integration
- Future C++ standards compatibility

**To continue, please reply with "Next" or "PART-12" to receive the next part of the course.**


# **PART-12: Modern C++ Features with CMake**

### **Topic: C++20/23 Modules, Concepts, Coroutines, and Advanced Language Features**

### **Introduction to Modern C++ in CMake**

C++20 and C++23 bring revolutionary features like modules, concepts, coroutines, and ranges. CMake has evolved to support these features, but they require careful configuration. This part covers how to use modern C++ features effectively with CMake.

### **Project Structure for This Part**

```
modern_cpp_project/
├── CMakeLists.txt
├── src/
│   ├── modules/
│   ├── concepts/
│   ├── coroutines/
│   ├── ranges/
│   └── traditional/
├── tests/
├── benchmarks/
└── cmake/
    └── ModernCpp.cmake
```

### **Root CMakeLists.txt with Modern C++ Support**

```cmake
cmake_minimum_required(VERSION 3.28)  # Required for full C++20/23 support
project(ModernCppDemo VERSION 1.0.0
    DESCRIPTION "Demonstration of modern C++ features with CMake"
    LANGUAGES CXX
)

# Include modern C++ utilities
include(cmake/ModernCpp.cmake)

# Set modern C++ standard
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Enable C++ modules if supported
option(ENABLE_MODULES "Enable C++20 modules support" ON)

# Enable experimental features
option(ENABLE_EXPERIMENTAL_FEATURES "Enable experimental C++ features" OFF)

# Detect compiler features
detect_cxx_features()

# Print compiler capabilities
message(STATUS "=== Compiler Capabilities ===")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "C++ Standard: ${CMAKE_CXX_STANDARD}")
message(STATUS "Modules supported: ${CXX_MODULES_SUPPORTED}")
message(STATUS "Concepts supported: ${CXX_CONCEPTS_SUPPORTED}")
message(STATUS "Coroutines supported: ${CXX_COROUTINES_SUPPORTED}")
message(STATUS "Ranges supported: ${CXX_RANGES_SUPPORTED}")
message(STATUS "Format library supported: ${CXX_FORMAT_SUPPORTED}")
message(STATUS "=============================")

# Output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Add source directories
add_subdirectory(src/traditional)
add_subdirectory(src/concepts)
add_subdirectory(src/ranges)

if(CXX_COROUTINES_SUPPORTED)
    add_subdirectory(src/coroutines)
endif()

if(CXX_MODULES_SUPPORTED AND ENABLE_MODULES)
    add_subdirectory(src/modules)
endif()

# Add tests and benchmarks
add_subdirectory(tests)
add_subdirectory(benchmarks)

# Create a summary of enabled features
generate_feature_summary()
```

### **Modern C++ Detection Module**

**cmake/ModernCpp.cmake:**
```cmake
# ModernCpp.cmake - Detection and configuration for modern C++ features

# ==================== COMPILER FEATURE DETECTION ====================

function(detect_cxx_features)
    # Check C++ standard support
    include(CheckCXXCompilerFlag)
    
    # Check for C++23 support
    check_cxx_compiler_flag(-std=c++23 HAS_CXX23)
    check_cxx_compiler_flag(-std=c++20 HAS_CXX20)
    
    # C++20 feature detection
    set(CMAKE_REQUIRED_FLAGS "-std=c++20")
    
    # Modules
    check_cxx_source_compiles("
        export module test;
        export int foo() { return 42; }
    " CXX_MODULES_SUPPORTED)
    
    # Concepts
    check_cxx_source_compiles("
        template<typename T>
        concept Integral = std::is_integral_v<T>;
        
        template<Integral T>
        T add(T a, T b) { return a + b; }
        
        int main() { return add(1, 2); }
    " CXX_CONCEPTS_SUPPORTED)
    
    # Coroutines
    check_cxx_source_compiles("
        #include <coroutine>
        struct task {
            struct promise_type {
                task get_return_object() { return {}; }
                std::suspend_never initial_suspend() { return {}; }
                std::suspend_never final_suspend() noexcept { return {}; }
                void return_void() {}
                void unhandled_exception() {}
            };
        };
        
        task example() { co_return; }
        
        int main() { example(); return 0; }
    " CXX_COROUTINES_SUPPORTED)
    
    # Ranges
    check_cxx_source_compiles("
        #include <ranges>
        #include <vector>
        
        int main() {
            std::vector<int> vec{1, 2, 3, 4, 5};
            auto even = vec | std::views::filter([](int n) { return n % 2 == 0; });
            return 0;
        }
    " CXX_RANGES_SUPPORTED)
    
    # Format library
    check_cxx_source_compiles("
        #include <format>
        
        int main() {
            auto str = std::format(\"Hello, {}!\", \"world\");
            return 0;
        }
    " CXX_FORMAT_SUPPORTED)
    
    # Three-way comparison (spaceship operator)
    check_cxx_source_compiles("
        #include <compare>
        
        struct Point {
            int x, y;
            auto operator<=>(const Point&) const = default;
        };
        
        int main() {
            Point p1{1, 2}, p2{3, 4};
            auto cmp = p1 <=> p2;
            return 0;
        }
    " CXX_THREEWAY_COMPARISON_SUPPORTED)
    
    # Reset required flags
    set(CMAKE_REQUIRED_FLAGS "")
    
    # Set results in parent scope
    set(CXX_MODULES_SUPPORTED ${CXX_MODULES_SUPPORTED} PARENT_SCOPE)
    set(CXX_CONCEPTS_SUPPORTED ${CXX_CONCEPTS_SUPPORTED} PARENT_SCOPE)
    set(CXX_COROUTINES_SUPPORTED ${CXX_COROUTINES_SUPPORTED} PARENT_SCOPE)
    set(CXX_RANGES_SUPPORTED ${CXX_RANGES_SUPPORTED} PARENT_SCOPE)
    set(CXX_FORMAT_SUPPORTED ${CXX_FORMAT_SUPPORTED} PARENT_SCOPE)
    set(CXX_THREEWAY_COMPARISON_SUPPORTED ${CXX_THREEWAY_COMPARISON_SUPPORTED} PARENT_SCOPE)
endfunction()

# ==================== C++ MODULES CONFIGURATION ====================

function(configure_cxx_modules)
    if(NOT CXX_MODULES_SUPPORTED)
        message(WARNING "C++ modules not supported by compiler")
        return()
    endif()
    
    # Compiler-specific module flags
    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        # MSVC modules
        add_compile_options(/experimental:module)
        add_compile_options(/std:c++latest)
        
        # Module output directory
        set(CMAKE_CXX_MODULE_DIRECTORY "${CMAKE_BINARY_DIR}/modules" CACHE PATH "C++ modules output directory")
        file(MAKE_DIRECTORY ${CMAKE_CXX_MODULE_DIRECTORY})
        
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        # Clang modules
        add_compile_options(-fmodules)
        add_compile_options(-fmodules-ts)
        add_compile_options(-fbuiltin-module-map)
        
        # Module cache directory
        set(CMAKE_CXX_MODULE_DIRECTORY "${CMAKE_BINARY_DIR}/modules" CACHE PATH "C++ modules cache directory")
        file(MAKE_DIRECTORY ${CMAKE_CXX_MODULE_DIRECTORY})
        
        add_compile_options(-fmodules-cache-path=${CMAKE_CXX_MODULE_DIRECTORY})
        
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        # GCC modules (experimental)
        add_compile_options(-fmodules-ts)
        add_compile_options(-fmodules)
        
        message(STATUS "GCC modules support is experimental")
    endif()
    
    message(STATUS "C++ modules configured for ${CMAKE_CXX_COMPILER_ID}")
endfunction()

# ==================== MODULE TARGET UTILITIES ====================

# Add a C++ module target
function(add_cxx_module_target target)
    set(options INTERFACE)
    set(one_value_args)
    set(multi_value_args SOURCES DEPENDS)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    if(NOT CXX_MODULES_SUPPORTED)
        message(FATAL_ERROR "Cannot create module target: C++ modules not supported")
    endif()
    
    # Create module library
    if(ARG_INTERFACE)
        add_library(${target} INTERFACE)
    else()
        add_library(${target})
    endif()
    
    # Add module sources
    if(ARG_SOURCES)
        target_sources(${target} PRIVATE ${ARG_SOURCES})
    endif()
    
    # Set module-specific properties
    set_target_properties(${target} PROPERTIES
        CXX_STANDARD 23
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
    )
    
    # Mark sources as C++ module units
    if(ARG_SOURCES AND NOT ARG_INTERFACE)
        set_source_files_properties(${ARG_SOURCES} PROPERTIES
            CXX_MODULE ON
        )
    endif()
    
    # Add dependencies
    if(ARG_DEPENDS)
        target_link_libraries(${target} PRIVATE ${ARG_DEPENDS})
    endif()
    
    message(STATUS "Created C++ module target: ${target}")
endfunction()

# Scan for module dependencies
function(scan_module_dependencies target)
    if(NOT CXX_MODULES_SUPPORTED)
        return()
    endif()
    
    # Get target sources
    get_target_property(target_sources ${target} SOURCES)
    
    # Scan sources for module dependencies
    set(module_deps "")
    
    foreach(source IN LISTS target_sources)
        if(EXISTS ${source})
            file(READ ${source} source_content)
            
            # Look for import declarations
            if(source_content MATCHES "import[[:space:]]+([^;]+);")
                set(imported_module "${CMAKE_MATCH_1}")
                string(STRIP "${imported_module}" imported_module)
                
                # Remove comments and quotes
                string(REGEX REPLACE "//.*" "" imported_module "${imported_module}")
                string(REGEX REPLACE "/\\*.*\\*/" "" imported_module "${imported_module}")
                string(STRIP "${imported_module}" imported_module)
                
                if(imported_module AND NOT imported_module STREQUAL "import")
                    list(APPEND module_deps "${imported_module}")
                endif()
            endif()
        endif()
    endforeach()
    
    # Remove duplicates
    list(REMOVE_DUPLICATES module_deps)
    
    if(module_deps)
        message(STATUS "Module ${target} depends on: ${module_deps}")
        set(${target}_MODULE_DEPS ${module_deps} PARENT_SCOPE)
    endif()
endfunction()

# ==================== CONCEPTS CONFIGURATION ====================

function(configure_cxx_concepts)
    if(NOT CXX_CONCEPTS_SUPPORTED)
        message(WARNING "C++ concepts not supported by compiler")
        return()
    endif()
    
    # Compiler-specific concept flags
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        # GCC may need additional flags for older versions
        if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS 10.0)
            add_compile_options(-fconcepts)
        endif()
    endif()
    
    message(STATUS "C++ concepts configured for ${CMAKE_CXX_COMPILER_ID}")
endfunction()

# Add a concept library
function(add_concept_library target)
    add_library(${target} INTERFACE)
    
    target_compile_features(${target} INTERFACE cxx_concepts)
    
    message(STATUS "Created concept library: ${target}")
endfunction()

# ==================== COROUTINES CONFIGURATION ====================

function(configure_cxx_coroutines)
    if(NOT CXX_COROUTINES_SUPPORTED)
        message(WARNING "C++ coroutines not supported by compiler")
        return()
    endif()
    
    # Compiler-specific coroutine flags
    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        add_compile_options(/await)
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        add_compile_options(-fcoroutines-ts)
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER_EQUAL 10.0)
            add_compile_options(-fcoroutines)
        endif()
    endif()
    
    message(STATUS "C++ coroutines configured for ${CMAKE_CXX_COMPILER_ID}")
endfunction()

# ==================== RANGES CONFIGURATION ====================

function(configure_cxx_ranges)
    if(NOT CXX_RANGES_SUPPORTED)
        message(WARNING "C++ ranges not supported by compiler")
        return()
    endif()
    
    message(STATUS "C++ ranges configured for ${CMAKE_CXX_COMPILER_ID}")
endfunction()

# ==================== FEATURE SUMMARY ====================

function(generate_feature_summary)
    message(STATUS "")
    message(STATUS "=== Modern C++ Feature Summary ===")
    message(STATUS "Modules:         ${CXX_MODULES_SUPPORTED}")
    message(STATUS "Concepts:        ${CXX_CONCEPTS_SUPPORTED}")
    message(STATUS "Coroutines:      ${CXX_COROUTINES_SUPPORTED}")
    message(STATUS "Ranges:          ${CXX_RANGES_SUPPORTED}")
    message(STATUS "Format Library:  ${CXX_FORMAT_SUPPORTED}")
    message(STATUS "Spaceship Op:    ${CXX_THREEWAY_COMPARISON_SUPPORTED}")
    message(STATUS "================================")
    message(STATUS "")
endfunction()

# ==================== COMPILER-SPECIFIC WORKAROUNDS ====================

function(apply_compiler_workarounds)
    # GCC workarounds
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        # GCC 11 has incomplete modules support
        if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS 12.0 AND CXX_MODULES_SUPPORTED)
            message(WARNING "GCC < 12.0 has experimental modules support")
            set(CXX_MODULES_SUPPORTED FALSE PARENT_SCOPE)
        endif()
        
        # GCC coroutines require additional library on some systems
        if(CXX_COROUTINES_SUPPORTED)
            find_library(COROUTINES_LIB pthread)
            if(COROUTINES_LIB)
                link_libraries(${COROUTINES_LIB})
            endif()
        endif()
    endif()
    
    # Clang workarounds
    if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        # Clang modules may require standard library modules
        if(CXX_MODULES_SUPPORTED)
            find_library(STDLIB_MODULES libc++experimental.a)
        endif()
    endif()
    
    # MSVC workarounds
    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        # MSVC may need latest standard library
        if(CXX_MODULES_SUPPORTED OR CXX_RANGES_SUPPORTED)
            add_compile_definitions(_HAS_CXX20=1)
        endif()
    endif()
endfunction()

# Initialize feature detection
detect_cxx_features()
apply_compiler_workarounds()

# Configure features if supported
if(CXX_MODULES_SUPPORTED)
    configure_cxx_modules()
endif()

if(CXX_CONCEPTS_SUPPORTED)
    configure_cxx_concepts()
endif()

if(CXX_COROUTINES_SUPPORTED)
    configure_cxx_coroutines()
endif()

if(CXX_RANGES_SUPPORTED)
    configure_cxx_ranges()
endif()

# End of ModernCpp.cmake
```

### **C++20 Modules Example**

**src/modules/CMakeLists.txt:**
```cmake
# C++20 Modules demonstration

if(NOT CXX_MODULES_SUPPORTED OR NOT ENABLE_MODULES)
    message(STATUS "Skipping modules - not supported or disabled")
    return()
endif()

message(STATUS "Building C++20 modules...")

# Create module interface (header unit)
add_cxx_module_target(math_module INTERFACE)

# Add module sources
target_sources(math_module INTERFACE
    FILE_SET CXX_MODULES TYPE CXX_MODULES FILES
        math.ixx
)

# Create a module implementation
add_cxx_module_target(math_impl
    SOURCES
        math.cpp
        math_impl.cpp
    DEPENDS
        math_module
)

# Create module partition
add_cxx_module_target(math_advanced
    SOURCES
        advanced.cpp
    DEPENDS
        math_impl
)

# Create an executable that uses modules
add_executable(module_demo
    main.cpp
)

# Link with modules
target_link_libraries(module_demo PRIVATE
    math_advanced
)

# Scan for module dependencies
scan_module_dependencies(module_demo)

message(STATUS "Module demo configured")
```

**src/modules/math.ixx (module interface):**
```cpp
// math.ixx - C++20 module interface
export module math;

export namespace math {
    
    // Basic arithmetic
    export int add(int a, int b);
    export int subtract(int a, int b);
    export int multiply(int a, int b);
    export double divide(double a, double b);
    
    // Constants
    export constexpr double PI = 3.14159265358979323846;
    export constexpr double E = 2.71828182845904523536;
    
    // Vector type
    export template<typename T>
    class Vector {
    private:
        T x, y, z;
        
    public:
        Vector(T x = T{}, T y = T{}, T z = T{});
        
        T get_x() const;
        T get_y() const;
        T get_z() const;
        
        Vector operator+(const Vector& other) const;
        Vector operator-(const Vector& other) const;
        T dot(const Vector& other) const;
        Vector cross(const Vector& other) const;
        
        export static Vector zero();
        export static Vector unit_x();
        export static Vector unit_y();
        export static Vector unit_z();
    };
    
    // Function templates
    export template<typename T>
    T square(T value) {
        return value * value;
    }
    
    export template<typename T>
    T cube(T value) {
        return value * value * value;
    }
    
    // Concepts
    export template<typename T>
    concept Arithmetic = requires(T a, T b) {
        { a + b } -> std::convertible_to<T>;
        { a - b } -> std::convertible_to<T>;
        { a * b } -> std::convertible_to<T>;
        { a / b } -> std::convertible_to<T>;
    };
    
    export template<Arithmetic T>
    T average(const std::vector<T>& values);
    
} // namespace math
```

**src/modules/math.cpp (module implementation):**
```cpp
// math.cpp - Module implementation unit
module math;

namespace math {
    
    int add(int a, int b) {
        return a + b;
    }
    
    int subtract(int a, int b) {
        return a - b;
    }
    
    int multiply(int a, int b) {
        return a * b;
    }
    
    double divide(double a, double b) {
        if (b == 0.0) {
            throw std::runtime_error("Division by zero");
        }
        return a / b;
    }
    
    // Vector implementation
    template<typename T>
    Vector<T>::Vector(T x, T y, T z) : x(x), y(y), z(z) {}
    
    template<typename T>
    T Vector<T>::get_x() const { return x; }
    
    template<typename T>
    T Vector<T>::get_y() const { return y; }
    
    template<typename T>
    T Vector<T>::get_z() const { return z; }
    
    template<typename T>
    Vector<T> Vector<T>::operator+(const Vector& other) const {
        return Vector(x + other.x, y + other.y, z + other.z);
    }
    
    template<typename T>
    Vector<T> Vector<T>::operator-(const Vector& other) const {
        return Vector(x - other.x, y - other.y, z - other.z);
    }
    
    template<typename T>
    T Vector<T>::dot(const Vector& other) const {
        return x * other.x + y * other.y + z * other.z;
    }
    
    template<typename T>
    Vector<T> Vector<T>::cross(const Vector& other) const {
        return Vector(
            y * other.z - z * other.y,
            z * other.x - x * other.z,
            x * other.y - y * other.x
        );
    }
    
    template<typename T>
    Vector<T> Vector<T>::zero() {
        return Vector(T{}, T{}, T{});
    }
    
    template<typename T>
    Vector<T> Vector<T>::unit_x() {
        return Vector(T{1}, T{}, T{});
    }
    
    template<typename T>
    Vector<T> Vector<T>::unit_y() {
        return Vector(T{}, T{1}, T{});
    }
    
    template<typename T>
    Vector<T> Vector<T>::unit_z() {
        return Vector(T{}, T{}, T{1});
    }
    
    // Explicit template instantiations
    template class Vector<int>;
    template class Vector<float>;
    template class Vector<double>;
    
    template<Arithmetic T>
    T average(const std::vector<T>& values) {
        if (values.empty()) {
            return T{};
        }
        
        T sum{};
        for (const auto& value : values) {
            sum += value;
        }
        
        return sum / static_cast<T>(values.size());
    }
    
    // Explicit concept instantiations
    template int average<int>(const std::vector<int>&);
    template float average<float>(const std::vector<float>&);
    template double average<double>(const std::vector<double>&);
    
} // namespace math
```

**src/modules/main.cpp (module consumer):**
```cpp
// main.cpp - Consumer of C++20 modules
import math;
import <iostream>;
import <vector>;
import <format>;

int main() {
    using namespace math;
    
    std::cout << "=== C++20 Modules Demo ===\n\n";
    
    // Basic arithmetic
    std::cout << std::format("5 + 3 = {}\n", add(5, 3));
    std::cout << std::format("10 - 4 = {}\n", subtract(10, 4));
    std::cout << std::format("6 * 7 = {}\n", multiply(6, 7));
    std::cout << std::format("15.0 / 4.0 = {:.2f}\n", divide(15.0, 4.0));
    
    // Constants
    std::cout << std::format("\nConstants:\n");
    std::cout << std::format("  PI = {:.10f}\n", PI);
    std::cout << std::format("  E  = {:.10f}\n", E);
    
    // Vector operations
    Vector<double> v1{1.0, 2.0, 3.0};
    Vector<double> v2{4.0, 5.0, 6.0};
    
    auto v_sum = v1 + v2;
    auto v_dot = v1.dot(v2);
    auto v_cross = v1.cross(v2);
    
    std::cout << std::format("\nVector Operations:\n");
    std::cout << std::format("  v1 = ({:.1f}, {:.1f}, {:.1f})\n", 
                           v1.get_x(), v1.get_y(), v1.get_z());
    std::cout << std::format("  v2 = ({:.1f}, {:.1f}, {:.1f})\n", 
                           v2.get_x(), v2.get_y(), v2.get_z());
    std::cout << std::format("  v1 + v2 = ({:.1f}, {:.1f}, {:.1f})\n",
                           v_sum.get_x(), v_sum.get_y(), v_sum.get_z());
    std::cout << std::format("  v1 · v2 = {:.1f}\n", v_dot);
    std::cout << std::format("  v1 × v2 = ({:.1f}, {:.1f}, {:.1f})\n",
                           v_cross.get_x(), v_cross.get_y(), v_cross.get_z());
    
    // Template functions with concepts
    std::cout << std::format("\nTemplate Functions:\n");
    std::cout << std::format("  square(5) = {}\n", square(5));
    std::cout << std::format("  cube(3) = {}\n", cube(3));
    std::cout << std::format("  square(2.5) = {:.2f}\n", square(2.5));
    
    // Concept-constrained function
    std::vector<int> int_values{1, 2, 3, 4, 5};
    std::vector<double> double_values{1.1, 2.2, 3.3, 4.4, 5.5};
    
    std::cout << std::format("\nAverage with Concepts:\n");
    std::cout << std::format("  average(int_values) = {:.2f}\n", 
                           average(int_values));
    std::cout << std::format("  average(double_values) = {:.2f}\n", 
                           average(double_values));
    
    // Module-only features
    std::cout << std::format("\nModule Features:\n");
    std::cout << std::format("  Zero vector: ({:.1f}, {:.1f}, {:.1f})\n",
                           Vector<double>::zero().get_x(),
                           Vector<double>::zero().get_y(),
                           Vector<double>::zero().get_z());
    
    return 0;
}
```

### **C++20 Concepts Example**

**src/concepts/CMakeLists.txt:**
```cmake
# C++20 Concepts demonstration

if(NOT CXX_CONCEPTS_SUPPORTED)
    message(STATUS "Skipping concepts - not supported")
    return()
endif()

message(STATUS "Building C++20 concepts...")

# Create a concept library
add_concept_library(concepts_lib)

# Add concept definitions
add_library(concepts_impl STATIC
    arithmetic_concepts.cpp
    container_concepts.cpp
    iterator_concepts.cpp
)

target_link_libraries(concepts_impl INTERFACE concepts_lib)

# Create executable demonstrating concepts
add_executable(concepts_demo
    main.cpp
    algorithm_examples.cpp
)

target_link_libraries(concepts_demo PRIVATE concepts_impl)

# Enable concepts
target_compile_features(concepts_demo PRIVATE cxx_concepts)

message(STATUS "Concepts demo configured")
```

**src/concepts/arithmetic_concepts.cpp:**
```cpp
// arithmetic_concepts.cpp - C++20 arithmetic concepts
#include <concepts>
#include <type_traits>

namespace concepts {
    
    // Basic arithmetic concept
    template<typename T>
    concept Arithmetic = std::is_arithmetic_v<T>;
    
    // Signed arithmetic concept
    template<typename T>
    concept Signed = Arithmetic<T> && std::is_signed_v<T>;
    
    // Unsigned arithmetic concept
    template<typename T>
    concept Unsigned = Arithmetic<T> && std::is_unsigned_v<T>;
    
    // Floating point concept
    template<typename T>
    concept FloatingPoint = std::is_floating_point_v<T>;
    
    // Integral concept
    template<typename T>
    concept Integral = std::is_integral_v<T>;
    
    // Addable concept
    template<typename T, typename U = T>
    concept Addable = requires(T a, U b) {
        { a + b } -> std::convertible_to<T>;
    };
    
    // Subtractable concept
    template<typename T, typename U = T>
    concept Subtractable = requires(T a, U b) {
        { a - b } -> std::convertible_to<T>;
    };
    
    // Multiplicable concept
    template<typename T, typename U = T>
    concept Multiplicable = requires(T a, U b) {
        { a * b } -> std::convertible_to<T>;
    };
    
    // Divisible concept
    template<typename T, typename U = T>
    concept Divisible = requires(T a, U b) {
        { a / b } -> std::convertible_to<T>;
    };
    
    // Comparable concept
    template<typename T>
    concept Comparable = requires(T a, T b) {
        { a == b } -> std::convertible_to<bool>;
        { a != b } -> std::convertible_to<bool>;
        { a < b } -> std::convertible_to<bool>;
        { a > b } -> std::convertible_to<bool>;
        { a <= b } -> std::convertible_to<bool>;
        { a >= b } -> std::convertible_to<bool>;
    };
    
    // Ordered concept (total order)
    template<typename T>
    concept Ordered = Comparable<T> && requires(T a, T b, T c) {
        requires std::is_same_v<decltype(a <= b), bool>;
        requires std::is_same_v<decltype(a >= b), bool>;
        requires std::totally_ordered<T>;
    };
    
    // Number concept combining multiple arithmetic properties
    template<typename T>
    concept Number = Arithmetic<T> && Addable<T> && Subtractable<T> && 
                     Multiplicable<T> && Comparable<T>;
    
    // Real number concept
    template<typename T>
    concept RealNumber = FloatingPoint<T> && Number<T>;
    
    // Complex number requirements
    template<typename T>
    concept HasRealPart = requires(T a) {
        { a.real() } -> FloatingPoint;
    };
    
    template<typename T>
    concept HasImagPart = requires(T a) {
        { a.imag() } -> FloatingPoint;
    };
    
    template<typename T>
    concept ComplexNumber = HasRealPart<T> && HasImagPart<T> && 
                           Addable<T> && Subtractable<T> && 
                           Multiplicable<T>;
    
} // namespace concepts
```

**src/concepts/container_concepts.cpp:**
```cpp
// container_concepts.cpp - C++20 container concepts
#include <concepts>
#include <iterator>
#include <ranges>

namespace concepts {
    
    // Container concept
    template<typename C>
    concept Container = requires(C c) {
        typename C::value_type;
        typename C::reference;
        typename C::const_reference;
        typename C::iterator;
        typename C::const_iterator;
        typename C::difference_type;
        typename C::size_type;
        
        { c.begin() } -> std::same_as<typename C::iterator>;
        { c.end() } -> std::same_as<typename C::iterator>;
        { c.cbegin() } -> std::same_as<typename C::const_iterator>;
        { c.cend() } -> std::same_as<typename C::const_iterator>;
        { c.size() } -> std::same_as<typename C::size_type>;
        { c.max_size() } -> std::same_as<typename C::size_type>;
        { c.empty() } -> std::same_as<bool>;
    };
    
    // Sequence container concept
    template<typename C>
    concept SequenceContainer = Container<C> && requires(C c, typename C::value_type v) {
        { c.front() } -> std::same_as<typename C::reference>;
        { c.back() } -> std::same_as<typename C::reference>;
        c.push_back(v);
        c.pop_back();
    };
    
    // Random access container concept
    template<typename C>
    concept RandomAccessContainer = SequenceContainer<C> && requires(C c, typename C::size_type i) {
        { c[i] } -> std::same_as<typename C::reference>;
        { c.at(i) } -> std::same_as<typename C::reference>;
    };
    
    // Associative container concept
    template<typename C>
    concept AssociativeContainer = Container<C> && requires(C c, typename C::key_type k) {
        typename C::key_type;
        typename C::mapped_type;
        { c.find(k) } -> std::same_as<typename C::iterator>;
        { c.count(k) } -> std::same_as<typename C::size_type>;
        c.insert({k, typename C::mapped_type{}});
    };
    
    // Allocator-aware container concept
    template<typename C>
    concept AllocatorAwareContainer = Container<C> && requires {
        typename C::allocator_type;
        requires std::allocator<typename C::value_type>;
    };
    
    // Contiguous container concept (C++20)
    template<typename C>
    concept ContiguousContainer = Container<C> && requires(C c) {
        requires std::contiguous_iterator<typename C::iterator>;
        { c.data() } -> std::same_as<typename C::value_type*>;
    };
    
    // Range concept (C++20)
    template<typename R>
    concept Range = std::ranges::range<R>;
    
    // Sized range concept
    template<typename R>
    concept SizedRange = std::ranges::sized_range<R>;
    
    // Input range concept
    template<typename R>
    concept InputRange = std::ranges::input_range<R>;
    
    // Forward range concept
    template<typename R>
    concept ForwardRange = std::ranges::forward_range<R>;
    
    // Bidirectional range concept
    template<typename R>
    concept BidirectionalRange = std::ranges::bidirectional_range<R>;
    
    // Random access range concept
    template<typename R>
    concept RandomAccessRange = std::ranges::random_access_range<R>;
    
    // Contiguous range concept
    template<typename R>
    concept ContiguousRange = std::ranges::contiguous_range<R>;
    
    // View concept (C++20)
    template<typename V>
    concept View = std::ranges::view<V>;
    
    // Common view concept
    template<typename V>
    concept CommonView = View<V> && std::ranges::common_range<V>;
    
} // namespace concepts
```

**src/concepts/algorithm_examples.cpp:**
```cpp
// algorithm_examples.cpp - Algorithms using C++20 concepts
#include <concepts>
#include <vector>
#include <list>
#include <array>
#include <algorithm>
#include <numeric>
#include <iostream>
#include <format>

namespace algorithm {
    
    using namespace concepts;
    
    // Concept-constrained sort function
    template<RandomAccessContainer Container>
    void sort_container(Container& container) {
        std::sort(container.begin(), container.end());
    }
    
    // Alternative with ranges
    template<RandomAccessRange Range>
    void sort_range(Range& range) {
        std::ranges::sort(range);
    }
    
    // Find element using concepts
    template<InputRange Range, typename T>
    requires std::equality_comparable_with<std::ranges::range_value_t<Range>, T>
    auto find_in_range(Range&& range, const T& value) {
        return std::ranges::find(range, value);
    }
    
    // Transform with concepts
    template<InputRange InRange, std::output_iterator<std::ranges::range_value_t<InRange>> OutIter,
             std::invocable<std::ranges::range_value_t<InRange>> Function>
    void transform_range(InRange&& input, OutIter output, Function func) {
        std::ranges::transform(input, output, func);
    }
    
    // Filter with concepts
    template<InputRange Range, std::predicate<std::ranges::range_value_t<Range>> Predicate>
    auto filter_range(Range&& range, Predicate pred) {
        return range | std::views::filter(pred);
    }
    
    // Accumulate with concepts
    template<InputRange Range, typename T>
    requires std::constructible_from<T, std::ranges::range_value_t<Range>> &&
             Addable<T>
    T accumulate_range(Range&& range, T init) {
        return std::accumulate(std::ranges::begin(range), std::ranges::end(range), init);
    }
    
    // Map with concepts
    template<typename Key, typename Value>
    concept Mappable = requires {
        requires std::default_initializable<Key>;
        requires std::default_initializable<Value>;
        requires std::totally_ordered<Key>;
    };
    
    template<Mappable Key, Mappable Value>
    class SafeMap {
    private:
        std::vector<std::pair<Key, Value>> data;
        
    public:
        void insert(const Key& key, const Value& value) {
            data.emplace_back(key, value);
            std::ranges::sort(data, [](const auto& a, const auto& b) {
                return a.first < b.first;
            });
        }
        
        std::optional<Value> find(const Key& key) const {
            auto it = std::ranges::lower_bound(data, key, 
                [](const auto& pair, const Key& k) {
                    return pair.first < k;
                });
            
            if (it != data.end() && it->first == key) {
                return it->second;
            }
            return std::nullopt;
        }
    };
    
    // Mathematical operations with concepts
    template<Number T>
    T compute_polynomial(const std::vector<T>& coefficients, T x) {
        T result{};
        T power{1};
        
        for (const auto& coeff : coefficients) {
            result += coeff * power;
            power *= x;
        }
        
        return result;
    }
    
    // Matrix operations with concepts
    template<Number T>
    class Matrix {
    private:
        std::vector<std::vector<T>> data;
        size_t rows, cols;
        
    public:
        Matrix(size_t r, size_t c) : rows(r), cols(c), data(r, std::vector<T>(c)) {}
        
        template<Container ContainerType>
        requires std::same_as<std::ranges::range_value_t<ContainerType>, T>
        Matrix(size_t r, size_t c, const ContainerType& values) 
            : rows(r), cols(c), data(r, std::vector<T>(c)) {
            
            auto it = std::ranges::begin(values);
            for (size_t i = 0; i < rows; ++i) {
                for (size_t j = 0; j < cols; ++j) {
                    if (it != std::ranges::end(values)) {
                        data[i][j] = *it++;
                    }
                }
            }
        }
        
        T& operator()(size_t i, size_t j) { return data[i][j]; }
        const T& operator()(size_t i, size_t j) const { return data[i][j]; }
        
        Matrix operator+(const Matrix& other) const {
            Matrix result(rows, cols);
            for (size_t i = 0; i < rows; ++i) {
                for (size_t j = 0; j < cols; ++j) {
                    result(i, j) = data[i][j] + other(i, j);
                }
            }
            return result;
        }
        
        Matrix operator*(const Matrix& other) const requires Multiplicable<T> {
            if (cols != other.rows) {
                throw std::invalid_argument("Matrix dimensions don't match");
            }
            
            Matrix result(rows, other.cols);
            for (size_t i = 0; i < rows; ++i) {
                for (size_t k = 0; k < cols; ++k) {
                    for (size_t j = 0; j < other.cols; ++j) {
                        result(i, j) += data[i][k] * other(k, j);
                    }
                }
            }
            return result;
        }
    };
    
} // namespace algorithm
```

**src/concepts/main.cpp:**
```cpp
// main.cpp - C++20 Concepts demonstration
#include <iostream>
#include <vector>
#include <list>
#include <array>
#include <format>
#include <ranges>
#include "arithmetic_concepts.cpp"
#include "container_concepts.cpp"
#include "algorithm_examples.cpp"

using namespace concepts;
using namespace algorithm;

int main() {
    std::cout << "=== C++20 Concepts Demo ===\n\n";
    
    // Test arithmetic concepts
    std::cout << "Arithmetic Concepts:\n";
    static_assert(Arithmetic<int>, "int should be Arithmetic");
    static_assert(Arithmetic<double>, "double should be Arithmetic");
    static_assert(Integral<int>, "int should be Integral");
    static_assert(FloatingPoint<double>, "double should be FloatingPoint");
    static_assert(Addable<int>, "int should be Addable");
    std::cout << "  All arithmetic concepts satisfied\n\n";
    
    // Test container concepts
    std::cout << "Container Concepts:\n";
    static_assert(Container<std::vector<int>>, "vector should be a Container");
    static_assert(SequenceContainer<std::vector<int>>, "vector should be a SequenceContainer");
    static_assert(RandomAccessContainer<std::vector<int>>, "vector should be RandomAccessContainer");
    static_assert(ContiguousContainer<std::vector<int>>, "vector should be ContiguousContainer");
    static_assert(Range<std::vector<int>>, "vector should be a Range");
    std::cout << "  All container concepts satisfied\n\n";
    
    // Demonstrate concept-constrained algorithms
    std::cout << "Concept-Constrained Algorithms:\n";
    
    // Sort with concepts
    std::vector<int> numbers{5, 2, 8, 1, 9, 3};
    std::cout << std::format("  Original: {}\n", std::views::all(numbers));
    
    sort_container(numbers);  // Using RandomAccessContainer concept
    std::cout << std::format("  Sorted: {}\n", std::views::all(numbers));
    
    // Find with concepts
    auto it = find_in_range(numbers, 5);
    if (it != numbers.end()) {
        std::cout << std::format("  Found 5 at position: {}\n", 
                                std::distance(numbers.begin(), it));
    }
    
    // Transform with concepts
    std::vector<int> doubled(numbers.size());
    transform_range(numbers, doubled.begin(), [](int n) { return n * 2; });
    std::cout << std::format("  Doubled: {}\n", std::views::all(doubled));
    
    // Filter with concepts
    auto evens = numbers | std::views::filter([](int n) { return n % 2 == 0; });
    std::cout << std::format("  Even numbers: {}\n", evens);
    
    // Accumulate with concepts
    int sum = accumulate_range(numbers, 0);
    std::cout << std::format("  Sum: {}\n", sum);
    
    // SafeMap with concepts
    SafeMap<std::string, int> age_map;
    age_map.insert("Alice", 30);
    age_map.insert("Bob", 25);
    age_map.insert("Charlie", 35);
    
    if (auto age = age_map.find("Bob")) {
        std::cout << std::format("  Bob's age: {}\n", *age);
    }
    
    // Polynomial with concepts
    std::vector<double> coefficients{1.0, 2.0, 3.0}; // 1 + 2x + 3x²
    double result = compute_polynomial(coefficients, 2.0);
    std::cout << std::format("  Polynomial at x=2: {:.2f}\n", result);
    
    // Matrix operations with concepts
    Matrix<int> A(2, 2, std::vector{1, 2, 3, 4});
    Matrix<int> B(2, 2, std::vector{5, 6, 7, 8});
    
    auto C = A + B;
    std::cout << "  Matrix addition:\n";
    for (size_t i = 0; i < 2; ++i) {
        std::cout << "    [ ";
        for (size_t j = 0; j < 2; ++j) {
            std::cout << C(i, j) << " ";
        }
        std::cout << "]\n";
    }
    
    std::cout << "\nAll concept-constrained operations completed successfully!\n";
    
    return 0;
}
```

### **C++20 Coroutines Example**

**src/coroutines/CMakeLists.txt:**
```cmake
# C++20 Coroutines demonstration

if(NOT CXX_COROUTINES_SUPPORTED)
    message(STATUS "Skipping coroutines - not supported")
    return()
endif()

message(STATUS "Building C++20 coroutines...")

# Create coroutine library
add_library(coroutines_lib STATIC
    generator.cpp
    task.cpp
    async_operations.cpp
)

# Enable coroutines
target_compile_features(coroutines_lib PRIVATE cxx_coroutines)

# Create coroutine demo executable
add_executable(coroutines_demo
    main.cpp
    examples.cpp
)

target_link_libraries(coroutines_demo PRIVATE coroutines_lib)

# Enable coroutines for demo
target_compile_features(coroutines_demo PRIVATE cxx_coroutines)

message(STATUS "Coroutines demo configured")
```

**src/coroutines/generator.cpp:**
```cpp
// generator.cpp - C++20 generator coroutine
#include <coroutine>
#include <exception>
#include <iostream>
#include <ranges>
#include <format>

namespace coroutines {
    
    // Simple generator coroutine
    template<typename T>
    class Generator {
    public:
        struct promise_type {
            T value_;
            std::exception_ptr exception_;
            
            Generator get_return_object() {
                return Generator(std::coroutine_handle<promise_type>::from_promise(*this));
            }
            
            std::suspend_always initial_suspend() { return {}; }
            std::suspend_always final_suspend() noexcept { return {}; }
            
            void unhandled_exception() {
                exception_ = std::current_exception();
            }
            
            template<std::convertible_to<T> From>
            std::suspend_always yield_value(From&& from) {
                value_ = std::forward<From>(from);
                return {};
            }
            
            void return_void() {}
        };
        
        using handle_type = std::coroutine_handle<promise_type>;
        
        Generator(handle_type h) : coro_(h) {}
        
        ~Generator() {
            if (coro_) {
                coro_.destroy();
            }
        }
        
        Generator(const Generator&) = delete;
        Generator& operator=(const Generator&) = delete;
        
        Generator(Generator&& other) noexcept : coro_(other.coro_) {
            other.coro_ = nullptr;
        }
        
        Generator& operator=(Generator&& other) noexcept {
            if (this != &other) {
                if (coro_) {
                    coro_.destroy();
                }
                coro_ = other.coro_;
                other.coro_ = nullptr;
            }
            return *this;
        }
        
        T operator()() {
            if (!coro_) {
                throw std::runtime_error("Empty generator");
            }
            
            coro_.resume();
            if (coro_.done()) {
                throw std::runtime_error("Generator exhausted");
            }
            
            if (coro_.promise().exception_) {
                std::rethrow_exception(coro_.promise().exception_);
            }
            
            return coro_.promise().value_;
        }
        
        bool done() const {
            return !coro_ || coro_.done();
        }
        
        // Range support
        struct iterator {
            using iterator_category = std::input_iterator_tag;
            using value_type = T;
            using difference_type = std::ptrdiff_t;
            using pointer = T*;
            using reference = T&;
            
            handle_type coro_ = nullptr;
            
            iterator() = default;
            explicit iterator(handle_type coro) : coro_(coro) {
                if (coro_ && !coro_.done()) {
                    coro_.resume();
                }
            }
            
            iterator& operator++() {
                if (!coro_ || coro_.done()) {
                    coro_ = nullptr;
                } else {
                    coro_.resume();
                }
                return *this;
            }
            
            iterator operator++(int) {
                iterator tmp = *this;
                ++(*this);
                return tmp;
            }
            
            bool operator==(const iterator& other) const {
                return coro_ == other.coro_;
            }
            
            bool operator!=(const iterator& other) const {
                return !(*this == other);
            }
            
            const T& operator*() const {
                if (!coro_) {
                    throw std::runtime_error("Dereferencing empty iterator");
                }
                return coro_.promise().value_;
            }
            
            const T* operator->() const {
                return &coro_.promise().value_;
            }
        };
        
        iterator begin() {
            if (!coro_ || coro_.done()) {
                return iterator{};
            }
            return iterator{coro_};
        }
        
        iterator end() {
            return iterator{};
        }
        
    private:
        handle_type coro_;
    };
    
    // Fibonacci generator
    Generator<long long> fibonacci(int n) {
        if (n <= 0) co_return;
        
        long long a = 0, b = 1;
        co_yield a;
        
        if (n == 1) co_return;
        co_yield b;
        
        for (int i = 2; i < n; ++i) {
            long long next = a + b;
            co_yield next;
            a = b;
            b = next;
        }
    }
    
    // Range generator
    Generator<int> range(int start, int end, int step = 1) {
        for (int i = start; i < end; i += step) {
            co_yield i;
        }
    }
    
    // Filter generator
    template<typename Predicate>
    Generator<int> filter_range(int start, int end, Predicate pred) {
        for (int i = start; i < end; ++i) {
            if (pred(i)) {
                co_yield i;
            }
        }
    }
    
} // namespace coroutines
```

**src/coroutines/task.cpp:**
```cpp
// task.cpp - C++20 task coroutine
#include <coroutine>
#include <exception>
#include <functional>
#include <future>
#include <thread>
#include <queue>
#include <mutex>
#include <condition_variable>
#include <format>

namespace coroutines {
    
    // Simple task coroutine
    template<typename T = void>
    class Task {
    public:
        struct promise_type {
            T value_;
            std::exception_ptr exception_;
            std::coroutine_handle<> continuation_ = nullptr;
            
            Task get_return_object() {
                return Task(std::coroutine_handle<promise_type>::from_promise(*this));
            }
            
            std::suspend_always initial_suspend() { return {}; }
            
            struct final_awaiter {
                bool await_ready() noexcept { return false; }
                
                template<typename Promise>
                std::coroutine_handle<> await_suspend(std::coroutine_handle<Promise> h) noexcept {
                    auto& promise = h.promise();
                    if (promise.continuation_) {
                        return promise.continuation_;
                    }
                    return std::noop_coroutine();
                }
                
                void await_resume() noexcept {}
            };
            
            final_awaiter final_suspend() noexcept { return {}; }
            
            void unhandled_exception() {
                exception_ = std::current_exception();
            }
            
            template<std::convertible_to<T> From>
            void return_value(From&& from) {
                value_ = std::forward<From>(from);
            }
            
            void return_void() {}
        };
        
        using handle_type = std::coroutine_handle<promise_type>;
        
        Task(handle_type h) : coro_(h) {}
        
        ~Task() {
            if (coro_) {
                coro_.destroy();
            }
        }
        
        Task(const Task&) = delete;
        Task& operator=(const Task&) = delete;
        
        Task(Task&& other) noexcept : coro_(other.coro_) {
            other.coro_ = nullptr;
        }
        
        Task& operator=(Task&& other) noexcept {
            if (this != &other) {
                if (coro_) {
                    coro_.destroy();
                }
                coro_ = other.coro_;
                other.coro_ = nullptr;
            }
            return *this;
        }
        
        bool await_ready() const { return false; }
        
        template<typename Promise>
        void await_suspend(std::coroutine_handle<Promise> continuation) {
            coro_.promise().continuation_ = continuation;
            coro_.resume();
        }
        
        T await_resume() {
            if (coro_.promise().exception_) {
                std::rethrow_exception(coro_.promise().exception_);
            }
            return coro_.promise().value_;
        }
        
        void resume() {
            if (!coro_.done()) {
                coro_.resume();
            }
        }
        
        bool done() const {
            return !coro_ || coro_.done();
        }
        
    private:
        handle_type coro_;
    };
    
    // Async task scheduler
    class TaskScheduler {
    private:
        std::queue<std::coroutine_handle<>> tasks_;
        std::mutex mutex_;
        std::condition_variable cv_;
        std::vector<std::thread> workers_;
        bool stop_ = false;
        
        void worker_thread() {
            while (true) {
                std::coroutine_handle<> task;
                
                {
                    std::unique_lock lock(mutex_);
                    cv_.wait(lock, [this]() {
                        return !tasks_.empty() || stop_;
                    });
                    
                    if (stop_ && tasks_.empty()) {
                        return;
                    }
                    
                    task = tasks_.front();
                    tasks_.pop();
                }
                
                if (task) {
                    task.resume();
                    if (!task.done()) {
                        // Task suspended again, requeue it
                        std::lock_guard lock(mutex_);
                        tasks_.push(task);
                    }
                }
            }
        }
        
    public:
        TaskScheduler(size_t num_workers = std::thread::hardware_concurrency()) {
            workers_.reserve(num_workers);
            for (size_t i = 0; i < num_workers; ++i) {
                workers_.emplace_back(&TaskScheduler::worker_thread, this);
            }
        }
        
        ~TaskScheduler() {
            {
                std::lock_guard lock(mutex_);
                stop_ = true;
            }
            cv_.notify_all();
            
            for (auto& worker : workers_) {
                if (worker.joinable()) {
                    worker.join();
                }
            }
        }
        
        template<typename Task>
        void schedule(Task&& task) {
            std::lock_guard lock(mutex_);
            tasks_.push(std::forward<Task>(task));
            cv_.notify_one();
        }
    };
    
    // Async sleep
    Task<void> async_sleep(int milliseconds) {
        struct sleep_awaiter {
            int ms_;
            
            sleep_awaiter(int ms) : ms_(ms) {}
            
            bool await_ready() const noexcept { return ms_ <= 0; }
            
            void await_suspend(std::coroutine_handle<> h) const {
                std::thread([h, ms = ms_]() mutable {
                    std::this_thread::sleep_for(std::chrono::milliseconds(ms));
                    h.resume();
                }).detach();
            }
            
            void await_resume() const noexcept {}
        };
        
        co_await sleep_awaiter{milliseconds};
    }
    
    // Async computation
    template<typename T>
    Task<T> async_compute(std::function<T()> computation) {
        struct compute_awaiter {
            std::function<T()> computation_;
            T result_;
            
            compute_awaiter(std::function<T()> comp) : computation_(std::move(comp)) {}
            
            bool await_ready() const noexcept { return false; }
            
            void await_suspend(std::coroutine_handle<> h) const {
                std::thread([h, this]() mutable {
                    result_ = computation_();
                    h.resume();
                }).detach();
            }
            
            T await_resume() const noexcept { return result_; }
        };
        
        co_return co_await compute_awaiter{std::move(computation)};
    }
    
} // namespace coroutines
```

**src/coroutines/main.cpp:**
```cpp
// main.cpp - C++20 Coroutines demonstration
#include <iostream>
#include <format>
#include <chrono>
#include <ranges>
#include "generator.cpp"
#include "task.cpp"

using namespace coroutines;

int main() {
    std::cout << "=== C++20 Coroutines Demo ===\n\n";
    
    // Test generator coroutine
    std::cout << "Generator Coroutines:\n";
    
    // Fibonacci generator
    std::cout << "  Fibonacci sequence (first 10): ";
    auto fib = fibonacci(10);
    for (int i = 0; i < 10; ++i) {
        std::cout << fib() << " ";
    }
    std::cout << "\n";
    
    // Range generator
    std::cout << "  Range 1-10: ";
    for (auto i : range(1, 11)) {
        std::cout << i << " ";
    }
    std::cout << "\n";
    
    // Filter generator
    std::cout << "  Even numbers 1-20: ";
    auto evens = filter_range(1, 21, [](int n) { return n % 2 == 0; });
    for (auto i : evens) {
        std::cout << i << " ";
    }
    std::cout << "\n\n";
    
    // Test task coroutines
    std::cout << "Task Coroutines:\n";
    
    // Simple async task
    auto compute_sum = []() -> Task<int> {
        std::cout << "    Starting async computation...\n";
        co_await async_sleep(100);  // Simulate work
        std::cout << "    Computation completed\n";
        co_return 42;
    };
    
    // Chain tasks
    auto process_result = [](int value) -> Task<std::string> {
        std::cout << "    Processing result: " << value << "\n";
        co_await async_sleep(50);
        co_return std::format("Processed: {}", value * 2);
    };
    
    // Run tasks (simplified - in real code you'd use a scheduler)
    std::cout << "  Running async tasks...\n";
    
    // Note: In a real application, you would use a proper scheduler
    // This is a simplified demonstration
    
    std::cout << "\nCoroutine features demonstrated successfully!\n";
    
    // Demonstrate coroutine with ranges
    std::cout << "\nCoroutine Ranges:\n";
    std::cout << "  Squares of 1-5: ";
    
    auto squares = range(1, 6) | std::views::transform([](int n) {
        return n * n;
    });
    
    for (auto square : squares) {
        std::cout << square << " ";
    }
    std::cout << "\n";
    
    return 0;
}
```

### **C++20 Ranges Example**

**src/ranges/CMakeLists.txt:**
```cmake
# C++20 Ranges demonstration

if(NOT CXX_RANGES_SUPPORTED)
    message(STATUS "Skipping ranges - not supported")
    return()
endif()

message(STATUS "Building C++20 ranges...")

# Create ranges library
add_library(ranges_lib STATIC
    range_algorithms.cpp
    custom_views.cpp
    range_utilities.cpp
)

# Enable ranges
target_compile_features(ranges_lib PRIVATE cxx_std_20)

# Create ranges demo executable
add_executable(ranges_demo
    main.cpp
)

target_link_libraries(ranges_demo PRIVATE ranges_lib)

# Enable ranges for demo
target_compile_features(ranges_demo PRIVATE cxx_std_20)

message(STATUS "Ranges demo configured")
```

### **Traditional C++ for Comparison**

**src/traditional/CMakeLists.txt:**
```cmake
# Traditional C++ code for comparison
add_executable(traditional_demo
    main.cpp
    traditional_algorithms.cpp
)

message(STATUS "Traditional C++ demo configured")
```

### **Building and Testing Modern C++ Features**

**scripts/build_modern.sh:**
```bash
#!/bin/bash
set -e

# Build script for modern C++ features
BUILD_DIR="build-modern"
GENERATOR="Ninja"

echo "Building modern C++ features..."

# Clean previous build
rm -rf ${BUILD_DIR}
mkdir -p ${BUILD_DIR}
cd ${BUILD_DIR}

# Configure with modern C++ features
cmake .. \
    -G "${GENERATOR}" \
    -DCMAKE_CXX_STANDARD=23 \
    -DCMAKE_CXX_STANDARD_REQUIRED=ON \
    -DCMAKE_CXX_EXTENSIONS=OFF \
    -DENABLE_MODULES=ON \
    -DENABLE_EXPERIMENTAL_FEATURES=ON

# Build all targets
cmake --build . --parallel

echo "Build completed!"
echo ""
echo "Available executables:"
echo "  ./modules_demo       - C++20 Modules"
echo "  ./concepts_demo      - C++20 Concepts"
echo "  ./coroutines_demo    - C++20 Coroutines"
echo "  ./ranges_demo        - C++20 Ranges"
echo "  ./traditional_demo   - Traditional C++ for comparison"
```

### **Best Practices for Modern C++ with CMake**

1. **Feature Detection**: Always detect compiler capabilities before using modern features
2. **Gradual Adoption**: Mix traditional and modern code, migrating gradually
3. **Compiler Compatibility**: Provide fallbacks for older compilers
4. **Module Organization**: Plan module dependencies carefully
5. **Concept Constraints**: Use concepts to improve error messages and code clarity
6. **Coroutine Safety**: Handle coroutine lifetimes and exceptions properly
7. **Range Pipelines**: Use ranges for expressive data transformations
8. **Testing**: Test modern features extensively, as compilers may have bugs
9. **Documentation**: Document which features require which C++ standard
10. **CI/CD**: Test with multiple compilers and C++ standards

### **Common Issues and Solutions**

**Issue 1: Modules not working with GCC**
```cmake
# Check GCC version and enable experimental support
if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU" AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 12.0)
    set(CXX_MODULES_SUPPORTED FALSE)
    message(WARNING "GCC < 12.0 has incomplete modules support")
endif()
```

**Issue 2: Coroutines require additional libraries**
```cmake
# Find and link coroutine library
find_library(COROUTINES_LIB pthread)
if(COROUTINES_LIB)
    target_link_libraries(your_target PRIVATE ${COROUTINES_LIB})
endif()
```

**Issue 3: Concepts not available in older standards**
```cmake
# Provide traditional template fallback
#ifdef __cpp_concepts
    template<std::integral T>
#else
    template<typename T, typename = std::enable_if_t<std::is_integral_v<T>>>
#endif
T add(T a, T b) { return a + b; }
```

### **Exercise: Create a Modern C++ Library**

Create a library that demonstrates:

1. **C++20 Modules**: Export a public API using modules
2. **Concepts**: Constrain template parameters with concepts
3. **Coroutines**: Implement async operations with coroutines
4. **Ranges**: Provide range-based algorithms and views
5. **Format Library**: Use std::format for string formatting
6. **Three-way Comparison**: Implement spaceship operator where appropriate
7. **Feature Detection**: Graceful fallbacks for older compilers
8. **Testing**: Comprehensive tests for all modern features
9. **Benchmarks**: Compare modern vs traditional implementations
10. **Documentation**: Explain modern features with examples

### **Next Steps Preview**
In **PART-13**, we'll cover:
- Advanced CMake with CUDA and GPU programming
- Integrating CUDA kernels with CMake
- OpenCL and SYCL support
- Multi-architecture GPU builds
- Performance optimization for GPU code
- Debugging GPU applications
- Cross-platform GPU development

**To continue, please reply with "Next" or "PART-13" to receive the next part of the course.**









# **PART-13: GPU Programming with CMake - CUDA, OpenCL, and SYCL**

### **Topic: Integrating GPU Computing, Multi-Architecture Builds, and Performance Optimization**

### **Introduction to GPU Programming with CMake**

GPU programming unlocks massive parallel computing power. CMake provides excellent support for CUDA, OpenCL, and SYCL, allowing you to build cross-platform GPU applications. This part covers how to configure CMake for various GPU programming models.

### **Project Structure for This Part**

```
gpu_project/
├── CMakeLists.txt
├── cmake/
│   ├── FindCUDA.cmake
│   ├── FindOpenCL.cmake
│   └── GPUToolkit.cmake
├── src/
│   ├── cuda/
│   ├── opencl/
│   ├── sycl/
│   ├── common/
│   └── host/
├── kernels/
│   ├── cuda/
│   ├── opencl/
│   └── sycl/
└── benchmarks/
```

### **Root CMakeLists.txt with GPU Support**

```cmake
cmake_minimum_required(VERSION 3.18)
project(GPUDemo VERSION 1.0.0
    DESCRIPTION "Cross-platform GPU programming with CMake"
    LANGUAGES CXX C
)

# Include GPU utilities
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include(GPUToolkit)

# GPU backend options
option(ENABLE_CUDA "Enable CUDA support" ON)
option(ENABLE_OPENCL "Enable OpenCL support" ON)
option(ENABLE_SYCL "Enable SYCL support" OFF)
option(ENABLE_HIP "Enable AMD HIP support" OFF)
option(ENABLE_OPENMP "Enable OpenMP offloading" OFF)

# GPU architecture options
set(GPU_ARCH "auto" CACHE STRING "GPU architecture (auto, native, specific)")
set_property(CACHE GPU_ARCH PROPERTY STRINGS "auto" "native" "sm_50" "sm_60" "sm_70" "sm_75" "sm_80" "sm_86" "sm_89")

# Performance options
option(ENABLE_GPU_PROFILING "Enable GPU profiling support" OFF)
option(ENABLE_GPU_DEBUG "Enable GPU debugging symbols" OFF)
option(ENABLE_UNIFIED_MEMORY "Enable CUDA Unified Memory" OFF)

# Output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Print configuration
message(STATUS "=== GPU Configuration ===")
message(STATUS "CUDA:      ${ENABLE_CUDA}")
message(STATUS "OpenCL:    ${ENABLE_OPENCL}")
message(STATUS "SYCL:      ${ENABLE_SYCL}")
message(STATUS "HIP:       ${ENABLE_HIP}")
message(STATUS "OpenMP:    ${ENABLE_OPENMP}")
message(STATUS "GPU Arch:  ${GPU_ARCH}")
message(STATUS "========================")

# Detect available GPU toolkits
detect_gpu_toolkits()

# Add source directories based on available toolkits
add_subdirectory(src/host)
add_subdirectory(src/common)

if(CUDA_FOUND AND ENABLE_CUDA)
    add_subdirectory(src/cuda)
    add_subdirectory(kernels/cuda)
endif()

if(OPENCL_FOUND AND ENABLE_OPENCL)
    add_subdirectory(src/opencl)
    add_subdirectory(kernels/opencl)
endif()

if(SYCL_FOUND AND ENABLE_SYCL)
    add_subdirectory(src/sycl)
    add_subdirectory(kernels/sycl)
endif()

if(ENABLE_OPENMP)
    add_subdirectory(src/openmp)
endif()

# Add benchmarks
add_subdirectory(benchmarks)

# Create a unified GPU application
if(CUDA_FOUND OR OPENCL_FOUND OR SYCL_FOUND)
    add_subdirectory(apps)
endif()

# Generate GPU capability report
generate_gpu_report()
```

### **GPU Toolkit Detection Module**

**cmake/GPUToolkit.cmake:**
```cmake
# GPUToolkit.cmake - GPU toolkit detection and configuration

# ==================== CUDA DETECTION ====================

function(detect_cuda)
    if(NOT ENABLE_CUDA)
        set(CUDA_FOUND FALSE PARENT_SCOPE)
        return()
    endif()
    
    # Find CUDA using CMake's built-in support
    find_package(CUDA QUIET)
    
    if(CUDA_FOUND)
        message(STATUS "CUDA found: ${CUDA_VERSION}")
        message(STATUS "  Toolkit: ${CUDA_TOOLKIT_ROOT_DIR}")
        message(STATUS "  NVIDIA GPU: ${CUDA_NVCC_EXECUTABLE}")
        
        # Detect available GPU architectures
        execute_process(
            COMMAND ${CUDA_NVCC_EXECUTABLE} --list-gpu-arch
            OUTPUT_VARIABLE cuda_arch_list
            OUTPUT_STRIP_TRAILING_WHITESPACE
        )
        
        if(cuda_arch_list)
            message(STATUS "  Available architectures: ${cuda_arch_list}")
        endif()
        
        # Set CUDA language standard
        set(CMAKE_CUDA_STANDARD 17)
        set(CMAKE_CUDA_STANDARD_REQUIRED ON)
        
        # Configure CUDA architecture
        configure_cuda_architecture()
        
        # Set CUDA flags
        set_cuda_flags()
        
    else()
        message(STATUS "CUDA not found")
    endif()
    
    set(CUDA_FOUND ${CUDA_FOUND} PARENT_SCOPE)
endfunction()

function(configure_cuda_architecture)
    if(GPU_ARCH STREQUAL "auto")
        # Auto-detect best architecture
        if(CUDA_VERSION VERSION_GREATER_EQUAL 11.0)
            set(CUDA_ARCH_FLAGS "-gencode=arch=compute_52,code=sm_52")
            if(CUDA_VERSION VERSION_GREATER_EQUAL 11.8)
                set(CUDA_ARCH_FLAGS "${CUDA_ARCH_FLAGS} -gencode=arch=compute_89,code=sm_89")
            elseif(CUDA_VERSION VERSION_GREATER_EQUAL 11.0)
                set(CUDA_ARCH_FLAGS "${CUDA_ARCH_FLAGS} -gencode=arch=compute_80,code=sm_80")
            endif()
        else()
            set(CUDA_ARCH_FLAGS "-gencode=arch=compute_30,code=sm_30 -gencode=arch=compute_50,code=sm_50")
        endif()
        
    elseif(GPU_ARCH STREQUAL "native")
        # Generate code for the current GPU
        set(CUDA_ARCH_FLAGS "-arch=native")
        
    else()
        # User-specified architecture
        set(CUDA_ARCH_FLAGS "-gencode=arch=compute_${GPU_ARCH},code=sm_${GPU_ARCH}")
    endif()
    
    set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} ${CUDA_ARCH_FLAGS}" PARENT_SCOPE)
    message(STATUS "CUDA architecture flags: ${CUDA_ARCH_FLAGS}")
endfunction()

function(set_cuda_flags)
    # Common NVCC flags
    set(cuda_flags
        "--compiler-options" "-Wall,-Wextra"
        "--expt-extended-lambda"
        "--expt-relaxed-constexpr"
        "--use_fast_math"
        "-Xcompiler" "-fPIC"
    )
    
    # Debug flags
    if(ENABLE_GPU_DEBUG)
        list(APPEND cuda_flags "-G" "-lineinfo")
    else()
        list(APPEND cuda_flags "-O3")
    endif()
    
    # Unified memory
    if(ENABLE_UNIFIED_MEMORY)
        list(APPEND cuda_flags "-dc")
    endif()
    
    # C++17 features
    if(CUDA_VERSION VERSION_GREATER_EQUAL 11.0)
        list(APPEND cuda_flags "--std=c++17")
    endif()
    
    # Set the flags
    set(CMAKE_CUDA_FLAGS "${cuda_flags}" PARENT_SCOPE)
    
    # Set separate flags for device linking if needed
    if(ENABLE_UNIFIED_MEMORY)
        set(CMAKE_CUDA_LINK_FLAGS "-dlink" PARENT_SCOPE)
    endif()
endfunction()

# ==================== OPENCL DETECTION ====================

function(detect_opencl)
    if(NOT ENABLE_OPENCL)
        set(OPENCL_FOUND FALSE PARENT_SCOPE)
        return()
    endif()
    
    # Try to find OpenCL
    find_package(OpenCL QUIET)
    
    if(OpenCL_FOUND)
        message(STATUS "OpenCL found")
        message(STATUS "  Include: ${OpenCL_INCLUDE_DIRS}")
        message(STATUS "  Library: ${OpenCL_LIBRARIES}")
        
        # Detect platforms and devices
        include(CheckOpenCLPlatforms)
        
    else()
        # Try alternative detection
        find_path(OPENCL_INCLUDE_DIR
            NAMES CL/cl.h OpenCL/opencl.h
            PATHS
                /usr/include
                /usr/local/include
                /opt/local/include
                /usr/include/nvidia-current
                /usr/local/cuda/include
        )
        
        find_library(OPENCL_LIBRARY
            NAMES OpenCL
            PATHS
                /usr/lib
                /usr/local/lib
                /opt/local/lib
                /usr/lib/nvidia-current
                /usr/local/cuda/lib64
        )
        
        if(OPENCL_INCLUDE_DIR AND OPENCL_LIBRARY)
            set(OpenCL_FOUND TRUE)
            set(OpenCL_INCLUDE_DIRS ${OPENCL_INCLUDE_DIR})
            set(OpenCL_LIBRARIES ${OPENCL_LIBRARY})
            message(STATUS "OpenCL found (manual detection)")
        else()
            message(STATUS "OpenCL not found")
        endif()
    endif()
    
    set(OPENCL_FOUND ${OpenCL_FOUND} PARENT_SCOPE)
    set(OPENCL_INCLUDE_DIRS ${OpenCL_INCLUDE_DIRS} PARENT_SCOPE)
    set(OPENCL_LIBRARIES ${OpenCL_LIBRARIES} PARENT_SCOPE)
endfunction()

# ==================== SYCL DETECTION ====================

function(detect_sycl)
    if(NOT ENABLE_SYCL)
        set(SYCL_FOUND FALSE PARENT_SCOPE)
        return()
    endif()
    
    # Try to find SYCL (Intel oneAPI DPC++)
    find_package(SYCL QUIET)
    
    if(SYCL_FOUND)
        message(STATUS "SYCL found: ${SYCL_VERSION}")
        
    else()
        # Try to find Intel oneAPI DPC++ compiler
        find_program(DPCPP_COMPILER
            NAMES dpcpp icpx clang++
            PATHS
                /opt/intel/oneapi/compiler/latest/linux/bin
                /usr/bin
        )
        
        if(DPCPP_COMPILER)
            set(SYCL_FOUND TRUE)
            set(SYCL_COMPILER ${DPCPP_COMPILER})
            
            # Find SYCL includes
            find_path(SYCL_INCLUDE_DIR
                NAMES sycl/sycl.hpp
                PATHS
                    /opt/intel/oneapi/compiler/latest/linux/include
                    /usr/include
            )
            
            message(STATUS "SYCL found via DPC++: ${DPCPP_COMPILER}")
        else()
            message(STATUS "SYCL not found")
        endif()
    endif()
    
    set(SYCL_FOUND ${SYCL_FOUND} PARENT_SCOPE)
    set(SYCL_COMPILER ${SYCL_COMPILER} PARENT_SCOPE)
    set(SYCL_INCLUDE_DIR ${SYCL_INCLUDE_DIR} PARENT_SCOPE)
endfunction()

# ==================== HIP DETECTION ====================

function(detect_hip)
    if(NOT ENABLE_HIP)
        set(HIP_FOUND FALSE PARENT_SCOPE)
        return()
    endif()
    
    # Try to find HIP (AMD's CUDA counterpart)
    find_package(HIP QUIET)
    
    if(HIP_FOUND)
        message(STATUS "HIP found: ${HIP_VERSION}")
        
        # Configure HIP architecture
        if(GPU_ARCH STREQUAL "auto")
            set(HIP_ARCH "gfx900,gfx906,gfx908,gfx90a" CACHE STRING "HIP architectures")
        else()
            set(HIP_ARCH ${GPU_ARCH})
        endif()
        
        set(HIP_HCC_FLAGS
            "--amdgpu-target=${HIP_ARCH}"
            "-fno-gpu-rdc"
            "-Wno-unused-command-line-argument"
        )
        
    else()
        message(STATUS "HIP not found")
    endif()
    
    set(HIP_FOUND ${HIP_FOUND} PARENT_SCOPE)
endfunction()

# ==================== OPENMP OFFLOAD DETECTION ====================

function(detect_openmp_offload)
    if(NOT ENABLE_OPENMP)
        set(OPENMP_OFFLOAD_FOUND FALSE PARENT_SCOPE)
        return()
    endif()
    
    # Check for OpenMP offload support
    include(CheckOpenMPOffload)
    
    if(OPENMP_OFFLOAD_FOUND)
        message(STATUS "OpenMP offload found")
        
        # Set OpenMP target flags
        if(GPU_ARCH STREQUAL "auto")
            set(OPENMP_TARGET_FLAGS "-fopenmp-targets=nvptx64-nvidia-cuda")
        else()
            set(OPENMP_TARGET_FLAGS "-fopenmp-targets=nvptx64-nvidia-cuda -Xopenmp-target -march=${GPU_ARCH}")
        endif()
        
    else()
        message(STATUS "OpenMP offload not found")
    endif()
    
    set(OPENMP_OFFLOAD_FOUND ${OPENMP_OFFLOAD_FOUND} PARENT_SCOPE)
    set(OPENMP_TARGET_FLAGS ${OPENMP_TARGET_FLAGS} PARENT_SCOPE)
endfunction()

# ==================== UNIFIED GPU INTERFACE ====================

# Create a unified GPU target that works with multiple backends
function(add_gpu_library target)
    set(options STATIC SHARED)
    set(one_value_args)
    set(multi_value_args SOURCES HEADERS)
    
    cmake_parse_arguments(ARG "${options}" "${one_value_args}" "${multi_value_args}" ${ARGN})
    
    # Determine library type
    if(ARG_STATIC)
        set(lib_type STATIC)
    elseif(ARG_SHARED)
        set(lib_type SHARED)
    else()
        set(lib_type STATIC)
    endif()
    
    # Create the library
    add_library(${target} ${lib_type})
    
    # Add sources
    if(ARG_SOURCES)
        target_sources(${target} PRIVATE ${ARG_SOURCES})
    endif()
    
    # Add headers
    if(ARG_HEADERS)
        target_sources(${target} PUBLIC FILE_SET HEADERS FILES ${ARG_HEADERS})
    endif()
    
    # Set compile features
    target_compile_features(${target} PUBLIC cxx_std_17)
    
    # Add GPU backends based on availability
    if(CUDA_FOUND)
        target_compile_definitions(${target} PUBLIC HAVE_CUDA=1)
        if(ARG_SOURCES AND ARG_SOURCES MATCHES "\\.cu$")
            set_source_files_properties(${ARG_SOURCES} PROPERTIES LANGUAGE CUDA)
        endif()
    endif()
    
    if(OPENCL_FOUND)
        target_compile_definitions(${target} PUBLIC HAVE_OPENCL=1)
        target_include_directories(${target} PUBLIC ${OPENCL_INCLUDE_DIRS})
        target_link_libraries(${target} PUBLIC ${OPENCL_LIBRARIES})
    endif()
    
    if(SYCL_FOUND)
        target_compile_definitions(${target} PUBLIC HAVE_SYCL=1)
        if(SYCL_INCLUDE_DIR)
            target_include_directories(${target} PUBLIC ${SYCL_INCLUDE_DIR})
        endif()
    endif()
    
    message(STATUS "Created GPU library: ${target}")
endfunction()

# ==================== GPU KERNEL COMPILATION ====================

# Compile CUDA kernels to PTX or binary
function(compile_cuda_kernel kernel_file output_format)
    get_filename_component(kernel_name ${kernel_file} NAME_WE)
    
    if(output_format STREQUAL "ptx")
        set(output_file "${CMAKE_CURRENT_BINARY_DIR}/${kernel_name}.ptx")
        set(compile_flags "-ptx")
    elseif(output_format STREQUAL "cubin")
        set(output_file "${CMAKE_CURRENT_BINARY_DIR}/${kernel_name}.cubin")
        set(compile_flags "-cubin")
    else()
        message(FATAL_ERROR "Unknown output format: ${output_format}")
    endif()
    
    add_custom_command(
        OUTPUT ${output_file}
        COMMAND ${CUDA_NVCC_EXECUTABLE}
            ${CMAKE_CUDA_FLAGS}
            ${compile_flags}
            ${CUDA_NVCC_FLAGS}
            -o ${output_file}
            ${kernel_file}
        DEPENDS ${kernel_file}
        COMMENT "Compiling CUDA kernel ${kernel_file} to ${output_format}"
    )
    
    add_custom_target(${kernel_name}_${output_format}
        DEPENDS ${output_file}
    )
    
    message(STATUS "  CUDA kernel target: ${kernel_name}_${output_format}")
endfunction()

# Compile OpenCL kernels
function(compile_opencl_kernel kernel_file)
    get_filename_component(kernel_name ${kernel_file} NAME_WE)
    set(output_file "${CMAKE_CURRENT_BINARY_DIR}/${kernel_name}.cl.h")
    
    # Convert OpenCL kernel to C header
    add_custom_command(
        OUTPUT ${output_file}
        COMMAND ${CMAKE_COMMAND} -DINPUT_FILE=${kernel_file}
                   -DOUTPUT_FILE=${output_file}
                   -P ${CMAKE_CURRENT_SOURCE_DIR}/cmake/OpenCLToHeader.cmake
        DEPENDS ${kernel_file}
        COMMENT "Converting OpenCL kernel ${kernel_file} to C header"
    )
    
    add_custom_target(${kernel_name}_opencl
        DEPENDS ${output_file}
    )
    
    message(STATUS "  OpenCL kernel target: ${kernel_name}_opencl")
endfunction()

# ==================== GPU PROFILING ====================

function(enable_gpu_profiling target)
    if(ENABLE_GPU_PROFILING)
        if(CUDA_FOUND)
            # NVIDIA Tools Extension (NVTX)
            find_package(NVTX QUIET)
            if(NVTX_FOUND)
                target_compile_definitions(${target} PUBLIC HAVE_NVTX=1)
                target_include_directories(${target} PUBLIC ${NVTX_INCLUDE_DIRS})
                target_link_libraries(${target} PUBLIC ${NVTX_LIBRARIES})
                message(STATUS "  NVTX profiling enabled for ${target}")
            endif()
            
            # CUDA Profiling Tools Interface (CUPTI)
            find_library(CUPTI_LIBRARY cupti
                PATHS ${CUDA_TOOLKIT_ROOT_DIR}/extras/CUPTI/lib64
            )
            if(CUPTI_LIBRARY)
                target_link_libraries(${target} PUBLIC ${CUPTI_LIBRARY})
                target_compile_definitions(${target} PUBLIC HAVE_CUPTI=1)
                message(STATUS "  CUPTI profiling enabled for ${target}")
            endif()
        endif()
        
        if(OPENCL_FOUND)
            # OpenCL profiling extensions
            target_compile_definitions(${target} PUBLIC CL_USE_DEPRECATED_OPENCL_1_2_APIS)
            target_compile_definitions(${target} PUBLIC CL_TARGET_OPENCL_VERSION=300)
            message(STATUS "  OpenCL profiling enabled for ${target}")
        endif()
    endif()
endfunction()

# ==================== GPU REPORT GENERATION ====================

function(generate_gpu_report)
    set(report_file "${CMAKE_BINARY_DIR}/gpu_report.txt")
    
    file(WRITE ${report_file} "GPU Capability Report\n")
    file(APPEND ${report_file} "=====================\n\n")
    file(APPEND ${report_file} "Generated: ${CMAKE_DATE} ${CMAKE_TIME}\n")
    file(APPEND ${report_file} "System: ${CMAKE_SYSTEM_NAME} ${CMAKE_SYSTEM_PROCESSOR}\n\n")
    
    if(CUDA_FOUND)
        file(APPEND ${report_file} "CUDA:\n")
        file(APPEND ${report_file} "  Version: ${CUDA_VERSION}\n")
        file(APPEND ${report_file} "  Toolkit: ${CUDA_TOOLKIT_ROOT_DIR}\n")
        file(APPEND ${report_file} "  NVCC: ${CUDA_NVCC_EXECUTABLE}\n")
        file(APPEND ${report_file} "  Architectures: ${CUDA_ARCH_FLAGS}\n\n")
    endif()
    
    if(OPENCL_FOUND)
        file(APPEND ${report_file} "OpenCL:\n")
        file(APPEND ${report_file} "  Include: ${OPENCL_INCLUDE_DIRS}\n")
        file(APPEND ${report_file} "  Library: ${OPENCL_LIBRARIES}\n\n")
    endif()
    
    if(SYCL_FOUND)
        file(APPEND ${report_file} "SYCL:\n")
        file(APPEND ${report_file} "  Compiler: ${SYCL_COMPILER}\n")
        if(SYCL_INCLUDE_DIR)
            file(APPEND ${report_file} "  Include: ${SYCL_INCLUDE_DIR}\n")
        endif()
        file(APPEND ${report_file} "\n")
    endif()
    
    if(HIP_FOUND)
        file(APPEND ${report_file} "HIP:\n")
        file(APPEND ${report_file} "  Version: ${HIP_VERSION}\n\n")
    endif()
    
    if(OPENMP_OFFLOAD_FOUND)
        file(APPEND ${report_file} "OpenMP Offload:\n")
        file(APPEND ${report_file} "  Supported: Yes\n")
        file(APPEND ${report_file} "  Flags: ${OPENMP_TARGET_FLAGS}\n\n")
    endif()
    
    message(STATUS "GPU report generated: ${report_file}")
    
    # Create a target to view the report
    add_custom_target(gpu-report
        COMMAND ${CMAKE_COMMAND} -E cat ${report_file}
        COMMENT "Displaying GPU capability report"
    )
endfunction()

# ==================== MAIN DETECTION FUNCTION ====================

function(detect_gpu_toolkits)
    message(STATUS "Detecting GPU toolkits...")
    
    detect_cuda()
    detect_opencl()
    detect_sycl()
    detect_hip()
    detect_openmp_offload()
    
    # Set global variables
    set(CUDA_FOUND ${CUDA_FOUND} PARENT_SCOPE)
    set(OPENCL_FOUND ${OPENCL_FOUND} PARENT_SCOPE)
    set(SYCL_FOUND ${SYCL_FOUND} PARENT_SCOPE)
    set(HIP_FOUND ${HIP_FOUND} PARENT_SCOPE)
    set(OPENMP_OFFLOAD_FOUND ${OPENMP_OFFLOAD_FOUND} PARENT_SCOPE)
    
    # Set include and library variables
    set(OPENCL_INCLUDE_DIRS ${OPENCL_INCLUDE_DIRS} PARENT_SCOPE)
    set(OPENCL_LIBRARIES ${OPENCL_LIBRARIES} PARENT_SCOPE)
    set(SYCL_COMPILER ${SYCL_COMPILER} PARENT_SCOPE)
    set(SYCL_INCLUDE_DIR ${SYCL_INCLUDE_DIR} PARENT_SCOPE)
    
    # Set CUDA variables
    if(CUDA_FOUND)
        set(CMAKE_CUDA_FLAGS ${CMAKE_CUDA_FLAGS} PARENT_SCOPE)
        set(CMAKE_CUDA_LINK_FLAGS ${CMAKE_CUDA_LINK_FLAGS} PARENT_SCOPE)
        set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS} PARENT_SCOPE)
    endif()
    
    # Set OpenMP variables
    if(OPENMP_OFFLOAD_FOUND)
        set(OPENMP_TARGET_FLAGS ${OPENMP_TARGET_FLAGS} PARENT_SCOPE)
    endif()
endfunction()

# End of GPUToolkit.cmake
```

### **CUDA Implementation**

**src/cuda/CMakeLists.txt:**
```cmake
# CUDA-specific implementation

if(NOT CUDA_FOUND)
    message(FATAL_ERROR "CUDA not found but CUDA directory included")
endif()

message(STATUS "Configuring CUDA components...")

# CUDA library with mixed CUDA/C++ code
add_library(gpu_cuda STATIC
    cuda_utils.cpp
    cuda_kernels.cu
    cuda_manager.cpp
)

# Set CUDA properties
set_target_properties(gpu_cuda PROPERTIES
    CUDA_SEPARABLE_COMPILATION ON
    CUDA_RESOLVE_DEVICE_SYMBOLS ON
)

# Set language properties for CUDA files
set_source_files_properties(
    cuda_kernels.cu
    PROPERTIES
        LANGUAGE CUDA
        CUDA_ARCHITECTURES "${GPU_ARCH}"
)

# Include directories
target_include_directories(gpu_cuda PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../common
    ${CUDA_INCLUDE_DIRS}
)

# Link CUDA libraries
target_link_libraries(gpu_cuda PUBLIC
    ${CUDA_LIBRARIES}
    ${CUDA_CUBLAS_LIBRARIES}
    ${CUDA_CURAND_LIBRARIES}
)

# Enable profiling if requested
enable_gpu_profiling(gpu_cuda)

# Add compile definitions
target_compile_definitions(gpu_cuda PUBLIC
    USE_CUDA=1
    CUDA_NO_HOST_DEVICE_MIXED_ERRORS
)

# Create CUDA executable example
add_executable(cuda_example
    cuda_example.cpp
)

target_link_libraries(cuda_example PRIVATE gpu_cuda)

# Enable profiling for example
enable_gpu_profiling(cuda_example)

# Add test for CUDA
if(BUILD_TESTING)
    add_executable(test_cuda
        test_cuda.cpp
    )
    
    target_link_libraries(test_cuda PRIVATE gpu_cuda)
    
    add_test(NAME test_cuda
        COMMAND test_cuda
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
endif()

message(STATUS "CUDA components configured")
```

**src/cuda/cuda_kernels.cu:**
```cpp
// cuda_kernels.cu - CUDA kernel implementations
#include <cuda_runtime.h>
#include <device_launch_parameters.h>
#include <cuda_fp16.h>
#include <cub/cub.cuh>

#include "cuda_utils.h"
#include "../common/vector_math.h"

#ifdef HAVE_NVTX
#include <nvtx3/nvToolsExt.h>
#endif

// Simple vector addition kernel
__global__ void vector_add_kernel(const float* a, const float* b, float* c, int n) {
    int idx = blockIdx.x * blockDim.x + threadIdx.x;
    if (idx < n) {
        c[idx] = a[idx] + b[idx];
    }
}

// Optimized vector addition with shared memory
__global__ void vector_add_optimized_kernel(const float* a, const float* b, float* c, int n) {
    extern __shared__ float shared_mem[];
    
    int tid = threadIdx.x;
    int idx = blockIdx.x * blockDim.x + tid;
    
    // Load data to shared memory
    if (idx < n) {
        shared_mem[tid] = a[idx] + b[idx];
    }
    
    __syncthreads();
    
    // Store result
    if (idx < n) {
        c[idx] = shared_mem[tid];
    }
}

// Matrix multiplication kernel (naive)
__global__ void matmul_naive_kernel(const float* A, const float* B, float* C,
                                   int M, int N, int K) {
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    int col = blockIdx.x * blockDim.x + threadIdx.x;
    
    if (row < M && col < N) {
        float sum = 0.0f;
        for (int k = 0; k < K; ++k) {
            sum += A[row * K + k] * B[k * N + col];
        }
        C[row * N + col] = sum;
    }
}

// Matrix multiplication with shared memory (tiled)
__global__ void matmul_tiled_kernel(const float* A, const float* B, float* C,
                                   int M, int N, int K) {
    // Tile size
    const int TILE_SIZE = 32;
    
    __shared__ float As[TILE_SIZE][TILE_SIZE];
    __shared__ float Bs[TILE_SIZE][TILE_SIZE];
    
    int bx = blockIdx.x;
    int by = blockIdx.y;
    int tx = threadIdx.x;
    int ty = threadIdx.y;
    
    int row = by * TILE_SIZE + ty;
    int col = bx * TILE_SIZE + tx;
    
    float sum = 0.0f;
    
    for (int t = 0; t < (K + TILE_SIZE - 1) / TILE_SIZE; ++t) {
        // Load tiles to shared memory
        if (row < M && t * TILE_SIZE + tx < K) {
            As[ty][tx] = A[row * K + t * TILE_SIZE + tx];
        } else {
            As[ty][tx] = 0.0f;
        }
        
        if (t * TILE_SIZE + ty < K && col < N) {
            Bs[ty][tx] = B[(t * TILE_SIZE + ty) * N + col];
        } else {
            Bs[ty][tx] = 0.0f;
        }
        
        __syncthreads();
        
        // Compute partial product
        for (int k = 0; k < TILE_SIZE; ++k) {
            sum += As[ty][k] * Bs[k][tx];
        }
        
        __syncthreads();
    }
    
    if (row < M && col < N) {
        C[row * N + col] = sum;
    }
}

// Reduction kernel (sum)
template<typename T>
__global__ void reduce_sum_kernel(const T* input, T* output, int n) {
    extern __shared__ T shared_data[];
    
    int tid = threadIdx.x;
    int idx = blockIdx.x * blockDim.x + tid;
    
    // Load data
    shared_data[tid] = (idx < n) ? input[idx] : 0;
    __syncthreads();
    
    // Reduction in shared memory
    for (int s = blockDim.x / 2; s > 0; s >>= 1) {
        if (tid < s) {
            shared_data[tid] += shared_data[tid + s];
        }
        __syncthreads();
    }
    
    // Write result for this block
    if (tid == 0) {
        output[blockIdx.x] = shared_data[0];
    }
}

// Scan (prefix sum) kernel
template<typename T>
__global__ void scan_kernel(const T* input, T* output, int n) {
    extern __shared__ T temp[];
    
    int tid = threadIdx.x;
    int idx = blockIdx.x * blockDim.x * 2 + tid;
    
    // Load input
    temp[tid] = (idx < n) ? input[idx] : 0;
    if (idx + blockDim.x < n) {
        temp[tid + blockDim.x] = input[idx + blockDim.x];
    }
    
    __syncthreads();
    
    // Reduction phase
    for (int d = 1; d <= blockDim.x; d <<= 1) {
        int index = (tid + 1) * d * 2 - 1;
        if (index < 2 * blockDim.x) {
            temp[index] += temp[index - d];
        }
        __syncthreads();
    }
    
    // Post-scan phase
    for (int d = blockDim.x / 2; d > 0; d >>= 1) {
        int index = (tid + 1) * d * 2 - 1;
        if (index + d < 2 * blockDim.x) {
            temp[index + d] += temp[index];
        }
        __syncthreads();
    }
    
    // Write output
    if (idx < n) {
        output[idx] = temp[tid];
    }
    if (idx + blockDim.x < n) {
        output[idx + blockDim.x] = temp[tid + blockDim.x];
    }
}

// Convolution kernel
__global__ void convolution_kernel(const float* input, const float* kernel,
                                  float* output, int width, int height,
                                  int kernel_size, int padding) {
    int x = blockIdx.x * blockDim.x + threadIdx.x;
    int y = blockIdx.y * blockDim.y + threadIdx.y;
    
    if (x >= width || y >= height) return;
    
    int half_kernel = kernel_size / 2;
    float sum = 0.0f;
    
    for (int ky = -half_kernel; ky <= half_kernel; ++ky) {
        for (int kx = -half_kernel; kx <= half_kernel; ++kx) {
            int ix = x + kx - padding;
            int iy = y + ky - padding;
            
            if (ix >= 0 && ix < width && iy >= 0 && iy < height) {
                float pixel = input[iy * width + ix];
                float weight = kernel[(ky + half_kernel) * kernel_size + (kx + half_kernel)];
                sum += pixel * weight;
            }
        }
    }
    
    output[y * width + x] = sum;
}

// FP16 matrix multiplication (Tensor Core compatible)
#if __CUDA_ARCH__ >= 700
__global__ void matmul_fp16_kernel(const __half* A, const __half* B, __half* C,
                                  int M, int N, int K) {
    // Using WMMA (Warp Matrix Multiply Accumulate) API for Tensor Cores
    const int WMMA_M = 16;
    const int WMMA_N = 16;
    const int WMMA_K = 16;
    
    // Declare fragments
    nvcuda::wmma::fragment<nvcuda::wmma::matrix_a, WMMA_M, WMMA_N, WMMA_K, __half, 
                          nvcuda::wmma::row_major> a_frag;
    nvcuda::wmma::fragment<nvcuda::wmma::matrix_b, WMMA_M, WMMA_N, WMMA_K, __half,
                          nvcuda::wmma::col_major> b_frag;
    nvcuda::wmma::fragment<nvcuda::wmma::accumulator, WMMA_M, WMMA_N, WMMA_K, __half> c_frag;
    
    // Initialize accumulator to zero
    nvcuda::wmma::fill_fragment(c_frag, __float2half(0.0f));
    
    // Loop over K dimension
    for (int k = 0; k < K; k += WMMA_K) {
        // Load A and B fragments
        nvcuda::wmma::load_matrix_sync(a_frag, A + blockIdx.x * WMMA_M * K + k, K);
        nvcuda::wmma::load_matrix_sync(b_frag, B + k * N + blockIdx.y * WMMA_N, N);
        
        // Perform matrix multiplication
        nvcuda::wmma::mma_sync(c_frag, a_frag, b_frag, c_frag);
    }
    
    // Store result
    nvcuda::wmma::store_matrix_sync(C + blockIdx.x * WMMA_M * N + blockIdx.y * WMMA_N,
                                    c_frag, N, nvcuda::wmma::mem_row_major);
}
#endif

// C++ wrapper functions
extern "C" {

void cuda_vector_add(const float* a, const float* b, float* c, int n) {
#ifdef HAVE_NVTX
    nvtxRangePushA("cuda_vector_add");
#endif
    
    dim3 block_size(256);
    dim3 grid_size((n + block_size.x - 1) / block_size.x);
    
    vector_add_kernel<<<grid_size, block_size>>>(a, b, c, n);
    
    cudaError_t err = cudaGetLastError();
    if (err != cudaSuccess) {
        fprintf(stderr, "CUDA error: %s\n", cudaGetErrorString(err));
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
}

void cuda_matmul(const float* A, const float* B, float* C,
                int M, int N, int K) {
#ifdef HAVE_NVTX
    nvtxRangePushA("cuda_matmul");
#endif
    
    dim3 block_size(16, 16);
    dim3 grid_size((N + block_size.x - 1) / block_size.x,
                   (M + block_size.y - 1) / block_size.y);
    
    matmul_tiled_kernel<<<grid_size, block_size>>>(A, B, C, M, N, K);
    
    cudaError_t err = cudaGetLastError();
    if (err != cudaSuccess) {
        fprintf(stderr, "CUDA error: %s\n", cudaGetErrorString(err));
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
}

void cuda_reduce_sum(const float* input, float* output, int n) {
#ifdef HAVE_NVTX
    nvtxRangePushA("cuda_reduce_sum");
#endif
    
    const int block_size = 256;
    const int grid_size = (n + block_size - 1) / block_size;
    const int shared_mem_size = block_size * sizeof(float);
    
    // Temporary storage for block results
    float* block_results;
    cudaMalloc(&block_results, grid_size * sizeof(float));
    
    // First reduction
    reduce_sum_kernel<<<grid_size, block_size, shared_mem_size>>>(input, block_results, n);
    
    // Final reduction if needed
    if (grid_size > 1) {
        reduce_sum_kernel<<<1, block_size, shared_mem_size>>>(block_results, output, grid_size);
    } else {
        cudaMemcpy(output, block_results, sizeof(float), cudaMemcpyDeviceToDevice);
    }
    
    cudaFree(block_results);
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
}

} // extern "C"
```

**src/cuda/cuda_manager.cpp:**
```cpp
// cuda_manager.cpp - CUDA device and memory management
#include "cuda_manager.h"
#include <iostream>
#include <stdexcept>
#include <vector>

#ifdef HAVE_NVTX
#include <nvtx3/nvToolsExt.h>
#endif

CUDAManager::CUDAManager() : device_id(0), stream(nullptr) {
    initialize();
}

CUDAManager::~CUDAManager() {
    cleanup();
}

void CUDAManager::initialize() {
#ifdef HAVE_NVTX
    nvtxRangePushA("CUDAManager::initialize");
#endif
    
    // Get number of CUDA devices
    int device_count = 0;
    cudaError_t err = cudaGetDeviceCount(&device_count);
    
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to get CUDA device count: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
    if (device_count == 0) {
        throw std::runtime_error("No CUDA-capable devices found");
    }
    
    std::cout << "Found " << device_count << " CUDA device(s):\n";
    
    // Print device information
    for (int i = 0; i < device_count; ++i) {
        cudaDeviceProp props;
        cudaGetDeviceProperties(&props, i);
        
        std::cout << "  Device " << i << ": " << props.name << "\n";
        std::cout << "    Compute Capability: " << props.major << "." << props.minor << "\n";
        std::cout << "    Global Memory: " << props.totalGlobalMem / (1024 * 1024) << " MB\n";
        std::cout << "    CUDA Cores: " << props.multiProcessorCount * 
                    _ConvertSMVer2Cores(props.major, props.minor) << "\n";
        std::cout << "    Max Threads per Block: " << props.maxThreadsPerBlock << "\n";
        
        if (i == 0) {
            // Select the first device by default
            device_id = i;
            device_props = props;
        }
    }
    
    // Set device
    err = cudaSetDevice(device_id);
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to set CUDA device: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
    // Create default stream
    err = cudaStreamCreate(&stream);
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to create CUDA stream: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
    // Enable peer access if multiple devices
    if (device_count > 1) {
        for (int i = 0; i < device_count; ++i) {
            if (i != device_id) {
                int can_access = 0;
                cudaDeviceCanAccessPeer(&can_access, device_id, i);
                if (can_access) {
                    cudaDeviceEnablePeerAccess(i, 0);
                    std::cout << "  Enabled peer access from device " << device_id 
                              << " to device " << i << "\n";
                }
            }
        }
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
    
    std::cout << "CUDA initialized successfully on device " << device_id << "\n";
}

void CUDAManager::cleanup() {
    if (stream) {
        cudaStreamDestroy(stream);
        stream = nullptr;
    }
    
    // Synchronize and reset device
    cudaDeviceSynchronize();
    cudaDeviceReset();
}

void* CUDAManager::allocate_device(size_t size) {
#ifdef HAVE_NVTX
    nvtxRangePushA("CUDAManager::allocate_device");
#endif
    
    void* ptr = nullptr;
    cudaError_t err = cudaMalloc(&ptr, size);
    
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to allocate device memory: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
    
    return ptr;
}

void* CUDAManager::allocate_host(size_t size, bool pinned) {
#ifdef HAVE_NVTX
    nvtxRangePushA("CUDAManager::allocate_host");
#endif
    
    void* ptr = nullptr;
    cudaError_t err;
    
    if (pinned) {
        err = cudaMallocHost(&ptr, size);
    } else {
        ptr = malloc(size);
        err = (ptr != nullptr) ? cudaSuccess : cudaErrorMemoryAllocation;
    }
    
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to allocate host memory: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
    
    return ptr;
}

void CUDAManager::copy_to_device(void* dst, const void* src, size_t size) {
#ifdef HAVE_NVTX
    nvtxRangePushA("CUDAManager::copy_to_device");
#endif
    
    cudaError_t err = cudaMemcpy(dst, src, size, cudaMemcpyHostToDevice);
    
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to copy to device: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
}

void CUDAManager::copy_from_device(void* dst, const void* src, size_t size) {
#ifdef HAVE_NVTX
    nvtxRangePushA("CUDAManager::copy_from_device");
#endif
    
    cudaError_t err = cudaMemcpy(dst, src, size, cudaMemcpyDeviceToHost);
    
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to copy from device: " + 
                                std::string(cudaGetErrorString(err)));
    }
    
#ifdef HAVE_NVTX
    nvtxRangePop();
#endif
}

void CUDAManager::synchronize() {
    cudaError_t err = cudaStreamSynchronize(stream);
    
    if (err != cudaSuccess) {
        throw std::runtime_error("Failed to synchronize stream: " + 
                                std::string(cudaGetErrorString(err)));
    }
}

int CUDAManager::_ConvertSMVer2Cores(int major, int minor) {
    // Defines for GPU Architecture types (using more than one per generation)
    typedef struct {
        int SM;  // 0xMm (hex), M = SM Major version, m = SM minor version
        int Cores;
    } sSMtoCores;

    sSMtoCores nGpuArchCoresPerSM[] = {
        {0x30, 192},
        {0x32, 192},
        {0x35, 192},
        {0x37, 192},
        {0x50, 128},
        {0x52, 128},
        {0x53, 128},
        {0x60,  64},
        {0x61, 128},
        {0x62, 128},
        {0x70,  64},
        {0x72,  64},
        {0x75,  64},
        {0x80,  64},
        {0x86, 128},
        {0x87, 128},
        {0x89, 128},
        {0x90, 128},
        {-1, -1}
    };

    int index = 0;
    while (nGpuArchCoresPerSM[index].SM != -1) {
        if (nGpuArchCoresPerSM[index].SM == ((major << 4) + minor)) {
            return nGpuArchCoresPerSM[index].Cores;
        }
        index++;
    }

    // If we don't find the values, default to a reasonable value
    return 128;
}
```

### **OpenCL Implementation**

**src/opencl/CMakeLists.txt:**
```cmake
# OpenCL-specific implementation

if(NOT OPENCL_FOUND)
    message(FATAL_ERROR "OpenCL not found but OpenCL directory included")
endif()

message(STATUS "Configuring OpenCL components...")

# OpenCL library
add_library(gpu_opencl STATIC
    opencl_manager.cpp
    opencl_kernels.cpp
)

# Include directories
target_include_directories(gpu_opencl PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../common
    ${OPENCL_INCLUDE_DIRS}
)

# Link OpenCL library
target_link_libraries(gpu_opencl PUBLIC ${OPENCL_LIBRARIES})

# Add compile definitions
target_compile_definitions(gpu_opencl PUBLIC
    USE_OPENCL=1
    CL_TARGET_OPENCL_VERSION=300
)

# Enable profiling if requested
enable_gpu_profiling(gpu_opencl)

# Create OpenCL executable example
add_executable(opencl_example
    opencl_example.cpp
)

target_link_libraries(opencl_example PRIVATE gpu_opencl)

# Enable profiling for example
enable_gpu_profiling(opencl_example)

# Add test for OpenCL
if(BUILD_TESTING)
    add_executable(test_opencl
        test_opencl.cpp
    )
    
    target_link_libraries(test_opencl PRIVATE gpu_opencl)
    
    add_test(NAME test_opencl
        COMMAND test_opencl
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
endif()

message(STATUS "OpenCL components configured")
```

**kernels/opencl/vector_add.cl:**
```opencl
// vector_add.cl - OpenCL kernel for vector addition

#ifdef cl_khr_fp64
    #pragma OPENCL EXTENSION cl_khr_fp64 : enable
#endif

// Simple vector addition
__kernel void vector_add(__global const float* a,
                         __global const float* b,
                         __global float* c,
                         const unsigned int n) {
    int idx = get_global_id(0);
    
    if (idx < n) {
        c[idx] = a[idx] + b[idx];
    }
}

// Vector addition with local memory
__kernel void vector_add_local(__global const float* a,
                              __global const float* b,
                              __global float* c,
                              const unsigned int n,
                              __local float* local_mem) {
    int global_id = get_global_id(0);
    int local_id = get_local_id(0);
    int group_id = get_group_id(0);
    int group_size = get_local_size(0);
    
    // Load data to local memory
    if (global_id < n) {
        local_mem[local_id] = a[global_id] + b[global_id];
    }
    
    barrier(CLK_LOCAL_MEM_FENCE);
    
    // Store result
    if (global_id < n) {
        c[global_id] = local_mem[local_id];
    }
}

// Matrix multiplication
__kernel void matmul_naive(__global const float* A,
                          __global const float* B,
                          __global float* C,
                          const unsigned int M,
                          const unsigned int N,
                          const unsigned int K) {
    int row = get_global_id(1);
    int col = get_global_id(0);
    
    if (row < M && col < N) {
        float sum = 0.0f;
        for (unsigned int k = 0; k < K; ++k) {
            sum += A[row * K + k] * B[k * N + col];
        }
        C[row * N + col] = sum;
    }
}

// Tiled matrix multiplication
__kernel void matmul_tiled(__global const float* A,
                          __global const float* B,
                          __global float* C,
                          const unsigned int M,
                          const unsigned int N,
                          const unsigned int K) {
    const int TILE_SIZE = 16;
    
    __local float As[TILE_SIZE][TILE_SIZE];
    __local float Bs[TILE_SIZE][TILE_SIZE];
    
    int bx = get_group_id(0);
    int by = get_group_id(1);
    int tx = get_local_id(0);
    int ty = get_local_id(1);
    
    int row = by * TILE_SIZE + ty;
    int col = bx * TILE_SIZE + tx;
    
    float sum = 0.0f;
    
    for (int t = 0; t < (K + TILE_SIZE - 1) / TILE_SIZE; ++t) {
        // Load tiles to local memory
        if (row < M && t * TILE_SIZE + tx < K) {
            As[ty][tx] = A[row * K + t * TILE_SIZE + tx];
        } else {
            As[ty][tx] = 0.0f;
        }
        
        if (t * TILE_SIZE + ty < K && col < N) {
            Bs[ty][tx] = B[(t * TILE_SIZE + ty) * N + col];
        } else {
            Bs[ty][tx] = 0.0f;
        }
        
        barrier(CLK_LOCAL_MEM_FENCE);
        
        // Compute partial product
        for (int k = 0; k < TILE_SIZE; ++k) {
            sum += As[ty][k] * Bs[k][tx];
        }
        
        barrier(CLK_LOCAL_MEM_FENCE);
    }
    
    if (row < M && col < N) {
        C[row * N + col] = sum;
    }
}

// Reduction (sum)
__kernel void reduce_sum(__global const float* input,
                        __global float* output,
                        __local float* local_mem,
                        const unsigned int n) {
    int global_id = get_global_id(0);
    int local_id = get_local_id(0);
    int group_id = get_group_id(0);
    int group_size = get_local_size(0);
    
    // Load data to local memory
    local_mem[local_id] = (global_id < n) ? input[global_id] : 0.0f;
    
    barrier(CLK_LOCAL_MEM_FENCE);
    
    // Reduction in local memory
    for (int s = group_size / 2; s > 0; s >>= 1) {
        if (local_id < s) {
            local_mem[local_id] += local_mem[local_id + s];
        }
        barrier(CLK_LOCAL_MEM_FENCE);
    }
    
    // Write result for this work group
    if (local_id == 0) {
        output[group_id] = local_mem[0];
    }
}

// Convolution
__kernel void convolution(__global const float* input,
                         __global const float* kernel,
                         __global float* output,
                         const unsigned int width,
                         const unsigned int height,
                         const unsigned int kernel_size,
                         const unsigned int padding) {
    int x = get_global_id(0);
    int y = get_global_id(1);
    
    if (x >= width || y >= height) return;
    
    int half_kernel = kernel_size / 2;
    float sum = 0.0f;
    
    for (int ky = -half_kernel; ky <= half_kernel; ++ky) {
        for (int kx = -half_kernel; kx <= half_kernel; ++kx) {
            int ix = x + kx - padding;
            int iy = y + ky - padding;
            
            if (ix >= 0 && ix < width && iy >= 0 && iy < height) {
                float pixel = input[iy * width + ix];
                float weight = kernel[(ky + half_kernel) * kernel_size + (kx + half_kernel)];
                sum += pixel * weight;
            }
        }
    }
    
    output[y * width + x] = sum;
}
```

### **SYCL Implementation (Intel oneAPI DPC++)**

**src/sycl/CMakeLists.txt:**
```cmake
# SYCL-specific implementation

if(NOT SYCL_FOUND)
    message(FATAL_ERROR "SYCL not found but SYCL directory included")
endif()

message(STATUS "Configuring SYCL components...")

# Set SYCL compiler if found
if(SYCL_COMPILER)
    set(CMAKE_CXX_COMPILER ${SYCL_COMPILER})
endif()

# SYCL library
add_library(gpu_sycl STATIC
    sycl_manager.cpp
    sycl_kernels.cpp
)

# Include directories
target_include_directories(gpu_sycl PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../common
)

if(SYCL_INCLUDE_DIR)
    target_include_directories(gpu_sycl PUBLIC ${SYCL_INCLUDE_DIR})
endif()

# SYCL requires C++17 or later
target_compile_features(gpu_sycl PUBLIC cxx_std_17)

# Add compile definitions
target_compile_definitions(gpu_sycl PUBLIC
    USE_SYCL=1
    SYCL_LANGUAGE_VERSION=2020
)

# Create SYCL executable example
add_executable(sycl_example
    sycl_example.cpp
)

target_link_libraries(sycl_example PRIVATE gpu_sycl)

# Enable profiling for example
enable_gpu_profiling(sycl_example)

# Add test for SYCL
if(BUILD_TESTING)
    add_executable(test_sycl
        test_sycl.cpp
    )
    
    target_link_libraries(test_sycl PRIVATE gpu_sycl)
    
    add_test(NAME test_sycl
        COMMAND test_sycl
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
endif()

message(STATUS "SYCL components configured")
```

**src/sycl/sycl_kernels.cpp:**
```cpp
// sycl_kernels.cpp - SYCL kernel implementations
#include <CL/sycl.hpp>
#include <iostream>
#include <vector>

namespace sycl = cl::sycl;

class VectorAddKernel;
class MatrixMulKernel;
class ReduceSumKernel;

// Vector addition
void sycl_vector_add(const float* a, const float* b, float* c, size_t n,
                    sycl::queue& q) {
    try {
        auto event = q.submit([&](sycl::handler& h) {
            h.parallel_for<VectorAddKernel>(
                sycl::range<1>{n},
                [=](sycl::id<1> idx) {
                    c[idx] = a[idx] + b[idx];
                }
            );
        });
        
        event.wait();
    } catch (const sycl::exception& e) {
        std::cerr << "SYCL exception: " << e.what() << std::endl;
        throw;
    }
}

// Matrix multiplication
void sycl_matmul(const float* A, const float* B, float* C,
                size_t M, size_t N, size_t K,
                sycl::queue& q) {
    try {
        auto event = q.submit([&](sycl::handler& h) {
            h.parallel_for<MatrixMulKernel>(
                sycl::range<2>{M, N},
                [=](sycl::id<2> idx) {
                    size_t i = idx[0];
                    size_t j = idx[1];
                    
                    float sum = 0.0f;
                    for (size_t k = 0; k < K; ++k) {
                        sum += A[i * K + k] * B[k * N + j];
                    }
                    
                    C[i * N + j] = sum;
                }
            );
        });
        
        event.wait();
    } catch (const sycl::exception& e) {
        std::cerr << "SYCL exception: " << e.what() << std::endl;
        throw;
    }
}

// Optimized matrix multiplication with local memory
void sycl_matmul_optimized(const float* A, const float* B, float* C,
                          size_t M, size_t N, size_t K,
                          sycl::queue& q) {
    constexpr size_t TILE_SIZE = 16;
    
    try {
        auto event = q.submit([&](sycl::handler& h) {
            sycl::accessor<float, 2, sycl::access::mode::read_write,
                          sycl::access::target::local>
                local_A(sycl::range<2>(TILE_SIZE, TILE_SIZE), h);
            
            sycl::accessor<float, 2, sycl::access::mode::read_write,
                          sycl::access::target::local>
                local_B(sycl::range<2>(TILE_SIZE, TILE_SIZE), h);
            
            h.parallel_for<MatrixMulKernel>(
                sycl::nd_range<2>(
                    sycl::range<2>(M, N),
                    sycl::range<2>(TILE_SIZE, TILE_SIZE)
                ),
                [=](sycl::nd_item<2> item) {
                    size_t local_i = item.get_local_id(0);
                    size_t local_j = item.get_local_id(1);
                    size_t global_i = item.get_global_id(0);
                    size_t global_j = item.get_global_id(1);
                    
                    size_t group_i = item.get_group(0);
                    size_t group_j = item.get_group(1);
                    
                    float sum = 0.0f;
                    
                    // Loop over tiles
                    for (size_t tile = 0; tile < K; tile += TILE_SIZE) {
                        // Load tile from A to local memory
                        if (global_i < M && (tile + local_j) < K) {
                            local_A[local_i][local_j] = A[global_i * K + tile + local_j];
                        } else {
                            local_A[local_i][local_j] = 0.0f;
                        }
                        
                        // Load tile from B to local memory
                        if ((tile + local_i) < K && global_j < N) {
                            local_B[local_i][local_j] = B[(tile + local_i) * N + global_j];
                        } else {
                            local_B[local_i][local_j] = 0.0f;
                        }
                        
                        item.barrier(sycl::access::fence_space::local_space);
                        
                        // Compute partial product
                        for (size_t k = 0; k < TILE_SIZE; ++k) {
                            sum += local_A[local_i][k] * local_B[k][local_j];
                        }
                        
                        item.barrier(sycl::access::fence_space::local_space);
                    }
                    
                    // Store result
                    if (global_i < M && global_j < N) {
                        C[global_i * N + global_j] = sum;
                    }
                }
            );
        });
        
        event.wait();
    } catch (const sycl::exception& e) {
        std::cerr << "SYCL exception: " << e.what() << std::endl;
        throw;
    }
}

// Reduction (sum)
float sycl_reduce_sum(const float* input, size_t n, sycl::queue& q) {
    std::vector<float> result(1);
    
    try {
        // First, reduce in parallel
        size_t work_group_size = 256;
        size_t num_groups = (n + work_group_size - 1) / work_group_size;
        
        float* intermediate = sycl::malloc_shared<float>(num_groups, q);
        
        auto event1 = q.submit([&](sycl::handler& h) {
            h.parallel_for<ReduceSumKernel>(
                sycl::nd_range<1>(sycl::range<1>(num_groups * work_group_size),
                                 sycl::range<1>(work_group_size)),
                [=](sycl::nd_item<1> item) {
                    size_t local_id = item.get_local_id(0);
                    size_t group_id = item.get_group(0);
                    size_t global_id = item.get_global_id(0);
                    
                    sycl::local_accessor<float, 1> local_mem(work_group_size, item);
                    
                    // Load data
                    local_mem[local_id] = (global_id < n) ? input[global_id] : 0.0f;
                    
                    item.barrier(sycl::access::fence_space::local_space);
                    
                    // Reduction in local memory
                    for (size_t s = work_group_size / 2; s > 0; s >>= 1) {
                        if (local_id < s) {
                            local_mem[local_id] += local_mem[local_id + s];
                        }
                        item.barrier(sycl::access::fence_space::local_space);
                    }
                    
                    // Store group result
                    if (local_id == 0) {
                        intermediate[group_id] = local_mem[0];
                    }
                }
            );
        });
        
        event1.wait();
        
        // Final reduction on host or device
        float sum = 0.0f;
        for (size_t i = 0; i < num_groups; ++i) {
            sum += intermediate[i];
        }
        
        sycl::free(intermediate, q);
        
        return sum;
        
    } catch (const sycl::exception& e) {
        std::cerr << "SYCL exception: " << e.what() << std::endl;
        throw;
    }
}

// USM (Unified Shared Memory) version
void sycl_vector_add_usm(float* a, float* b, float* c, size_t n,
                        sycl::queue& q) {
    try {
        auto event = q.parallel_for(
            sycl::range<1>{n},
            [=](sycl::id<1> idx) {
                c[idx] = a[idx] + b[idx];
            }
        );
        
        event.wait();
    } catch (const sycl::exception& e) {
        std::cerr << "SYCL exception: " << e.what() << std::endl;
        throw;
    }
}

// Multi-device execution
void sycl_multi_device_vector_add(const float* a, const float* b, float* c,
                                 size_t n) {
    try {
        // Get all available devices
        auto platforms = sycl::platform::get_platforms();
        
        for (auto& platform : platforms) {
            auto devices = platform.get_devices();
            
            for (auto& device : devices) {
                if (device.is_gpu() || device.is_cpu()) {
                    sycl::queue q(device);
                    
                    std::cout << "Running on: " 
                              << device.get_info<sycl::info::device::name>()
                              << std::endl;
                    
                    // Split work among devices
                    size_t device_count = devices.size();
                    size_t chunk_size = n / device_count;
                    size_t start = 0;
                    
                    for (size_t i = 0; i < device_count; ++i) {
                        size_t end = (i == device_count - 1) ? n : start + chunk_size;
                        size_t size = end - start;
                        
                        if (size > 0) {
                            sycl_vector_add(&a[start], &b[start], &c[start], size, q);
                        }
                        
                        start = end;
                    }
                }
            }
        }
    } catch (const sycl::exception& e) {
        std::cerr << "SYCL exception: " << e.what() << std::endl;
        throw;
    }
}
```

### **Common Host Interface**

**src/common/gpu_interface.h:**
```cpp
// gpu_interface.h - Common GPU interface for multiple backends
#ifndef GPU_INTERFACE_H
#define GPU_INTERFACE_H

#include <cstddef>
#include <memory>
#include <vector>

// GPU backend types
enum class GPUBackend {
    AUTO,      // Auto-select based on availability
    CUDA,      // NVIDIA CUDA
    OPENCL,    // OpenCL
    SYCL,      // SYCL (Intel oneAPI, Codeplay)
    HIP,       // AMD HIP
    OPENMP     // OpenMP offload
};

// GPU device information
struct GPUDeviceInfo {
    std::string name;
    std::string vendor;
    size_t global_memory;      // in bytes
    size_t max_work_group_size;
    size_t compute_units;
    std::string extensions;
    bool double_precision;
    bool unified_memory;
};

// Abstract GPU context
class GPUContext {
public:
    virtual ~GPUContext() = default;
    
    // Memory management
    virtual void* allocate_device(size_t size) = 0;
    virtual void free_device(void* ptr) = 0;
    virtual void* allocate_host(size_t size, bool pinned = false) = 0;
    virtual void free_host(void* ptr, bool pinned = false) = 0;
    
    // Data transfer
    virtual void copy_to_device(void* dst, const void* src, size_t size) = 0;
    virtual void copy_from_device(void* dst, const void* src, size_t size) = 0;
    virtual void copy_device_to_device(void* dst, const void* src, size_t size) = 0;
    
    // Synchronization
    virtual void synchronize() = 0;
    
    // Device information
    virtual std::vector<GPUDeviceInfo> get_devices() const = 0;
    virtual int get_current_device() const = 0;
    virtual void set_device(int device_id) = 0;
    
    // Kernel execution
    virtual void launch_kernel(const std::string& kernel_name,
                              const std::vector<void*>& args,
                              const std::vector<size_t>& global_work_size,
                              const std::vector<size_t>& local_work_size) = 0;
    
    // Profiling
    virtual void start_profiling() = 0;
    virtual void stop_profiling() = 0;
    virtual std::string get_profiling_report() const = 0;
};

// GPU operations interface
class GPUOperations {
public:
    virtual ~GPUOperations() = default;
    
    // Vector operations
    virtual void vector_add(const float* a, const float* b, float* c, size_t n) = 0;
    virtual void vector_sub(const float* a, const float* b, float* c, size_t n) = 0;
    virtual void vector_mul(const float* a, const float* b, float* c, size_t n) = 0;
    virtual float vector_dot(const float* a, const float* b, size_t n) = 0;
    virtual void vector_scale(float* a, float scalar, size_t n) = 0;
    
    // Matrix operations
    virtual void matrix_add(const float* A, const float* B, float* C,
                           size_t M, size_t N) = 0;
    virtual void matrix_mul(const float* A, const float* B, float* C,
                           size_t M, size_t N, size_t K) = 0;
    virtual void matrix_transpose(const float* A, float* B,
                                 size_t M, size_t N) = 0;
    
    // Reduction operations
    virtual float reduce_sum(const float* data, size_t n) = 0;
    virtual float reduce_max(const float* data, size_t n) = 0;
    virtual float reduce_min(const float* data, size_t n) = 0;
    
    // Convolution operations
    virtual void convolution_2d(const float* input, const float* kernel,
                               float* output, size_t width, size_t height,
                               size_t kernel_size, size_t padding) = 0;
    
    // FFT operations
    virtual void fft_forward(const std::complex<float>* input,
                            std::complex<float>* output, size_t n) = 0;
    virtual void fft_inverse(const std::complex<float>* input,
                            std::complex<float>* output, size_t n) = 0;
};

// Factory functions
std::unique_ptr<GPUContext> create_gpu_context(GPUBackend backend = GPUBackend::AUTO);
std::unique_ptr<GPUOperations> create_gpu_operations(GPUBackend backend = GPUBackend::AUTO);

// Utility functions
GPUBackend detect_best_backend();
std::vector<GPUBackend> get_available_backends();
std::string backend_to_string(GPUBackend backend);

#endif // GPU_INTERFACE_H
```

### **Benchmarks and Performance Testing**

**benchmarks/CMakeLists.txt:**
```cmake
# GPU benchmarks

message(STATUS "Configuring GPU benchmarks...")

# Add benchmark executable
add_executable(gpu_benchmarks
    benchmark_vector.cpp
    benchmark_matrix.cpp
    benchmark_reduction.cpp
    benchmark_convolution.cpp
    main.cpp
)

# Link with available GPU libraries
if(CUDA_FOUND)
    target_link_libraries(gpu_benchmarks PRIVATE gpu_cuda)
endif()

if(OPENCL_FOUND)
    target_link_libraries(gpu_benchmarks PRIVATE gpu_opencl)
endif()

if(SYCL_FOUND)
    target_link_libraries(gpu_benchmarks PRIVATE gpu_sycl)
endif()

# Link with common libraries
target_link_libraries(g






```











# Complete CMake Course - Part 13 to Part 20
## Advanced Topics for Production-Ready Projects

---

## **PART-13: Advanced Dependency Management with FetchContent and find_package**

### **Topic: Modern Dependency Handling in CMake**

#### **Concept Overview**
Modern CMake provides two primary ways to handle dependencies:
1. **FetchContent**: Download and build dependencies during configure time
2. **find_package**: Find pre-installed system packages

#### **Code Structure**
```
PART-13/
├── CMakeLists.txt
├── src/
│   └── main.cpp
├── external/
│   └── CMakeLists.txt
└── cmake/
    └── FindCustomLib.cmake
```

#### **CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.14)
project(AdvancedDependencies VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Option to use system packages or FetchContent
option(USE_SYSTEM_DEPS "Use system-installed dependencies" OFF)

# Method 1: Using FetchContent for dependencies
if(NOT USE_SYSTEM_DEPS)
    include(FetchContent)
    
    # Example: Fetching GoogleTest
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG release-1.11.0
    )
    
    # Example: Fetching nlohmann/json
    FetchContent_Declare(
        nlohmann_json
        URL https://github.com/nlohmann/json/releases/download/v3.10.5/json.tar.xz
        URL_HASH SHA256=5daca6ca216495edf89d167f808d1d03c4a4d929cef7da5e10f135ae1540c7e4
    )
    
    # Make available in CMake
    FetchContent_MakeAvailable(googletest nlohmann_json)
    
    # Add your targets
    add_executable(main_app src/main.cpp)
    
    # Link with fetched dependencies
    target_link_libraries(main_app PRIVATE 
        GTest::gtest 
        GTest::gtest_main 
        nlohmann_json::nlohmann_json
    )
endif()

# Method 2: Using find_package for system libraries
if(USE_SYSTEM_DEPS)
    # Find packages with components
    find_package(GTest REQUIRED COMPONENTS gtest gtest_main)
    find_package(nlohmann_json REQUIRED)
    
    # Custom find module example
    find_package(CustomLib REQUIRED)
    
    add_executable(main_app src/main.cpp)
    target_link_libraries(main_app PRIVATE 
        GTest::gtest 
        GTest::gtest_main 
        nlohmann_json::nlohmann_json
        CustomLib::CustomLib
    )
endif()

# Method 3: Using ExternalProject for more control
if(ENABLE_CUSTOM_BUILD)
    include(ExternalProject)
    
    ExternalProject_Add(
        my_external_lib
        URL "https://example.com/mylib-1.0.tar.gz"
        URL_HASH SHA256=abc123...
        CMAKE_ARGS -DBUILD_SHARED_LIBS=OFF
        INSTALL_COMMAND ""
        BUILD_BYPRODUCTS "<BINARY_DIR>/libmylib.a"
    )
    
    # Create imported target
    add_library(external_lib STATIC IMPORTED)
    add_dependencies(external_lib my_external_lib)
    set_target_properties(external_lib PROPERTIES
        IMPORTED_LOCATION "${CMAKE_BINARY_DIR}/my_external_lib-prefix/src/my_external_lib-build/libmylib.a"
        INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_BINARY_DIR}/my_external_lib-prefix/src/my_external_lib/include"
    )
endif()
```

#### **Custom Find Module: cmake/FindCustomLib.cmake**
```cmake
# cmake/FindCustomLib.cmake
find_path(CustomLib_INCLUDE_DIR
    NAMES customlib.h
    PATHS
        /usr/include
        /usr/local/include
        ${CMAKE_INSTALL_PREFIX}/include
    PATH_SUFFIXES customlib
)

find_library(CustomLib_LIBRARY
    NAMES customlib
    PATHS
        /usr/lib
        /usr/local/lib
        ${CMAKE_INSTALL_PREFIX}/lib
)

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(CustomLib
    REQUIRED_VARS CustomLib_LIBRARY CustomLib_INCLUDE_DIR
    VERSION_VAR CustomLib_VERSION
)

if(CustomLib_FOUND AND NOT TARGET CustomLib::CustomLib)
    add_library(CustomLib::CustomLib UNKNOWN IMPORTED)
    set_target_properties(CustomLib::CustomLib PROPERTIES
        IMPORTED_LOCATION "${CustomLib_LIBRARY}"
        INTERFACE_INCLUDE_DIRECTORIES "${CustomLib_INCLUDE_DIR}"
    )
endif()

mark_as_advanced(CustomLib_INCLUDE_DIR CustomLib_LIBRARY)
```

#### **src/main.cpp**
```cpp
#include <iostream>
#include <nlohmann/json.hpp>  // From fetched dependency
#include <gtest/gtest.h>       // From fetched dependency

// Simple function to test
int add(int a, int b) {
    return a + b;
}

// GoogleTest test case
TEST(MathTest, Addition) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_EQ(add(-1, 1), 0);
}

// JSON example using nlohmann/json
void json_example() {
    nlohmann::json j;
    j["name"] = "CMake Project";
    j["version"] = "1.0.0";
    j["dependencies"] = {"gtest", "nlohmann_json"};
    
    std::cout << "JSON Output: " << j.dump(2) << std::endl;
}

int main(int argc, char** argv) {
    // Run tests
    ::testing::InitGoogleTest(&argc, argv);
    
    json_example();
    
    return RUN_ALL_TESTS();
}
```

#### **Build and Run Instructions**
```bash
# Clone and build with FetchContent
mkdir build && cd build
cmake ..  # Will download and build dependencies
cmake --build .
./main_app

# Build with system packages
mkdir build-system && cd build-system
cmake .. -DUSE_SYSTEM_DEPS=ON
cmake --build .
./main_app
```

#### **Advanced Tips**
1. **Version Pinning**: Always pin specific versions in production
2. **Mirror URLs**: Provide fallback URLs for reliability
3. **Dependency Graph**: Use `FetchContent_Declare` with `OVERRIDE_FIND_PACKAGE`
4. **Caching**: Cache downloaded sources to avoid re-downloads

---

## **PART-14: Cross-Platform Build Configuration**

### **Topic: Platform-Specific Code and Toolchains**

#### **Concept Overview**
Handling differences between Windows, Linux, macOS, and embedded systems.

#### **Code Structure**
```
PART-14/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── platform/
│   │   ├── windows.cpp
│   │   ├── linux.cpp
│   │   └── macos.cpp
│   └── config.h.in
├── toolchains/
│   ├── arm-gcc.cmake
│   ├── mingw.cmake
│   └── ios.cmake
└── scripts/
    └── deploy/
        ├── deploy_windows.ps1
        └── deploy_linux.sh
```

#### **CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.14)
project(CrossPlatformApp VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Platform detection
message(STATUS "System: ${CMAKE_SYSTEM_NAME}")
message(STATUS "Processor: ${CMAKE_SYSTEM_PROCESSOR}")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID}")

# Platform-specific compiler flags
if(WIN32)
    add_definitions(-DWIN32_LEAN_AND_MEAN -DNOMINMAX)
    if(MSVC)
        add_compile_options(/W4 /WX /MP)
    else()
        add_compile_options(-Wall -Wextra -Werror)
    endif()
elseif(APPLE)
    add_compile_options(-Wall -Wextra -Werror)
    if(IOS)
        set(CMAKE_OSX_SYSROOT iphoneos)
        set(CMAKE_OSX_ARCHITECTURES "arm64")
    endif()
elseif(UNIX AND NOT APPLE)  # Linux
    add_compile_options(-Wall -Wextra -Werror -pthread)
    
    # Check for specific Linux distributions
    if(EXISTS /etc/os-release)
        file(READ /etc/os-release OS_RELEASE)
        if(OS_RELEASE MATCHES "Ubuntu")
            message(STATUS "Building on Ubuntu")
        elseif(OS_RELEASE MATCHES "Fedora")
            message(STATUS "Building on Fedora")
        endif()
    endif()
endif()

# Configure header for platform-specific defines
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/src/config.h.in
    ${CMAKE_CURRENT_BINARY_DIR}/config.h
)

# Platform-specific source files
set(PLATFORM_SOURCES)
if(WIN32)
    set(PLATFORM_SOURCES src/platform/windows.cpp)
    set(PLATFORM_LIBS)
elseif(APPLE)
    set(PLATFORM_SOURCES src/platform/macos.cpp)
    set(PLATFORM_LIBS "-framework CoreFoundation")
elseif(UNIX AND NOT APPLE)
    set(PLATFORM_SOURCES src/platform/linux.cpp)
    set(PLATFORM_LIBS pthread dl m)
endif()

# Add executable with platform-specific files
add_executable(cross_platform_app
    src/main.cpp
    ${PLATFORM_SOURCES}
)

target_include_directories(cross_platform_app PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}
)

target_link_libraries(cross_platform_app PRIVATE ${PLATFORM_LIBS})

# Platform-specific installation
if(WIN32)
    install(TARGETS cross_platform_app
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
    )
    
    # Install DLLs on Windows
    install(FILES $<TARGET_RUNTIME_DLLS:cross_platform_app>
        DESTINATION bin
    )
else()
    install(TARGETS cross_platform_app
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib/${CMAKE_PROJECT_NAME}
        LIBRARY DESTINATION lib/${CMAKE_PROJECT_NAME}
    )
endif()

# Cross-compilation support
if(CMAKE_CROSSCOMPILING)
    message(STATUS "Cross-compiling for ${CMAKE_SYSTEM_PROCESSOR}")
    
    # Embedded system example
    if(CMAKE_SYSTEM_PROCESSOR MATCHES "arm")
        add_definitions(-DEMBEDDED_SYSTEM)
        set(CMAKE_EXECUTABLE_SUFFIX ".elf")
    endif()
endif()

# Deploy scripts
if(WIN32)
    configure_file(
        scripts/deploy/deploy_windows.ps1.in
        ${CMAKE_CURRENT_BINARY_DIR}/deploy.ps1
        @ONLY
    )
else()
    configure_file(
        scripts/deploy/deploy_linux.sh.in
        ${CMAKE_CURRENT_BINARY_DIR}/deploy.sh
        @ONLY
    )
endif()
```

#### **Platform-Specific Source Files**

**src/platform/windows.cpp:**
```cpp
#include "config.h"
#include <windows.h>
#include <iostream>

void platform_specific_init() {
    std::cout << "Windows platform initialization" << std::endl;
    
    // Windows-specific code
    OSVERSIONINFOEX osvi;
    ZeroMemory(&osvi, sizeof(OSVERSIONINFOEX));
    osvi.dwOSVersionInfoSize = sizeof(OSVERSIONINFOEX);
    
    if(GetVersionEx((OSVERSIONINFO*)&osvi)) {
        std::cout << "Windows Version: " 
                  << osvi.dwMajorVersion << "." 
                  << osvi.dwMinorVersion << std::endl;
    }
}

std::string get_platform_name() {
    return "Windows";
}
```

**src/platform/linux.cpp:**
```cpp
#include "config.h"
#include <unistd.h>
#include <sys/utsname.h>
#include <iostream>

void platform_specific_init() {
    std::cout << "Linux platform initialization" << std::endl;
    
    struct utsname unameData;
    if(uname(&unameData) == 0) {
        std::cout << "System: " << unameData.sysname << std::endl;
        std::cout << "Release: " << unameData.release << std::endl;
        std::cout << "Version: " << unameData.version << std::endl;
        std::cout << "Machine: " << unameData.machine << std::endl;
    }
}

std::string get_platform_name() {
    return "Linux";
}
```

#### **Toolchain File: toolchains/arm-gcc.cmake**
```cmake
# ARM GCC Toolchain
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

# Specify the cross compiler
set(CMAKE_C_COMPILER arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER arm-linux-gnueabihf-g++)

# Where is the target environment
set(CMAKE_FIND_ROOT_PATH /opt/arm-toolchain)

# Search for programs in the build host directories
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

# Search for libraries and headers in the target directories
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

# Target-specific flags
add_definitions(-DEMBEDDED -DARM_CORTEX_M4)

# Size optimization for embedded
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Os -mcpu=cortex-m4 -mthumb")
```

#### **src/config.h.in**
```cpp
// Configured by CMake
#ifndef CONFIG_H
#define CONFIG_H

// Platform detection
#cmakedefine WINDOWS_PLATFORM
#cmakedefine LINUX_PLATFORM
#cmakedefine MACOS_PLATFORM
#cmakedefine EMBEDDED_SYSTEM

// Version info
#define PROJECT_NAME "@PROJECT_NAME@"
#define PROJECT_VERSION "@PROJECT_VERSION@"
#define PROJECT_DESCRIPTION "@PROJECT_DESCRIPTION@"

// Compiler info
#define COMPILER_ID "@CMAKE_CXX_COMPILER_ID@"
#define COMPILER_VERSION "@CMAKE_CXX_COMPILER_VERSION@"

// Build type
#define BUILD_TYPE "@CMAKE_BUILD_TYPE@"
#define BUILD_TIMESTAMP "@TIMESTAMP@"

#endif // CONFIG_H
```

#### **Build Instructions**
```bash
# Native build for current platform
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build .

# Cross-compile for ARM
mkdir build-arm && cd build-arm
cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchains/arm-gcc.cmake
cmake --build .

# MinGW cross-compile from Linux to Windows
mkdir build-mingw && cd build-mingw
cmake .. -DCMAKE_TOOLCHAIN_FILE=../toolchains/mingw.cmake
cmake --build .
```

#### **Advanced Tips**
1. **Toolchain Files**: Create reusable toolchain files for different targets
2. **Feature Detection**: Use `CheckSymbolExists` and `CheckIncludeFile`
3. **Package Managers**: Support vcpkg, Conan, and system package managers
4. **Android/iOS**: Use CMake's built-in support for mobile platforms

---

## **PART-15: Advanced Generator Expressions**

### **Topic: Conditional Expressions and Target Properties**

#### **Concept Overview**
Generator expressions are evaluated during build system generation, allowing conditional logic based on configuration, platform, and target properties.

#### **Code Structure**
```
PART-15/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   ├── feature_a.cpp
│   └── feature_b.cpp
├── include/
│   └── features.h
└── tests/
    └── test_generators.cpp
```

#### **CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.14)
project(GeneratorExpressions VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Multi-config generator support
set(CMAKE_CONFIGURATION_TYPES "Debug;Release;RelWithDebInfo;MinSizeRel")

# Example 1: Conditional compilation based on build type
add_library(my_lib STATIC
    src/feature_a.cpp
    src/feature_b.cpp
)

# Set different flags for different build types
target_compile_definitions(my_lib PRIVATE
    $<$<CONFIG:Debug>:DEBUG_MODE=1>
    $<$<CONFIG:Release>:RELEASE_MODE=1>
    $<$<CONFIG:RelWithDebInfo>:RELEASE_WITH_DEBUG=1>
)

# Set optimization levels per configuration
target_compile_options(my_lib PRIVATE
    $<$<CONFIG:Debug>:-O0 -g>
    $<$<CONFIG:Release>:-O3>
    $<$<CONFIG:RelWithDebInfo>:-O2 -g>
    $<$<CONFIG:MinSizeRel>:-Os>
)

# Example 2: Platform-specific properties
add_executable(genexpr_demo src/main.cpp)
target_link_libraries(genexpr_demo PRIVATE my_lib)

# Different output names for different platforms
set_target_properties(genexpr_demo PROPERTIES
    OUTPUT_NAME "genexpr-$<LOWER_CASE:$<CONFIG>>"
    # Windows adds .exe, others don't
    SUFFIX "$<$<PLATFORM_ID:Windows>:.exe>"
)

# Example 3: Conditional linking based on features
option(ENABLE_FEATURE_X "Enable advanced feature X" OFF)
option(ENABLE_FEATURE_Y "Enable advanced feature Y" OFF)

if(ENABLE_FEATURE_X)
    target_compile_definitions(my_lib PRIVATE HAS_FEATURE_X)
endif()

if(ENABLE_FEATURE_Y)
    target_compile_definitions(my_lib PRIVATE HAS_FEATURE_Y)
endif()

# Example 4: Imported target with generator expressions
add_library(imported_lib STATIC IMPORTED)
set_target_properties(imported_lib PROPERTIES
    IMPORTED_LOCATION
        $<$<CONFIG:Debug>:/path/to/debug/lib>
        $<$<CONFIG:Release>:/path/to/release/lib>
    INTERFACE_INCLUDE_DIRECTORIES
        $<$<CONFIG:Debug>:/path/to/debug/include>
        $<$<CONFIG:Release>:/path/to/release/include>
)

# Example 5: Conditional source files
set(EXTRA_SOURCES)
if(WIN32)
    list(APPEND EXTRA_SOURCES src/win_specific.cpp)
else()
    list(APPEND EXTRA_SOURCES src/unix_specific.cpp)
endif()

target_sources(genexpr_demo PRIVATE ${EXTRA_SOURCES})

# Example 6: Using generator expressions in install()
install(TARGETS genexpr_demo
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib/$<CONFIG>
    ARCHIVE DESTINATION lib/$<CONFIG>
)

# Example 7: Complex conditional expressions
target_compile_definitions(my_lib PRIVATE
    $<$<AND:$<CONFIG:Release>,$<BOOL:${ENABLE_FEATURE_X}>>:OPTIMIZED_WITH_X>
    $<$<OR:$<PLATFORM_ID:Linux>,$<PLATFORM_ID:Darwin>>:UNIX_SYSTEM>
    $<$<VERSION_GREATER_EQUAL:${CMAKE_CXX_COMPILER_VERSION},10.0>:HAS_CPP20_FEATURES>
)

# Example 8: Generator expressions for custom commands
add_custom_command(
    OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/generated_file.h"
    COMMAND ${CMAKE_COMMAND} -E echo
        "// Generated for config: $<CONFIG>" > generated_file.h
    DEPENDS src/main.cpp
)

# Example 9: Target property queries
add_custom_target(print_info ALL
    COMMAND ${CMAKE_COMMAND} -E echo
        "Target: $<TARGET_FILE:genexpr_demo>"
    COMMAND ${CMAKE_COMMAND} -E echo
        "Config: $<CONFIG>"
    COMMAND ${CMAKE_COMMAND} -E echo
        "Compiler: $<TARGET_PROPERTY:genexpr_demo,COMPILE_DEFINITIONS>"
)

# Example 10: Platform-dependent library names
find_library(MATH_LIBRARY m)
target_link_libraries(genexpr_demo PRIVATE
    $<$<PLATFORM_ID:Linux>:${MATH_LIBRARY}>
    $<$<PLATFORM_ID:Windows>:winmm.lib>
)

# Example 11: Build event with generator expressions
add_custom_command(TARGET genexpr_demo POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E echo
        "Build completed: $<TARGET_FILE:genexpr_demo>"
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "$<TARGET_FILE:genexpr_demo>"
        "$<TARGET_FILE_DIR:genexpr_demo>/latest_build$<TARGET_FILE_SUFFIX:genexpr_demo>"
)

# Example 12: Conditional installation
install(FILES README.md
    DESTINATION
        $<$<CONFIG:Debug>:share/${PROJECT_NAME}/debug>
        $<$<CONFIG:Release>:share/${PROJECT_NAME}>
)

# Test generator expressions
enable_testing()
add_test(NAME test_generator_expressions
    COMMAND genexpr_demo
    CONFIGURATIONS Debug Release
)
```

#### **src/main.cpp**
```cpp
#include <iostream>
#include <string>

void print_build_info() {
    std::cout << "=== Build Information ===" << std::endl;
    
    #ifdef DEBUG_MODE
    std::cout << "Build: Debug Mode" << std::endl;
    #endif
    
    #ifdef RELEASE_MODE
    std::cout << "Build: Release Mode" << std::endl;
    #endif
    
    #ifdef HAS_FEATURE_X
    std::cout << "Feature X: Enabled" << std::endl;
    #endif
    
    #ifdef HAS_FEATURE_Y
    std::cout << "Feature Y: Enabled" << std::endl;
    #endif
    
    #ifdef OPTIMIZED_WITH_X
    std::cout << "Optimized with Feature X" << std::endl;
    #endif
    
    #ifdef UNIX_SYSTEM
    std::cout << "Unix System Detected" << std::endl;
    #endif
    
    #ifdef HAS_CPP20_FEATURES
    std::cout << "C++20 Features Available" << std::endl;
    #endif
}

int main() {
    print_build_info();
    
    #ifdef _WIN32
    std::cout << "\nRunning on Windows" << std::endl;
    #elif __linux__
    std::cout << "\nRunning on Linux" << std::endl;
    #elif __APPLE__
    std::cout << "\nRunning on macOS" << std::endl;
    #endif
    
    return 0;
}
```

#### **src/feature_a.cpp**
```cpp
#include <iostream>

#ifdef HAS_FEATURE_X
#include "advanced_feature_x.h"
#endif

void feature_a_function() {
    std::cout << "Feature A Function" << std::endl;
    
    #ifdef HAS_FEATURE_X
    std::cout << "Using Advanced Feature X" << std::endl;
    advanced_feature_x();
    #endif
}
```

#### **tests/test_generators.cpp**
```cpp
#include <iostream>
#include <cassert>

void test_configuration() {
    #ifdef DEBUG_MODE
    std::cout << "Test running in Debug mode" << std::endl;
    assert(true);  // Debug-specific test
    #elif RELEASE_MODE
    std::cout << "Test running in Release mode" << std::endl;
    #endif
}

int main() {
    test_configuration();
    std::cout << "All generator expression tests passed!" << std::endl;
    return 0;
}
```

#### **Build and Test Instructions**
```bash
# Configure with different options
mkdir build && cd build

# Debug build with Feature X
cmake .. -DCMAKE_BUILD_TYPE=Debug -DENABLE_FEATURE_X=ON
cmake --build .
ctest -V

# Release build with both features
cd ..
mkdir build-release && cd build-release
cmake .. -DCMAKE_BUILD_TYPE=Release -DENABLE_FEATURE_X=ON -DENABLE_FEATURE_Y=ON
cmake --build .
./genexpr-release  # Note the name includes config

# Multi-config generator (Visual Studio, Xcode)
mkdir build-multi && cd build-multi
cmake .. -G "Visual Studio 16 2019"
cmake --build . --config Debug
cmake --build . --config Release
```

#### **Common Generator Expressions Reference**

| Expression | Description | Example |
|------------|-------------|---------|
| `$<CONFIG:cfg>` | True if config equals cfg | `$<CONFIG:Debug>` |
| `$<PLATFORM_ID:id>` | True for platform id | `$<PLATFORM_ID:Linux>` |
| `$<COMPILE_LANGUAGE:lang>` | True for source file language | `$<COMPILE_LANGUAGE:CXX>` |
| `$<BOOL:value>` | Convert to boolean | `$<BOOL:${VAR}>` |
| `$<AND:cond1,cond2>` | Logical AND | `$<AND:cond1,cond2>` |
| `$<OR:cond1,cond2>` | Logical OR | `$<OR:cond1,cond2>` |
| `$<NOT:cond>` | Logical NOT | `$<NOT:cond>` |
| `$<TARGET_FILE:tgt>` | Full path to target file | `$<TARGET_FILE:myapp>` |
| `$<TARGET_FILE_NAME:tgt>` | File name of target | `$<TARGET_FILE_NAME:myapp>` |
| `$<TARGET_PROPERTY:tgt,prop>` | Get target property | `$<TARGET_PROPERTY:tgt,INCLUDE_DIRECTORIES>` |
| `$<INSTALL_PREFIX>` | Install prefix | `$<INSTALL_PREFIX>` |
| `$<LOWER_CASE:string>` | Convert to lowercase | `$<LOWER_CASE:$<CONFIG>>` |
| `$<UPPER_CASE:string>` | Convert to uppercase | `$<UPPER_CASE:$<CONFIG>>` |
| `$<MAKE_C_IDENTIFIER:string>` | Make C identifier | `$<MAKE_C_IDENTIFIER:${NAME}>` |

#### **Advanced Tips**
1. **Debugging**: Use `cmake --trace-expand` to debug generator expressions
2. **Performance**: Complex expressions can slow down configure time
3. **Readability**: Break complex expressions into variables
4. **Compatibility**: Some expressions require newer CMake versions

---

## **PART-16: CPack for Package Generation**

### **Topic: Creating Installers and Distribution Packages**

#### **Concept Overview**
CPack is CMake's packaging tool that generates installers for various platforms (DEB, RPM, NSIS, ZIP, TGZ, etc.).

#### **Code Structure**
```
PART-16/
├── CMakeLists.txt
├── CPackConfig.cmake.in
├── src/
│   ├── main.cpp
│   ├── library.cpp
│   └── library.h
├── data/
│   ├── config.json
│   └── resources/
├── man/
│   └── app.1
├── icons/
│   ├── app.ico
│   └── app.png
└── scripts/
    ├── postinstall.sh
    └── prerm.sh
```

#### **CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.14)
project(PackagingDemo VERSION 2.1.0
        DESCRIPTION "A demo application for packaging"
        HOMEPAGE_URL "https://github.com/example/packaging-demo")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set default install prefix if not specified
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "/opt/${PROJECT_NAME}" CACHE PATH "Install prefix" FORCE)
endif()

# Create library
add_library(demo_lib SHARED src/library.cpp)
target_include_directories(demo_lib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:include>
)

# Create executable
add_executable(demo_app src/main.cpp)
target_link_libraries(demo_app PRIVATE demo_lib)

# Installation
install(TARGETS demo_lib
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(TARGETS demo_app
    RUNTIME DESTINATION bin
)

install(FILES src/library.h DESTINATION include)
install(DIRECTORY data/ DESTINATION share/${PROJECT_NAME}/data)
install(FILES README.md LICENSE DESTINATION share/doc/${PROJECT_NAME})

# Install man pages if they exist
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/man/app.1)
    install(FILES man/app.1 DESTINATION share/man/man1)
endif()

# Install icons for desktop integration
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/icons)
    install(FILES icons/app.png DESTINATION share/icons/hicolor/64x64/apps)
    install(FILES icons/app.ico DESTINATION share/icons)
endif()

# Create and install export targets
install(EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION lib/cmake/${PROJECT_NAME}
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION lib/cmake/${PROJECT_NAME}
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    DESTINATION lib/cmake/${PROJECT_NAME}
)

# Include CPack module
include(CPack)

# CPack basic configuration
set(CPACK_PACKAGE_NAME "${PROJECT_NAME}")
set(CPACK_PACKAGE_VENDOR "Example Corporation")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Demo application for CPack")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "${PROJECT_NAME}")
set(CPACK_PACKAGE_CONTACT "support@example.com")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/LICENSE")
set(CPACK_RESOURCE_FILE_README "${CMAKE_CURRENT_SOURCE_DIR}/README.md")

# Platform-specific settings
if(WIN32)
    set(CPACK_GENERATOR "NSIS;ZIP")
    set(CPACK_PACKAGE_INSTALL_REGISTRY_KEY "${PROJECT_NAME}")
    set(CPACK_NSIS_DISPLAY_NAME "${PROJECT_NAME}")
    set(CPACK_NSIS_PACKAGE_NAME "${PROJECT_NAME}")
    set(CPACK_NSIS_INSTALL_ROOT "$PROGRAMFILES64")
    set(CPACK_NSIS_MODIFY_PATH ON)
    set(CPACK_NSIS_MENU_LINKS
        "bin/demo_app.exe" "Demo Application"
        "share/doc/${PROJECT_NAME}/README.md" "Readme"
    )
    
    # Create desktop shortcut
    set(CPACK_NSIS_CREATE_ICONS_EXTRA "
        CreateShortCut '$DESKTOP\\\\${PROJECT_NAME}.lnk' '$INSTDIR\\\\bin\\\\demo_app.exe'
    ")
    set(CPACK_NSIS_DELETE_ICONS_EXTRA "
        Delete '$DESKTOP\\\\${PROJECT_NAME}.lnk'
    ")
    
elseif(APPLE)
    set(CPACK_GENERATOR "DragNDrop;TGZ")
    set(CPACK_DMG_VOLUME_NAME "${PROJECT_NAME}")
    set(CPACK_DMG_FORMAT "UDBZ")
    set(CPACK_DMG_BACKGROUND_IMAGE "${CMAKE_CURRENT_SOURCE_DIR}/icons/background.png")
    
elseif(UNIX)
    # Linux package generators
    set(CPACK_GENERATOR "DEB;RPM;TGZ")
    
    # DEB package settings
    set(CPACK_DEBIAN_PACKAGE_NAME "${PROJECT_NAME}")
    set(CPACK_DEBIAN_FILE_NAME "${PROJECT_NAME}-${PROJECT_VERSION}.deb")
    set(CPACK_DEBIAN_PACKAGE_VERSION "${PROJECT_VERSION}")
    set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "amd64")
    set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6 (>= 2.14), libstdc++6 (>= 4.9)")
    set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Example Maintainer <maintainer@example.com>")
    set(CPACK_DEBIAN_PACKAGE_SECTION "utils")
    set(CPACK_DEBIAN_PACKAGE_PRIORITY "optional")
    set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "${PROJECT_HOMEPAGE_URL}")
    set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA
        "${CMAKE_CURRENT_SOURCE_DIR}/scripts/postinst;${CMAKE_CURRENT_SOURCE_DIR}/scripts/prerm"
    )
    
    # RPM package settings
    set(CPACK_RPM_PACKAGE_NAME "${PROJECT_NAME}")
    set(CPACK_RPM_PACKAGE_VERSION "${PROJECT_VERSION}")
    set(CPACK_RPM_PACKAGE_RELEASE "1")
    set(CPACK_RPM_PACKAGE_ARCHITECTURE "x86_64")
    set(CPACK_RPM_PACKAGE_VENDOR "Example Corporation")
    set(CPACK_RPM_PACKAGE_LICENSE "MIT")
    set(CPACK_RPM_PACKAGE_GROUP "Development/Tools")
    set(CPACK_RPM_PACKAGE_URL "${PROJECT_HOMEPAGE_URL}")
    set(CPACK_RPM_PACKAGE_REQUIRES "libstdc++ >= 4.9.0, glibc >= 2.14")
    set(CPACK_RPM_POST_INSTALL_SCRIPT_FILE "${CMAKE_CURRENT_SOURCE_DIR}/scripts/postinstall.sh")
    set(CPACK_RPM_PRE_UNINSTALL_SCRIPT_FILE "${CMAKE_CURRENT_SOURCE_DIR}/scripts/prerm.sh")
    
    # Create desktop entry for Linux
    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/scripts/demo_app.desktop.in
        ${CMAKE_CURRENT_BINARY_DIR}/demo_app.desktop
        @ONLY
    )
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/demo_app.desktop
        DESTINATION share/applications
    )
endif()

# Component-based packaging
set(CPACK_COMPONENTS_ALL libraries applications documentation)
set(CPACK_COMPONENT_LIBRARIES_DISPLAY_NAME "Shared Libraries")
set(CPACK_COMPONENT_APPLICATIONS_DISPLAY_NAME "Applications")
set(CPACK_COMPONENT_DOCUMENTATION_DISPLAY_NAME "Documentation")
set(CPACK_COMPONENT_LIBRARIES_DESCRIPTION "Runtime libraries")
set(CPACK_COMPONENT_APPLICATIONS_DESCRIPTION "Executable applications")
set(CPACK_COMPONENT_DOCUMENTATION_DESCRIPTION "Documentation and examples")

# Source package configuration
set(CPACK_SOURCE_GENERATOR "TGZ;ZIP")
set(CPACK_SOURCE_IGNORE_FILES
    "/build/"
    "/.git/"
    "/.vscode/"
    "\\\\.swp$"
    "\\\\.swo$"
)
set(CPACK_SOURCE_PACKAGE_FILE_NAME "${PROJECT_NAME}-${PROJECT_VERSION}-src")

# Advanced: Create package for multiple architectures
if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64|AMD64")
    set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "amd64")
    set(CPACK_RPM_PACKAGE_ARCHITECTURE "x86_64")
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64|ARM64")
    set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE "arm64")
    set(CPACK_RPM_PACKAGE_ARCHITECTURE "aarch64")
endif()

# Include component install for CPack
cpack_add_component_group(Runtime
    DISPLAY_NAME "Runtime Components"
    DESCRIPTION "Required runtime components"
)

cpack_add_component(libraries
    DISPLAY_NAME "Libraries"
    DESCRIPTION "Shared libraries"
    GROUP Runtime
    REQUIRED
)

cpack_add_component(applications
    DISPLAY_NAME "Applications"
    DESCRIPTION "Executable applications"
    GROUP Runtime
    REQUIRED
)

cpack_add_component_group(Development
    DISPLAY_NAME "Development Files"
    DESCRIPTION "Headers and CMake files for development"
)

cpack_add_component(headers
    DISPLAY_NAME "Header Files"
    DESCRIPTION "C++ header files"
    GROUP Development
)

cpack_add_component(cmake
    DISPLAY_NAME "CMake Files"
    DESCRIPTION "CMake package configuration"
    GROUP Development
)

# Create symbolic links in packages (Linux specific)
if(UNIX AND NOT APPLE)
    set(CPACK_DEBIAN_PACKAGE_CONTROL_EXTRA
        "${CMAKE_CURRENT_SOURCE_DIR}/scripts/postinst"
        "${CMAKE_CURRENT_SOURCE_DIR}/scripts/prerm"
    )
    
    # Create post-install script
    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/scripts/postinst.in
        ${CMAKE_CURRENT_SOURCE_DIR}/scripts/postinst
        @ONLY
    )
    
    # Create pre-remove script
    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/scripts/prerm.in
        ${CMAKE_CURRENT_SOURCE_DIR}/scripts/prerm
        @ONLY
    )
endif()

# Finally include CPack (must be last)
set(CPACK_PROJECT_CONFIG_FILE "${CMAKE_CURRENT_SOURCE_DIR}/CPackConfig.cmake.in")
include(CPack)
```

#### **CPackConfig.cmake.in**
```cmake
# Custom CPack configuration
set(CPACK_PACKAGE_DIRECTORY "${CMAKE_BINARY_DIR}/packages")
set(CPACK_OUTPUT_FILE_PREFIX "${CMAKE_BINARY_DIR}/packages")

# Set package file name
set(CPACK_PACKAGE_FILE_NAME
    "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${CMAKE_SYSTEM_NAME}-${CMAKE_SYSTEM_PROCESSOR}"
)

# Override settings based on build type
if("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
    set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_FILE_NAME}-debug")
endif()

# Additional component configuration
if(UNIX AND NOT APPLE)
    # For DEB packages
    set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
    set(CPACK_DEBIAN_PACKAGE_GENERATE_SHLIBS ON)
    
    # For RPM packages
    set(CPACK_RPM_PACKAGE_AUTOREQ ON)
    set(CPACK_RPM_PACKAGE_AUTOPROV ON)
endif()
```

#### **src/main.cpp**
```cpp
#include "library.h"
#include <iostream>
#include <cstdlib>

int main(int argc, char* argv[]) {
    std::cout << "Packaging Demo Application v2.1.0" << std::endl;
    std::cout << "================================" << std::endl;
    
    if(argc > 1) {
        std::string command = argv[1];
        if(command == "--version") {
            std::cout << "Version: 2.1.0" << std::endl;
            return 0;
        } else if(command == "--help") {
            std::cout << "Usage: demo_app [--version|--help]" << std::endl;
            return 0;
        }
    }
    
    // Use the library
    DemoLibrary lib;
    lib.initialize();
    lib.processData("Sample data");
    lib.shutdown();
    
    std::cout << "Application completed successfully!" << std::endl;
    return 0;
}
```

#### **Scripts for Package Management**

**scripts/postinstall.sh:**
```bash
#!/bin/bash
# Post-installation script for DEB/RPM packages

set -e

echo "Running post-installation script for PackagingDemo"

# Create symlink in /usr/local/bin if it doesn't exist
if [ ! -L /usr/local/bin/demo_app ]; then
    ln -sf /opt/PackagingDemo/bin/demo_app /usr/local/bin/demo_app
    echo "Created symlink in /usr/local/bin"
fi

# Update system cache
if command -v ldconfig &> /dev/null; then
    ldconfig
fi

# Create user data directory
mkdir -p /var/lib/packaging-demo/data
chmod 755 /var/lib/packaging-demo

echo "Post-installation completed successfully"
```

**scripts/prerm.sh:**
```bash
#!/bin/bash
# Pre-removal script for DEB/RPM packages

set -e

echo "Running pre-removal script for PackagingDemo"

# Remove symlink if it exists
if [ -L /usr/local/bin/demo_app ]; then
    rm -f /usr/local/bin/demo_app
    echo "Removed symlink from /usr/local/bin"
fi

echo "Pre-removal completed successfully"
```

**scripts/demo_app.desktop.in:**
```ini
[Desktop Entry]
Type=Application
Name=@PROJECT_NAME@
Comment=Demo application for CPack packaging
Exec=/opt/@PROJECT_NAME@/bin/demo_app
Icon=/opt/@PROJECT_NAME@/share/icons/app.png
Terminal=false
Categories=Development;Utility;
Keywords=demo;cmake;cpack;
```

#### **Build and Package Instructions**
```bash
# Configure and build
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release
cmake --build .
cmake --install . --prefix ../install

# Generate packages
cpack -G DEB  # For Debian/Ubuntu
cpack -G RPM  # For RedHat/Fedora
cpack -G NSIS # For Windows
cpack -G TGZ  # Generic tarball
cpack -G ZIP  # Windows zip

# Generate source package
cpack --config CPackSourceConfig.cmake

# List available generators
cpack --help

# Package with specific configuration
cpack -C Release -G DEB

# Install generated package (Linux example)
sudo dpkg -i PackagingDemo-2.1.0-Linux-x86_64.deb

# Check package contents
dpkg -c PackagingDemo-2.1.0-Linux-x86_64.deb
rpm -qlp PackagingDemo-2.1.0-Linux-x86_64.rpm
```

#### **Advanced CPack Features**

1. **Component-Based Packaging**:
```cmake
# Install components separately
install(TARGETS demo_lib
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY
        DESTINATION lib
        COMPONENT libraries
    ARCHIVE
        DESTINATION lib
        COMPONENT libraries
    RUNTIME
        DESTINATION bin
        COMPONENT applications
    INCLUDES
        DESTINATION include
        COMPONENT headers
)
```

2. **Bundle Packages for macOS**:
```cmake
set(CPACK_BUNDLE_NAME "${PROJECT_NAME}")
set(CPACK_BUNDLE_ICON "${CMAKE_CURRENT_SOURCE_DIR}/icons/app.icns")
set(CPACK_BUNDLE_PLIST "${CMAKE_CURRENT_BINARY_DIR}/Info.plist")
set(CPACK_DMG_DS_STORE "${CMAKE_CURRENT_SOURCE_DIR}/resources/.DS_Store")
set(CPACK_DMG_BACKGROUND_IMAGE "${CMAKE_CURRENT_SOURCE_DIR}/resources/background.png")
```

3. **Windows WiX Installer**:
```cmake
set(CPACK_GENERATOR "WIX")
set(CPACK_WIX_PRODUCT_GUID "YOUR-GUID-HERE")
set(CPACK_WIX_UPGRADE_GUID "YOUR-UPGRADE-GUID")
set(CPACK_WIX_PROPERTY_ARPURLINFOABOUT "${PROJECT_HOMEPAGE_URL}")
set(CPACK_WIX_PROPERTY_ARPCONTACT "${CPACK_PACKAGE_CONTACT}")
set(CPACK_WIX_CULTURES "en-US")
```

#### **Best Practices**
1. **Versioning**: Use semantic versioning for packages
2. **Dependencies**: Specify exact dependencies in package metadata
3. **Testing**: Test packages in clean environments
4. **Signing**: Sign packages for distribution
5. **Reproducible**: Ensure builds are reproducible

---

## **PART-17: Advanced Testing with CTest and Fixtures**

### **Topic: Comprehensive Testing Strategies and Continuous Integration**

#### **Concept Overview**
Advanced testing features including fixtures, property-based testing, coverage, and integration with CI/CD systems.

#### **Code Structure**
```
PART-17/
├── CMakeLists.txt
├── src/
│   ├── math_operations.cpp
│   ├── math_operations.h
│   ├── data_processor.cpp
│   └── data_processor.h
├── tests/
│   ├── unit/
│   │   ├── test_math.cpp
│   │   ├── test_data.cpp
│   │   └── fixtures.cpp
│   ├── integration/
│   │   └── test_integration.cpp
│   ├── performance/
│   │   └── bench_operations.cpp
│   └── fixtures/
│       ├── database_setup.py
│       └── test_data.json
├── cmake/
│   ├── CodeCoverage.cmake
│   └── MemCheck.cmake
└── .github/
    └── workflows/
        └── ci-cd.yml
```

#### **CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.14)
project(AdvancedTesting VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Enable testing
enable_testing()

# Add source library
add_library(math_operations STATIC src/math_operations.cpp)
add_library(data_processor STATIC src/data_processor.cpp)

# Include directories
target_include_directories(math_operations PUBLIC src)
target_include_directories(data_processor PUBLIC src)

# Unit Tests Configuration
option(ENABLE_UNIT_TESTS "Enable unit tests" ON)
option(ENABLE_INTEGRATION_TESTS "Enable integration tests" OFF)
option(ENABLE_PERFORMANCE_TESTS "Enable performance tests" OFF)
option(ENABLE_COVERAGE "Enable code coverage" OFF)
option(ENABLE_MEMCHECK "Enable memory checking" OFF)

if(ENABLE_UNIT_TESTS)
    # Add GoogleTest via FetchContent
    include(FetchContent)
    FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG release-1.11.0
    )
    FetchContent_MakeAvailable(googletest)
    
    # Unit Test 1: Math operations
    add_executable(test_math tests/unit/test_math.cpp)
    target_link_libraries(test_math PRIVATE math_operations GTest::gtest GTest::gtest_main)
    
    # Unit Test 2: Data processing
    add_executable(test_data tests/unit/test_data.cpp)
    target_link_libraries(test_data PRIVATE data_processor GTest::gtest GTest::gtest_main)
    
    # Fixture-based tests
    add_executable(test_fixtures tests/unit/fixtures.cpp)
    target_link_libraries(test_fixtures PRIVATE math_operations data_processor GTest::gtest GTest::gtest_main)
    
    # Add tests to CTest
    add_test(NAME MathOperationsTest COMMAND test_math)
    add_test(NAME DataProcessorTest COMMAND test_data)
    add_test(NAME FixtureBasedTest COMMAND test_fixtures)
    
    # Set test properties
    set_tests_properties(MathOperationsTest PROPERTIES
        LABELS "unit;math"
        TIMEOUT 30
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
    
    set_tests_properties(DataProcessorTest PROPERTIES
        LABELS "unit;data"
        TIMEOUT 30
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
    
    set_tests_properties(FixtureBasedTest PROPERTIES
        LABELS "unit;fixture"
        TIMEOUT 60
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
endif()

# Integration Tests
if(ENABLE_INTEGRATION_TESTS)
    add_executable(test_integration tests/integration/test_integration.cpp)
    target_link_libraries(test_integration PRIVATE math_operations data_processor GTest::gtest GTest::gtest_main)
    
    add_test(NAME IntegrationTest COMMAND test_integration)
    set_tests_properties(IntegrationTest PROPERTIES
        LABELS "integration"
        TIMEOUT 120
        REQUIRED_FILES "${CMAKE_CURRENT_SOURCE_DIR}/tests/fixtures/test_data.json"
    )
endif()

# Performance Tests (Benchmarking)
if(ENABLE_PERFORMANCE_TESTS)
    # Add Google Benchmark
    FetchContent_Declare(
        googlebenchmark
        GIT_REPOSITORY https://github.com/google/benchmark.git
        GIT_TAG v1.6.0
    )
    FetchContent_MakeAvailable(googlebenchmark)
    
    add_executable(bench_operations tests/performance/bench_operations.cpp)
    target_link_libraries(bench_operations PRIVATE math_operations benchmark::benchmark)
    
    # Add as test but don't fail on performance issues
    add_test(NAME PerformanceBenchmark COMMAND bench_operations)
    set_tests_properties(PerformanceBenchmark PROPERTIES
        LABELS "performance;benchmark"
        WILL_FAIL FALSE  # Don't fail build on benchmark
    )
endif()

# Test Fixtures (Setup/Teardown)
if(ENABLE_INTEGRATION_TESTS)
    # Database setup fixture
    add_test(NAME SetupTestDatabase
        COMMAND ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/tests/fixtures/database_setup.py
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
    
    set_tests_properties(SetupTestDatabase PROPERTIES
        FIXTURES_SETUP DB_FIXTURE
        LABELS "fixture;setup"
    )
    
    # Database cleanup fixture
    add_test(NAME CleanupTestDatabase
        COMMAND ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/tests/fixtures/database_cleanup.py
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    )
    
    set_tests_properties(CleanupTestDatabase PROPERTIES
        FIXTURES_CLEANUP DB_FIXTURE
        LABELS "fixture;cleanup"
    )
    
    # Tests that require the database fixture
    set_tests_properties(IntegrationTest PROPERTIES
        FIXTURES_REQUIRED DB_FIXTURE
    )
endif()

# Test Discovery for large projects
if(ENABLE_UNIT_TESTS)
    # Auto-discover GoogleTest tests
    gtest_discover_tests(test_math
        EXTRA_ARGS "--gtest_output=xml:${CMAKE_CURRENT_BINARY_DIR}/test_results/test_math.xml"
        PROPERTIES LABELS "unit;math"
    )
    
    gtest_discover_tests(test_data
        EXTRA_ARGS "--gtest_output=xml:${CMAKE_CURRENT_BINARY_DIR}/test_results/test_data.xml"
        PROPERTIES LABELS "unit;data"
    )
endif()

# Code Coverage
if(ENABLE_COVERAGE)
    include(cmake/CodeCoverage.cmake)
    append_coverage_compiler_flags()
    
    # Add coverage target
    add_custom_target(coverage
        COMMAND ${CMAKE_MAKE_PROGRAM} test
        COMMAND lcov --capture --directory . --output-file coverage.info
        COMMAND lcov --remove coverage.info '/usr/*' --output-file coverage.info
        COMMAND genhtml coverage.info --output-directory coverage_report
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating code coverage report"
    )
endif()

# Memory Checking
if(ENABLE_MEMCHECK)
    include(cmake/MemCheck.cmake)
    
    # Configure Valgrind if available
    find_program(MEMORYCHECK_COMMAND valgrind)
    if(MEMORYCHECK_COMMAND)
        set(MEMORYCHECK_COMMAND_OPTIONS "--trace-children=yes --leak-check=full")
        set(MEMORYCHECK_SUPPRESSIONS_FILE ${CMAKE_CURRENT_SOURCE_DIR}/valgrind.supp)
    endif()
    
    # Custom target for memory check
    add_custom_target(memcheck
        COMMAND ctest -T memcheck
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Running memory check with Valgrind"
    )
endif()

# Test timeout policies
set(CTEST_TEST_TIMEOUT 300)  # 5 minutes default timeout

# Custom test output directory
set(CTEST_OUTPUT_ON_FAILURE ON)
set(CTEST_USE_LAUNCHERS ON)  # For compile_commands.json integration

# Add custom test command with XML output
if(ENABLE_UNIT_TESTS)
    add_custom_command(TARGET test_math POST_BUILD
        COMMAND $<TARGET_FILE:test_math> --gtest_output=xml:${CMAKE_CURRENT_BINARY_DIR}/test_math_results.xml
        COMMENT "Running math tests with XML output"
    )
endif()

# Test data file handling
configure_file(
    tests/fixtures/test_data.json
    ${CMAKE_CURRENT_BINARY_DIR}/test_data.json
    COPYONLY
)

# Create test summary target
add_custom_target(test_summary
    COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure --output-junit TestResults.xml
    DEPENDS test_math test_data
    COMMENT "Running all tests and generating JUnit report"
)

# Install test binaries (optional)
if(INSTALL_TESTS)
    install(TARGETS test_math test_data
        RUNTIME DESTINATION tests
    )
    
    install(FILES tests/fixtures/test_data.json
        DESTINATION tests/fixtures
    )
endif()

# Generate test dashboard script
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/DashboardScript.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/DashboardScript.cmake
    @ONLY
)

# Add custom test that depends on build
add_test(NAME BuildAndTest
    COMMAND ${CMAKE_COMMAND}
        --build ${CMAKE_CURRENT_BINARY_DIR}
        --target test_summary
)
```

#### **cmake/CodeCoverage.cmake**
```cmake
# Code coverage setup
find_program(LCOV_PATH lcov)
find_program(GENHTML_PATH genhtml)
find_program(GCOV_PATH gcov)

if(NOT LCOV_PATH)
    message(WARNING "lcov not found! Code coverage will not be available.")
    set(ENABLE_COVERAGE OFF CACHE BOOL "Code coverage not available" FORCE)
endif()

if(ENABLE_COVERAGE)
    # Add coverage flags for GCC/Clang
    if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
        add_compile_options(--coverage -fprofile-arcs -ftest-coverage)
        add_link_options(--coverage)
        
        # Custom target for coverage report
        add_custom_target(coverage-report
            COMMAND ${LCOV_PATH} --capture --directory . --output-file coverage.info
            COMMAND ${LCOV_PATH} --remove coverage.info '*/tests/*' '*/usr/*' --output-file coverage.info
            COMMAND ${GENHTML_PATH} coverage.info --output-directory coverage_report
            COMMAND ${CMAKE_COMMAND} -E echo "Coverage report generated in coverage_report/index.html"
            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
            DEPENDS test_math test_data
        )
    endif()
endif()
```

#### **Unit Test Examples**

**tests/unit/test_math.cpp:**
```cpp
#include "math_operations.h"
#include <gtest/gtest.h>
#include <cmath>

// Test fixture for math operations
class MathTest : public ::testing::Test {
protected:
    void SetUp() override {
        // Setup code before each test
        epsilon = 1e-6;
    }
    
    void TearDown() override {
        // Cleanup code after each test
    }
    
    double epsilon;
};

// Parameterized test for addition
TEST(MathTest, Addition) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_EQ(add(-1, 1), 0);
    EXPECT_EQ(add(0, 0), 0);
}

// Parameterized test using values
class AdditionTest : public ::testing::TestWithParam<std::tuple<int, int, int>> {};

TEST_P(AdditionTest, AddsNumbers) {
    auto [a, b, expected] = GetParam();
    EXPECT_EQ(add(a, b), expected);
}

INSTANTIATE_TEST_SUITE_P(
    AdditionValues,
    AdditionTest,
    ::testing::Values(
        std::make_tuple(1, 2, 3),
        std::make_tuple(-1, -1, -2),
        std::make_tuple(100, 200, 300)
    )
);

// Test with floating point comparison
TEST_F(MathTest, FloatingPointDivision) {
    EXPECT_NEAR(divide(10.0, 3.0), 3.333333, epsilon);
    EXPECT_NEAR(divide(1.0, 0.0), INFINITY, epsilon);  // Division by zero
}

// Death test for invalid input
TEST(MathTest, InvalidInputDeathTest) {
    EXPECT_DEATH({
        sqrt_negative(-1);
    }, "Invalid input");
}

// Typed test for different numeric types
template<typename T>
class NumericOperationsTest : public ::testing::Test {};

using NumericTypes = ::testing::Types<int, float, double>;
TYPED_TEST_SUITE(NumericOperationsTest, NumericTypes);

TYPED_TEST(NumericOperationsTest, Square) {
    TypeParam value = 5;
    TypeParam expected = 25;
    EXPECT_EQ(square(value), expected);
}
```

**tests/unit/fixtures.cpp:**
```cpp
#include <gtest/gtest.h>
#include <fstream>
#include <filesystem>

// Fixture for file-based tests
class FileFixture : public ::testing::Test {
protected:
    void SetUp() override {
        test_file = "test_fixture_data.txt";
        std::ofstream file(test_file);
        file << "Test data\n";
        file.close();
    }
    
    void TearDown() override {
        std::filesystem::remove(test_file);
    }
    
    std::string test_file;
};

TEST_F(FileFixture, FileExists) {
    std::ifstream file(test_file);
    EXPECT_TRUE(file.is_open());
}

TEST_F(FileFixture, FileContent) {
    std::ifstream file(test_file);
    std::string content;
    std::getline(file, content);
    EXPECT_EQ(content, "Test data");
}

// Fixture with shared resource
class DatabaseFixture : public ::testing::Test {
protected:
    static void SetUpTestSuite() {
        // Called once before all tests
        std::cout << "Setting up database connection pool\n";
    }
    
    static void TearDownTestSuite() {
        // Called once after all tests
        std::cout << "Cleaning up database connections\n";
    }
    
    void SetUp() override {
        // Called before each test
        std::cout << "Starting database transaction\n";
    }
    
    void TearDown() override {
        // Called after each test
        std::cout << "Rolling back database transaction\n";
    }
};

TEST_F(DatabaseFixture, DatabaseOperation1) {
    EXPECT_TRUE(true);
}

TEST_F(DatabaseFixture, DatabaseOperation2) {
    EXPECT_TRUE(true);
}
```

#### **Performance Test Example**

**tests/performance/bench_operations.cpp:**
```cpp
#include "math_operations.h"
#include <benchmark/benchmark.h>

static void BM_Addition(benchmark::State& state) {
    double a = 123.456;
    double b = 789.012;
    
    for (auto _ : state) {
        benchmark::DoNotOptimize(add(a, b));
    }
    
    state.SetComplexityN(state.range(0));
}
BENCHMARK(BM_Addition)->Range(8, 8<<10)->Complexity();

static void BM_Multiplication(benchmark::State& state) {
    double a = 123.456;
    double b = 789.012;
    
    for (auto _ : state) {
        benchmark::DoNotOptimize(multiply(a, b));
    }
}
BENCHMARK(BM_Multiplication);

BENCHMARK_MAIN();
```

#### **CI/CD Integration**

**.github/workflows/ci-cd.yml:**
```yaml
name: CMake CI/CD

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        build_type: [Debug, Release]
        cpp_standard: [17, 20]
    
    runs-on: ${{ matrix.os }}
    
    steps:
    - uses: actions/checkout@v3
      with:
        submodules: recursive
    
    - name: Configure CMake
      run: |
        cmake -B ${{github.workspace}}/build \
              -DCMAKE_BUILD_TYPE=${{ matrix.build_type }} \
              -DCMAKE_CXX_STANDARD=${{ matrix.cpp_standard }} \
              -DENABLE_UNIT_TESTS=ON \
              -DENABLE_COVERAGE=${{ matrix.os == 'ubuntu-latest' }}
    
    - name: Build
      run: cmake --build ${{github.workspace}}/build --config ${{ matrix.build_type }}
    
    - name: Test
      run: ctest --test-dir ${{github.workspace}}/build --output-on-failure
    
    - name: Upload Test Results
      if: always()
      uses: actions/upload-artifact@v3
      with:
        name: test-results-${{ matrix.os }}-${{ matrix.build_type }}
        path: ${{github.workspace}}/build/Testing/**/*.xml
    
    - name: Generate Coverage Report
      if: matrix.os == 'ubuntu-latest' && matrix.build_type == 'Debug'
      run: |
        cd ${{github.workspace}}/build
        make coverage-report
    
    - name: Upload Coverage
      uses: codecov/codecov-action@v3
      with:
        files: ${{github.workspace}}/build/coverage.info
  
  package:
    needs: build-and-test
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Configure for Packaging
      run: cmake -B build -DCMAKE_BUILD_TYPE=Release
    
    - name: Build
      run: cmake --build build
    
    - name: Create Packages
      run: |
        cd build
        cpack -G DEB
        cpack -G RPM
        cpack -G TGZ
    
    - name: Upload Packages
      uses: actions/upload-artifact@v3
      with:
        name: packages
        path: build/*.deb build/*.rpm build/*.tar.gz
```

#### **Build and Test Instructions**
```bash
# Configure with all test options
mkdir build && cd build
cmake .. -DENABLE_UNIT_TESTS=ON \
         -DENABLE_INTEGRATION_TESTS=ON \
         -DENABLE_COVERAGE=ON \
         -DCMAKE_BUILD_TYPE=Debug

# Build all targets
cmake --build .

# Run all tests
ctest --output-on-failure

# Run specific test labels
ctest -L unit
ctest -L integration
ctest -L performance

# Run tests with verbose output
ctest -V -R MathOperationsTest

# Run tests with parallel execution
ctest -j4

# Generate coverage report
make coverage-report
# Open coverage_report/index.html

# Run memory check
make memcheck
ctest -T memcheck

# Generate test dashboard
ctest -D Experimental

# Run benchmarks (if enabled)
./bench_operations --benchmark_format=json
```

#### **Advanced Testing Features**

1. **Test Properties**:
```cmake
# Set test costs for scheduling
set_tests_properties(MyTest PROPERTIES
    COST 2.5
    PROCESSORS 2
    PROCESSOR_AFFINITY ON
)

# Test dependencies
set_tests_properties(TestB PROPERTIES
    DEPENDS TestA
)

# Resource locking for exclusive access
set_tests_properties(DatabaseTest PROPERTIES
    RESOURCE_LOCK DatabaseConnection
)
```

2. **Custom Test Command**:
```cmake
add_test(NAME CustomPythonTest
    COMMAND ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/tests/custom_test.py
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
)

# Test with environment variables
set_tests_properties(CustomPythonTest PROPERTIES
    ENVIRONMENT "TEST_MODE=verbose;DB_HOST=localhost"
)
```

3. **GoogleTest Advanced Features**:
```cmake
# Configure GoogleTest with custom flags
gtest_discover_tests(my_test_target
    EXTRA_ARGS
        "--gtest_color=yes"
        "--gtest_repeat=2"
        "--gtest_shuffle"
        "--gtest_random_seed=12345"
    PROPERTIES
        TIMEOUT 60
        SKIP_RETURN_CODE 77
)
```

#### **Best Practices**
1. **Test Organization**: Group tests by component and type
2. **Fixture Management**: Use proper setup/teardown for resource-heavy tests
3. **Continuous Integration**: Automate testing on multiple platforms
4. **Performance Baselines**: Establish performance baselines and track regressions
5. **Flaky Tests**: Identify and fix or quarantine flaky tests

---

## **PART-18: Project Organization and Large-Scale CMake**

### **Topic: Structuring Large Projects with Multiple Components**

#### **Concept Overview**
Managing complex projects with multiple libraries, executables, and dependencies using modern CMake practices.

#### **Project Structure**
```
PART-18/
├── CMakeLists.txt                 # Root CMakeLists.txt
├── cmake/                         # CMake modules and scripts
│   ├── ProjectDefaults.cmake
│   ├── DependencyManagement.cmake
│   ├── CodeQuality.cmake
│   └── Documentation.cmake
├── thirdparty/                    # External dependencies
│   ├── CMakeLists.txt
│   └── README.md
├── common/                        # Common utilities and types
│   ├── CMakeLists.txt
│   ├── include/
│   │   └── common/
│   └── src/
├── core/                          # Core library
│   ├── CMakeLists.txt
│   ├── include/
│   │   └── core/
│   └── src/
├── modules/                       # Optional modules
│   ├── module_a/
│   ├── module_b/
│   └── CMakeLists.txt
├── apps/                          # Applications
│   ├── console_app/
│   ├── gui_app/
│   └── CMakeLists.txt
├── tests/                         # Test suites
│   ├── unit/
│   ├── integration/
│   └── CMakeLists.txt
├── docs/                          # Documentation
│   ├── CMakeLists.txt
│   └── Doxyfile.in
└── scripts/                       # Build and deployment scripts
```

#### **Root CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.21)
project(LargeScaleProject VERSION 1.0.0 LANGUAGES CXX C)

# Set policies for modern CMake
cmake_policy(SET CMP0077 NEW)  # option() honors normal variables
cmake_policy(SET CMP0091 NEW)  # MSVC runtime library flags
cmake_policy(SET CMP0092 NEW)  # MSVC warning flags
cmake_policy(SET CMP0115 NEW)  # file(GENERATE) behavior

# Define project-wide options
include(cmake/ProjectDefaults.cmake)

# Set default build type if not specified
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "Build type" FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
        "Debug" "Release" "RelWithDebInfo" "MinSizeRel")
endif()

# Global project settings
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output directory organization
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Per-configuration output directories
foreach(config Debug Release RelWithDebInfo MinSizeRel)
    string(TOUPPER ${config} config_upper)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${config_upper} ${CMAKE_BINARY_DIR}/lib/${config})
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_${config_upper} ${CMAKE_BINARY_DIR}/lib/${config})
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_${config_upper} ${CMAKE_BINARY_DIR}/bin/${config})
endforeach()

# Add subdirectories in dependency order
add_subdirectory(thirdparty)     # External dependencies first
add_subdirectory(common)         # Common utilities
add_subdirectory(core)           # Core library
add_subdirectory(modules)        # Optional modules
add_subdirectory(apps)           # Applications
add_subdirectory(tests)          # Tests
add_subdirectory(docs)           # Documentation (optional)

# Code quality checks
if(ENABLE_CODE_QUALITY)
    include(cmake/CodeQuality.cmake)
    setup_code_quality_checks()
endif()

# Package configuration for external users
include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION lib/cmake/${PROJECT_NAME}
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

# Export targets for external use
export(EXPORT ${PROJECT_NAME}Targets
    FILE ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake
)

# Create a superbuild target
add_custom_target(superbuild
    COMMENT "Building all dependencies and the project"
    DEPENDS
        common
        core
        ${PROJECT_NAME}_apps
)

# Create development convenience targets
add_custom_target(format
    COMMAND find . -name '*.cpp' -o -name '*.hpp' -o -name '*.h' | xargs clang-format -i
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMENT "Formatting source code"
)

add_custom_target(clean-all
    COMMAND ${CMAKE_COMMAND} --build . --target clean
    COMMAND ${CMAKE_COMMAND} -E remove_directory ${CMAKE_BINARY_DIR}
    COMMENT "Cleaning build directory"
)

# Add summary of configured options
include(cmake/ProjectSummary.cmake)
print_project_summary()
```

#### **cmake/ProjectDefaults.cmake**
```cmake
# Project-wide default options and settings

# Build options
option(BUILD_SHARED_LIBS "Build shared libraries instead of static" OFF)
option(BUILD_TESTING "Build tests" ON)
option(BUILD_DOCS "Build documentation" OFF)
option(BUILD_EXAMPLES "Build example applications" ON)
option(ENABLE_COVERAGE "Enable code coverage" OFF)
option(ENABLE_SANITIZERS "Enable sanitizers (ASan, UBSan, etc.)" OFF)
option(ENABLE_LTO "Enable link-time optimization" OFF)
option(WARNINGS_AS_ERRORS "Treat warnings as errors" ON)

# Feature toggles
option(WITH_MODULE_A "Enable Module A" ON)
option(WITH_MODULE_B "Enable Module B" OFF)
option(WITH_GPU "Enable GPU acceleration" OFF)
option(WITH_MPI "Enable MPI support" OFF)
option(WITH_OPENMP "Enable OpenMP support" ON)

# Installation options
option(INSTALL_DEVELOPMENT_FILES "Install development files (headers, cmake)" ON)
option(INSTALL_EXAMPLES "Install example applications" OFF)

# Platform-specific defaults
if(WIN32)
    set(DEFAULT_RUNTIME_TYPE "MD" CACHE STRING "MSVC runtime type")
    set_property(CACHE DEFAULT_RUNTIME_TYPE PROPERTY STRINGS "MT" "MD" "MTd" "MDd")
else()
    set(DEFAULT_RUNTIME_TYPE "" CACHE STRING "Runtime type (not applicable)")
endif()

# Set default install prefix for development
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE PATH "Install prefix" FORCE)
endif()

# Configure compiler warnings
include(cmake/CompilerWarnings.cmake)
set_project_warnings()

# Setup sanitizers if enabled
if(ENABLE_SANITIZERS)
    include(cmake/Sanitizers.cmake)
    enable_sanitizers()
endif()

# Setup coverage if enabled
if(ENABLE_COVERAGE)
    include(cmake/CodeCoverage.cmake)
    enable_coverage()
endif()

# Setup LTO if enabled
if(ENABLE_LTO)
    include(cmake/LTO.cmake)
    enable_lto()
endif()
```

#### **common/CMakeLists.txt**
```cmake
# Common utilities library
project(Common LANGUAGES CXX)

# Create library
add_library(common)
add_library(${PROJECT_NAME}::common ALIAS common)

# Sources
target_sources(common
    PRIVATE
        src/logger.cpp
        src/config_parser.cpp
        src/utils.cpp
        src/thread_pool.cpp
)

# Headers (public interface)
target_sources(common
    PUBLIC
        FILE_SET HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
        FILES
            include/common/logger.h
            include/common/config_parser.h
            include/common/utils.h
            include/common/thread_pool.h
            include/common/types.h
)

# Properties
target_include_directories(common
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_compile_features(common PUBLIC cxx_std_20)

# Link libraries
target_link_libraries(common
    PUBLIC
        Threads::Threads
    PRIVATE
        $<$<BOOL:${WITH_OPENMP}>:OpenMP::OpenMP_CXX>
)

# Installation
install(TARGETS common
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(
    DIRECTORY include/
    DESTINATION include
    FILE_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
    DIRECTORY_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
)
```

#### **core/CMakeLists.txt**
```cmake
# Core library (depends on common)
project(Core LANGUAGES CXX)

# Create library
add_library(core)

# Sources
file(GLOB_RECURSE CORE_SOURCES CONFIGURE_DEPENDS src/*.cpp)
target_sources(core PRIVATE ${CORE_SOURCES})

# Headers
target_sources(core
    PUBLIC
        FILE_SET HEADERS
        BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
        FILES
            include/core/engine.h
            include/core/processor.h
            include/core/api.h
)

# Dependencies
target_link_libraries(core
    PUBLIC
        ${PROJECT_NAME}::common
    PRIVATE
        # Private dependencies
)

# Properties
target_include_directories(core
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        src
)

target_compile_definitions(core
    PRIVATE
        CORE_LIBRARY
        $<$<CONFIG:Debug>:CORE_DEBUG>
)

# Version information
set_target_properties(core PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    OUTPUT_NAME "${PROJECT_NAME}_core"
)

# Installation
install(TARGETS core
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)
```

#### **modules/CMakeLists.txt**
```cmake
# Optional modules
cmake_minimum_required(VERSION 3.21)

# Module A
if(WITH_MODULE_A)
    add_subdirectory(module_a)
endif()

# Module B
if(WITH_MODULE_B)
    add_subdirectory(module_b)
endif()

# Function to easily add modules
function(add_project_module name)
    set(options REQUIRED)
    set(oneValueArgs VERSION DESCRIPTION)
    set(multiValueArgs DEPENDS SOURCES INCLUDES)
    cmake_parse_arguments(ARG "${options}" "${oneValueArgs}" "${multiValueArgs}" ${ARGN})
    
    # Check if module should be built
    string(TOUPPER "WITH_${name}" option_name)
    option(${option_name} "Build module ${name}" ${ARG_REQUIRED})
    
    if(${option_name})
        message(STATUS "Building module: ${name}")
        
        # Create library
        add_library(${name} ${ARG_SOURCES})
        
        # Add includes
        target_include_directories(${name}
            PUBLIC
                $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/${name}/include>
                $<INSTALL_INTERFACE:include>
        )
        
        # Link dependencies
        foreach(dep IN LISTS ARG_DEPENDS)
            if(TARGET ${dep})
                target_link_libraries(${name} PUBLIC ${dep})
            else()
                find_package(${dep} REQUIRED)
                target_link_libraries(${name} PUBLIC ${dep}::${dep})
            endif()
        endforeach()
        
        # Export target
        add_library(${PROJECT_NAME}::${name} ALIAS ${name})
        
        # Installation
        install(TARGETS ${name}
            EXPORT ${PROJECT_NAME}Targets
            LIBRARY DESTINATION lib
            ARCHIVE DESTINATION lib
            RUNTIME DESTINATION bin
            INCLUDES DESTINATION include
        )
    else()
        message(STATUS "Skipping module: ${name}")
    endif()
endfunction()

# Example module definitions
add_project_module(module_a
    VERSION 1.0
    DESCRIPTION "Advanced algorithms module"
    DEPENDS common core
    SOURCES
        module_a/src/algorithms.cpp
        module_a/src/optimization.cpp
    INCLUDES
        module_a/include
)

add_project_module(module_b
    VERSION 1.0
    DESCRIPTION "Data processing module"
    DEPENDS common core
    REQUIRED FALSE
    SOURCES
        module_b/src/processor.cpp
        module_b/src/transform.cpp
    INCLUDES
        module_b/include
)
```

#### **apps/CMakeLists.txt**
```cmake
# Applications

# Console application
add_executable(console_app apps/console_app/main.cpp)
target_link_libraries(console_app PRIVATE ${PROJECT_NAME}::core)

# Set application properties
set_target_properties(console_app PROPERTIES
    OUTPUT_NAME "myapp"
    WIN32_EXECUTABLE FALSE
    MACOSX_BUNDLE FALSE
)

# Install application
install(TARGETS console_app
    RUNTIME DESTINATION bin
    BUNDLE DESTINATION bin
)

# GUI application (optional)
if(WITH_GUI)
    find_package(Qt6 COMPONENTS Core Widgets REQUIRED)
    
    add_executable(gui_app apps/gui_app/main.cpp)
    target_link_libraries(gui_app PRIVATE
        ${PROJECT_NAME}::core
        Qt6::Core
        Qt6::Widgets
    )
    
    set_target_properties(gui_app PROPERTIES
        WIN32_EXECUTABLE TRUE
        MACOSX_BUNDLE TRUE
    )
    
    # Install GUI app with resources
    install(TARGETS gui_app
        RUNTIME DESTINATION bin
        BUNDLE DESTINATION Applications
    )
endif()

# Example applications
if(BUILD_EXAMPLES)
    file(GLOB EXAMPLE_APPS CONFIGURE_DEPENDS apps/examples/*.cpp)
    foreach(example_src IN LISTS EXAMPLE_APPS)
        get_filename_component(example_name ${example_src} NAME_WE)
        
        add_executable(example_${example_name} ${example_src})
        target_link_libraries(example_${example_name} PRIVATE ${PROJECT_NAME}::core)
        
        # Mark as example
        set_target_properties(example_${example_name} PROPERTIES
            FOLDER "Examples"
            EXCLUDE_FROM_ALL TRUE
        )
        
        # Install examples if requested
        if(INSTALL_EXAMPLES)
            install(TARGETS example_${example_name}
                RUNTIME DESTINATION share/${PROJECT_NAME}/examples
            )
        endif()
    endforeach()
endif()
```

#### **tests/CMakeLists.txt**
```cmake
# Test suite
if(BUILD_TESTING)
    enable_testing()
    
    # Find GoogleTest
    find_package(GTest REQUIRED)
    
    # Common test setup
    function(add_project_test test_name)
        set(options INTEGRATION PERFORMANCE)
        set(oneValueArgs TIMEOUT WORKING_DIRECTORY)
        set(multiValueArgs SOURCES LIBS)
        cmake_parse_arguments(ARG "${options}" "${oneValueArgs}" "${multiValueArgs}" ${ARGN})
        
        # Create test executable
        add_executable(${test_name} ${ARG_SOURCES})
        
        # Link libraries
        target_link_libraries(${test_name}
            PRIVATE
                GTest::gtest
                GTest::gtest_main
                ${ARG_LIBS}
        )
        
        # Add test
        if(ARG_INTEGRATION)
            add_test(NAME ${test_name}_integration
                COMMAND ${test_name}
                WORKING_DIRECTORY ${ARG_WORKING_DIRECTORY}
            )
            set_tests_properties(${test_name}_integration PROPERTIES
                LABELS "integration"
                TIMEOUT ${ARG_TIMEOUT}
            )
        elseif(ARG_PERFORMANCE)
            add_test(NAME ${test_name}_performance
                COMMAND ${test_name}
            )
            set_tests_properties(${test_name}_performance PROPERTIES
                LABELS "performance"
                WILL_FAIL FALSE
            )
        else()
            add_test(NAME ${test_name}
                COMMAND ${test_name}
            )
            set_tests_properties(${test_name} PROPERTIES
                LABELS "unit"
                TIMEOUT ${ARG_TIMEOUT}
            )
        endif()
        
        # Folder organization
        set_target_properties(${test_name} PROPERTIES FOLDER "Tests")
    endfunction()
    
    # Unit tests
    add_project_test(test_common
        SOURCES
            tests/unit/test_common.cpp
        LIBS
            ${PROJECT_NAME}::common
    )
    
    add_project_test(test_core
        SOURCES
            tests/unit/test_core.cpp
        LIBS
            ${PROJECT_NAME}::core
    )
    
    # Integration tests
    if(WITH_MODULE_A)
        add_project_test(test_module_a
            SOURCES
                tests/integration/test_module_a.cpp
            LIBS
                ${PROJECT_NAME}::module_a
                ${PROJECT_NAME}::core
            INTEGRATION
            TIMEOUT 60
            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        )
    endif()
    
    # Test fixtures
    configure_file(
        tests/fixtures/test_data.json
        ${CMAKE_CURRENT_BINARY_DIR}/test_data.json
        COPYONLY
    )
    
    # Test discovery for GoogleTest
    include(GoogleTest)
    gtest_discover_tests(test_common
        EXTRA_ARGS "--gtest_output=xml:${CMAKE_CURRENT_BINARY_DIR}/test_results/test_common.xml"
    )
    
    # Create test target that depends on all tests
    add_custom_target(run_all_tests
        COMMAND ctest --output-on-failure
        DEPENDS test_common test_core
        COMMENT "Running all tests"
    )
    
    # Install tests if requested
    if(INSTALL_TESTS)
        install(TARGETS test_common test_core
            RUNTIME DESTINATION tests
        )
    endif()
endif()
```

#### **cmake/CodeQuality.cmake**
```cmake
# Code quality checks and tools
find_program(CLANG_TIDY clang-tidy)
find_program(CPPCHECK cppcheck)
find_program(CLANG_FORMAT clang-format)
find_program(INCLUDE_WHAT_YOU_USE include-what-you-use)

function(setup_code_quality_checks)
    # Clang-Tidy
    if(CLANG_TIDY)
        set(CMAKE_CXX_CLANG_TIDY
            ${CLANG_TIDY}
            -checks=*
            -warnings-as-errors=*
        )
        message(STATUS "Clang-Tidy enabled")
    endif()
    
    # Cppcheck
    if(CPPCHECK AND NOT CMAKE_CXX_CPPCHECK)
        set(CMAKE_CXX_CPPCHECK
            ${CPPCHECK}
            --enable=all
            --inconclusive
            --suppress=missingIncludeSystem
            --suppress=unmatchedSuppression
        )
        message(STATUS "Cppcheck enabled")
    endif()
    
    # Include What You Use
    if(INCLUDE_WHAT_YOU_USE)
        set(CMAKE_CXX_INCLUDE_WHAT_YOU_USE ${INCLUDE_WHAT_YOU_USE})
        message(STATUS "Include What You Use enabled")
    endif()
    
    # Create format target
    if(CLANG_FORMAT)
        add_custom_target(format
            COMMAND ${CLANG_FORMAT} -i --style=file
                ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
                ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h
                ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cpp
            COMMENT "Formatting source code with clang-format"
        )
    endif()
    
    # Create code analysis target
    add_custom_target(analyze
        COMMAND ${CMAKE_COMMAND} --build . --target clean
        COMMAND ${CMAKE_COMMAND} --build . --target all
        COMMENT "Running code analysis"
    )
endfunction()
```

#### **Build and Development Instructions**
```bash
# Configure with default options
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo

# Configure with specific options
cmake .. \
    -DCMAKE_BUILD_TYPE=Debug \
    -DWITH_MODULE_A=ON \
    -DWITH_MODULE_B=OFF \
    -DENABLE_COVERAGE=ON \
    -DBUILD_TESTING=ON \
    -DBUILD_EXAMPLES=ON

# Build everything
cmake --build . --parallel 8

# Build specific targets
cmake --build . --target common
cmake --build . --target core
cmake --build . --target console_app

# Run tests
ctest --output-on-failure
ctest -L unit
ctest -L integration

# Run code quality checks
cmake --build . --target analyze

# Format code
cmake --build . --target format

# Install project
cmake --build . --target install

# Create packages
cpack -G DEB
cpack -G RPM

# Clean build
cmake --build . --target clean-all
```

#### **Advanced Project Management Features**

1. **Version Control Integration**:
```cmake
# Get Git information
find_package(Git)
if(GIT_FOUND)
    execute_process(
        COMMAND ${GIT_EXECUTABLE} rev-parse --short HEAD
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        OUTPUT_VARIABLE GIT_COMMIT_HASH
        OUTPUT_STRIP_TRAILING_WHITESPACE
        ERROR_QUIET
    )
    
    execute_process(
        COMMAND ${GIT_EXECUTABLE} describe --tags --always
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        OUTPUT_VARIABLE GIT_DESCRIBE
        OUTPUT_STRIP_TRAILING_WHITESPACE
        ERROR_QUIET
    )
endif()

# Add version info to targets
target_compile_definitions(my_target PRIVATE
    GIT_COMMIT_HASH="${GIT_COMMIT_HASH}"
    GIT_DESCRIBE="${GIT_DESCRIBE}"
)
```

2. **Custom Build Types**:
```cmake
# Add custom build type
set(CMAKE_CONFIGURATION_TYPES
    "${CMAKE_CONFIGURATION_TYPES};Profile;ASan;UBSan;TSan"
)

# Configure custom build types
set(CMAKE_C_FLAGS_PROFILE
    "${CMAKE_C_FLAGS_RELEASE} -pg"
)
set(CMAKE_CXX_FLAGS_PROFILE
    "${CMAKE_CXX_FLAGS_RELEASE} -pg"
)
set(CMAKE_EXE_LINKER_FLAGS_PROFILE
    "${CMAKE_EXE_LINKER_FLAGS_RELEASE} -pg"
)
```

3. **Cross-Platform Package Management**:
```cmake
# Support for vcpkg
if(DEFINED ENV{VCPKG_ROOT})
    set(CMAKE_TOOLCHAIN_FILE
        "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
        CACHE STRING "Vcpkg toolchain file"
    )
endif()

# Support for Conan
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/conanfile.txt)
    find_program(CONAN conan)
    if(CONAN)
        add_custom_target(conan-install
            COMMAND ${CONAN} install ${CMAKE_CURRENT_SOURCE_DIR}
                --build=missing
                --settings build_type=${CMAKE_BUILD_TYPE}
            WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
            COMMENT "Installing dependencies with Conan"
        )
        add_dependencies(configure conan-install)
    endif()
endif()
```

#### **Best Practices for Large Projects**
1. **Modular Design**: Keep components independent and well-defined
2. **Consistent Interface**: Use uniform naming and API patterns
3. **Dependency Management**: Explicitly declare all dependencies
4. **Build Performance**: Use unity builds and parallel compilation
5. **Documentation**: Keep CMake files well-commented and organized
6. **CI/CD Integration**: Automate builds, tests, and deployments
7. **Version Control**: Track CMakeLists.txt changes carefully

---

## **PART-19: CMake with Conan and vcpkg Integration**

### **Topic: Modern Dependency Management Systems**

#### **Concept Overview**
Integration of CMake with package managers like Conan and vcpkg for robust dependency management.

#### **Project Structure**
```
PART-19/
├── CMakeLists.txt
├── conanfile.txt              # Conan dependencies
├── vcpkg.json                 # vcpkg dependencies
├── src/
│   └── main.cpp
├── cmake/
│   ├── ConanSetup.cmake
│   └── VcpkgSetup.cmake
└── thirdparty/
    └── CMakeLists.txt
```

#### **CMakeLists.txt with Both Package Managers**
```cmake
cmake_minimum_required(VERSION 3.15)
project(PackageManagerDemo VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Option to choose package manager
set(PACKAGE_MANAGER "auto" CACHE STRING "Package manager to use (conan, vcpkg, system, auto)")
set_property(CACHE PACKAGE_MANAGER PROPERTY STRINGS conan vcpkg system auto)

# Detect available package managers
if(PACKAGE_MANAGER STREQUAL "auto")
    if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/conanfile.txt)
        set(PACKAGE_MANAGER "conan")
    elseif(EXISTS ${CMAKE_SOURCE_DIR}/vcpkg.json OR DEFINED ENV{VCPKG_ROOT})
        set(PACKAGE_MANAGER "vcpkg")
    else()
        set(PACKAGE_MANAGER "system")
    endif()
endif()

message(STATUS "Using package manager: ${PACKAGE_MANAGER}")

# Option 1: Conan integration
if(PACKAGE_MANAGER STREQUAL "conan")
    include(cmake/ConanSetup.cmake)
    setup_conan()
    
    # Find packages via Conan
    find_package(spdlog REQUIRED)
    find_package(fmt REQUIRED)
    find_package(nlohmann_json REQUIRED)
    find_package(Boost REQUIRED COMPONENTS filesystem system)
    
    # Conan provides these targets
    set(DEPENDENCIES 
        spdlog::spdlog 
        fmt::fmt 
        nlohmann_json::nlohmann_json
        Boost::filesystem 
        Boost::system
    )

# Option 2: vcpkg integration
elseif(PACKAGE_MANAGER STREQUAL "vcpkg")
    # vcpkg toolchain should be passed via -DCMAKE_TOOLCHAIN_FILE
    # or set via environment variable
    
    find_package(spdlog CONFIG REQUIRED)
    find_package(fmt CONFIG REQUIRED)
    find_package(nlohmann_json CONFIG REQUIRED)
    find_package(Boost CONFIG REQUIRED COMPONENTS filesystem system)
    
    set(DEPENDENCIES
        spdlog::spdlog
        fmt::fmt
        nlohmann_json::nlohmann_json
        Boost::filesystem
        Boost::system
    )

# Option 3: System packages
elseif(PACKAGE_MANAGER STREQUAL "system")
    find_package(spdlog REQUIRED)
    find_package(fmt REQUIRED)
    find_package(nlohmann_json REQUIRED)
    find_package(Boost REQUIRED COMPONENTS filesystem system)
    
    set(DEPENDENCIES
        spdlog::spdlog
        fmt::fmt
        nlohmann_json::nlohmann_json
        Boost::filesystem
        Boost::system
    )
endif()

# Add executable
add_executable(package_demo src/main.cpp)

# Link dependencies
target_link_libraries(package_demo PRIVATE ${DEPENDENCIES})

# Include directories (usually handled by targets)
target_include_directories(package_demo PRIVATE
    $<TARGET_PROPERTY:spdlog::spdlog,INTERFACE_INCLUDE_DIRECTORIES>
    $<TARGET_PROPERTY:fmt::fmt,INTERFACE_INCLUDE_DIRECTORIES>
)

# Installation
install(TARGETS package_demo
    RUNTIME DESTINATION bin
)

# Export for reuse
export(TARGETS package_demo
    FILE ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake
)

# Package configuration
include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION lib/cmake/${PROJECT_NAME}
)
```

#### **Conan Integration Files**

**conanfile.txt:**
```ini
[requires]
spdlog/1.11.0
fmt/9.1.0
nlohmann_json/3.11.2
boost/1.81.0
openssl/1.1.1t
zlib/1.2.13

[generators]
CMakeDeps
CMakeToolchain

[options]
boost:shared=False
spdlog:shared=False

[layout]
cmake_layout

[imports]
bin, *.dll -> ./bin
lib, *.dylib* -> ./bin
lib, *.so* -> ./bin
```

**conanfile.py (Advanced):**
```python
from conan import ConanFile
from conan.tools.cmake import CMakeToolchain, CMake, cmake_layout

class PackageManagerDemoConan(ConanFile):
    name = "package-manager-demo"
    version = "1.0.0"
    
    # Binary configuration
    settings = "os", "compiler", "build_type", "arch"
    options = {
        "shared": [True, False],
        "fPIC": [True, False],
    }
    default_options = {
        "shared": False,
        "fPIC": True,
        "boost:shared": False,
        "spdlog:shared": False,
    }
    
    # Dependencies
    requires = (
        "spdlog/1.11.0",
        "fmt/9.1.0",
        "nlohmann_json/3.11.2",
        "boost/1.81.0",
    )
    
    generators = "CMakeDeps", "CMakeToolchain"
    
    def config_options(self):
        if self.settings.os == "Windows":
            del self.options.fPIC
    
    def layout(self):
        cmake_layout(self)
    
    def generate(self):
        tc = CMakeToolchain(self)
        tc.variables["BUILD_SHARED_LIBS"] = self.options.shared
        tc.generate()
    
    def build(self):
        cmake = CMake(self)
        cmake.configure()
        cmake.build()
    
    def package(self):
        cmake = CMake(self)
        cmake.install()
    
    def package_info(self):
        self.cpp_info.libs = ["package_manager_demo"]
```

**cmake/ConanSetup.cmake:**
```cmake
# Conan setup for CMake
function(setup_conan)
    # Check if conan is available
    find_program(CONAN_CMD conan)
    if(NOT CONAN_CMD)
        message(WARNING "Conan not found. Please install Conan to manage dependencies.")
        return()
    endif()
    
    # Detect conanfile location
    if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/conanfile.py)
        set(CONANFILE ${CMAKE_CURRENT_SOURCE_DIR}/conanfile.py)
    elseif(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/conanfile.txt)
        set(CONANFILE ${CMAKE_CURRENT_SOURCE_DIR}/conanfile.txt)
    else()
        message(WARNING "No conanfile found")
        return()
    endif()
    
    # Run conan install
    execute_process(
        COMMAND ${CONAN_CMD} install ${CONANFILE}
            --build=missing
            --settings build_type=${CMAKE_BUILD_TYPE}
            --output-folder=${CMAKE_BINARY_DIR}
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        RESULT_VARIABLE CONAN_RESULT
    )
    
    if(NOT CONAN_RESULT EQUAL 0)
        message(FATAL_ERROR "Conan install failed")
    endif()
    
    # Include Conan generated files
    include(${CMAKE_BINARY_DIR}/conan_toolchain.cmake)
    
    message(STATUS "Conan setup completed successfully")
endfunction()
```

#### **vcpkg Integration Files**

**vcpkg.json:**
```json
{
  "name": "package-manager-demo",
  "version": "1.0.0",
  "description": "Demo project using vcpkg dependencies",
  "dependencies": [
    {
      "name": "spdlog",
      "version>=": "1.11.0",
      "features": ["fmt"]
    },
    {
      "name": "fmt",
      "version>=": "9.1.0"
    },
    {
      "name": "nlohmann-json",
      "version>=": "3.11.2"
    },
    {
      "name": "boost",
      "version>=": "1.81.0",
      "features": ["filesystem", "system"]
    },
    {
      "name": "openssl",
      "version>=": "1.1.1"
    },
    {
      "name": "zlib",
      "version>=": "1.2.13"
    }
  ],
  "builtin-baseline": "3426db05b996481ca31e95fff3734cf23e0f51bc",
  "overrides": [
    {
      "name": "fmt",
      "version": "9.1.0"
    }
  ]
}
```

**vcpkg-configuration.json:**
```json
{
  "default-registry": {
    "kind": "git",
    "repository": "https://github.com/Microsoft/vcpkg",
    "baseline": "3426db05b996481ca31e95fff3734cf23e0f51bc"
  },
  "registries": [
    {
      "kind": "git",
      "repository": "https://github.com/custom-registry/vcpkg-registry",
      "baseline": "abc123def456",
      "packages": ["custom-package"]
    }
  ]
}
```

**cmake/VcpkgSetup.cmake:**
```cmake
# vcpkg setup for CMake
function(setup_vcpkg)
    # Check if vcpkg is available
    if(DEFINED ENV{VCPKG_ROOT})
        set(VCPKG_ROOT $ENV{VCPKG_ROOT})
    elseif(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg)
        set(VCPKG_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg)
    else()
        message(STATUS "vcpkg not found. Using system packages.")
        return()
    endif()
    
    # Set toolchain file
    set(CMAKE_TOOLCHAIN_FILE
        ${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake
        CACHE STRING "vcpkg toolchain file"
    )
    
    # vcpkg manifest mode
    if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg.json)
        set(VCPKG_MANIFEST_MODE ON CACHE BOOL "Enable vcpkg manifest mode")
        set(VCPKG_MANIFEST_DIR ${CMAKE_CURRENT_SOURCE_DIR} CACHE PATH "vcpkg manifest directory")
    endif()
    
    message(STATUS "vcpkg setup completed: ${VCPKG_ROOT}")
endfunction()
```

#### **src/main.cpp (Using Dependencies)**
```cpp
#include <spdlog/spdlog.h>
#include <spdlog/sinks/stdout_color_sinks.h>
#include <fmt/format.h>
#include <nlohmann/json.hpp>
#include <boost/filesystem.hpp>
#include <boost/system/error_code.hpp>
#include <iostream>
#include <vector>

namespace fs = boost::filesystem;

void demo_spdlog() {
    auto logger = spdlog::stdout_color_mt("demo");
    logger->set_level(spdlog::level::debug);
    
    logger->info("Welcome to Package Manager Demo!");
    logger->debug("Debug message");
    logger->warn("Warning message");
    logger->error("Error message");
    
    spdlog::info("Using spdlog version: {}.{}.{}", 
        SPDLOG_VER_MAJOR, SPDLOG_VER_MINOR, SPDLOG_VER_PATCH);
}

void demo_fmt() {
    std::string formatted = fmt::format("Hello, {}!", "World");
    std::cout << "fmt output: " << formatted << std::endl;
    
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    std::string list = fmt::format("Numbers: {}", fmt::join(numbers, ", "));
    std::cout << list << std::endl;
}

void demo_json() {
    nlohmann::json j;
    j["project"] = "Package Manager Demo";
    j["version"] = "1.0.0";
    j["dependencies"] = {"spdlog", "fmt", "nlohmann_json", "boost"};
    j["features"] = {
        {"logging", true},
        {"filesystem", true},
        {"json_parsing", true}
    };
    
    std::cout << "JSON output:\n" << j.dump(2) << std::endl;
    
    // Parse JSON
    std::string json_str = R"({"name": "test", "value": 42})";
    auto parsed = nlohmann::json::parse(json_str);
    std::cout << "Parsed name: " << parsed["name"] << std::endl;
}

void demo_boost_filesystem() {
    boost::system::error_code ec;
    
    // Create directory
    fs::path dir("test_directory");
    if(fs::create_directory(dir, ec)) {
        std::cout << "Created directory: " << dir << std::endl;
    }
    
    // Create file
    fs::path file = dir / "test.txt";
    std::ofstream(file.string()) << "Test content";
    
    // Check file size
    if(fs::exists(file, ec)) {
        auto size = fs::file_size(file, ec);
        std::cout << "File size: " << size << " bytes" << std::endl;
    }
    
    // Cleanup
    fs::remove_all(dir, ec);
}

void demo_cross_manager() {
    // This code works regardless of package manager used
    spdlog::info("Package manager integration successful!");
    
    // Demonstrate features from all dependencies
    demo_fmt();
    demo_json();
    demo_boost_filesystem();
}

int main() {
    try {
        demo_spdlog();
        demo_cross_manager();
        
        spdlog::info("All demos completed successfully!");
        return 0;
    } catch(const std::exception& e) {
        spdlog::error("Exception: {}", e.what());
        return 1;
    }
}
```

#### **Build Scripts**

**build_with_conan.sh:**
```bash
#!/bin/bash
# Build with Conan

set -e

BUILD_DIR="build-conan"
CMAKE_OPTS="-DCMAKE_BUILD_TYPE=Release"

# Install dependencies with Conan
conan install . --build=missing --output-folder=$BUILD_DIR

# Configure with CMake
cd $BUILD_DIR
cmake .. $CMAKE_OPTS -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake

# Build
cmake --build . --parallel

# Run
./bin/package_demo

echo "Build with Conan completed successfully!"
```

**build_with_vcpkg.sh:**
```bash
#!/bin/bash
# Build with vcpkg

set -e

BUILD_DIR="build-vcpkg"
VCPKG_ROOT="${VCPKG_ROOT:-$HOME/vcpkg}"

# Install vcpkg if not present
if [ ! -d "$VCPKG_ROOT" ]; then
    git clone https://github.com/Microsoft/vcpkg.git $VCPKG_ROOT
    cd $VCPKG_ROOT
    ./bootstrap-vcpkg.sh
    cd -
fi

# Configure with vcpkg toolchain
cmake -B $BUILD_DIR \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_TOOLCHAIN_FILE=$VCPKG_ROOT/scripts/buildsystems/vcpkg.cmake

# Build
cmake --build $BUILD_DIR --parallel

# Run
$BUILD_DIR/package_demo

echo "Build with vcpkg completed successfully!"
```

**build_with_system.sh:**
```bash
#!/bin/bash
# Build with system packages

set -e

BUILD_DIR="build-system"

# Configure
cmake -B $BUILD_DIR \
    -DCMAKE_BUILD_TYPE=Release \
    -DPACKAGE_MANAGER=system

# Build
cmake --build $BUILD_DIR --parallel

# Run
$BUILD_DIR/package_demo

echo "Build with system packages completed successfully!"
```

#### **CMake Presets (Modern Approach)**

**CMakePresets.json:**
```json
{
  "version": 3,
  "configurePresets": [
    {
      "name": "conan-debug",
      "displayName": "Conan Debug",
      "description": "Debug build with Conan",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/conan-debug",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug",
        "CMAKE_TOOLCHAIN_FILE": "${sourceDir}/build/conan-debug/conan_toolchain.cmake"
      },
      "environment": {
        "CONAN_REVISIONS_ENABLED": "1"
      }
    },
    {
      "name": "vcpkg-release",
      "displayName": "vcpkg Release",
      "description": "Release build with vcpkg",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/vcpkg-release",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Release",
        "CMAKE_TOOLCHAIN_FILE": "$env{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
      }
    },
    {
      "name": "system-relwithdebinfo",
      "displayName": "System RelWithDebInfo",
      "description": "RelWithDebInfo build with system packages",
      "generator": "Unix Makefiles",
      "binaryDir": "${sourceDir}/build/system-relwithdebinfo",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "RelWithDebInfo",
        "PACKAGE_MANAGER": "system"
      }
    }
  ],
  "buildPresets": [
    {
      "name": "conan-debug",
      "configurePreset": "conan-debug",
      "jobs": 8
    },
    {
      "name": "vcpkg-release",
      "configurePreset": "vcpkg-release",
      "jobs": 8
    }
  ],
  "testPresets": [
    {
      "name": "conan-debug",
      "configurePreset": "conan-debug",
      "output": {
        "outputOnFailure": true
      }
    }
  ]
}
```

#### **Build Instructions**
```bash
# Method 1: Using Conan
./build_with_conan.sh

# Method 2: Using vcpkg
./build_with_vcpkg.sh

# Method 3: Using system packages
./build_with_system.sh

# Method 4: Using CMake Presets (modern)
cmake --preset conan-debug
cmake --build --preset conan-debug
ctest --preset conan-debug

# Method 5: Manual build with Conan
mkdir build && cd build
conan install .. --build=missing
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake
cmake --build .
./package_demo

# Method 6: Manual build with vcpkg
mkdir build && cd build
cmake .. -DCMAKE_TOOLCHAIN_FILE=[path/to/vcpkg]/scripts/buildsystems/vcpkg.cmake
cmake --build .
./package_demo
```

#### **Comparison Table: Conan vs vcpkg**

| Feature | Conan | vcpkg |
|---------|-------|-------|
| **Language** | Python-based | C++-based |
| **Package Source** | Multiple remotes (conan-center, Artifactory, custom) | Central registry + ports |
| **Cross-Platform** | Excellent (Windows, Linux, macOS, cross-compilation) | Good (Windows, Linux, macOS) |
| **Build Systems** | CMake, Meson, Autotools, custom | CMake-focused |
| **Binary Packages** | Yes (Artifactory, Bintray) | Yes (binary caching) |
| **Dependency Graph** | Advanced (transitive dependencies, version ranges) | Basic |
| **Custom Packages** | Easy (conanfile.py) | Moderate (portfiles) |
| **Integration** | CMakeDeps, CMakeToolchain generators | CMake toolchain file |
| **CI/CD** | Excellent (Artifactory integration) | Good (GitHub Actions) |

#### **Best Practices for Package Manager Integration**

1. **Lock Files**: Use lock files for reproducible builds
2. **Version Pinning**: Pin versions in production
3. **Private Repositories**: Set up private repositories for proprietary dependencies
4. **CI/CD Integration**: Cache package downloads in CI
5. **Fallback Strategy**: Provide fallback to system packages
6. **Documentation**: Document package manager requirements
7. **Team Coordination**: Ensure team uses same package versions

---

## **PART-20: CMake Best Practices and Production Ready Project Template**

### **Topic: Enterprise-Grade CMake Project Structure and Practices**

#### **Concept Overview**
Complete production-ready project template with all best practices implemented.

#### **Project Structure**
```
PART-20/
├── CMakeLists.txt                 # Root CMakeLists.txt
├── CMakePresets.json              # Modern CMake presets
├── .gitignore                     # Git ignore rules
├── .gitattributes                 # Git attributes
├── .clang-format                  # Code formatting
├── .clang-tidy                    # Static analysis
├── README.md                      # Project documentation
├── LICENSE                        # License file
├── CHANGELOG.md                   # Change log
├── CONTRIBUTING.md                # Contribution guidelines
├── CODE_OF_CONDUCT.md             # Code of conduct
├── cmake/                         # CMake modules
│   ├── CompilerWarnings.cmake
│   ├── CodeCoverage.cmake
│   ├── Sanitizers.cmake
│   ├── StaticAnalyzers.cmake
│   ├── Dependencies.cmake
│   ├── InstallUtils.cmake
│   ├── GitUtils.cmake
│   ├── ProjectOptions.cmake
│   └── PackageConfig.cmake.in
├── src/                           # Source code
│   ├── CMakeLists.txt
│   ├── core/                      # Core library
│   │   ├── CMakeLists.txt
│   │   ├── include/
│   │   └── src/
│   ├── utils/                     # Utilities
│   │   ├── CMakeLists.txt
│   │   ├── include/
│   │   └── src/
│   └── app/                       # Main application
│       ├── CMakeLists.txt
│       └── src/
├── tests/                         # Tests
│   ├── CMakeLists.txt
│   ├── unit/
│   ├── integration/
│   └── benchmarks/
├── examples/                      # Examples
│   ├── CMakeLists.txt
│   └── src/
├── docs/                          # Documentation
│   ├── CMakeLists.txt
│   ├── Doxyfile.in
│   └── pages/
├── scripts/                       # Build and utility scripts
│   ├── ci/
│   ├── dev/
│   └── release/
├── thirdparty/                    # Third-party dependencies
│   ├── CMakeLists.txt
│   └── README.md
└── .github/                       # GitHub workflows
    └── workflows/
        ├── ci.yml
        ├── release.yml
        └── codeql-analysis.yml
```

#### **Root CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.21)
project(ProductionProject
    VERSION 1.0.0
    DESCRIPTION "Production-ready CMake project template"
    HOMEPAGE_URL "https://github.com/yourusername/production-project"
    LANGUAGES CXX C
)

# Set modern CMake policies
include(cmake/SetPolicies.cmake)
set_modern_policies()

# Load project options and defaults
include(cmake/ProjectOptions.cmake)
set_project_options()

# Set output directories
set_output_directories()

# Add subdirectories in dependency order
add_subdirectory(thirdparty)  # External dependencies first
add_subdirectory(src)         # Main source code
add_subdirectory(tests)       # Tests
add_subdirectory(examples)    # Example programs
add_subdirectory(docs)        # Documentation (optional)

# Package configuration
include(cmake/PackageConfig.cmake)
configure_package()

# Summary
print_project_summary()

# Create convenience targets
add_custom_target(check-all
    COMMAND ${CMAKE_COMMAND} --build . --target test
    COMMAND ${CMAKE_COMMAND} --build . --target format-check
    COMMAND ${CMAKE_COMMAND} --build . --target analyze
    COMMENT "Running all checks"
)

# Generate compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
```

#### **cmake/ProjectOptions.cmake**
```cmake
# Project-wide options and settings
function(set_project_options)
    # Default build type
    if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
        set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "Build type")
        set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
            "Debug" "Release" "RelWithDebInfo" "MinSizeRel" "Profile")
    endif()

    # Compiler standards
    set(CMAKE_CXX_STANDARD 20)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)
    
    set(CMAKE_C_STANDARD 11)
    set(CMAKE_C_STANDARD_REQUIRED ON)
    set(CMAKE_C_EXTENSIONS OFF)

    # Project options
    option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
    option(BUILD_TESTING "Build tests" ON)
    option(BUILD_EXAMPLES "Build examples" ON)
    option(BUILD_DOCS "Build documentation" OFF)
    option(ENABLE_COVERAGE "Enable code coverage" OFF)
    option(ENABLE_SANITIZERS "Enable sanitizers" OFF)
    option(ENABLE_LTO "Enable link-time optimization" OFF)
    option(WARNINGS_AS_ERRORS "Treat warnings as errors" ON)
    option(ENABLE_IPO "Enable interprocedural optimization" OFF)
    option(ENABLE_PCH "Enable precompiled headers" ON)

    # Feature flags
    option(WITH_TESTS "Enable tests" ON)
    option(WITH_EXAMPLES "Enable examples" ON)
    option(WITH_BENCHMARKS "Enable benchmarks" OFF)
    option(WITH_FEATURE_A "Enable feature A" ON)
    option(WITH_FEATURE_B "Enable feature B" OFF)

    # Installation options
    set(CMAKE_INSTALL_DEFAULT_COMPONENT_NAME "default" CACHE STRING
        "Default install component name")
    
    if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
        set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE PATH "Install prefix" FORCE)
    endif()

    # Setup compiler warnings
    include(cmake/CompilerWarnings.cmake)
    set_project_warnings()

    # Setup sanitizers if enabled
    if(ENABLE_SANITIZERS)
        include(cmake/Sanitizers.cmake)
        enable_sanitizers()
    endif()

    # Setup coverage if enabled
    if(ENABLE_COVERAGE)
        include(cmake/CodeCoverage.cmake)
        enable_coverage()
    endif()

    # Setup static analyzers
    if(CMAKE_CXX_COMPILER_ID MATCHES "Clang|GNU")
        include(cmake/StaticAnalyzers.cmake)
        enable_static_analyzers()
    endif()

    # Setup LTO if enabled
    if(ENABLE_LTO)
        include(cmake/LTO.cmake)
        enable_lto()
    endif()

    # Setup IPO if enabled
    if(ENABLE_IPO)
        include(CheckIPOSupported)
        check_ipo_supported(RESULT ipo_supported)
        if(ipo_supported)
            set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
        else()
            message(WARNING "IPO is not supported by the compiler")
        endif()
    endif()
endfunction()

# Set output directories
function(set_output_directories)
    # Base output directories
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

    # Per-configuration output directories
    foreach(config Debug Release RelWithDebInfo MinSizeRel Profile)
        string(TOUPPER ${config} config_upper)
        set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${config_upper} ${CMAKE_BINARY_DIR}/lib/${config})
        set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_${config_upper} ${CMAKE_BINARY_DIR}/lib/${config})
        set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_${config_upper} ${CMAKE_BINARY_DIR}/bin/${config})
    endforeach()

    # Test output directory
    set(TEST_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/tests)
endfunction()
```

#### **cmake/CompilerWarnings.cmake**
```cmake
# Compiler warning configuration
function(set_project_warnings)
    set(MSVC_WARNINGS
        /W4     # Baseline reasonable warnings
        /w14242 # 'identifier': conversion from 'type1' to 'type1', possible loss of data
        /w14254 # 'operator': conversion from 'type1:field_bits' to 'type2:field_bits', possible loss of data
        /w14263 # 'function': member function does not override any base class virtual member function
        /w14265 # 'classname': class has virtual functions, but destructor is not virtual
        /w14287 # 'operator': unsigned/negative constant mismatch
        /we4289 # nonstandard extension used: 'variable': loop control variable declared in the for-loop is used outside the for-loop scope
        /w14296 # 'operator': expression is always 'boolean_value'
        /w14311 # 'variable': pointer truncation from 'type1' to 'type2'
        /w14545 # expression before comma evaluates to a function which is missing an argument list
        /w14546 # function call before comma missing argument list
        /w14547 # 'operator': operator before comma has no effect; expected operator with side-effect
        /w14549 # 'operator': operator before comma has no effect; did you intend 'operator'?
        /w14555 # expression has no effect; expected expression with side-effect
        /w14619 # pragma warning: there is no warning number 'number'
        /w14640 # Enable warning on thread un-safe static member initialization
        /w14826 # Conversion from 'type1' to 'type2' is sign-extended. This may cause unexpected runtime behavior.
        /w14905 # wide string literal cast to 'LPSTR'
        /w14906 # string literal cast to 'LPWSTR'
        /w14928 # illegal copy-initialization; more than one user-defined conversion has been implicitly applied
    )

    set(CLANG_WARNINGS
        -Wall
        -Wextra
        -Wpedantic
        -Wshadow
        -Wnon-virtual-dtor
        -Wold-style-cast
        -Wcast-align
        -Wunused
        -Woverloaded-virtual
        -Wpedantic
        -Wconversion
        -Wsign-conversion
        -Wnull-dereference
        -Wdouble-promotion
        -Wformat=2
        -Wimplicit-fallthrough
    )

    set(GCC_WARNINGS
        ${CLANG_WARNINGS}
        -Wmisleading-indentation
        -Wduplicated-cond
        -Wduplicated-branches
        -Wlogical-op
        -Wuseless-cast
        -Wnull-dereference
        -Wduplicated-cond
    )

    if(WARNINGS_AS_ERRORS)
        set(CLANG_WARNINGS ${CLANG_WARNINGS} -Werror)
        set(GCC_WARNINGS ${GCC_WARNINGS} -Werror)
        set(MSVC_WARNINGS ${MSVC_WARNINGS} /WX)
    endif()

    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        set(PROJECT_WARNINGS ${MSVC_WARNINGS})
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
        set(PROJECT_WARNINGS ${CLANG_WARNINGS})
    elseif(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
        set(PROJECT_WARNINGS ${GCC_WARNINGS})
    else()
        message(AUTHOR_WARNING "No compiler warnings set for '${CMAKE_CXX_COMPILER_ID}' compiler")
    endif()

    add_library(project_warnings INTERFACE)
    add_library(project::warnings ALIAS project_warnings)
    
    target_compile_options(project_warnings INTERFACE ${PROJECT_WARNINGS})
    
    # Add to all targets
    if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
        target_compile_options(project_warnings INTERFACE /utf-8)
    endif()
endfunction()
```

#### **src/CMakeLists.txt**
```cmake
# Main source directory
add_subdirectory(core)
add_subdirectory(utils)
add_subdirectory(app)

# Create an umbrella target for all libraries
add_library(project_libs INTERFACE)
add_library(project::libs ALIAS project_libs)

target_link_libraries(project_libs
    INTERFACE
        project::core
        project::utils
)

# Installation of all libraries
install(TARGETS
        project_libs
        core
        utils
    EXPORT ProjectTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)
```

#### **src/core/CMakeLists.txt**
```cmake
# Core library
project(core VERSION ${PROJECT_VERSION})

# Create library
add_library(core)
add_library(project::core ALIAS core)

# Source files (using GLOB with CONFIGURE_DEPENDS)
file(GLOB_RECURSE CORE_SOURCES CONFIGURE_DEPENDS
    src/*.cpp
    src/*.c
)

file(GLOB_RECURSE CORE_HEADERS CONFIGURE_DEPENDS
    include/*.h
    include/*.hpp
)

target_sources(core
    PRIVATE
        ${CORE_SOURCES}
    PUBLIC
        FILE_SET HEADERS
        BASE_DIRS
            ${CMAKE_CURRENT_SOURCE_DIR}
            ${CMAKE_CURRENT_BINARY_DIR}
        FILES
            ${CORE_HEADERS}
)

# Include directories
target_include_directories(core
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        src
)

# Compile features
target_compile_features(core PUBLIC cxx_std_20)

# Link dependencies
target_link_libraries(core
    PUBLIC
        project::warnings
        Threads::Threads
    PRIVATE
        # Private dependencies
)

# Set properties
set_target_properties(core PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    OUTPUT_NAME "${PROJECT_NAME}-core"
    DEBUG_POSTFIX "-d"
    CXX_EXTENSIONS OFF
)

# Precompiled headers
if(ENABLE_PCH)
    target_precompile_headers(core
        PUBLIC
            <vector>
            <memory>
            <string>
            <iostream>
    )
endif()

# Generate export header
include(GenerateExportHeader)
generate_export_header(core
    BASE_NAME CORE
    EXPORT_FILE_NAME ${CMAKE_CURRENT_BINARY_DIR}/include/core/export.h
)

# Installation
install(TARGETS core
    EXPORT ProjectTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(
    DIRECTORY include/
    DESTINATION include
    FILE_PERMISSIONS OWNER_READ GROUP_READ WORLD_READ
)
```

#### **tests/CMakeLists.txt**
```cmake
# Test suite
if(BUILD_TESTING AND WITH_TESTS)
    enable_testing()
    
    # Find testing frameworks
    include(cmake/FindTesting.cmake)
    find_testing_frameworks()
    
    # Add test directories
    add_subdirectory(unit)
    add_subdirectory(integration)
    
    if(WITH_BENCHMARKS)
        add_subdirectory(benchmarks)
    endif()
    
    # Create test runner
    add_custom_target(run-tests
        COMMAND ctest --output-on-failure --label-exclude "benchmark"
        COMMENT "Running all tests"
    )
    
    # Create benchmark runner
    if(WITH_BENCHMARKS)
        add_custom_target(run-benchmarks
            COMMAND ctest --output-on-failure --label-regex "benchmark"
            COMMENT "Running benchmarks"
        )
    endif()
    
    # Create coverage target if enabled
    if(ENABLE_COVERAGE)
        add_custom_target(coverage
            COMMAND ${CMAKE_CTEST_COMMAND} --output-on-failure
            COMMAND ${CMAKE_COMMAND} -E echo "Generating coverage report..."
            COMMAND ${CMAKE_COMMAND} --build . --target coverage-report
            DEPENDS project::core project::utils
            COMMENT "Running tests and generating coverage report"
        )
    endif()
endif()
```

#### **CMakePresets.json**
```json
{
  "version": 4,
  "cmakeMinimumRequired": {
    "major": 3,
    "minor": 21
  },
  "configurePresets": [
    {
      "name": "default",
      "displayName": "Default Config",
      "description": "Default build with system packages",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/default",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "RelWithDebInfo",
        "CMAKE_EXPORT_COMPILE_COMMANDS": "ON",
        "BUILD_TESTING": "ON",
        "BUILD_EXAMPLES": "ON",
        "WARNINGS_AS_ERRORS": "ON"
      },
      "environment": {
        "CC": "gcc",
        "CXX": "g++"
      }
    },
    {
      "name": "debug",
      "displayName": "Debug",
      "description": "Debug build with sanitizers",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/debug",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug",
        "ENABLE_SANITIZERS": "ON",
        "ENABLE_COVERAGE": "ON",
        "CMAKE_EXPORT_COMPILE_COMMANDS": "ON"
      }
    },
    {
      "name": "release",
      "displayName": "Release",
      "description": "Release build with optimizations",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/release",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Release",
        "ENABLE_LTO": "ON",
        "ENABLE_IPO": "ON",
        "CMAKE_INTERPROCEDURAL_OPTIMIZATION": "ON"
      }
    },
    {
      "name": "conan-debug",
      "displayName": "Conan Debug",
      "description": "Debug build using Conan",
      "generator": "Ninja",
      "binaryDir": "${sourceDir}/build/conan-debug",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug",
        "CMAKE_TOOLCHAIN_FILE": "${sourceDir}/build/conan-debug/conan_toolchain.cmake"
      }
    },
    {
      "name": "msvc-release",
      "displayName": "MSVC Release",
      "description": "Release build with Visual Studio",
      "generator": "Visual Studio 17 2022",
      "architecture": "x64",
      "binaryDir": "${sourceDir}/build/msvc-release",
      "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Release"
      }
    }
  ],
  "buildPresets": [
    {
      "name": "default",
      "configurePreset": "default",
      "jobs": 8
    },
    {
      "name": "debug",
      "configurePreset": "debug"
    },
    {
      "name": "release",
      "configurePreset": "release",
      "targets": ["install"]
    }
  ],
  "testPresets": [
    {
      "name": "default",
      "configurePreset": "default",
      "output": {
        "outputOnFailure": true,
        "verbosity": "verbose"
      },
      "execution": {
        "noTestsAction": "error",
        "stopOnFailure": false
      }
    },
    {
      "name": "debug",
      "configurePreset": "debug",
      "configuration": "Debug",
      "output": {
        "outputOnFailure": true
      }
    }
  ],
  "packagePresets": [
    {
      "name": "default",
      "configurePreset": "release",
      "generators": [
        "DEB",
        "RPM",
        "TGZ"
      ]
    }
  ]
}
```

#### **GitHub Workflow: .github/workflows/ci.yml**
```yaml
name: CMake CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        build_type: [Debug, Release]
        cpp_standard: [17, 20]
    
    runs-on: ${{ matrix.os }}
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Configure CMake
      run: |
        cmake -B ${{github.workspace}}/build \
              -DCMAKE_BUILD_TYPE=${{ matrix.build_type }} \
              -DCMAKE_CXX_STANDARD=${{ matrix.cpp_standard }} \
              -DBUILD_TESTING=ON \
              -DENABLE_COVERAGE=${{ matrix.build_type == 'Debug' && matrix.os == 'ubuntu-latest' }}
    
    - name: Build
      run: cmake --build ${{github.workspace}}/build --config ${{ matrix.build_type }}
    
    - name: Test
      run: ctest --test-dir ${{github.workspace}}/build --output-on-failure
    
    - name: Upload Test Results
      if: always()
      uses: actions/upload-artifact@v3
      with:
        name: test-results-${{ matrix.os }}-${{ matrix.build_type }}
        path: ${{github.workspace}}/build/Testing/**/*.xml
    
    - name: Generate Coverage
      if: matrix.build_type == 'Debug' && matrix.os == 'ubuntu-latest'
      run: |
        cd ${{github.workspace}}/build
        make coverage-report
    
    - name: Upload Coverage
      if: matrix.build_type == 'Debug' && matrix.os == 'ubuntu-latest'
      uses: codecov/codecov-action@v3
  
  analyze:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Install dependencies
      run: |
        sudo apt-get update
        sudo apt-get install -y clang-tidy cppcheck
    
    - name: Configure with analyzers
      run: |
        cmake -B build \
              -DCMAKE_BUILD_TYPE=Debug \
              -DCMAKE_CXX_CLANG_TIDY=clang-tidy \
              -DCMAKE_CXX_CPPCHECK=cppcheck
    
    - name: Build with analyzers
      run: cmake --build build --parallel
  
  package:
    needs: build
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Configure for packaging
      run: cmake -B build -DCMAKE_BUILD_TYPE=Release
    
    - name: Build
      run: cmake --build build --target install
    
    - name: Create packages
      run: |
        cd build
        cpack -G DEB
        cpack -G RPM
        cpack -G TGZ
    
    - name: Upload packages
      uses: actions/upload-artifact@v3
      with:
        name: packages
        path: build/*.deb build/*.rpm build/*.tar.gz
```

#### **Build and Development Commands**
```bash
# Using CMake Presets (recommended)
cmake --preset default
cmake --build --preset default
ctest --preset default

# Debug build with sanitizers
cmake --preset debug
cmake --build --preset debug

# Release build with optimizations
cmake --preset release
cmake --build --preset release

# Run all checks
cmake --build --preset default --target check-all

# Generate documentation
cmake --build --preset default --target docs

# Create packages
cpack --preset default

# Install project
cmake --build --preset default --target install

# Clean build
cmake --build --preset default --target clean

# Format code
cmake --build --preset default --target format

# Run static analysis
cmake --build --preset default --target analyze
```

#### **Production Checklist**

1. **✅ Project Structure**
   - [ ] Logical separation of concerns
   - [ ] Clear directory hierarchy
   - [ ] Consistent naming conventions

2. **✅ CMake Configuration**
   - [ ] Modern CMake (3.21+)
   - [ ] Proper policy settings
   - [ ] Standard compliance settings
   - [ ] Output directory organization

3. **✅ Dependency Management**
   - [ ] External dependencies handled
   - [ ] Version pinning
   - [ ] Fallback mechanisms
   - [ ] Private dependency support

4. **✅ Testing**
   - [ ] Unit tests
   - [ ] Integration tests
   - [ ] Benchmark tests
   - [ ] Test fixtures
   - [ ] Coverage reporting

5. **✅ Code Quality**
   - [ ] Compiler warnings
   - [ ] Static analysis
   - [ ] Formatting
   - [ ] Sanitizers

6. **✅ Packaging**
   - [ ] Installation rules
   - [ ] CPack configuration
   - [ ] Multiple package formats
   - [ ] Component-based packaging

7. **✅ Documentation**
   - [ ] API documentation
   - [ ] Installation instructions
   - [ ] Contribution guidelines
   - [ ] Code comments

8. **✅ CI/CD**
   - [ ] Automated builds
   - [ ] Automated testing
   - [ ] Automated packaging
   - [ ] Quality gates

9. **✅ Cross-Platform**
   - [ ] Windows support
   - [ ] Linux support
   - [ ] macOS support
   - [ ] Cross-compilation

10. **✅ Performance**
    - [ ] Optimized builds
    - [ ] LTO support
    - [ ] PCH support
    - [ ] Profiling builds

#### **Troubleshooting Common Issues**

1. **Dependency Conflicts**
```bash
# Clear CMake cache
rm -rf build
# Use specific package manager
cmake -B build -DCMAKE_TOOLCHAIN_FILE=vcpkg.cmake
```

2. **Compiler Compatibility**
```bash
# Specify compiler
CC=gcc-11 CXX=g++-11 cmake -B build
# Check compiler features
cmake -B build -DCMAKE_CXX_COMPILER_FEATURES=cxx_std_20
```

3. **Build Performance**
```bash
# Use Ninja generator
cmake -B build -G Ninja
# Enable unity builds
cmake -B build -DCMAKE_UNITY_BUILD=ON
# Use CCache
cmake -B build -DCMAKE_CXX_COMPILER_LAUNCHER=ccache
```

4. **Package Generation Issues**
```bash
# Check CPack configuration
cpack --debug --verbose
# Verify installation
cmake --build build --target install --verbose
```

#### **Final Best Practices Summary**

1. **Always use target-based CMake**: `target_include_directories()`, `target_link_libraries()`
2. **Export your targets**: Make your project usable by others
3. **Use modern CMake features**: FetchContent, generator expressions, CMakePresets
4. **Support multiple package managers**: vcpkg, Conan, system packages
5. **Implement comprehensive testing**: Unit, integration, benchmarks
6. **Enable code quality tools**: Static analysis, formatting, sanitizers
7. **Provide good documentation**: README, API docs, examples
8. **Set up CI/CD**: Automated builds, tests, and deployments
9. **Support cross-platform**: Windows, Linux, macOS
10. **Optimize for performance**: LTO, PCH, unity builds

This completes the 20-part CMake course! You now have the knowledge to create production-ready CMake projects from simple executables to complex multi-component systems with proper dependency management, testing, packaging, and CI/CD integration.