cmake_minimum_required(VERSION 3.8)

project(MySnippetsHub)

# produce a file called compile_commands.json, only require if you use Clang as a compiler
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 20) # C++ 20

# use relative addresses in shared object 
set(CMAKE_POSITION_INDEPENDENT_CODE ON) # -fPIC

# run some basic optimization on the code
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")

add_library(Core src/core.cpp)
add_executable(Main main/main.cpp)
target_link_libraries(Main Core)