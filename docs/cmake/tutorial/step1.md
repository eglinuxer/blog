# 第 1 步：基本起点

如何开始使用 CMake？这一步将介绍 CMake 的基本语法、命令和变量。在介绍这些概念的同时，我们将完成三个练习并创建一个简单的 CMake 项目。

本步骤中的每个练习都会提供一些背景信息。然后，给出一个目标和一些有用的资源列表。**"要编辑的文件"**部分中的每个文件都位于 step1 目录中，并包含一个或多个 TODO 注释。每个 TODO 代表需要更改或添加的一行或两行代码。TODO 应按照数字顺序完成，先完成 TODO 1，然后是 TODO 2，依此类推。**"开始"**部分将给出一些提示，并引导您完成练习。然后，在**"构建并运行"**部分，将逐步介绍如何构建和测试练习。最后，在每个练习结束时，将讨论预期的解决方案。

此外，请注意，教程中的每个步骤都建立在前一个步骤的基础上。例如，step2 的起始代码是 step1 的完整解决方案。

## 练习 1 - 构建一个基本项目

最基本的 CMake 项目是由单个源代码文件构建的可执行文件。对于这样简单的项目，只需要一个包含三个命令的 ```CMakeLists.txt``` 文件即可。

**注意：**虽然 CMake 支持大写、小写和大小写混合的命令，但是小写命令是首选，并将在整个教程中使用。

任何项目的最顶层 ```CMakeLists.txt``` 文件都必须使用 ```cmake_minimum_required()``` 命令指定一个最低 CMake 版本。这将建立策略设置，并确保以下 CMake 函数在兼容的 CMake 版本上运行。

要启动一个项目，我们使用 ```project()``` 命令设置项目名称。每个项目都需要这个调用，并且应该在 ```cmake_minimum_required()``` 之后尽快调用。正如我们将在后面看到的，这个命令还可以用于指定其他项目级别的信息，比如语言或版本号。

最后，```add_executable()``` 命令告诉 CMake 使用指定的源代码文件创建一个可执行文件。

### 目标

了解如何创建简单的 CMake 项目。

### 有用的资源

- [add_executable()](https://cmake.org/cmake/help/latest/command/add_executable.html#command:add_executable)
- [cmake_minimum_required()](https://cmake.org/cmake/help/latest/command/cmake_minimum_required.html#command:cmake_minimum_required)
- [project()](https://cmake.org/cmake/help/latest/command/project.html#command:project)

### 要编辑的文件
- ```CMakeLists.txt```

### 开始
```tutorial.cxx``` 的源代码在 [eglinuxer/study_cmake/tutorial](https://github.com/eglinuxer/study_cmake/tree/main/tutorial/step1) 目录中提供，可用于计算数字的平方根。此文件不需要在此步骤中进行编辑。

在同一目录中有一个您将完成的 ```CMakeLists.txt``` 文件。从 TODO 1 开始，完成 TODO 3。

``` CMake
# TODO 1: Set the minimum required version of CMake to be 3.26

# TODO 2: Create a project named Tutorial

# TODO 7: Set the project version number as 1.0 in the above project command

# TODO 6: Set the variable CMAKE_CXX_STANDARD to 11
#         and the variable CMAKE_CXX_STANDARD_REQUIRED to True

# TODO 8: Use configure_file to configure and copy TutorialConfig.h.in to
#         TutorialConfig.h

# TODO 3: Add an executable called Tutorial to the project
# Hint: Be sure to specify the source file as tutorial.cxx

# TODO 9: Use target_include_directories to include ${PROJECT_BINARY_DIR}
```

### 构建并运行
一旦 TODO 1 到 TODO 3 完成，我们就可以构建和运行我们的项目了！首先，运行 cmake 可执行文件或 cmake-gui 来配置项目，然后使用您选择的构建工具构建它。

例如，我们可以将 [eglinuxer/study_cmake/tutorial](https://github.com/eglinuxer/study_cmake/tree/main/tutorial/step1) 整个目录放到本地，然后进入本地这个目录执行如下命令：

``` bash
cmake -S . -B build
cmake --build build
```

第一条命令是配置项目并生成构建脚本，第二条命令是执行构建，编译出可执行文件。

最后我们可以在 build 目录中找到 Tutorial 可执行文件，可以使用如下命令运行它：

``` bash
Tutorial 4294967296
Tutorial 10
Tutorial
```

### 解决方案
如上所述，我们需要启动和运行三行 ```CMakeLists.txt```。第一行是使用 ```cmake_minimum_required()``` 设置 CMake 版本，如下：

``` CMake
# TODO 1
cmake_minimum_required(VERSION 3.26)
```

定义项目的下一步是使用 ```project()``` 命令，如下所示设置项目名称：

``` CMake
# TODO 2
project(Tutorial)
```

最后一步，使用 ```add_executable()``` 命令定义一个 Target，如下：

``` CMake
# TODO 3
add_executable(Tutorial tutorial.cxx)
```

## 练习 2 - 指定 C++ 标准

CMake 有一些特殊的变量，它们要么在幕后自动创建，要么在由项目代码设置时对 CMake 具有特殊含义。其中许多变量以 ```CMAKE_``` 开头。在创建项目变量时，应避免使用这种命名约定。其中两个特殊的可由用户设置的变量是 ```CMAKE_CXX_STANDARD``` 和 ```CMAKE_CXX_STANDARD_REQUIRED```。它们可以一起使用，用于指定构建项目所需的 C++ 标准。

### 目标

将项目的 C++ 标准设置为 C++11

### 有用的资源
- [CMAKE_CXX_STANDARD](https://cmake.org/cmake/help/latest/variable/CMAKE_CXX_STANDARD.html#variable:CMAKE_CXX_STANDARD)
- [CMAKE_CXX_STANDARD_REQUIRED](https://cmake.org/cmake/help/latest/variable/CMAKE_CXX_STANDARD_REQUIRED.html#variable:CMAKE_CXX_STANDARD_REQUIRED)
- [set()](https://cmake.org/cmake/help/latest/command/set.html#command:set)

### 要编辑的文件
- CMakeLists.txt
- tutorial.cxx

### 开始
继续编辑 step1 目录中的文件。从 TODO 4 开始，完成 TODO 6。

首先，通过添加需要 C++11 的功能来编辑 ```tutorial.cxx```。然后更新 ```CMakeLists.txt``` 以要求 C++11。

### 构建并运行
``` bash
cmake -S . -B build
cmake --build build

./build/Tutorial 4294967296
./build/Tutorial 10
./build/Tutorial
```

### 解决方案
我们首先通过将 ```tutorial.cxx``` 中的 ```atof``` 替换为 ```std::stod``` 来向我们的项目添加一些 C++11 功能。这看起来像下面这样：

``` c++
// TODO 4
const double inputValue = std::stod(argv[1]);
```

要完成 TODO 5，只需删除 ```#include <cstdlib>```。

我们需要在 CMake 代码中明确声明它应该使用正确的标志。在 CMake 中启用对特定 C++ 标准的支持的一种方法是使用 ```CMAKE_CXX_STANDARD``` 变量。对于本教程，将 ```CMakeLists.txt``` 文件中的 ```CMAKE_CXX_STANDARD``` 变量设置为 11，将 ```CMAKE_CXX_STANDARD_REQUIRED``` 设置为 True。确保在对 ```add_executable()``` 的调用之前添加 ```CMAKE_CXX_STANDARD``` 声明。

``` CMake
# TODO 6
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

## 练习 3 - 添加版本号和配置头文件
有时在 ```CMakeLists.txt``` 文件中定义的变量在源代码中也可用，这可能很有用。在这种情况下，我们希望打印项目版本号。

一种实现方法是使用配置的头文件。我们创建一个输入文件，其中包含一个或多个要替换的变量。这些变量具有特殊的语法，看起来像是@VAR@。然后，我们使用 ```configure_file()``` 命令将输入文件复制到指定的输出文件，并将这些变量替换为 ```CMakeLists.txt``` 文件中 ```VAR``` 的当前值。

虽然我们可以直接在源代码中编辑版本号，但使用这个功能更可取，因为它创建了一个单一的真实数据源，避免了重复。

### 目标
定义并报告项目的版本号。

### 有用的资源

- [<PROJECT-NAME\>_VERSION_MAJOR](https://cmake.org/cmake/help/latest/variable/PROJECT-NAME_VERSION_MAJOR.html#variable:%3CPROJECT-NAME%3E_VERSION_MAJOR)
- [<PROJECT-NAME\>_VERSION_MINOR](https://cmake.org/cmake/help/latest/variable/PROJECT-NAME_VERSION_MINOR.html#variable:%3CPROJECT-NAME%3E_VERSION_MINOR)
- [configure_file()](https://cmake.org/cmake/help/latest/command/configure_file.html#command:configure_file)
- [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)

### 要编辑的文件
- CMakeLists.txt
- tutorial.cxx

### 开始
继续编辑 step1 中的文件。从 TODO 7 开始，并完成到 TODO 12。在这个练习中，我们首先在 ```CMakeLists.txt``` 中添加项目版本号。在同一个文件中，使用 ```configure_file()``` 函数将给定的输入文件复制到一个输出文件，并在输入文件内容中替换一些变量值。

接下来，创建一个输入头文件 ```TutorialConfig.h.in```，定义接受从 ```configure_file()``` 传递的变量的版本号。

最后，更新 ```tutorial.cxx``` 文件，以打印出其版本号。

### 构建并运行
``` bash
cmake -S . -B build
cmake --build build

./build/Tutorial
```

验证打印出的版本号。

### 解决方案

在这个练习中，我们通过打印版本号来改进我们的可执行文件。虽然我们可以在源代码中完成这个任务，但是使用 ```CMakeLists.txt``` 可以让我们维护一个单一的版本号数据源。

首先，我们修改 ```CMakeLists.txt``` 文件，使用 ```project()``` 命令设置项目名称和版本号。当调用 ```project()``` 命令时，CMake 会在幕后定义 ```Tutorial_VERSION_MAJOR``` 和 ```Tutorial_VERSION_MINOR``` 这两个变量。

``` CMake
# TODO 7
project(Tutorial VERSION 1.0)
```

然后我们使用 ```configure_file()``` 复制输入文件并替换指定的 CMake 变量：

``` CMake
# TODO 8
configure_file(TutorialConfig.h.in TutorialConfig.h)
```

由于配置文件将写入项目二进制目录，我们必须将该目录添加到路径列表中以搜索包含文件。

**注意：**在本教程中，我们将交替引用项目构建和项目二进制目录。这些是相同的，并不意味着引用 ```bin/``` 目录。

我们使用 ```target_include_directories()``` 来指定可执行目标应该在哪里寻找包含文件。

``` CMake
# TODO 9
target_include_directories(Tutorial
    PUBLIC
        "${PROJECT_BINARY_DIR}"
)
```

```TutorialConfig.h.in``` 是要配置的输入头文件。当从我们的 ```CMakeLists.txt``` 调用 ```configure_file()``` 函数时，```@Tutorial_VERSION_MAJOR@``` 和 ```@Tutorial_VERSION_MINOR@``` 的值将被替换为 ```TutorialConfig.h``` 中项目对应的版本号。

``` c++
// TODO 10
// the configured options and settings for Tutorial
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```

接下来，我们需要修改 ```tutorial.cxx``` 以包含配置的头文件 ```TutorialConfig.h```。

``` c++
// TODO 11
#include "TutorialConfig.h"
```

最后，我们通过更新 ```tutorial.cxx``` 打印出可执行文件名称和版本号，如下所示：

``` c++
// TODO 12
if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
}
```