cmake_minimum_required(VERSION 2.8.9...3.20.5)

project(realsense-file)

include(config.cmake)

FILE(GLOB_RECURSE AllSources
    ${ROSBAG_DIR}/*.h
    ${ROSBAG_DIR}/*.cpp
    ${ROSBAG_DIR}/*.c
)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

add_library(${PROJECT_NAME} SHARED
    ${AllSources}
)

find_package(PkgConfig REQUIRED)
pkg_check_modules(LZ4 REQUIRED liblz4)

target_include_directories(${PROJECT_NAME} PRIVATE
    ${ROSBAG_HEADER_DIRS}
)

target_link_libraries(${PROJECT_NAME} PRIVATE ${LZ4_LIBRARIES})

set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER Library)

set(CMAKECONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/../cmake/realsense2")

install(TARGETS ${PROJECT_NAME}
    EXPORT realsense2Targets
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
