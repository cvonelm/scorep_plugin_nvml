cmake_minimum_required(VERSION 3.10)

project(scorep_plugin_nvml)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Intialize git submodules if not done already
include(cmake/GitSubmoduleUpdate.cmake)
git_submodule_update()

# Find dependencies
find_package(CUDAToolkit REQUIRED)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(NITRO_POSITION_INDEPENDENT_CODE ON CACHE INTERNAL "Whether to build Nitro libraries with position independent code")
add_subdirectory(lib/scorep_plugin_cxx_wrapper)


# nvml_plugin
add_library(nvml_plugin MODULE src/nvml_plugin.cpp)
target_compile_features(nvml_plugin PUBLIC cxx_std_14)
target_link_libraries(nvml_plugin PUBLIC Scorep::scorep-plugin-cxx CUDA::nvml)
target_include_directories(nvml_plugin PUBLIC include)


#nvml_sync_plugin
add_library(nvml_sync_plugin MODULE src/nvml_sync_plugin.cpp)
target_compile_features(nvml_sync_plugin PUBLIC cxx_std_14)
target_link_libraries(nvml_sync_plugin PUBLIC Scorep::scorep-plugin-cxx CUDA::nvml)
target_include_directories(nvml_sync_plugin PUBLIC include)


#nvml_sampling_plugin
add_library(nvml_sampling_plugin MODULE src/nvml_sampling_plugin.cpp)
target_compile_features(nvml_sampling_plugin PUBLIC cxx_std_14)
target_link_libraries(nvml_sampling_plugin PUBLIC Scorep::scorep-plugin-cxx CUDA::nvml)
target_include_directories(nvml_sampling_plugin PUBLIC include)


install(TARGETS nvml_plugin
        LIBRARY DESTINATION lib
        )

install(TARGETS nvml_sync_plugin
        LIBRARY DESTINATION lib
        )

install(TARGETS nvml_sampling_plugin
        LIBRARY DESTINATION lib
        )
