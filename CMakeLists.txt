cmake_minimum_required(VERSION 3.22)
project(codm_tex)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/libs)
set(DEBUG_SYMBOLS_PATH ${CMAKE_BINARY_DIR}/symbols)

# Silence policy noise from older subprojects (LSPlt still declares 3.4.1).
if(POLICY CMP0000)
  cmake_policy(SET CMP0000 NEW)
endif()
set(CMAKE_POLICY_VERSION_MINIMUM 3.5 CACHE STRING "" FORCE)

# LSPlt (PLT hook library) is cloned next to this file by the CI workflow.
add_subdirectory(LSPlt/lsplt/src/main/jni lsplt)

# Zygisk module library; packaged as zygisk/<abi>.so.
add_library(codm_tex SHARED main.cpp)
target_include_directories(codm_tex PRIVATE
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/LSPlt/lsplt/src/main/jni/include)
target_link_libraries(codm_tex lsplt_static log android)
