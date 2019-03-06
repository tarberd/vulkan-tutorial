cmake_minimum_required(VERSION 3.7)

project(testing_vulkan)

find_package(Vulkan REQUIRED)
find_package(glfw3 3.3 REQUIRED)

add_custom_command(
    OUTPUT vert.spv
    COMMAND glslangValidator -V ${CMAKE_SOURCE_DIR}/shader.vert -o ${PROJECT_BINARY_DIR}/vert.spv
)

add_custom_command(
    OUTPUT frag.spv
    COMMAND glslangValidator -V ${CMAKE_SOURCE_DIR}/shader.frag -o ${PROJECT_BINARY_DIR}/frag.spv
    DEPENDS ${PROJECT_BINARY_DIR}/vert.spv
)

add_custom_target(
    shaders
    DEPENDS ${PROJECT_BINARY_DIR}/vert.spv
    DEPENDS ${PROJECT_BINARY_DIR}/frag.spv
)

add_executable(test main.cpp)

add_dependencies(test shaders)

set_target_properties(test PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)

target_link_libraries(test Vulkan::Vulkan glfw)
