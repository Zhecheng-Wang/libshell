cmake_minimum_required(VERSION 3.1)
project(libshell)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

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
