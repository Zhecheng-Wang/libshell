cmake_minimum_required(VERSION 3.1)
project(libshell)

set(CMAKE_CXX_STANDARD 17)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

if(NOT CMAKE_BUILD_TYPE)
    message(STATUS "No build type selected, default to Release")
    set(CMAKE_BUILD_TYPE Release)
endif()

# Find OpenMP
find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

# libigl
option(LIBIGL_USE_STATIC_LIBRARY     "Use libigl as static library" OFF)
option(LIBIGL_OPENGL 	 "Use OpenGL"          ON)
option(LIBIGL_GLFW       "Use GLFW"            ON)
option(LIBIGL_IMGUI 	 "Use ImGui"           ON)
include(libigl)

# Add your project files
file(GLOB LIBFILES src/*.cpp src/SecondFundamentalForm/*.cpp src/MaterialModel/*.cpp)
add_library(${PROJECT_NAME} STATIC ${LIBFILES})
target_link_libraries(${PROJECT_NAME} igl::core)

file(GLOB EXAMPLEFILES example/*.cpp)
add_executable(example_${PROJECT_NAME} ${EXAMPLEFILES})
target_link_libraries(example_${PROJECT_NAME} ${PROJECT_NAME} igl::core igl::opengl igl::glfw igl::imgui)

file(GLOB TESTFILES tests/*.cpp)
add_executable(tests_${PROJECT_NAME} ${TESTFILES})
target_link_libraries(tests_${PROJECT_NAME} ${PROJECT_NAME} igl::core)
