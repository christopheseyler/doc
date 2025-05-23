# Using a Yocto SDK for Cross-Compiling Applications

This document explains how to generate and use a Yocto SDK for developing applications targeting an embedded Linux system built with Yocto. It includes setting up the SDK, compiling a C++ application, and using CMake with the cross-toolchain.

---

## 1. Generate the SDK

Once your image is built, generate the SDK by running:

```bash
bitbake <image-name> -c populate_sdk
```

**Example:**

```bash
bitbake core-image-base -c populate_sdk
```

This creates an installable SDK script in:

```
tmp/deploy/sdk/
```

---

## 2. Install the SDK

Run the generated installer script:

```bash
./poky-glibc-<...>-toolchain-<date>.sh
```

Choose the installation directory (e.g., `~/opt/sdk`).

---

## 3. Set Up the SDK Environment

Source the SDK environment script:

```bash
source /path/to/sdk/environment-setup-<triplet>
```

**Example:**

```bash
source ~/opt/sdk/environment-setup-cortexa55-poky-linux
```

This sets up cross-compilation variables like `CC`, `CXX`, `PKG_CONFIG_PATH`, etc.

---

## 4. Add Packages to the SDK

To include libraries (e.g., Boost, OpenCV) in the SDK, add them to the image recipe:

```bitbake
IMAGE_INSTALL:append = " boost opencv"
```

Then regenerate the SDK:

```bash
bitbake <image-name> -c populate_sdk
```

---

## 5. (Optional) Generate an Extensible SDK

For a full BitBake development environment:

```bash
bitbake <image-name> -c populate_sdk_ext
```

This includes the metadata and recipes needed for deeper development.

---

## 6. Example Application Project

### Directory Structure

```
my_app/
├── CMakeLists.txt
└── src/
    └── main.cpp
```

### main.cpp

```cpp
#include <iostream>

int main() {
    std::cout << "Hello from Yocto SDK!" << std::endl;
    return 0;
}
```

### CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.10)
project(my_app)

set(CMAKE_CXX_STANDARD 17)

add_executable(my_app src/main.cpp)
```

### Build the Application

After sourcing the SDK environment:

```bash
mkdir -p build && cd build
cmake ..
make
```

---

## 7. Using Libraries (e.g., libgpiod)

CMake can use `pkg-config` to find libraries:

```cmake
find_package(PkgConfig REQUIRED)
pkg_check_modules(GPIOD REQUIRED IMPORTED_TARGET libgpiod)
target_link_libraries(my_app PkgConfig::GPIOD)
```

---

## 8. Next Steps

You can now:

* Package your application into a Yocto layer or recipe.
* Deploy it using `scp`, `rsync`, or as part of a custom Yocto image.

---

For further customization, consider integrating Conan or other dependency managers compatible with the Yocto SDK environment.
