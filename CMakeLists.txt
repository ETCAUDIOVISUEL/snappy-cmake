cmake_minimum_required(VERSION 2.8)
PROJECT(snappy)

include(CheckIncludeFile)

# version 
set(SNAPPY_MAJOR 1)
set(SNAPPY_MINOR 0)
set(SNAPPY_PATCHLEVEL 4)


set(SRCS 
    snappy.cc snappy-sinksource.cc snappy-stubs-internal.cc snappy-c.cc)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

CHECK_INCLUDE_FILE(stdint.h HAVE_STDINT_H)
CHECK_INCLUDE_FILE(stddef.h HAVE_STDDEF_H)

if(HAVE_STDINT_H)
    set(ac_cv_have_stdint_h 1)
else()
    set(ac_cv_have_stdint_h 0)
endif()

if(HAVE_STDDEF_H)
    set(ac_cv_have_stddef_h 1)
else()
    set(ac_cv_have_stddef_h 0)
endif()

if(WIN32)
    add_definitions(-DHAVE_WINDOWS_H)
endif()


configure_file(snappy-stubs-public.h.in 
    ${CMAKE_CURRENT_BINARY_DIR}/snappy-stubs-public.h
    @ONLY)

add_library(snappy ${SRCS})

ENABLE_TESTING()
find_package(GTest)
add_definitions(-DHAVE_GTEST)
include_directories(${GTEST_INCLUDE_DIRS})
add_executable(snappy_unittest
    snappy_unittest.cc
    snappy-test.cc)

target_link_libraries(snappy_unittest
    snappy
    ${GTEST_BOTH_LIBRARIES})
