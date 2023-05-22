# 第 2 步：添加一个库

通过第一步的练习，我们已经能够使用 CMake 创建一个简单的项目了，在这一个步骤中，我们将学习如何在我们的项目中添加一个 library。同时我们还会学习如何让我们的 library 对于项目来说是可选的。

## 练习 1 - 创建库

在 CMake 项目中，如果要添加一个 library，我们使用的命令是 [add_library()](https://cmake.org/cmake/help/latest/command/add_library.html#command:add_library)。

添加 library 的时候需要指定编译 library 需要的源码文件，如果源码文件很少，那可能会全部放在一个目录下，但是无论项目大小，都强烈建议一开始就做好源代码目录结构的组织。这就难免会新建很多子目录，在 CMake 项目中，将子目录引入 CMake 管理的命令是 [add_subdirectory()](https://cmake.org/cmake/help/latest/command/add_subdirectory.html#command:add_subdirectory)。

库编译出来当然是要给其他程序使用的，我们使用 gcc 命令进行编译的时候，如果要链接某个库，通常我们需要使用 -L 选项指定要链接的库所在的路径，然后使用 -l 选项去链接这个库，让链接这个库的程序能够正确找到并链接上该库。

在 CMake 中，我们使用 [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories) 命令指定要链接的库，使用 [target_link_directories()](https://cmake.org/cmake/help/latest/command/target_link_directories.html) 命令指定要链接的库所在的路径。

我们在使用 gcc 命令行编译程序的时候，如果程序使用了头文件，我们通常还需要使用 -I 选项指定头文件的路径，这样编译器才能正确的找到头文件，CMake 中使用 [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories) 命令指定包含的头文件的路径。

### 目标

添加并使用 library。

### 有用的资源

- [add_library()](https://cmake.org/cmake/help/latest/command/add_library.html#command:add_library)
- [add_subdirectory()](https://cmake.org/cmake/help/latest/command/add_subdirectory.html#command:add_subdirectory)
- [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)
- [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_link_libraries.html#command:target_link_libraries)
- [target_link_directories](https://cmake.org/cmake/help/latest/command/target_link_directories.html)
- [PROJECT_SOURCE_DIR](https://cmake.org/cmake/help/latest/variable/PROJECT_SOURCE_DIR.html#variable:PROJECT_SOURCE_DIR)

### 要编辑的文件

- CMakeLists.txt
- tutorial.cxx
- MathFunctions/CMakeLists.txt

### 开始

在本练习中，我们将向我们的项目添加一个 library，其中包含我们自己用于计算数字平方根的实现。然后可执行文件可以使用这个库而不是编译器提供的标准平方根函数。

对于本教程，我们会将库放入名为 ```MathFunctions``` 的子目录中。该目录已包含头文件 ```MathFunctions.h``` 和源文件 ```mysqrt.cxx```。这两个文件我们无需更改。源文件有一个名为 mysqrt 的函数，它提供与编译器的 sqrt 函数类似的功能。

下载本练习的源码 [tutorial/step2](https://github.com/eglinuxer/study_cmake/tree/main/tutorial/step2)，全局搜索 ```TODO```，按照提示完成 ```TODO 1``` 到 ```TODO 6```。

首先，填写 ```MathFunctions``` 子目录下的一行 ```CMakeLists.txt```。

接下来，编辑顶级 ```CMakeLists.txt```。

最后，在 ```tutorial.cxx``` 中使用新创建的 ```MathFunctions``` 库

### 构建并运行

``` bash
cmake -S . -B build
cmake --build build
```

### 解决方案

在 ```MathFunctions``` 目录的 ```CMakeLists.txt``` 文件中，我们使用 [add_library()](https://cmake.org/cmake/help/latest/command/add_library.html#command:add_library) 创建了一个名为 ```MathFunctions``` 的库目标。库的源文件作为参数传递给 [add_library()](https://cmake.org/cmake/help/latest/command/add_library.html#command:add_library)。这看起来像下面一行：

``` CMake
add_library(MathFunctions mysqrt.cxx)
```

为了使用新库，我们将在顶级 ```CMakeLists.txt``` 文件中添加一个 [add_subdirectory()](https://cmake.org/cmake/help/latest/command/add_subdirectory.html#command:add_subdirectory) 调用，以便构建该库。

``` CMake
add_subdirectory(MathFunctions)
```

接下来，使用 [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_link_libraries.html#command:target_link_libraries) 将新库目标链接到可执行目标。

``` CMake
target_link_libraries(Tutorial PUBLIC MathFunctions)
```

最后我们需要指定库的头文件位置。修改 [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories) 以将 ```MathFunctions``` 子目录添加为包含目录，以便可以找到 ```MathFunctions.h``` 头文件。

``` CMake
target_include_directories(Tutorial
    PUBLIC
        "${PROJECT_BINARY_DIR}"
        "${PROJECT_SOURCE_DIR}/MathFunctions"
)
```

现在让我们使用我们的库。在 ```tutorial.cxx``` 中，包含 ```MathFunctions.h```：
``` c++
#include "MathFunctions.h"
```

最后，将 ```sqrt``` 替换为我们的库函数 ```mysqrt```。

## 练习 2 - 让我们的库成为可选的

现在让我们将 ```MathFunctions``` 库设为可选。虽然对于教程来说确实没有必要这样做，但对于较大的项目来说这是很常见的事情。

CMake 可以使用 [option()](https://cmake.org/cmake/help/latest/command/option.html#command:option) 命令执行此操作。这为用户提供了一个变量，他们可以在配置他们的 cmake 构建时更改该变量。此设置将存储在缓存中，因此用户无需在每次在构建目录上运行 CMake 时都设置该值。

### 目标

添加选项控制是否编译我们自己定义的 library。

### 有用的资源
- [if()](https://cmake.org/cmake/help/latest/command/if.html#command:if)
- [list()](https://cmake.org/cmake/help/latest/command/list.html#command:list)
- [option()](https://cmake.org/cmake/help/latest/command/option.html#command:option)
- [cmakedefine](https://cmake.org/cmake/help/latest/command/configure_file.html#command:configure_file)

### 要编辑的文件
- CMakeLists.txt
- tutorial.cxx
- TutorialConfig.h.in

### 开始

从练习 1 的结果文件开始。完成 ```TODO 7``` 到 ```TODO 13```。

首先在顶级 ```CMakeLists.txt``` 文件中使用 [option()](https://cmake.org/cmake/help/latest/command/option.html#command:option) 命令创建一个变量 ```USE_MYMATH```。在同一个文件中，使用该选项来确定是否构建和使用 ```MathFunctions``` 库。

然后，更新 ```tutorial.cxx``` 和 ```TutorialConfig.h.in``` 以使用 ```USE_MYMATH```。

### 构建并运行
由于我们已经从练习 1 中配置了构建目录，因此我们可以通过简单地调用以下命令来重建：

``` bash
cmake --build build
```

接下来，对几个数字运行 Tutorial 可执行文件以验证它是否仍然正确。

现在让我们将 ```USE_MYMATH``` 的值更新为 ```OFF```。

``` bash
cmake -S . -B build -DUSE_MYMATH=OFF
cmake --build build
```

然后，再次运行可执行文件以确保它仍然可以在 ```USE_MYMATH``` 设置为 ```OFF``` 的情况下工作。哪个函数提供更好的结果，```sqrt``` 或 ```mysqrt```？

### 解决方案

第一步是向顶级 ```CMakeLists.txt``` 文件添加一个选项。此选项将显示在 ```cmake-gui``` 和 ```ccmake``` 中，默认值为 ```ON```，用户可以更改。

``` CMake
option(USE_MYMATH "Use tutorial provided math implementation" ON)
```

接下来，使构建和链接 ```MathFunctions``` 库成为条件。

使用 [list()](https://cmake.org/cmake/help/latest/command/list.html#command:list) 命令定义一个列表变量 ```EXTRA_LIBS```，目前这个列表中只有 ```MathFunctions``` 一个值。

同样的，我们可以使用 [list()](https://cmake.org/cmake/help/latest/command/list.html#command:list) 命令定义一个列表变量 ```EXTRA_INCLUDES```，用于存放可选 library 的头文件路径。

接下来，创建一个 [if(https://cmake.org/cmake/help/latest/command/if.html#command:if)]() 语句来检查 ```USE_MYMATH``` 的值。在 [if()](https://cmake.org/cmake/help/latest/command/if.html#command:if) 块内，将练习 1 中的 [add_subdirectory()](https://cmake.org/cmake/help/latest/command/add_subdirectory.html#command:add_subdirectory) 命令与其他 [list()](https://cmake.org/cmake/help/latest/command/list.html#command:list) 命令放在一起。

当 ```USE_MYMATH``` 为 ```ON``` 时，将生成列表并将其添加到我们的项目中。当 ```USE_MYMATH``` 为 ```OFF``` 时，列表保持为空。通过这种策略，我们允许用户切换 ```USE_MYMATH``` 来操纵构建中使用的库。

顶级 CMakeLists.txt 文件现在如下所示：

``` CMake
# TODO 8
if(USE_MYMATH)
    add_subdirectory(MathFunctions)
    list(APPEND EXTRA_LIBS MathFunctions)
    list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()
```

现在我们有了这两个列表，我们需要更新 [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_link_libraries.html#command:target_link_libraries) 和 [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories) 来使用它们。更改它们非常简单。

对于 [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_link_libraries.html#command:target_link_libraries)，我们将写出的库名称替换为 ```EXTRA_LIBS```。这看起来像下面这样：

``` CMake
# TODO 9
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})
```

然后，我们对 [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories) 和 ```EXTRA_INCLUDES``` 做同样的事情。

``` CMake
# TODO 10
target_include_directories(Tutorial
    PUBLIC
        "${PROJECT_BINARY_DIR}"
        ${EXTRA_INCLUDES}
)
```

请注意，这是处理许多组件时的传统方法。我们将在本教程的第 3 步中介绍现代方法。

对源代码的相应更改非常简单。首先，在 ```tutorial.cxx``` 中，如果定义了 ```USE_MYMATH```，我们将包含 ```MathFunctions.h``` 头文件。

``` c++
// TODO 11
#ifdef USE_MYMATH
    #include "MathFunctions.h"
#endif
```

然后，在同一个文件中，我们让 ```USE_MYMATH``` 控制使用哪个平方根函数：

``` c++
// TODO 12
#ifdef USE_MYMATH
    const double outputValue = mysqrt(inputValue);
#else
    const double outputValue = sqrt(inputValue);
#endif
```

由于源代码现在需要 ```USE_MYMATH```，我们可以使用以下行将其添加到 ```TutorialConfig.h.in```：

``` c++
// TODO 13
#cmakedefine USE_MYMATH
```

通过这些更改，我们的库现在对于构建和使用它的任何人来说都是完全可选的。

思考题：

为什么在 ```USE_MYMATH``` 选项之后配置 ```TutorialConfig.h.in``` 很重要？如果我们将两者倒置会发生什么？

答案：

因为 ```TutorialConfig.h.in``` 使用了 ```USE_MYMATH``` 的值。如果我们在调用 [option()](https://cmake.org/cmake/help/latest/command/option.html#command:option) 之前配置文件，我们将不会使用 ```USE_MYMATH``` 的预期值。