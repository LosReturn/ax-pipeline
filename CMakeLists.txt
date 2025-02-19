project(ax_pipeline)
# check CMake version
cmake_minimum_required (VERSION 3.13 FATAL_ERROR)

# disable inplace builds to prevent source tree corruption.
if (" ${CMAKE_SOURCE_DIR}" STREQUAL " ${CMAKE_BINARY_DIR}")
    message (FATAL_ERROR "FATAL: Building inplace are not allowed. You should create a separate directory for Building.")
endif ()

# set cmake_install_prefix path
if(NOT DEFINED CMAKE_INSTALL_PREFIX)
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE PATH "Installation Directory")
endif()
message(STATUS "CMAKE_INSTALL_PREFIX = ${CMAKE_INSTALL_PREFIX}")


# enable the languages which in use
enable_language (C CXX)

# check toolchain about
if(CMAKE_TOOLCHAIN_FILE)
    set(LIBRARY_OUTPUT_PATH_ROOT ${CMAKE_BINARY_DIR} CACHE PATH "root for library output, set this to change where android libs are compiled to")
    # get absolute path, but get_filename_component ABSOLUTE only refer with source dir, so find_file here :(
    get_filename_component(CMAKE_TOOLCHAIN_FILE_NAME ${CMAKE_TOOLCHAIN_FILE} NAME)
    find_file(CMAKE_TOOLCHAIN_FILE ${CMAKE_TOOLCHAIN_FILE_NAME} PATHS ${CMAKE_SOURCE_DIR} NO_DEFAULT_PATH)
    message(STATUS "CMAKE_TOOLCHAIN_FILE = ${CMAKE_TOOLCHAIN_FILE}")
endif()

# check if building type is not set
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build" FORCE)
endif()

# bsp
if (NOT BSP_MSP_DIR)
    set (BSP_MSP_DIR ${CMAKE_SOURCE_DIR}/axpi_bsp_sdk/msp/out)
endif ()
message(STATUS "BSP_MSP_DIR = ${BSP_MSP_DIR}")


add_subdirectory(examples/sample_vin_ivps_joint_venc_rtsp_vo)
add_subdirectory(examples/sample_vin_ivps_joint_venc_rtsp)
add_subdirectory(examples/sample_vin_ivps_joint_vo)
add_subdirectory(examples/sample_vin_joint)

file(GLOB SAMPLE_RUN_JOINT_CONFIG_FILES "${CMAKE_SOURCE_DIR}/examples/sample_run_joint/config/*")
install (FILES ${SAMPLE_RUN_JOINT_CONFIG_FILES} DESTINATION bin/config)