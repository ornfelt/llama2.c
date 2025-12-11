# To build:
#mkdir build
#cd build
#cmake ..
#cmake --build . --config Release

cmake_minimum_required(VERSION 3.15)
project(llama2 C)

# Find and enable OpenMP
find_package(OpenMP REQUIRED)

# Add the executable
add_executable(llama2 run.c win.c)

# Link OpenMP
target_link_libraries(llama2 PRIVATE OpenMP::OpenMP_C)

# Include current directory for headers
target_include_directories(llama2 PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})

# Set optimization flags based on compiler
if(MSVC)
    # Fast floating point and maximum optimization (equivalent to /fp:fast /Ox)
    target_compile_options(llama2 PRIVATE /fp:fast /Ox)
    
    # Alternative optimization levels:
    #target_compile_options(llama2 PRIVATE /fp:fast /O2)  # Standard optimization
    #target_compile_options(llama2 PRIVATE /fp:fast /O1)  # Minimize size
else()
    # GCC/Clang: Fast optimization (equivalent to /fp:fast /Ox)
    target_compile_options(llama2 PRIVATE -Ofast)
    
    # Alternative optimization levels:
    #target_compile_options(llama2 PRIVATE -O3)  # Aggressive optimization
    #target_compile_options(llama2 PRIVATE -O2)  # Standard optimization
    #target_compile_options(llama2 PRIVATE -O1)  # Basic optimization
endif()
