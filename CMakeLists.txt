cmake_minimum_required(VERSION 3.21)
option(FORMAT_VALIDATION_ONLY "Configure formatting without compilers or native dependencies" OFF)
if(FORMAT_VALIDATION_ONLY)
    project(FormatValidation LANGUAGES NONE)
    include(cmake/FormatValidation.cmake)
    return()
endif()
include(cmake/Metamod.cmake)
project(ascurl LANGUAGES C CXX)
if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build configuration" FORCE)
endif()
include("${METAMOD_SOURCE_PATH}/cmake/SDK.cmake")
include(cmake/ASExt.cmake)

add_library(ascurl SHARED
    src/ascurl.cpp
    src/dllapi.cpp
    src/engine_api.cpp
    src/engine_hook.cpp
    src/h_export.cpp
    src/meta_api.cpp
    src/server_hook.cpp
)
mmfg_configure_target(ascurl)
target_link_libraries(ascurl PRIVATE ASExt::SDK)
if(WIN32)
    target_sources(ascurl PRIVATE src/dllmain.cpp)
    target_compile_definitions(ascurl PRIVATE ASCURL_EXPORTS)
endif()
target_include_directories(ascurl PRIVATE thirdparty/curl/include/curl thirdparty/openssl/include)

include(cmake/FormatValidation.cmake)
