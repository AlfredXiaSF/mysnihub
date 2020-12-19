cmake_minimum_required(VERSION 3.8)

project(MySnippetsHub)

# produce a file called compile_commands.json, only require if you use Clang as a compiler
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 20) # C++ 20

# use relative addresses in shared object 
set(CMAKE_POSITION_INDEPENDENT_CODE ON) # -fPIC

# run some basic optimization on the code
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -o3")

### the main binary ###

add_library(Core src/core.cpp)
add_executable(Main main/main.cpp)
target_include_directories(Main PRIVATE src)
target_link_libraries(Main Core)


### the tests ###

# use shared CRT for gtest lib anyway, if don't set this link will fail on windows
set (gtest_force_shared_crt ON CACHE BOOL "MSVC defaults to shared CRT" FORCE)

add_subdirectory(third_party/googletest)

## we specify that googletest library as follow
# use the latest C++ standard
# disables support for some STL Tuple classes which throw errors in some Windows environments 
target_compile_definitions(gtest PUBLIC GTEST_LANG_CXX20 GTEXT_HAS_TR1_TUPLE=0) 

add_executable(tests)
target_sources(tests PRIVATE tests/SomeTests.cpp)
set_target_properties(tests PROPERTIES COMPILE_FLAGS "${cxx_strict}")
target_include_directories(tests PRIVATE src)
target_link_libraries(tests gtest gtest_main Core)