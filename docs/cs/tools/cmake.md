---
title: CMake基础
date: 2024/02/10
math: true
categories:
  - [计算机科学, 工具]
  - [计算机科学, 编程语言]
---

## CMake 简介

CMake 官网：https://cmake.org

WIKI：https://en.wikipedia.org/wiki/CMake

CMake 自身也是一种编程语言，可以用它实现基本的程序逻辑，此如循环、条件分支等等，其中最典型的应用是根据工程的配置来选择性编译部分源代码，比如有些功能只有在测试版本中才会开启。

和其它的构建工具一样，CMake 的本质依然是定义各个目标之间的关联，比如您的项目由哪些可执行文件、动态库、资源文件组成，然后它们又是通过哪些源文件编译，用到了哪些外部关联等等，本身并没有编译和链接的功能。

除了基本的构建功能之外，CMake 还允许您添加单元测试、创建安装包、或者将工程拆分成若干个子目录更利于大型项目的管理。

!!! info 
    CMake 与 [(GNU) Make](https://www.gnu.org/software/make/) 类似，不同之处在于 Make 只是一个命令行工具，用于自动化构建和编译软件项目。Make 使用 Makefile 文件来定义构建过程和依赖关系而 CMake 使用 CMakeLists.txt 。Make 最初是为 Unix 系统设计的，但现在也可用于其他操作系统，且不同平台需要编写不同的 Makefile 文件，而 CMake 则支持跨平台，无需为不同平台分别编写 CMakeLists.txt 文件，还可以更好的实现复杂庞大的项目，功能更加强大，更加方便。

## CMakeLists.txt

一个比较完整的 CMakeLists.txt 内容如下：

工程所需最低 cmake 版本

```cmake cmake
cmake_minimum_required(VERSION 3.10)
```

工程名字 Example

```cmake cmake
project (Example)
```

需要的第三方库 example ，其中 REQUIRED 表示这个库是必须的
cmake 会自动在机器中寻找符合要求、支持 cmake 构建的第三方库

```cmake cmake
find_package(example REQUIRED)
```

用 GLOB 指令将所有源文件存到变量 SRC_FILE 中

```cmake cmake
File(GLOB SRC_FILES
	"${PROJECT_SOURCE_DIR}/src/*.h"
	"${PROJECT_SOURCE_DIR}/src/*.cpp"
	"${PROJECT_ SOURCE_DIR}/src/*.c"
	"${ PROJECT_SOURCE_DIR}/src/*.cc")	## 用正则表达式匹配所有源文件
```

构建一个可执行文件，名字为 CMAKE_PROJECT_NAME 即为 Example 工程名字，由 SRC_FILES 里所有文件编译而成

```cmake cmake
add_executable(${CMAKE_PROJECT_NAME} ${SRC_FILES})
```

使用了多个第三方库，需要进行第三方库的链接，否则会出现经典的符号无法解析的报错

```cmake cmake
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE imgui::imgui)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE glfw)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE GLEW: :GLEW)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE gim)
```

打开对 c++17 的语法支持

```cmake cmake
target_compile features (${CMAKE_PROJECT_MAME} PRIVATE cxx_std_17)
```

简单的自动化脚本，在构建结束后复制所有 文件

```cmake cmake
add_custom_command(
  TARGET ${CMAKE_ PROJECT_NAME}
  POST_BUILD		## 表示在【构建】完成后执行
  COMMAND ${CMAKE_COMMAND} -E copy_directory
  	"${PROJECT_SOURCE_DIR}/assets"
  	"$<TARGET_FILE_DIR:${CMAKE_PROJECT_NAME}>/assets")
```

```cmake cmake
add_custom_command (
  TARGET ${CMAKE_PROJECT_NAME}
  POST_BUILD
  COMMAND $｛CMAKE_COMMAND｝-E copy_directory
    "${PROJECT_SOURCE_DIR) /shader/"
    "$<TARGET_FILE_DIR:${CMAKE_PROJECT_NAME}>/shader")
```

## 配置 (Configure)

根据 CMakeLists 文件生成目标平台下的原生工程

```bash bash
cmake -S . -B build
```

使用了第三方库时，若使用 vcpkg 管理第三方库，则需要：

```bash bash
cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE=<vcpkg安装路径:.../vcpkg>/scripts/buildsystems/vcpkg.cmake
```

使用 VScode 则需在 settings.json 中添加：

```json json
{
  "make configureSettings": {
    "MAKE_TOOLCHAIN_FILE": "<vcpkg安装路径:.../vcpkg>/scripts/buildsystems/vcpkg.cmake"
  }
}
```

## 构建 (Build)

构建生成可执行文件

```bash bash
cmake --build build
```

## 第三方库

使用微软的跨平台开源工具 [vcpkg](https://github.com/microsoft/vcpkg) 可以更方便的安装和管理 c/c++ 第三方库，类似 python 的 pip
