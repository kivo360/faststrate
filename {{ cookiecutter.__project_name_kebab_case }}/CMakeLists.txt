cmake_minimum_required(VERSION 3.15...3.29)
project({{ cookiecutter.__project_name_snake_case }})

find_package(pybind11 CONFIG REQUIRED)

# Add Rust library
add_custom_target(
    cargo-build ALL
    COMMAND cargo build --release
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)

# Create Python extension module
pybind11_add_module({{ cookiecutter.__project_name_snake_case }}_rust src/main.cpp)

# Add dependency on Rust build
add_dependencies({{ cookiecutter.__project_name_snake_case }}_rust cargo-build)

# Link the Rust library
target_link_libraries({{ cookiecutter.__project_name_snake_case }}_rust PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/target/release/lib{{ cookiecutter.__project_name_snake_case }}_rust.a)

# Set properties
target_compile_definitions({{ cookiecutter.__project_name_snake_case }}_rust PRIVATE VERSION_INFO=${EXAMPLE_VERSION_INFO})