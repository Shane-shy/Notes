# CMakeLists.txt自动添加cpp文件

## 问题描述

使用Clion编译器写C++时，一整个项目内仅只能包含一个`main`函数。当我们只是想练习C++语言时，想在在同一个项目里写多个样例，这时需要我们手动去配置CmakeLists.txt。每次都去配置会比较麻烦，因此希望有自动化脚本。当我们在同一个项目中添加新的源文件时，自动在CmakeLists.txt中创建一个可执行文件。

## 脚本源码

每次都将以下脚本添加在该项目内的CMakeLists.txt中即可。

```cmake
cmake_minimum_required(VERSION 3.29)
project(your_project_name) # 需要更改为你的项目名

set(CMAKE_CXX_STANDARD 20)

# 遍历项目根目录下所有的 .cpp 文件。这种方法要求所有 cpp 文件命名不重复，不能含有中文，不能含有‘/’等字符！因为它就是直接 Copy 你的源码文件名的。
file(GLOB_RECURSE files *.cpp)
foreach (file ${files})
    string(REGEX REPLACE ".+/(.+)\\..*" "\\1" exe ${file}) # 通过正则表达式，选择.cpp的文件名
    add_executable(${exe} ${file}) # 添加可执行文件，${file}为源文件路径
    message(\ \ \ \ --\ src/${exe}.cpp\ will\ be\ compiled\ to\ bin/${exe}) # 终端打印添加信息
endforeach ()
```

