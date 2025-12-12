# CMAKE_BASIC

Perfect! Let’s start **Part 1** of your complete CMake course. This part will build your foundation step by step.

---

## **CMake Course – Part 1: Introduction and Setting Up Your First Project**

### **Topic:** Introduction to CMake and Creating a Basic Project

### **1. What is CMake?**

CMake is a **cross-platform build system generator**. It doesn’t compile code itself but generates build files for tools like:

* `Makefiles` (Linux/macOS)
* `Visual Studio solutions` (Windows)
* `Ninja` builds

**Why CMake?**

* Handles complex projects easily.
* Supports multiple platforms.
* Simplifies linking libraries and dependencies.
* Encourages modern C++ project structure.

---

### **2. Installing CMake**

* **Linux (Ubuntu/Debian):**

```bash
sudo apt update
sudo apt install cmake build-essential
```

* **Windows:** Download from [cmake.org](https://cmake.org/download/) and add it to your PATH.

* **macOS:**

```bash
brew install cmake
```

Check installation:

```bash
cmake --version
```

---

### **3. Your First CMake Project**

**Directory structure:**

```
HelloCMake/
├── CMakeLists.txt
└── main.cpp
```

**main.cpp**

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, CMake!" << std::endl;
    return 0;
}
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)  # Minimum CMake version
project(HelloCMake VERSION 1.0)        # Project name and version

add_executable(hello main.cpp)         # Create executable from main.cpp
```

---

### **4. Step-by-Step Build Instructions**

1. Open terminal in `HelloCMake` directory.
2. Create a **build directory** (good practice):

```bash
mkdir build
cd build
```

3. Generate build files:

```bash
cmake ..
```

4. Build the project:

```bash
cmake --build .
```

5. Run the program:

```bash
./hello    # Linux/macOS
hello.exe  # Windows
```

---

### **5. Concepts Learned**

* `cmake_minimum_required` → ensures compatibility.
* `project` → declares project name and version.
* `add_executable` → defines an executable target.
* Out-of-source build (`build/` folder) keeps the source clean.

---

### **6. Beginner Notes**

* Always use an out-of-source build.
* Use a recent CMake version (≥3.16) for modern features.
* CMake commands are case-insensitive but usually written in lowercase.

---

### **7. Advanced Tips**

* You can set C++ standard:

```cmake
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```

* Use `cmake -DCMAKE_BUILD_TYPE=Release ..` to specify build type (Debug/Release).

---

✅ **Next Step:** After mastering this, we’ll dive into **Part 2: Variables, Targets, and Basic Configuration**—handling multiple files and organizing your project.

---

If you want, I can give **Part 2** next with full detailed examples. Do you want me to continue?


Great! Let’s move on to **Part 2**. This part introduces **variables, targets, and basic project configuration**—essential for managing larger projects.

---

## **CMake Course – Part 2: Variables, Targets, and Basic Project Configuration**

### **Topic:** Using CMake Variables and Organizing Multiple Files

---

### **1. Understanding CMake Variables**

CMake allows you to define **variables** to avoid repeating paths or values.

**Example:**

```cmake
set(SOURCE_FILES main.cpp utils.cpp)
add_executable(hello ${SOURCE_FILES})
```

* `${SOURCE_FILES}` → expands to `main.cpp utils.cpp`.
* Variables are **case-sensitive** in CMake.
* Use `set(VAR_NAME value)` to define variables.
* Access with `${VAR_NAME}`.

---

### **2. Adding Multiple Source Files**

**Project structure:**

```
MyProject/
├── CMakeLists.txt
├── main.cpp
└── utils.cpp
```

**main.cpp**

```cpp
#include <iostream>
#include "utils.h"

int main() {
    printMessage();
    return 0;
}
```

**utils.cpp**

```cpp
#include <iostream>
#include "utils.h"

void printMessage() {
    std::cout << "Hello from utils!" << std::endl;
}
```

**utils.h**

```cpp
#pragma once
void printMessage();
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(MyProject VERSION 1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SOURCES main.cpp utils.cpp)
add_executable(my_app ${SOURCES})
```

---

### **3. Configuring Include Directories**

For larger projects, headers may reside in separate folders.

**Directory:**

```
MyProject/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── utils.cpp
└── include/
    └── utils.h
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(MyProject VERSION 1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include_directories(include)

set(SOURCES src/main.cpp src/utils.cpp)
add_executable(my_app ${SOURCES})
```

* `include_directories()` tells CMake where to find headers.
* Modern CMake prefers **target-based include directories** (covered in Part 3).

---

### **4. Step-by-Step Build Instructions**

1. Create a build directory:

```bash
mkdir build
cd build
```

2. Generate build files:

```bash
cmake ..
```

3. Build the project:

```bash
cmake --build .
```

4. Run the executable:

```bash
./my_app   # Linux/macOS
my_app.exe # Windows
```

---

### **5. Best Practices**

* Group source files logically.
* Avoid hardcoding paths—use variables.
* Use `src/` and `include/` folders for organization.
* Keep CMakeLists.txt minimal and readable.

---

### **6. Beginner Notes**

* `set()` is your friend for managing file lists and flags.
* Always check paths carefully.
* CMakeLists.txt is processed **top-down**, so define variables before use.

---

### **7. Advanced Tips**

* Use **`target_include_directories()`** instead of `include_directories()` for better modularity (we’ll cover in Part 3).
* CMake supports **conditional compilation**:

```cmake
if(MSVC)
    add_definitions(-DWIN32_LEAN_AND_MEAN)
endif()
```

* Use `message(STATUS "Variable=${VAR}")` for debugging.

---

✅ **Next Step:** In **Part 3**, we’ll cover **target-based commands, linking libraries, and modern CMake practices**. This is where your project becomes scalable and maintainable.

---

Do you want me to continue with **Part 3** now?


Perfect! Let’s dive into **Part 3**, where we introduce **modern CMake practices, targets, and library linking**—a crucial step for scalable projects.

---

## **CMake Course – Part 3: Targets, Modern CMake, and Linking Libraries**

### **Topic:** Using Targets and Linking Libraries in Modern CMake

---

### **1. What Are Targets?**

In modern CMake, **everything revolves around targets**—executables or libraries you define:

```cmake
add_executable(my_app main.cpp)
add_library(my_lib STATIC utils.cpp)
```

* `my_app` → an executable target.
* `my_lib` → a static library target (will generate `.a` or `.lib` file).
* Targets allow **fine-grained control** over properties like include directories, compiler options, and dependencies.

---

### **2. Target-Based Include Directories**

Instead of global `include_directories()`, use:

```cmake
target_include_directories(my_app PRIVATE include)
```

**Explanation of keywords:**

| Keyword   | Meaning                                              |
| --------- | ---------------------------------------------------- |
| PRIVATE   | Only for this target.                                |
| PUBLIC    | Used by this target and any target that links to it. |
| INTERFACE | Only for targets that link to this target.           |

**Example:**

```cmake
add_library(utils_lib STATIC src/utils.cpp)
target_include_directories(utils_lib PUBLIC include)
```

* Any target linking `utils_lib` automatically inherits `include/`.

---

### **3. Linking Libraries**

#### **Static Library**

```cmake
add_library(utils_lib STATIC src/utils.cpp)
target_include_directories(utils_lib PUBLIC include)

add_executable(my_app src/main.cpp)
target_link_libraries(my_app PRIVATE utils_lib)
```

* Generates `libutils_lib.a` (Linux/macOS) or `utils_lib.lib` (Windows).
* `my_app` can now call functions from `utils.cpp`.

#### **Dynamic (Shared) Library**

```cmake
add_library(utils_lib SHARED src/utils.cpp)
target_include_directories(utils_lib PUBLIC include)
```

* Generates `libutils_lib.so` (Linux), `utils_lib.dll` (Windows), or `libutils_lib.dylib` (macOS).
* Dynamic libraries are loaded at runtime.

---

### **4. Full Example: Modern CMake Project**

**Directory structure:**

```
ModernCMake/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── utils.cpp
└── include/
    └── utils.h
```

**main.cpp**

```cpp
#include "utils.h"

int main() {
    printMessage();
    return 0;
}
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(ModernCMake VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Create library
add_library(utils_lib STATIC src/utils.cpp)
target_include_directories(utils_lib PUBLIC include)

# Create executable
add_executable(my_app src/main.cpp)

# Link library to executable
target_link_libraries(my_app PRIVATE utils_lib)
```

---

### **5. Step-by-Step Build**

1. Create build directory:

```bash
mkdir build
cd build
```

2. Generate build files:

```bash
cmake ..
```

3. Build the project:

```bash
cmake --build .
```

4. Run:

```bash
./my_app   # Linux/macOS
my_app.exe # Windows
```

---

### **6. Best Practices**

* Always prefer **target-based commands** (`target_include_directories`, `target_link_libraries`).
* Avoid global variables that affect all targets.
* Separate executable and library targets for modularity.
* Name your targets clearly (`utils_lib`, `graphics_lib`).

---

### **7. Beginner Notes**

* `STATIC` → library compiled into the executable.
* `SHARED` → dynamic library, requires runtime presence.
* `target_link_libraries()` can use `PRIVATE`, `PUBLIC`, `INTERFACE` to control visibility.

---

### **8. Advanced Tips**

* You can link **third-party libraries** the same way:

```cmake
find_package(OpenCV REQUIRED)
target_link_libraries(my_app PRIVATE OpenCV::Core OpenCV::ImgProc)
```

* Use **generator expressions** for conditional linking:

```cmake
target_link_libraries(my_app PRIVATE $<$<CONFIG:Debug>:debug_lib>)
```

---

✅ **Next Step:** In **Part 4**, we’ll cover **CMake build types, compiler flags, and platform-specific settings**, so your project can compile cleanly on any system.

---

Do you want me to continue with **Part 4** now?


Great! Let’s move on to **Part 4**, where we focus on **build types, compiler flags, and platform-specific settings**—essential for professional, cross-platform C++ projects.

---

## **CMake Course – Part 4: Build Types, Compiler Flags, and Platform-Specific Settings**

### **Topic:** Controlling Builds and Optimizing Compiler Behavior

---

### **1. Build Types in CMake**

CMake supports multiple **build types**:

| Build Type     | Purpose                                     |
| -------------- | ------------------------------------------- |
| Debug          | Includes debug symbols, no optimizations.   |
| Release        | Optimized code for speed, no debug symbols. |
| RelWithDebInfo | Optimized but includes debug symbols.       |
| MinSizeRel     | Optimized for small binary size.            |

Set build type when configuring:

```bash
cmake -DCMAKE_BUILD_TYPE=Release ..
```

Check current build type in CMakeLists.txt:

```cmake
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
```

---

### **2. Setting Compiler Flags**

CMake allows **per-target or global compiler flags**.

**Global flags:**

```cmake
set(CMAKE_CXX_FLAGS_DEBUG "-g -O0")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")
```

**Target-specific flags (modern CMake preferred):**

```cmake
add_executable(my_app src/main.cpp)
target_compile_options(my_app PRIVATE
    $<$<CONFIG:Debug>:-Wall -Wextra -g>
    $<$<CONFIG:Release>:-O3>
)
```

* `$<CONFIG:Debug>` → only applies when building Debug.
* `$<CONFIG:Release>` → only applies when building Release.
* `PRIVATE` → affects this target only.

---

### **3. Platform-Specific Settings**

Use `if()` statements to differentiate compilers and platforms.

**Example:**

```cmake
if(MSVC)
    message(STATUS "Using Visual Studio compiler")
    add_compile_options(/W4 /permissive-)
else()
    message(STATUS "Using GCC or Clang")
    add_compile_options(-Wall -Wextra -pedantic)
endif()
```

* `MSVC` → Visual Studio compiler.
* `CMAKE_CXX_COMPILER_ID` → can be used to check compiler type (`GNU`, `Clang`, `MSVC`).

---

### **4. Full Example: Build Types and Flags**

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(BuildExample VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Build type message
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif()
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

# Platform-specific flags
if(MSVC)
    add_compile_options(/W4 /permissive-)
else()
    add_compile_options(-Wall -Wextra -pedantic)
endif()

# Sources
add_executable(my_app src/main.cpp)
```

---

### **5. Step-by-Step Build Instructions**

1. Create build directory:

```bash
mkdir build
cd build
```

2. Configure with build type:

```bash
cmake -DCMAKE_BUILD_TYPE=Release ..
```

3. Build:

```bash
cmake --build .
```

4. Run:

```bash
./my_app   # Linux/macOS
my_app.exe # Windows
```

---

### **6. Best Practices**

* Always explicitly set `CMAKE_BUILD_TYPE` for single-configuration generators (like Makefiles or Ninja).
* Use **target_compile_options** instead of global flags whenever possible.
* Use platform checks for portability across Windows, Linux, and macOS.

---

### **7. Beginner Notes**

* Debug builds are slower but easier to debug.
* Release builds are faster but harder to debug.
* You can mix and match optimization and debugging using `RelWithDebInfo`.

---

### **8. Advanced Tips**

* Use **generator expressions** for more flexible flag assignment:

```cmake
target_compile_options(my_app PRIVATE $<$<CONFIG:RelWithDebInfo>:-O2 -g>)
```

* Combine flags with **CMake presets** for reusable build configurations (`CMakePresets.json`).

---

✅ **Next Step:** In **Part 5**, we’ll cover **organizing larger projects with subdirectories and `add_subdirectory()`**, which is essential when your project grows.

---

Do you want me to continue with **Part 5** now?


Perfect! Let’s move on to **Part 5**, where we cover **organizing larger projects using subdirectories and `add_subdirectory()`**—a key practice for scalable CMake projects.

---

## **CMake Course – Part 5: Organizing Projects with Subdirectories**

### **Topic:** Structuring Larger Projects Using `add_subdirectory()`

---

### **1. Why Subdirectories?**

As your project grows, keeping **all code and CMake commands in a single file** becomes unmanageable.

**Benefits of subdirectories:**

* Modularizes your code.
* Keeps CMakeLists.txt files small and readable.
* Allows independent libraries to be developed and linked.
* Simplifies testing and reusability.

---

### **2. Example Project Structure**

```
MyBigProject/
├── CMakeLists.txt          # Root CMake file
├── src/
│   ├── CMakeLists.txt      # Source subdirectory
│   ├── main.cpp
│   └── app.cpp
└── lib/
    ├── CMakeLists.txt      # Library subdirectory
    ├── utils.cpp
    └── utils.h
```

---

### **3. Root CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(MyBigProject VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Include subdirectories
add_subdirectory(lib)
add_subdirectory(src)
```

* `add_subdirectory(lib)` → processes `lib/CMakeLists.txt`.
* `add_subdirectory(src)` → processes `src/CMakeLists.txt`.

---

### **4. Library Subdirectory (lib/CMakeLists.txt)**

```cmake
# Create static library
add_library(utils_lib STATIC utils.cpp)

# Set include directories for this library
target_include_directories(utils_lib PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
```

* `${CMAKE_CURRENT_SOURCE_DIR}` → path to current folder (`lib/`).
* `PUBLIC` → any target linking this library will inherit include path.

---

### **5. Source Subdirectory (src/CMakeLists.txt)**

```cmake
# Create executable
add_executable(my_app main.cpp app.cpp)

# Link to library
target_link_libraries(my_app PRIVATE utils_lib)
```

* Keeps executable definition separate from library.
* Makes linking clean and modular.

---

### **6. Step-by-Step Build**

1. Create build directory:

```bash
mkdir build
cd build
```

2. Configure:

```bash
cmake ..
```

3. Build:

```bash
cmake --build .
```

4. Run:

```bash
./src/my_app   # Linux/macOS
src\my_app.exe # Windows
```

---

### **7. Best Practices**

* Use `add_subdirectory()` for each logical module (lib, src, tests, examples).
* Keep library code separate from executables.
* Use `target_include_directories()` and `target_link_libraries()` for clean modularity.

---

### **8. Beginner Notes**

* `CMAKE_CURRENT_SOURCE_DIR` and `CMAKE_CURRENT_BINARY_DIR` help with relative paths inside subdirectories.
* Subdirectories can have their **own CMakeLists.txt** for easier management.

---

### **9. Advanced Tips**

* You can create **nested subdirectories** for even bigger projects:

```
lib/
└── graphics/
    ├── CMakeLists.txt
    ├── renderer.cpp
    └── renderer.h
```

* Use `target_link_libraries(graphics_lib PUBLIC core_lib)` to build a **dependency graph** between libraries.
* Avoid circular dependencies—CMake will fail if two libraries depend on each other.

---

✅ **Next Step:** In **Part 6**, we’ll cover **building and linking external third-party libraries**, which is crucial for using packages like OpenCV, Boost, or Eigen.

---

Do you want me to continue with **Part 6** now?


Great! Let’s move on to **Part 6**, where we tackle **integrating external libraries**, a crucial skill for real-world projects.

---

## **CMake Course – Part 6: Using External Libraries**

### **Topic:** Finding, Linking, and Using Third-Party Libraries

---

### **1. Why External Libraries?**

Many projects rely on existing libraries like:

* **Boost** → Utilities, filesystem, smart pointers.
* **OpenCV** → Computer vision.
* **Eigen** → Linear algebra.
* **SDL** → Graphics and input.

CMake simplifies **finding and linking** these libraries.

---

### **2. Using `find_package()`**

CMake provides `find_package()` to locate installed libraries.

**Example: Using Boost**

```cmake
cmake_minimum_required(VERSION 3.16)
project(ExternalLibExample VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find Boost
find_package(Boost 1.80 REQUIRED COMPONENTS filesystem system)

add_executable(my_app src/main.cpp)

# Link Boost libraries
target_link_libraries(my_app PRIVATE Boost::filesystem Boost::system)
```

* `REQUIRED` → stops configuration if Boost is not found.
* `COMPONENTS` → specifies needed modules.
* `Boost::filesystem` → modern target for linking.

---

### **3. Using `find_library()` and `find_path()`**

If `find_package()` is unavailable, locate manually:

```cmake
find_path(EIGEN_INCLUDE_DIR Eigen/Dense)
find_library(EIGEN_LIB NAMES eigen3)

add_executable(my_app src/main.cpp)
target_include_directories(my_app PRIVATE ${EIGEN_INCLUDE_DIR})
target_link_libraries(my_app PRIVATE ${EIGEN_LIB})
```

* `find_path()` → locates include directories.
* `find_library()` → locates library binaries.

---

### **4. Using `FetchContent` to Download Libraries**

CMake 3.14+ supports downloading dependencies at configure time:

```cmake
include(FetchContent)

FetchContent_Declare(
    spdlog
    GIT_REPOSITORY https://github.com/gabime/spdlog.git
    GIT_TAG v1.11.0
)

FetchContent_MakeAvailable(spdlog)

add_executable(my_app src/main.cpp)
target_link_libraries(my_app PRIVATE spdlog::spdlog)
```

* Automatically downloads, configures, and makes the library available.
* Eliminates manual installation steps.

---

### **5. Full Example: OpenCV Integration**

```cmake
cmake_minimum_required(VERSION 3.16)
project(OpenCVExample LANGUAGES CXX)

find_package(OpenCV REQUIRED)

add_executable(my_app src/main.cpp)
target_link_libraries(my_app PRIVATE ${OpenCV_LIBS})
target_include_directories(my_app PRIVATE ${OpenCV_INCLUDE_DIRS})
```

* `OpenCV_LIBS` and `OpenCV_INCLUDE_DIRS` come from `find_package()`.

---

### **6. Step-by-Step Build**

1. Make sure the library is installed (e.g., `sudo apt install libboost-all-dev` for Boost).
2. Create build directory:

```bash
mkdir build
cd build
```

3. Configure project:

```bash
cmake ..
```

4. Build:

```bash
cmake --build .
```

5. Run your application.

---

### **7. Best Practices**

* Prefer `find_package()` over `find_library()` when available.
* Use **modern CMake targets** provided by libraries (`Boost::filesystem`, `spdlog::spdlog`).
* Keep external dependencies **versioned and explicit**.

---

### **8. Beginner Notes**

* Make sure libraries are installed on your system or provide path hints:

```bash
cmake -DBOOST_ROOT=/path/to/boost ..
```

* Linking errors often occur due to missing `target_link_libraries()`.

---

### **9. Advanced Tips**

* Use `FetchContent` for **self-contained builds**, useful in CI/CD.
* Combine **submodules + FetchContent** for large projects.
* Prefer **static linking** for deployment when using small libraries; dynamic linking for large, shared libraries.

---

✅ **Next Step:** In **Part 7**, we’ll cover **creating and installing your own libraries**, including export targets for reuse in other projects.

---

Do you want me to continue with **Part 7** now?


Excellent! Let’s move on to **Part 7**, where we cover **creating, installing, and exporting your own libraries**—a key step for reusable CMake projects.

---

## **CMake Course – Part 7: Creating and Installing Your Own Libraries**

### **Topic:** Building Reusable Libraries and Exporting Targets

---

### **1. Library Types Recap**

CMake supports:

* **STATIC** → compiled into the executable (`.a` or `.lib`).
* **SHARED** → dynamic library loaded at runtime (`.so`, `.dll`, `.dylib`).
* **INTERFACE** → header-only library, no compiled output.

We will create a static library for this example.

---

### **2. Project Structure**

```
MyLibraryProject/
├── CMakeLists.txt          # Root
├── include/
│   └── mylib.h
└── src/
    └── mylib.cpp
```

**mylib.h**

```cpp
#pragma once
void greet();
```

**mylib.cpp**

```cpp
#include <iostream>
#include "mylib.h"

void greet() {
    std::cout << "Hello from MyLibrary!" << std::endl;
}
```

---

### **3. Root CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(MyLibraryProject VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Create library
add_library(mylib STATIC src/mylib.cpp)

# Specify include directories
target_include_directories(mylib PUBLIC include)

# Install library and headers
install(TARGETS mylib
        EXPORT MyLibTargets
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include)

install(DIRECTORY include/ DESTINATION include)

# Export CMake package configuration
install(EXPORT MyLibTargets
        FILE MyLibTargets.cmake
        NAMESPACE MyLib::
        DESTINATION lib/cmake/MyLib)
```

---

### **4. Step-by-Step Explanation**

1. **`add_library()`** → defines your library.
2. **`target_include_directories()`** → makes headers accessible to other targets.
3. **`install(TARGETS …)`** → installs the compiled library to `lib/` or `bin/`.
4. **`install(DIRECTORY …)`** → installs headers to `include/`.
5. **`install(EXPORT …)`** → creates a CMake package file to allow other projects to `find_package(MyLib)`.

---

### **5. Creating an Executable Using the Library**

```
ExampleApp/
├── CMakeLists.txt
└── main.cpp
```

**main.cpp**

```cpp
#include "mylib.h"

int main() {
    greet();
    return 0;
}
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(ExampleApp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find MyLib package
find_package(MyLib REQUIRED PATHS "/path/to/installed/MyLib")

add_executable(example main.cpp)
target_link_libraries(example PRIVATE MyLib::mylib)
```

* `MyLib::mylib` comes from the exported targets.
* `PATHS` points to the folder containing `lib/cmake/MyLib`.

---

### **6. Step-by-Step Build & Install**

1. Build the library:

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

2. Install library and headers:

```bash
cmake --install . --prefix /your/install/path
```

3. Build your executable linking the installed library:

```bash
mkdir ../example_build
cd ../example_build
cmake -DCMAKE_PREFIX_PATH=/your/install/path ..
cmake --build .
./example
```

---

### **7. Best Practices**

* Always **export targets** for reusable libraries.
* Keep headers separate in `include/`.
* Use `PUBLIC` for headers that consumers need.
* Version your library for proper dependency management.

---

### **8. Beginner Notes**

* Use `STATIC` libraries first; dynamic linking adds platform-specific complexity.
* Installing to a known path helps `find_package()` in other projects.

---

### **9. Advanced Tips**

* Use **`CMakePackageConfigHelpers`** to generate proper versioned config files (`MyLibConfig.cmake`).
* Support **both static and shared builds** via `option(BUILD_SHARED_LIBS "Build shared library" OFF)`.
* Use **namespace targets** (`MyLib::mylib`) to avoid name conflicts.

---

✅ **Next Step:** In **Part 8**, we’ll cover **CMake testing with `CTest`**, including unit testing integration and best practices.

---

Do you want me to continue with **Part 8** now?



Perfect! Let’s move on to **Part 8**, where we cover **testing in CMake using `CTest` and integrating unit tests**—essential for reliable projects.

---

## **CMake Course – Part 8: Testing with CTest**

### **Topic:** Adding Unit Tests and Running Them with CMake

---

### **1. Why Testing in CMake?**

* Ensures your code works correctly.
* Catch regressions early.
* Automates verification during builds and CI/CD pipelines.

CMake provides a **built-in testing tool called `CTest`**.

---

### **2. Enabling Testing**

In your root `CMakeLists.txt`:

```cmake
cmake_minimum_required(VERSION 3.16)
project(TestExample VERSION 1.0 LANGUAGES CXX)

enable_testing()   # Enable CTest
```

* Must call `enable_testing()` **before adding tests**.

---

### **3. Adding a Unit Test**

Suppose you have a library `mylib`:

**lib/mylib.h**

```cpp
#pragma once
int add(int a, int b);
```

**lib/mylib.cpp**

```cpp
#include "mylib.h"
int add(int a, int b) { return a + b; }
```

---

**Create a test executable:**

**test/test_add.cpp**

```cpp
#include "mylib.h"
#include <cassert>

int main() {
    assert(add(2, 3) == 5);
    assert(add(-1, 1) == 0);
    return 0;
}
```

---

### **4. CMakeLists.txt for Tests**

```cmake
# Add test executable
add_executable(test_add test/test_add.cpp)
target_link_libraries(test_add PRIVATE mylib)

# Add test to CTest
add_test(NAME AddTest COMMAND test_add)
```

* `add_test(NAME … COMMAND …)` → registers executable as a test.
* Tests can be run with `ctest` or integrated into CI.

---

### **5. Step-by-Step Build & Run Tests**

1. Create build directory:

```bash
mkdir build
cd build
cmake ..
```

2. Build:

```bash
cmake --build .
```

3. Run all tests:

```bash
ctest
```

* Run verbose:

```bash
ctest -V
```

* Only run a specific test:

```bash
ctest -R AddTest
```

---

### **6. Using GoogleTest (Optional Advanced Testing)**

CMake can integrate **GoogleTest** for richer unit testing.

**CMakeLists.txt**

```cmake
include(FetchContent)

FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG v1.15.0
)

FetchContent_MakeAvailable(googletest)

enable_testing()

add_executable(test_add test/test_add.cpp)
target_link_libraries(test_add PRIVATE gtest_main mylib)

include(GoogleTest)
gtest_discover_tests(test_add)
```

* Automatically detects and runs all GoogleTest tests.
* Provides richer assertions and test reporting.

---

### **7. Best Practices**

* Keep tests in a **separate `tests/` folder**.
* Use **unit tests for small functions** and **integration tests** for components.
* Always add `enable_testing()` at the top of the project.
* Prefer **modern CMake target linking** for test executables.

---

### **8. Beginner Notes**

* `ctest` runs tests after building—no need for separate scripts.
* Each test executable should **return 0 for success**, non-zero for failure.

---

### **9. Advanced Tips**

* Combine CTest with **CTest dashboard** for CI/CD integration.
* Use **tags** in tests:

```cmake
set_tests_properties(AddTest PROPERTIES LABELS "unit")
ctest -L unit   # run only unit tests
```

* Parallel test execution:

```bash
ctest -j4
```

---

✅ **Next Step:** In **Part 9**, we’ll cover **cross-platform builds and toolchain files**, essential for building your project on multiple OSs or custom compilers.

---

Do you want me to continue with **Part 9** now?



Great! Let’s move on to **Part 9**, focusing on **cross-platform builds and toolchain files**—essential when your project must compile on different OSs or with custom compilers.

---

## **CMake Course – Part 9: Cross-Platform Builds and Toolchains**

### **Topic:** Building Projects on Multiple Platforms and Using Toolchain Files

---

### **1. Why Cross-Platform Builds?**

CMake is designed for **multi-platform projects**. Key challenges include:

* Different compilers (GCC, Clang, MSVC).
* Different library paths and include directories.
* Platform-specific compiler flags.

**Solution:** Use **toolchain files** and CMake variables to configure builds consistently.

---

### **2. Toolchain Files**

A **toolchain file** defines:

* Compiler paths
* System root and libraries
* Flags and CMake variables

**Example: `gcc_toolchain.cmake`**

```cmake
# Specify compilers
set(CMAKE_C_COMPILER /usr/bin/gcc)
set(CMAKE_CXX_COMPILER /usr/bin/g++)

# Set build type
set(CMAKE_BUILD_TYPE Release)

# Optional: specify sysroot or other environment
# set(CMAKE_SYSROOT /path/to/sysroot)
```

---

### **3. Using a Toolchain File**

```bash
mkdir build
cd build
cmake -DCMAKE_TOOLCHAIN_FILE=../gcc_toolchain.cmake ..
cmake --build .
```

* Ensures the project always uses specified compilers and settings.
* Useful for **embedded systems or cross-compilation**.

---

### **4. Platform-Specific Flags in CMakeLists.txt**

Use `if()` statements to detect platform:

```cmake
if(WIN32)
    message(STATUS "Windows platform detected")
    add_compile_options(/W4)
elseif(UNIX)
    message(STATUS "Unix/Linux/Mac platform detected")
    add_compile_options(-Wall -Wextra -pedantic)
elseif(APPLE)
    message(STATUS "macOS platform detected")
endif()
```

* `WIN32` → Windows
* `UNIX` → Linux/macOS
* `APPLE` → macOS

---

### **5. Cross-Compilation Example (Linux → ARM)**

Toolchain file: `arm_toolchain.cmake`

```cmake
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)

set(CMAKE_C_COMPILER /usr/bin/arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER /usr/bin/arm-linux-gnueabihf-g++)
```

Configure project:

```bash
cmake -DCMAKE_TOOLCHAIN_FILE=arm_toolchain.cmake ..
cmake --build .
```

* Compiles for ARM instead of host machine.
* Works with libraries compiled for target architecture.

---

### **6. Full Example: Cross-Platform Project**

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(CrossPlatformExample LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(my_app src/main.cpp)

if(WIN32)
    target_compile_definitions(my_app PRIVATE PLATFORM_WINDOWS)
elseif(UNIX AND NOT APPLE)
    target_compile_definitions(my_app PRIVATE PLATFORM_LINUX)
elseif(APPLE)
    target_compile_definitions(my_app PRIVATE PLATFORM_MAC)
endif()
```

* Use `PLATFORM_WINDOWS`, `PLATFORM_LINUX`, `PLATFORM_MAC` macros in code for conditional compilation.

---

### **7. Best Practices**

* Always prefer **toolchain files** for non-standard setups or cross-compilation.
* Detect platforms using built-in CMake variables (`WIN32`, `UNIX`, `APPLE`).
* Avoid hardcoding paths; let toolchains handle compilers and sysroots.

---

### **8. Beginner Notes**

* Toolchain files are **optional** for normal desktop builds but essential for cross-platform.
* CMake automatically detects most desktop compilers if no toolchain is provided.

---

### **9. Advanced Tips**

* Combine **toolchain files + FetchContent** to fetch platform-specific dependencies automatically.
* Use `CMAKE_SYSTEM_VERSION` to target specific OS versions.
* For CI/CD, provide toolchain files for **reproducible builds**.

---

✅ **Next Step:** In **Part 10**, we’ll cover **installing and exporting CMake projects with versioned packages**, so other developers can easily use your libraries.

---

Do you want me to continue with **Part 10** now?




Perfect! Let’s move on to **Part 10**, focusing on **installing and exporting CMake projects with versioned packages**, which is essential for sharing libraries with other developers or projects.

---

## **CMake Course – Part 10: Installing and Exporting Versioned Packages**

### **Topic:** Creating Installable Libraries and CMake Packages

---

### **1. Why Versioned Packages?**

* Allows **other projects** to `find_package()` your library.
* Supports **multiple versions** on the same system.
* Makes libraries reusable and distributable.

---

### **2. Project Structure**

```
MyLib/
├── CMakeLists.txt        # Root
├── include/
│   └── mylib.h
└── src/
    └── mylib.cpp
```

**mylib.h**

```cpp
#pragma once
void greet();
```

**mylib.cpp**

```cpp
#include <iostream>
#include "mylib.h"

void greet() { std::cout << "Hello from MyLib!" << std::endl; }
```

---

### **3. Root CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(MyLib VERSION 1.2 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Library
add_library(mylib STATIC src/mylib.cpp)
target_include_directories(mylib PUBLIC include)

# Installation
install(TARGETS mylib
        EXPORT MyLibTargets
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include)

install(DIRECTORY include/ DESTINATION include)

# Export targets for other projects
install(EXPORT MyLibTargets
        FILE MyLibTargets.cmake
        NAMESPACE MyLib::
        DESTINATION lib/cmake/MyLib)

# Generate versioned config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/MyLibConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(FILES
        "${CMAKE_CURRENT_BINARY_DIR}/MyLibConfigVersion.cmake"
        DESTINATION lib/cmake/MyLib)
```

---

### **4. Step-by-Step Explanation**

1. **`add_library()`** → create your library target.
2. **`target_include_directories()`** → expose headers.
3. **`install(TARGETS …)`** → installs compiled library files.
4. **`install(DIRECTORY …)`** → installs headers.
5. **`install(EXPORT …)`** → creates a CMake package file for `find_package()`.
6. **`write_basic_package_version_file()`** → generates a version file to handle multiple versions.

---

### **5. Using the Installed Library in Another Project**

Suppose another project wants to use `MyLib`:

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(ExampleApp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Specify install prefix path
find_package(MyLib 1.2 REQUIRED PATHS "/path/to/installed/MyLib")

add_executable(example src/main.cpp)
target_link_libraries(example PRIVATE MyLib::mylib)
```

**main.cpp**

```cpp
#include "mylib.h"

int main() {
    greet();
    return 0;
}
```

* `MyLib::mylib` → comes from exported targets.
* `PATHS` → points to installation location of your library.

---

### **6. Step-by-Step Build & Install**

1. Build the library:

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

2. Install to a prefix path:

```bash
cmake --install . --prefix /your/install/path
```

3. Use the library in another project with:

```bash
cmake -DCMAKE_PREFIX_PATH=/your/install/path ..
cmake --build .
```

---

### **7. Best Practices**

* Always include a **version number** in the project (`project(MyLib VERSION x.y)`).
* Use `CMakePackageConfigHelpers` to generate **versioned packages**.
* Export targets with a **namespace** (`MyLib::`) to avoid conflicts.

---

### **8. Beginner Notes**

* Without `install()` and `export()`, `find_package()` will fail.
* Installing to `/usr/local` or a custom prefix allows multiple projects to reuse the library.

---

### **9. Advanced Tips**

* Use **`COMPATIBILITY AnyNewerVersion`** to allow new minor versions without breaking existing users.
* Consider **header-only libraries** using `INTERFACE` target for lightweight packages.
* Combine versioned packages with **CI/CD pipelines** for automated deployment.

---

✅ **Next Step:** In **Part 11**, we’ll cover **building header-only libraries and interface targets**, a common modern CMake pattern.

---

Do you want me to continue with **Part 11** now?


Great! Let’s move on to **Part 11**, where we focus on **header-only libraries and interface targets**, which are widely used in modern C++ projects.

---

## **CMake Course – Part 11: Header-Only Libraries and Interface Targets**

### **Topic:** Creating Reusable Header-Only Libraries in Modern CMake

---

### **1. What is a Header-Only Library?**

* A library with **no compiled source code**, only headers.
* Examples: `Eigen`, `fmt`, `spdlog`.
* Advantages:

  * No need to build or link separately.
  * Easy to distribute.
* Uses **INTERFACE targets** in CMake.

---

### **2. Project Structure**

```
HeaderOnlyLib/
├── CMakeLists.txt
└── include/
    └── mylib/
        └── utils.h
```

**utils.h**

```cpp
#pragma once
#include <iostream>

inline void greet() {
    std::cout << "Hello from Header-Only Library!" << std::endl;
}
```

* Use `inline` or templates to avoid multiple-definition errors.

---

### **3. CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(HeaderOnlyLib VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Create INTERFACE target
add_library(mylib INTERFACE)

# Include directories for consumers
target_include_directories(mylib INTERFACE include)
```

* `INTERFACE` → target has no compiled sources.
* Any target linking `mylib` automatically gets the include directories.

---

### **4. Using the Header-Only Library**

**Example project structure:**

```
ExampleApp/
├── CMakeLists.txt
└── main.cpp
```

**main.cpp**

```cpp
#include "mylib/utils.h"

int main() {
    greet();
    return 0;
}
```

**CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(ExampleApp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add header-only library directory
add_subdirectory(../HeaderOnlyLib HeaderOnlyBuild)

add_executable(example main.cpp)

# Link header-only library
target_link_libraries(example PRIVATE mylib)
```

* Linking an INTERFACE target propagates **include directories and compile options**.

---

### **5. Benefits of INTERFACE Targets**

* Consumers automatically get include paths.
* Can propagate **compile definitions** and **compiler flags**:

```cmake
target_compile_definitions(mylib INTERFACE USE_FEATURE_X)
target_compile_options(mylib INTERFACE -Wall)
```

* Simplifies modern CMake project structure.

---

### **6. Step-by-Step Build**

1. Create build directory:

```bash
mkdir build
cd build
```

2. Configure:

```bash
cmake ..
```

3. Build:

```bash
cmake --build .
```

4. Run:

```bash
./example   # Linux/macOS
example.exe # Windows
```

---

### **7. Best Practices**

* Use **INTERFACE targets** for header-only libraries.
* Keep headers in a separate `include/` directory.
* Inline functions or templates to avoid multiple definition errors.

---

### **8. Beginner Notes**

* No need to call `add_library()` with sources.
* Always link INTERFACE targets to propagate include directories.

---

### **9. Advanced Tips**

* Combine **INTERFACE targets + versioned package export** to distribute header-only libraries.
* Can propagate **compiler options, definitions, and dependencies** without creating binaries.
* Use `FetchContent` to include header-only dependencies in other projects.

---

✅ **Next Step:** In **Part 12**, we’ll cover **building and linking static vs dynamic libraries together**, a crucial skill for mixed library projects.

---

Do you want me to continue with **Part 12** now?




Great — here is **Part 13**, one of the MOST important parts of modern CMake.

---

# **CMake Course – Part 13: Dependency Management (PRIVATE, PUBLIC, INTERFACE)**

### **Topic:** Understanding Link Visibility and Propagation in Modern CMake

This topic is crucial. If you understand **PRIVATE, PUBLIC, INTERFACE**, you understand MOST of modern CMake.

---

# **1. Why Visibility Keywords Matter**

When you link libraries in CMake:

```cmake
target_link_libraries(app PRIVATE lib1)
```

CMake must decide:

* Should **include directories propagate** to dependant targets?
* Should **compiler options propagate**?
* Should **definitions propagate**?

These keywords control what is inherited by other targets.

---

# **2. The 3 Keywords Explained Simply**

## **PRIVATE**

* Used **ONLY when this target builds itself**.
* Does **NOT propagate** to other targets.

Example:

```cmake
target_link_libraries(app PRIVATE libA)
```

Meaning:

* `app` uses `libA`
* Nothing else gets access to `libA`

---

## **PUBLIC**

* Used for **targets that depend on you**.
* Affects **both**:

  * your current target
  * and targets that link to you

Example:

```cmake
target_link_libraries(mylib PUBLIC fmt)
```

Meaning:

* `mylib` uses `fmt`
* Anyone linking to `mylib` also gets `fmt`

---

## **INTERFACE**

* Applies **only** to consumers.
* Used mostly in header-only libraries.

Example:

```cmake
target_link_libraries(mylib INTERFACE header-only-lib)
```

Meaning:

* `mylib` itself doesn’t use it (no .cpp files)
* Consumers do.

---

# **3. Quick Summary Table**

| Keyword   | Affects current target | Affects dependents |
| --------- | ---------------------- | ------------------ |
| PRIVATE   | Yes                    | No                 |
| PUBLIC    | Yes                    | Yes                |
| INTERFACE | No                     | Yes                |

---

# **4. Example Project Using All Keywords**

### Project structure:

```
DepsExample/
├── CMakeLists.txt
├── libA/
│   └── libA.cpp / libA.h
├── libB/
│   └── libB.cpp / libB.h
└── main.cpp
```

### Assume:

* `libA` = static library
* `libB` = header-only library

---

## **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(DepsExample LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

# libA (normal static library)
add_library(libA STATIC libA/libA.cpp)
target_include_directories(libA PUBLIC libA)

# libB (header-only)
add_library(libB INTERFACE)
target_include_directories(libB INTERFACE libB)

# libA needs libB internally
# But consumers do NOT need libB
target_link_libraries(libA PRIVATE libB)

# Application uses both libs
add_executable(app main.cpp)

# app needs libA (public header) and libB (direct usage)
target_link_libraries(app
    PUBLIC libA      # app gets libA's headers
    PRIVATE libB     # only app uses libB
)
```

---

# **5. Code Example**

**libA.h**

```cpp
#pragma once
void helloA();
```

**libA.cpp**

```cpp
#include <iostream>
#include "libA.h"

void helloA() {
    std::cout << "Hello from A!" << std::endl;
}
```

**libB.h**

```cpp
#pragma once
#include <iostream>

inline void helloB() {
    std::cout << "Hello from B!" << std::endl;
}
```

**main.cpp**

```cpp
#include "libA.h"
#include "libB.h"

int main() {
    helloA();
    helloB();
    return 0;
}
```

---

# **6. What Actually Gets Propagated?**

When using:

### **target_include_directories()**

* PUBLIC → propagated
* PRIVATE → not propagated
* INTERFACE → propagated

### **target_compile_definitions()**

* Same rules

### **target_compile_options()**

* Same rules

### **target_link_libraries()**

* PUBLIC → linked + propagated
* PRIVATE → linked only
* INTERFACE → propagated only

---

# **7. Best Practices**

✔ If a library **is required to build the target**, use **PRIVATE**.
✔ If a library must be exposed to users, use **PUBLIC**.
✔ For header-only libraries, always use **INTERFACE**.
✔ For dependencies that should not leak out, use **PRIVATE**, not PUBLIC.
✔ Avoid overusing PUBLIC — it leads to dependency bloat.

---

# **8. Beginner Notes**

* Most errors in CMake happen because of wrong keyword usage.
* If your consumer cannot find headers → probably missing PUBLIC or INTERFACE.
* If your library exposes too much → probably used PUBLIC instead of PRIVATE.

---

# **9. Advanced Tips**

* Combine visibility rules with **exported targets** to make clean package APIs.
* Use **PRIVATE** to hide implementation details (pimpl pattern).
* Use **INTERFACE** to implement “meta libraries” (e.g., group many libs together).

---

### ✅ **Next Step:**

**Part 14** will cover **FetchContent and ExternalProject** — how to add third-party libraries automatically (without installing anything manually).

Do you want Part 14 now?



Great — here is **Part 14**, focused on **FetchContent** and **ExternalProject_Add**, the two most powerful methods for bringing external libraries into your CMake project.

---

# **CMake Course – Part 14: FetchContent & External Dependencies**

### **Topic:** Adding Third-Party Libraries Automatically

Modern CMake gives two main tools to bring external libraries into your build:

---

# **1. `FetchContent` (Recommended for MOST projects)**

* Downloads dependency **during configure time**
* Makes the library behave as if it is **part of your source tree**
* Best for header-only or CMake-based libraries
* Easy to integrate and clean API

---

## **Basic FetchContent Example**

```cmake
include(FetchContent)

FetchContent_Declare(
    json
    GIT_REPOSITORY https://github.com/nlohmann/json.git
    GIT_TAG v3.11.2
)

FetchContent_MakeAvailable(json)
```

Now you can simply:

```cmake
target_link_libraries(my_app PRIVATE nlohmann_json::nlohmann_json)
```

No install step needed.
No manual downloads needed.

---

# **2. Step-by-Step Practical Example**

### Project structure:

```
FCExample/
├── CMakeLists.txt
└── main.cpp
```

**main.cpp**

```cpp
#include <nlohmann/json.hpp>
#include <iostream>

int main() {
    nlohmann::json j = { {"name","Shadow"}, {"age", 21} };
    std::cout << j.dump(4) << std::endl;
    return 0;
}
```

---

## **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.16)
project(FCExample LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

include(FetchContent)

FetchContent_Declare(
    json
    GIT_REPOSITORY https://github.com/nlohmann/json.git
    GIT_TAG v3.11.2
)

FetchContent_MakeAvailable(json)

add_executable(app main.cpp)

target_link_libraries(app PRIVATE nlohmann_json::nlohmann_json)
```

---

# **3. How FetchContent Works Internally**

* Downloads the repo to
  `build/_deps/`
* Adds it to the current project using `add_subdirectory()`
* Exports all the library’s CMake targets for linking

It behaves like **vendorized code**, but fully automatic.

---

# **4. FetchContent for Zip/Tar URLs**

```cmake
FetchContent_Declare(
    cxxopts
    URL https://github.com/jarro2783/cxxopts/archive/refs/tags/v3.2.1.zip
)

FetchContent_MakeAvailable(cxxopts)
```

---

# **5. FetchContent Turning OFF Features**

Some libraries (like SDL, fmt, spdlog) have optional features.

You can disable them before `MakeAvailable`:

```cmake
set(JSON_BuildTests OFF CACHE INTERNAL "")
FetchContent_MakeAvailable(json)
```

---

# **6. Using `ExternalProject_Add` (For BIG Libraries)**

This is for:

* C libraries with custom build systems
* Libraries without CMake
* Very large projects with custom build steps
* Want **full control** of build, install, patch, configure steps

---

## **Example of ExternalProject_Add**

```cmake
include(ExternalProject)

ExternalProject_Add(
    zlib_ext
    URL https://zlib.net/zlib-1.3.1.tar.gz
    CONFIGURE_COMMAND ./configure
    BUILD_COMMAND make
    INSTALL_COMMAND make install
)
```

This downloads → configures → builds → installs.

But it **does not** provide easy CMake targets automatically.

You must manually link later using IMPORTED libraries.

---

# **7. FetchContent vs ExternalProject Comparison**

| Feature                    | FetchContent     | ExternalProject_Add |
| -------------------------- | ---------------- | ------------------- |
| Best for                   | CMake-based libs | Anything non-CMake  |
| Integration                | Excellent        | Medium              |
| Adds Targets Automatically | Yes              | No                  |
| Build Timing               | Configure        | Build               |
| Complex Build Scripts      | No               | Yes                 |
| Dependency Control         | Easy             | Full manual control |

**Most C++ developers should use FetchContent.**

---

# **8. Combined Example: Header-only + External Library**

### FetchContent brings fmt

### ExternalProject builds an old C library

This is common in real projects.

---

# **9. Best Practices**

✔ Prefer **FetchContent** when possible
✔ Pin dependencies to a **specific tag/version**
✔ Turn off unnecessary features using cache variables
✔ Don’t use ExternalProject unless required
✔ Avoid committing downloaded code — let CMake fetch it

---

# **10. Beginner Notes**

* If your library is CMake-based → **use FetchContent**
* Most modern C++ libraries support CMake
* ExternalProject is more complex and rarely needed

---

# **11. Advanced Tips**

* Use **FetchContent_GetProperties** to detect already-downloaded libraries
* Override options before `MakeAvailable`
* Wrap ExternalProject in an **IMPORTED INTERFACE target** to simplify linking
* Use dependency locks (like CPMLockfile) for stable version management

---

### ✅ **Next Step:**

**Part 15** will cover **CTest and building automated testing into your CMake project**, including test discovery and integration.

Would you like **Part 15**?

Great — here is **Part 15**, focused on **CTest**, testing, and integrating tests into your CMake workflow.
This is one of the most useful skills for real-world C++ development.

---

# **CMake Course – Part 15: Testing with CTest (Unit Tests, Test Discovery, Automation)**

### **Topic:** Adding Automated Tests Using CTest

Testing is essential for any real project.
CMake has a built-in testing framework called **CTest**.

---

# **1. Enabling CTest in Your Project**

Add this in your top-level **CMakeLists.txt**:

```cmake
enable_testing()
```

That’s it — CTest is activated.

---

# **2. Basic Test Example**

Project structure:

```
CTestExample/
├── CMakeLists.txt
├── src/
│   └── main.cpp
└── tests/
    └── test_hello.cpp
```

---

# **3. Create a Small Library to Test**

**src/main.cpp**

```cpp
#include <iostream>

int add(int a, int b) {
    return a + b;
}

int main() {
    std::cout << add(2, 3) << std::endl;
}
```

But to test it properly, move code into a library:

**src/mathlib.h**

```cpp
#pragma once
int add(int a, int b);
```

**src/mathlib.cpp**

```cpp
int add(int a, int b) {
    return a + b;
}
```

---

# **4. CMakeLists.txt (full)**

```cmake
cmake_minimum_required(VERSION 3.16)
project(CTestExample LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

enable_testing()

# Library
add_library(mathlib src/mathlib.cpp)
target_include_directories(mathlib PUBLIC src)

# Main app (optional)
add_executable(app src/main.cpp)
target_link_libraries(app PRIVATE mathlib)

# Tests
add_executable(test_add tests/test_hello.cpp)
target_link_libraries(test_add PRIVATE mathlib)

add_test(NAME AddTest COMMAND test_add)
```

---

# **5. Test Code**

**tests/test_hello.cpp**

```cpp
#include "mathlib.h"
#include <iostream>

int main() {
    if (add(2, 3) == 5) {
        std::cout << "PASS\n";
        return 0;
    } else {
        std::cout << "FAIL\n";
        return 1;
    }
}
```

---

# **6. Running Tests**

Inside build directory:

```bash
cmake ..
cmake --build .
ctest
```

Output:

```
1/1 Test #1: AddTest ............   Passed
```

---

# **7. Using GoogleTest with FetchContent (Real Use Case)**

You usually don’t write tests manually like above.
Most developers use **GoogleTest (gtest)**.

### **Add gtest using FetchContent:**

```cmake
include(FetchContent)

FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG        v1.14.0
)
FetchContent_MakeAvailable(googletest)
```

---

# **8. Create GoogleTest Unit Tests**

**tests/test_add.cpp**

```cpp
#include <gtest/gtest.h>
#include "mathlib.h"

TEST(MathTest, Add) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_EQ(add(-1, 3), 2);
}
```

---

# **9. Update CMakeLists for GTest**

```cmake
add_executable(test_math tests/test_add.cpp)
target_link_libraries(test_math PRIVATE mathlib gtest gtest_main)

add_test(NAME MathTest COMMAND test_math)
```

---

# **10. Running Google Tests**

```bash
ctest --output-on-failure
```

---

# **11. Advanced Testing Features**

### **Run tests in parallel**

```bash
ctest -j 8
```

### **Filter tests**

```bash
ctest -R Math
```

### **Stop on first failure**

```bash
ctest --stop-on-failure
```

### **Run only failed tests**

```bash
ctest --rerun-failed
```

---

# **12. Best Practices**

✔ Always use **enable_testing()** in root CMake
✔ Use **GoogleTest** for real testing
✔ Keep tests inside **tests/** folder
✔ One executable per test suite
✔ Use descriptive test names
✔ Integrate testing into CI/CD workflows (GitHub Actions, GitLab CI, Jenkins)

---

# **13. Beginner Notes**

* If `ctest` finds no tests → you forgot `add_test()`
* If your test cannot link → fix include dirs or linking in `target_link_libraries`

---

# **14. Advanced Tips**

* Use `gmock` for mocking external systems
* Combine tests with coverage tools (gcov, lcov)
* Use `CTestCustom.cmake` to customize test behavior
* Integrate **sanitizers** (ASan, TSan) using CMake options

---

### ✅ **Next Step:**

**Part 16** will cover **Configuring install rules, packaging, and generating `.deb`, `.rpm`, `.zip`, `.tar.gz` installers using CPack`.**

Do you want **Part 16** now?


### **PART 16 — Building & Linking External Dependencies (Using `FetchContent`)**

This part teaches you *modern, clean, and native* CMake techniques to download, configure, and link external libraries directly inside your project — **without manually installing anything**.

FetchContent is the recommended method in modern CMake (better than ExternalProject for most cases).

---

# **📌 PART 16 — Using `FetchContent` to Manage External Libraries**

### **Goal of this Part**

By the end, you will know how to:

* Use **FetchContent** to download third-party libraries at configure time
* Build and link them automatically
* Use dependencies like **fmt**, **googletest**, or even large projects like SDL, GLFW
* Organize external packages cleanly inside your project
* Avoid old `FindXXX.cmake` mess

---

# ✅ **1. What is FetchContent?**

`FetchContent` allows CMake to:

* Download a library source from GitHub or any URL
* Add it directly to your build
* Build it like part of your project
* Link it using proper targets

It works **the modern CMake way** with `targets`.

---

# ✅ **2. Basic FetchContent Workflow**

```cmake
include(FetchContent)

FetchContent_Declare(
    fmt
    GIT_REPOSITORY https://github.com/fmtlib/fmt.git
    GIT_TAG        10.2.0
)

FetchContent_MakeAvailable(fmt)
```

After this:

* The **fmt** library is downloaded
* Built quietly inside your project
* Provides target `fmt::fmt`

So you can simply:

```cmake
target_link_libraries(myapp PRIVATE fmt::fmt)
```

---

# ✅ **3. Full Practical Example (fmt library)**

Project structure:

```
part16/
 ├── CMakeLists.txt
 └── main.cpp
```

### **main.cpp**

```cpp
#include <fmt/core.h>

int main() {
    fmt::print("Hello from fmt library!\n");
    return 0;
}
```

### **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.21)
project(FetchContentExample)

set(CMAKE_CXX_STANDARD 17)

include(FetchContent)

FetchContent_Declare(
    fmt
    GIT_REPOSITORY https://github.com/fmtlib/fmt.git
    GIT_TAG        10.2.0
)

FetchContent_MakeAvailable(fmt)

add_executable(app main.cpp)

target_link_libraries(app PRIVATE fmt::fmt)
```

### **Build & Run**

```bash
cmake -S . -B build
cmake --build build
./build/app
```

---

# ✅ **4. Example: Fetch + Build GoogleTest (for Unit Testing)**

Project structure:

```
part16-gtest/
 ├── CMakeLists.txt
 ├── main.cpp
 └── test_math.cpp
```

### **main.cpp**

```cpp
int add(int a, int b) { return a + b; }
```

### **test_math.cpp**

```cpp
#include <gtest/gtest.h>

int add(int, int);

TEST(MathTest, Add) {
    EXPECT_EQ(add(3, 4), 7);
}
```

### **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.21)
project(GTestExample)

set(CMAKE_CXX_STANDARD 17)

include(FetchContent)

# Fetch Googletest
FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG        v1.14.0
)

FetchContent_MakeAvailable(googletest)

enable_testing()

add_library(core main.cpp)

add_executable(test_math test_math.cpp)

target_link_libraries(test_math PRIVATE
    core
    GTest::gtest_main
)

include(GoogleTest)
gtest_discover_tests(test_math)
```

### **Run tests**

```bash
cmake -S . -B build
cmake --build build
ctest --test-dir build
```

---

# ✅ **5. FetchContent Best Practices**

### **Always include the module**

```cmake
include(FetchContent)
```

### **Pin the exact Git tag/commit**

Helps avoid unexpected changes.

### **Do NOT repeat FetchContent_Declare() for same dependency**

Use a separate file if needed.

### **Put third-party libraries in “cmake/deps.cmake”**

Example layout:

```
project/
 ├── CMakeLists.txt
 └── cmake/
      └── deps.cmake
```

---

# ✅ **6. Advanced Example — Using Multiple Dependencies**

```
FetchContent_Declare(
    spdlog
    GIT_REPOSITORY https://github.com/gabime/spdlog.git
    GIT_TAG        v1.14.1
)

FetchContent_Declare(
    json
    GIT_REPOSITORY https://github.com/nlohmann/json.git
    GIT_TAG        v3.11.3
)

FetchContent_MakeAvailable(spdlog json)
```

Then:

```cmake
target_link_libraries(app PRIVATE spdlog::spdlog nlohmann_json::nlohmann_json)
```

---

# ✅ **7. When NOT to use FetchContent**

Use package managers for:

* Very large libraries
* System-level dependencies
* Reusable libraries shared across projects

Examples: Conan, vcpkg.

But for single-project use — FetchContent is perfect.

---

# 🎉 **End of PART 16**

You now know how to:

✔ Pull dependencies automatically
✔ Build + link external repos directly
✔ Write clean modern CMake with targets
✔ Bootstrap complex projects easily

---

If you're ready —

## **Say: "yes part 17"**

and I’ll give you **Handling Installation & Exporting Targets**.



### **PART 17 — Installing & Exporting Targets (The Right Modern CMake Way)**

This part teaches you how to make your project **installable**, **reusable**, and **importable** by other CMake projects — just like real open-source libraries.

This is one of the most important topics in modern CMake.

---

# **📌 PART 17 — INSTALL, EXPORT, PACKAGES**

## **Why this matters**

When you finish a library or application, others should be able to:

* Install it system-wide
* Use it via `find_package(MyLib)`
* Link it with `MyLib::MyLib`
* Get headers and libraries in the correct place
* Use your CMake config files

We’ll cover all of that here.

---

# **1. Basic Install of an Executable**

Example project layout:

```
part17/
 ├── CMakeLists.txt
 ├── main.cpp
 └── include/
       └── mylib.hpp
```

### **main.cpp**

```cpp
#include "mylib.hpp"

int main() {
    hello();
    return 0;
}
```

### **include/mylib.hpp**

```cpp
#pragma once

#include <iostream>

inline void hello() {
    std::cout << "Hello from MyLib!" << std::endl;
}
```

---

## **2. Create a library target**

### **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.21)
project(InstallExample)

set(CMAKE_CXX_STANDARD 17)

add_library(MyLib INTERFACE)
target_include_directories(MyLib INTERFACE include)

add_executable(app main.cpp)
target_link_libraries(app PRIVATE MyLib)
```

---

# **3. Installing Targets**

Add install rules:

```cmake
install(TARGETS MyLib app
        EXPORT MyLibTargets
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
)
```

This installs:

* executable → `bin/`
* library → `lib/`
* static/dynamic libs → `lib/`

---

# **4. Install the header files**

```cmake
install(DIRECTORY include/ DESTINATION include)
```

---

# **5. Export Targets (VERY IMPORTANT)**

This generates:

* `MyLibTargets.cmake`
* A config file other projects can use

```cmake
install(EXPORT MyLibTargets
        FILE MyLibTargets.cmake
        NAMESPACE MyLib::
        DESTINATION lib/cmake/MyLib
)
```

---

# **6. Generate the Config Package**

This allows other people to do:

```cmake
find_package(MyLib REQUIRED)
target_link_libraries(app PRIVATE MyLib::MyLib)
```

Add this:

```cmake
include(CMakePackageConfigHelpers)

configure_package_config_file(
    MyLibConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/MyLibConfig.cmake
    INSTALL_DESTINATION lib/cmake/MyLib
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/MyLibConfig.cmake
    DESTINATION lib/cmake/MyLib
)
```

---

# **7. Create `MyLibConfig.cmake.in`**

Create a file in project root:

### **MyLibConfig.cmake.in**

```cmake
@PACKAGE_INIT@

include("${CMAKE_CURRENT_LIST_DIR}/MyLibTargets.cmake")
```

That’s enough for a simple package.

---

# **8. Build & Install**

```bash
cmake -S . -B build
cmake --build build
cmake --install build --prefix install_dir
```

The structure will be:

```
install_dir/
 ├── bin/app
 ├── include/mylib.hpp
 └── lib/
      ├── cmake/MyLib/
      │     ├── MyLibTargets.cmake
      │     └── MyLibConfig.cmake
      └── libMyLib.a (if not interface)
```

---

# **9. Using the Installed Package in Another Project**

Create a new project:

```
other_project/
 ├── CMakeLists.txt
 └── main.cpp
```

### **main.cpp**

```cpp
#include "mylib.hpp"

int main() {
    hello();
}
```

### **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.21)
project(UseMyLib)

set(CMAKE_PREFIX_PATH "/absolute/path/to/install_dir")

find_package(MyLib REQUIRED)

add_executable(app main.cpp)
target_link_libraries(app PRIVATE MyLib::MyLib)
```

Then:

```bash
cmake -S . -B build
cmake --build build
./build/app
```

---

# **10. Common Mistakes Beginners Make**

❌ Manually copying header files
❌ Installing `.a` or `.so` directly without targets
❌ Forgetting `EXPORT`
❌ Forgetting namespace
❌ Using `add_library(mylib INTERFACE)` incorrectly
❌ Writing an empty or broken config file

Modern CMake avoids all of this using target exports.

---

# **11. Best Practices**

✔ Always export targets
✔ Always create `Config.cmake`
✔ Use a namespace (`MyLib::`)
✔ Use proper install paths
✔ Use INTERFACE libraries for header-only
✔ Use shared or static for compiled libs
✔ Use `GNUInstallDirs` for platform-correct paths

Example:

```cmake
include(GNUInstallDirs)

install(TARGETS MyLib
        EXPORT MyLibTargets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
```

---

# 🎉 **End of PART 17**

You learned everything needed to:

✔ Install libraries
✔ Export targets
✔ Create a find_package-compatible CMake package
✔ Use your library in another project
✔ Follow modern practices used in real industry projects

---

If you're ready—

## **Say: "yes part 18"**

Next: **Cross-Platform CMake (Windows, Linux, macOS Differences)**



### **PART 18 — Cross-Platform CMake (Windows / Linux / macOS)**

This part teaches you how to write CMake projects that **compile everywhere** without changing your C++ code or CMake files.
Cross-platform projects require handling:

* Different compilers (gcc, clang, MSVC)
* OS-specific APIs
* Platform-specific paths
* Library differences
* Conditional code

CMake gives clean tools for all of these.

---

# **📌 PART 18 — Cross-Platform CMake**

---

# **1. Detecting the OS**

CMake provides built-in condition checks:

```cmake
if (WIN32)
    message("Building on Windows")
elseif (APPLE)
    message("Building on macOS")
elseif (UNIX)
    message("Building on Linux")
endif()
```

Notes:

* `WIN32` = all Windows (even 64-bit)
* `UNIX` = Linux + macOS (not Windows)
* `APPLE` = macOS only

---

# **2. Detecting the Compiler**

```cmake
if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    message("MSVC compiler")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    message("GCC compiler")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    message("Clang compiler")
endif()
```

---

# **3. Adding Compiler-Specific Flags**

### GCC / Clang:

```cmake
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(myapp PRIVATE -Wall -Wextra -Wpedantic)
endif()
```

### MSVC:

```cmake
if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(myapp PRIVATE /W4 /permissive-)
endif()
```

---

# **4. Adding OS-Specific Libraries**

### Windows:

```cmake
if (WIN32)
    target_link_libraries(myapp PRIVATE ws2_32)
endif()
```

### Linux:

```cmake
if (UNIX AND NOT APPLE)
    target_link_libraries(myapp PRIVATE pthread dl)
endif()
```

### macOS:

```cmake
if (APPLE)
    target_link_libraries(myapp PRIVATE "-framework Cocoa")
endif()
```

---

# **5. Writing Cross-Platform C++ Code**

CMake provides predefined variables for platform macros.

### C++ file example:

```cpp
#include <iostream>

int main() {
#ifdef _WIN32
    std::cout << "Running on Windows\n";
#elif __APPLE__
    std::cout << "Running on macOS\n";
#elif __linux__
    std::cout << "Running on Linux\n";
#else
    std::cout << "Unknown platform\n";
#endif
}
```

These macros come from the compiler, not CMake.

---

# **6. Example Project — Fully Cross-Platform**

```
part18/
 ├── CMakeLists.txt
 └── main.cpp
```

### **main.cpp**

(Same as above)

---

### **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.21)
project(CrossPlatformExample)

set(CMAKE_CXX_STANDARD 17)

add_executable(myapp main.cpp)

# OS-specific libs
if (WIN32)
    target_compile_definitions(myapp PRIVATE PLATFORM_WINDOWS)
elseif (APPLE)
    target_compile_definitions(myapp PRIVATE PLATFORM_MAC)
elseif (UNIX)
    target_compile_definitions(myapp PRIVATE PLATFORM_LINUX)
endif()

# Compiler-specific flags
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(myapp PRIVATE -Wall -Wextra -Wpedantic)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(myapp PRIVATE /W4 /permissive-)
endif()
```

---

# **7. Cross-Platform Paths**

Never do this:

```cpp
std::string path = "C:\\data\\file.txt";    // Windows only
```

Instead use:

```cpp
#include <filesystem>
std::filesystem::path p = "data/file.txt";
```

CMake does not need special handling for filesystem.

---

# **8. Finding Platform-Specific Dependencies**

Example: `OpenGL`

```cmake
find_package(OpenGL REQUIRED)

if (APPLE)
    target_link_libraries(myapp PRIVATE "-framework OpenGL")
else()
    target_link_libraries(myapp PRIVATE OpenGL::GL)
endif()
```

---

# **9. Portable Install Locations (VERY IMPORTANT)**

Always use:

```cmake
include(GNUInstallDirs)
```

Then:

```cmake
install(TARGETS myapp
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
```

This ensures:

* Windows → install to `bin/`
* Linux → `/usr/local/bin`
* macOS → correct paths

Automatically handled.

---

# **10. Cross-Platform Project Structure**

Recommended layout:

```
project/
 ├── src/
 ├── include/
 ├── platform/
 │     ├── windows/
 │     ├── linux/
 │     └── mac/
 ├── CMakeLists.txt
 └── cmake/
```

Use conditionals to include files:

```cmake
if (WIN32)
    add_subdirectory(platform/windows)
elseif (APPLE)
    add_subdirectory(platform/mac)
endif()
```

---

# **11. Best Practices**

✔ Avoid absolute paths
✔ Avoid compiler-specific flags unless needed
✔ Use filesystem API
✔ Wrap OS code with macros
✔ Use target properties (modern CMake)
✔ NEVER use global functions like `add_definitions()`

---

# 🎉 **End of PART 18**

You now know how to:

✔ Write cross-platform CMake
✔ Handle OS and compiler detection
✔ Apply OS-specific libs and flags
✔ Create portable install rules
✔ Structure large multi-platform projects

---

If you're ready—

## **Say: "yes part 19"**

Next: **Building Shared + Static Libraries at the Same Time (Multi-Config CMake)**


### **PART 19 — Building BOTH Static & Shared Libraries (Multi-Config Setup)**

This part teaches you how to let users of your project choose:

* **Static library (`.a` / `.lib`)**
* **Shared library (`.so` / `.dll` / `.dylib`)**

Or **build both at the same time**.

This is a key “professional level” CMake skill.

---

# **📌 PART 19 — Static + Shared Library Builds (Modern CMake)**

---

# **1. Basic Idea**

You normally use:

```cmake
add_library(MyLib SHARED ...)
add_library(MyLib STATIC ...)
```

But **you cannot give both the same target name**.

So we create:

* `MyLib_shared`
* `MyLib_static`

And expose them under a single namespace:

* `MyLib::shared`
* `MyLib::static`

---

# **2. Example Project Layout**

```
part19/
 ├── CMakeLists.txt
 ├── src/
 │     └── mylib.cpp
 └── include/
       └── mylib.hpp
```

### **include/mylib.hpp**

```cpp
#pragma once
int add(int a, int b);
```

### **src/mylib.cpp**

```cpp
#include "mylib.hpp"
int add(int a, int b) { return a + b; }
```

---

# **3. Build Both Static & Shared Targets**

### **CMakeLists.txt**

```cmake
cmake_minimum_required(VERSION 3.21)
project(MultiLibraryExample)

set(CMAKE_CXX_STANDARD 17)

include(GNUInstallDirs)

# Source files for both
set(MYLIB_SOURCES
    src/mylib.cpp
)

set(MYLIB_HEADERS
    include/mylib.hpp
)

# Shared library
add_library(MyLib_shared SHARED ${MYLIB_SOURCES})
target_include_directories(MyLib_shared PUBLIC include)

# Static library
add_library(MyLib_static STATIC ${MYLIB_SOURCES})
target_include_directories(MyLib_static PUBLIC include)

# Give each target a clean namespace
add_library(MyLib::shared ALIAS MyLib_shared)
add_library(MyLib::static ALIAS MyLib_static)
```

Both libraries now exist.

---

# **4. Handling Windows DLL Export**

Windows requires **export macros** for shared libs.

Add:

```cmake
target_compile_definitions(MyLib_shared PRIVATE MYLIB_EXPORTS)
```

Your header:

```cpp
#pragma once

#ifdef _WIN32
#  ifdef MYLIB_EXPORTS
#    define MYLIB_API __declspec(dllexport)
#  else
#    define MYLIB_API __declspec(dllimport)
#  endif
#else
#  define MYLIB_API
#endif

MYLIB_API int add(int a, int b);
```

---

# **5. Install Rules (Static + Shared)**

```cmake
install(TARGETS
    MyLib_shared
    MyLib_static
    EXPORT MyLibTargets
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(DIRECTORY include/ DESTINATION include)
```

---

# **6. Export CMake Package (Supports Both)**

```cmake
install(EXPORT MyLibTargets
    FILE MyLibTargets.cmake
    NAMESPACE MyLib::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MyLib
)
```

Config file:

### **MyLibConfig.cmake.in**

```cmake
@PACKAGE_INIT@
include("${CMAKE_CURRENT_LIST_DIR}/MyLibTargets.cmake")
```

Add this to CMakeLists:

```cmake
include(CMakePackageConfigHelpers)

configure_package_config_file(
    MyLibConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/MyLibConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MyLib
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/MyLibConfig.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/MyLib
)
```

---

# **7. Optional: Choose Whether to Build Shared or Static**

Let the user pick with a simple option:

```cmake
option(BUILD_SHARED "Build shared library" ON)
option(BUILD_STATIC "Build static library" ON)

if (BUILD_SHARED)
    add_library(MyLib_shared SHARED ${MYLIB_SOURCES})
endif()

if (BUILD_STATIC)
    add_library(MyLib_static STATIC ${MYLIB_SOURCES})
endif()
```

User build commands:

* Only shared:
  `cmake -S . -B build -DBUILD_SHARED=ON -DBUILD_STATIC=OFF`

* Only static:
  `cmake -S . -B build -DBUILD_SHARED=OFF -DBUILD_STATIC=ON`

* Build both:
  `cmake -S . -B build -DBUILD_SHARED=ON -DBUILD_STATIC=ON`

---

# **8. Using the Installed Libraries in Another Project**

Another project can choose which to link:

```cmake
find_package(MyLib REQUIRED)

add_executable(app main.cpp)

# link static
target_link_libraries(app PRIVATE MyLib::static)

# OR link shared
# target_link_libraries(app PRIVATE MyLib::shared)
```

---

# **9. Best Practices**

✔ Provide both variants
✔ Use aliases (`MyLib::static`, `MyLib::shared`)
✔ Expose only target names, never `.so` or `.a`
✔ Use `GNUInstallDirs` for proper install paths
✔ Always export your library
✔ Provide DLL export macro on Windows

---

# 🎉 **End of PART 19**

You can now:

✔ Build both static and shared libs
✔ Allow user to select type
✔ Install & export multi-config libraries
✔ Support Windows DLL usage
✔ Use best practice target aliasing (`MyLib::...`)

---

When you're ready:

## **Say: "yes part 20"**

Next (final): **Packaging, CPack, and complete real-world project structure**.


### **PART 20 — Packaging Your Project (CPack) + Final Real-World Project Structure**

This is the final part.
Now you’ll make your project **fully distributable** like real software:

* Create `.zip`, `.tar.gz`, `.deb`, `.rpm`, `.exe installer`, etc.
* Package headers, libs, binaries, docs
* Include license + versioning
* Produce installable archives automatically
* Build final professional project layout

---

# **📌 PART 20 — Packaging With CPack + Final Project Template**

---

# **1. What is CPack?**

CPack is built into CMake and allows you to generate installers:

* `.zip` / `.tar.gz`
* Windows installer `.exe` / `.msi`
* Linux `.deb` / `.rpm`
* macOS `.dmg`

You only need **10 lines** to enable it.

---

# **2. Minimal CPack Setup**

Add at the **bottom** of your CMakeLists.txt:

```cmake
include(CPack)
```

This already enables default packaging.

Build a package:

```bash
cmake -S . -B build
cmake --build build
cpack --config build/CPackConfig.cmake
```

This produces:

* `.tar.gz` on Linux/macOS
* `.zip` on Windows

---

# **3. Adding Project Metadata**

```cmake
set(CPACK_PACKAGE_NAME "MyLib")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_DESCRIPTION "A cross-platform C++ library")
set(CPACK_PACKAGE_CONTACT "support@example.com")
set(CPACK_PACKAGE_VENDOR "Shadow Software")
```

---

# **4. Package Formats**

Enable specific formats:

```cmake
set(CPACK_GENERATOR "ZIP;TGZ;DEB;RPM")
```

Common values:

* `ZIP`
* `TGZ`
* `DEB` (Debian/Ubuntu)
* `RPM` (Fedora/RHEL)
* `NSIS` (Windows EXE installer)

---

# **5. Adding License + Readme**

```
project/
 ├── LICENSE
 └── README.md
```

Add:

```cmake
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/LICENSE")
set(CPACK_RESOURCE_FILE_README "${CMAKE_SOURCE_DIR}/README.md")
```

These files go into the packaged installer.

---

# **6. Packaging Install Targets**

Earlier you learned how to install:

```cmake
install(TARGETS MyLib_shared MyLib_static EXPORT MyLibTargets ...)
install(DIRECTORY include/ DESTINATION include)
install(FILES ... MyLibConfig.cmake DESTINATION lib/cmake/MyLib)
```

CPack packages **whatever you install**.

So if your install process is correct, your package will also be correct.

---

# **7. Building .deb Packages (Linux)**

Requires:

```cmake
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Shadow")
```

Now CPack will produce a `.deb`.

---

# **8. Building Windows Installer (.exe)**

Enable the NSIS generator:

```cmake
set(CPACK_GENERATOR "NSIS")
```

On Windows + NSIS installed:

```
cpack
```

Produces:

```
MyLib-1.0.0-win64.exe
```

A full GUI installer.

---

# **9. Building macOS `.dmg`**

```cmake
set(CPACK_GENERATOR "DragNDrop")
```

Produces `.dmg`.

---

# **10. Complete CPack Configuration Block**

Place at bottom of **CMakeLists.txt**:

```cmake
set(CPACK_PACKAGE_NAME "MyLib")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_CONTACT "support@example.com")
set(CPACK_PACKAGE_VENDOR "Shadow Software")
set(CPACK_PACKAGE_DESCRIPTION "Cross-platform C++ library example")

set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/LICENSE")
set(CPACK_RESOURCE_FILE_README "${CMAKE_SOURCE_DIR}/README.md")

set(CPACK_GENERATOR "ZIP;TGZ")

include(CPack)
```

---

# **11. REAL-WORLD CMake Project Structure (Industry Standard)**

Here is the final professional layout:

```
MyProject/
 ├── CMakeLists.txt
 ├── src/
 │     ├── CMakeLists.txt
 │     ├── lib.cpp
 │     └── internal/
 │           └── helpers.cpp
 ├── include/
 │     └── MyProject/
 │           └── lib.hpp
 ├── tests/
 │     ├── CMakeLists.txt
 │     └── test_basic.cpp
 ├── cmake/
 │     ├── deps.cmake
 │     ├── Config.cmake.in
 │     └── version.hpp.in
 ├── examples/
 │     └── simple_app.cpp
 ├── docs/
 │     └── overview.md
 ├── LICENSE
 └── README.md
```

### **Features of this structure**

✔ External dependencies go in `cmake/deps.cmake`
✔ Libraries live in `src/`
✔ Headers inside `include/MyProject`
✔ Tests with GoogleTest inside `tests/`
✔ Export/Config templates inside `cmake/`
✔ Examples are separate
✔ Documentation directory

This is what real companies use.

---

# **12. Final Industry-Style CMakeLists Template**

Put this at the project root:

```cmake
cmake_minimum_required(VERSION 3.21)
project(MyProject VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)

include(GNUInstallDirs)
include(cmake/deps.cmake)

add_subdirectory(src)
add_subdirectory(tests)
add_subdirectory(examples)

include(CPack)
```

---

# **13. Summary of Everything You Learned (ZERO → HERO)**

You now completely understand:

### **Beginner Level**

✔ Basic structure
✔ Minimum project
✔ Building executables
✔ Linking libraries

### **Intermediate**

✔ Static + shared libraries
✔ Include directories
✔ Project organization
✔ Compiler flags
✔ CMake variables
✔ Functions, macros

### **Advanced**

✔ FetchContent dependencies
✔ Exporting + installing your library
✔ Cross-platform builds
✔ Interface libraries
✔ Multi-target building
✔ Config package files
✔ Full CPack packaging
✔ Industry standard project layout

You now have the complete skillset needed for professional C++ & CMake development.

---

# 🎉 **END OF PART 20 — Course Complete!**

If you want, I can also provide:

⭐ A **full ready-to-use template project**
⭐ A **cheat sheet** for all modern CMake commands
⭐ A **one-command script** to generate a new CMake project
⭐ Or continue into **CMake Ninja, presets, cross-compilation, vcpkg, Conan**

Just tell me what you want next.
