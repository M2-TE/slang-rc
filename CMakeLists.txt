cmake_minimum_required(VERSION 3.22...3.24)
project(slang-rc LANGUAGES CXX)


set(SLANG_RC_TOP_LEVEL_PROJECT OFF)
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    set(SLANG_RC_TOP_LEVEL_PROJECT ON)
    set(SLANG_RC_SHADER_DIR "${CMAKE_CURRENT_SOURCE_DIR}/shaders")
endif()

# check if options are undefined
if(NOT DEFINED SLANG_RC_SHADER_DIR)
    message(FATAL_ERROR "SLANG_RC_SHADER_DIR must be defined, set it to the directory containing your shaders")
endif()

include(FetchContent)
set(CMAKE_POLICY_DEFAULT_CMP0077 NEW) # disallow option() from overwriting set()
if(SLANG_RC_TOP_LEVEL_PROJECT)
    set(FETCHCONTENT_QUIET OFF) # enable git output for FetchContent steps
    # set(FETCHCONTENT_UPDATES_DISCONNECTED ON) # speed up consecutive config runs
endif()

## SLANG
# main options
set(SLANG_EMBED_CORE_MODULE ON)
set(SLANG_EMBED_CORE_MODULE_SOURCE ON)
set(SLANG_ENABLE_DXIL OFF)
set(SLANG_ENABLE_ASAN OFF)
set(SLANG_ENABLE_FULL_IR_VALIDATION OFF)
set(SLANG_ENABLE_IR_BREAK_ALLOC OFF)
set(SLANG_ENABLE_GFX OFF) # ???
set(SLANG_ENABLE_SLANGD OFF)
set(SLANG_ENABLE_SLANGC ON)
set(SLANG_ENABLE_SLANGRT OFF)
set(SLANG_ENABLE_SLANG_GLSLANG OFF)
set(SLANG_ENABLE_TESTS OFF)
set(SLANG_ENABLE_EXAMPLES OFF)
set(SLANG_LIB_TYPE "SHARED")
set(SLANG_ENABLE_RELEASE_DEBUG_INFO OFF)
set(SLANG_ENABLE_SPLIT_DEBUG_INFO OFF)
set(SLANG_SLANG_LLVM_FLAVOR "DISABLE")
# set(SLANG_SLANG_LLVM_BINARY_URL "")
set(SLANG_GENERATORS_PATH "")
# auto options
set(SLANG_ENABLE_CUDA OFF)
set(SLANG_ENABLE_OPTIX OFF)
set(SLANG_ENABLE_NVAPI OFF)
set(SLANG_ENABLE_AFTERMATH OFF)
set(SLANG_ENABLE_XLIB OFF)
# advanced options
set(SLANG_ENABLE_DX_ON_VK OFF)
set(SLANG_ENABLE_SLANG_RHI OFF)
set(SLANG_USE_SYSTEM_MINIZ OFF)
set(SLANG_USE_SYSTEM_LZ4 OFF)
set(SLANG_USE_SYSTEM_VULKAN_HEADERS ON)
set(SLANG_USE_SYSTEM_SPIRV_HEADERS OFF)
set(SLANG_USE_SYSTEM_UNORDERED_DENSE OFF)
set(SLANG_SPIRV_HEADERS_INCLUDE_DIR "${spirv-headers_SOURCE_DIR}/include/")
FetchContent_Declare(slang
    GIT_REPOSITORY "https://github.com/shader-slang/slang.git"
    GIT_TAG "v2025.1"
    GIT_SHALLOW ON
    GIT_SUBMODULES
        "external/lz4"
        "external/miniz"
        "external/spirv-headers"
        "external/unordered_dense"
    OVERRIDE_FIND_PACKAGE
    EXCLUDE_FROM_ALL
    SYSTEM)

# build it all
FetchContent_MakeAvailable(slang)