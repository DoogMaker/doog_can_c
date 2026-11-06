cmake_minimum_required(VERSION 3.10.0)

# 定义项目：名称 cpplearn，版本 0.1.0，启用 C 和 C++ 两种语言
project(doog_can_c VERSION 0.1.0 LANGUAGES C CXX)

# 设置输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/out)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/out)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/out)

# 使用 C++17 标准编译
set(CMAKE_CXX_STANDARD 17)

# 把要编进动态库的源文件组装成一个列表变量
# 新增源文件时往这里加一行即可
set(TEST
    tests/test1.cpp
)

set(LIB_SRC
    src/doog_can_c.cpp
)

# 生成动态库：SHARED 表示 .so，产物为 build/doog_can_c.so
# 名字 doog_can_c 会被自动加上 lib 前缀和 .so 后缀
# 改成 STATIC 就是静态库 doog_can_c.a
add_library(doog_can_c SHARED ${LIB_SRC})

# 给库指定头文件搜索路径 include/
# ${PROJECT_SOURCE_DIR} 是项目根目录（CMakeLists.txt 所在目录）
# PUBLIC：库自己能用，链接它的目标（main）也自动继承这个路径
# 所以 doog_can_c.cpp 里能直接写 #include "XXXX.h"
target_include_directories(doog_can_c PUBLIC ${PROJECT_SOURCE_DIR}/include)

# 生成可执行文件 build/test1
# 带 test1() 的源文件不放进库，单独作为可执行目标
add_executable(test1 ${TEST})

# 把动态库 doog_can_c 链接给 test1
# PRIVATE：这个依赖只给 test1 用，不再向外传递
target_link_libraries(test1 PRIVATE doog_can_c)