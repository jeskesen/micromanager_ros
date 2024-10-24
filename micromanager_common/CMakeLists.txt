cmake_minimum_required(VERSION 3.8)
project(micromanager_common)

set(micromanager_common_MAJOR_VERSION 0)
set(micromanager_common_MINOR_VERSION 0)
set(micromanager_common_PATCH_VERSION 0)
set(micromanager_common_VERSION
  ${micromanager_common_MAJOR_VERSION}.${micromanager_common_MINOR_VERSION}.${micromanager_common_PATCH_VERSION})

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

include(ExternalProject)

# Define MMCore as an ExternalProject
set(MMCORE_BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/MMCore)
set(MMCORE_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../mmCoreAndDevices CACHE PATH "Path to MMCore source directory")
ExternalProject_Add(
  MMCoreAndDevices
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}
  CONFIGURE_COMMAND mkdir -p ${MMCORE_BUILD_DIR} && meson setup ${MMCORE_BUILD_DIR} ${MMCORE_SOURCE_DIR}/MMCore
  BUILD_COMMAND  meson compile -C ${MMCORE_BUILD_DIR}
  INSTALL_COMMAND ""
)

# install(
#   DIRECTORY include/
#   DESTINATION include
# )
install(
  FILES 
    ${MMCORE_BUILD_DIR}/libMMCore.so 
    ${MMCORE_BUILD_DIR}/subprojects/MMDevice/libMMDevice.so
  DESTINATION lib)
