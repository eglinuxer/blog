# 第 3 步：为库添加使用要求

我们在上一步中定义了 library，这一步我们为 library 添加更多的属性，来控制其使用要求。

规范 CMake target 使用要求的命令主要有：

- [target_compile_definitions()](https://cmake.org/cmake/help/latest/command/target_compile_definitions.html#command:target_compile_definitions)
- [target_compile_options()](https://cmake.org/cmake/help/latest/command/target_compile_options.html#command:target_compile_options)
- [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)
- [target_link_directories()](https://cmake.org/cmake/help/latest/command/target_link_directories.html#command:target_link_directories)
- [target_link_options()](https://cmake.org/cmake/help/latest/command/target_link_options.html#command:target_link_options)
- [target_precompile_headers()](https://cmake.org/cmake/help/latest/command/target_precompile_headers.html#command:target_precompile_headers)
- [target_sources()](https://cmake.org/cmake/help/latest/command/target_sources.html#command:target_sources)

在一个目标上添加的使用要求，可以传递到其依赖项上。关于属性的传播，这里不展开，不然大家又会迷失在细节中。

## 练习 1 - 添加库的使用要求

### 目标

这一步，我们的主要目标就是给之前在 [第 2 步](https://www.eglinux.com/cmake/tutorial/step2/)添加的库追加一些使用上的规范和要求。

### 有用的资源

- [CMAKE_CURRENT_SOURCE_DIR](https://cmake.org/cmake/help/latest/variable/CMAKE_CURRENT_SOURCE_DIR.html#variable:CMAKE_CURRENT_SOURCE_DIR)

### 要编辑的文件

- MathFunctions/CMakeLists.txt
- CMakeLists.txt

### 开始

在这个练习中，我们会使用 CMake 现代方法优化[第二步](https://www.eglinux.com/cmake/tutorial/step2/)中的 CMake。我们将让我们的库定义自己的使用要求，以便根据需要将它们传递给其他目标。在这种情况下，```MathFunctions``` 将自己指定任何需要的包含目录。然后，消费目标 ```Tutorial``` 只需要链接到 ```MathFunctions```，而不用担心任何额外的包含目录。

在这一步中，我们需要按照提示完成[源码](https://github.com/eglinuxer/study_cmake/tree/main/tutorial/step3)中的 ```TODO 1``` 至 ```TODO 3```。

### 构建并运行

``` bash
cmake -S . -B build
cmake --build build
```

接下来，使用新建的 Tutorial 并验证它是否按预期工作。

### 解决方案

让我们更新[上一步](https://www.eglinux.com/cmake/tutorial/step2/)中的代码，以使用现代 CMake 的使用要求方法。

我们想声明任何链接到 ```MathFunctions``` 的人都需要包含当前源目录，而 ```MathFunctions``` 本身不需要。这可以用 ```INTERFACE``` 使用要求来表达。请记住 ```INTERFACE``` 意味着消费者需要但生产者不需要的东西。

在 ```MathFunctions/CMakeLists.txt``` 的最后，使用带有 ```INTERFACE``` 关键字的 [target_include_directories()](https://cmake.org/cmake/help/latest/command/target_include_directories.html#command:target_include_directories)，如下：

``` CMake
# TODO 1
target_include_directories(MathFunctions
    INTERFACE
        ${CMAKE_CURRENT_SOURCE_DIR}
)
```

现在我们已经指定了 ```MathFunctions``` 的使用要求，我们可以安全地从顶级 ```CMakeLists.txt``` 中删除我们对 ```EXTRA_INCLUDES``` 变量的使用，这里：

``` CMake
# TODO 2
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()
```

``` CMake
# TODO 3
target_include_directories(Tutorial
    PUBLIC
        "${PROJECT_BINARY_DIR}"
)
```

请注意，使用这种技术，我们的可执行目标为使用我们的库所做的唯一事情就是使用库目标的名称调用 [target_link_libraries()](https://cmake.org/cmake/help/latest/command/target_link_libraries.html#command:target_link_libraries)。