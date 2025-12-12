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