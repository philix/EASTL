#-------------------------------------------------------------------------------------------
# Copyright (C) Electronic Arts Inc.  All rights reserved.
#-------------------------------------------------------------------------------------------

#-------------------------------------------------------------------------------------------
# CMake info 
#-------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.4)
project(EASTLBenchmarks)
include(CTest)

#-------------------------------------------------------------------------------------------
# Include directories 
#-------------------------------------------------------------------------------------------
include_directories(source)
include_directories(../test/packages/EABase/include/Common)
include_directories(../test/packages/EAStdC/include)
include_directories(../test/packages/EATest/include)
include_directories(../test/packages/EAMain/include)
include_directories(../test/packages/EAAssert/include)
include_directories(../test/source)

#-------------------------------------------------------------------------------------------
# Compiler Flags
#-------------------------------------------------------------------------------------------
set(CMAKE_CXX_STANDARD 11)
if(UNIX)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

#-------------------------------------------------------------------------------------------
# Libraries 
#-------------------------------------------------------------------------------------------
add_subdirectory(../ EASTL)

#-------------------------------------------------------------------------------------------
# Source files 
#-------------------------------------------------------------------------------------------
file(GLOB EASTLBENCHMARK_SOURCES "source/*.cpp")

#-------------------------------------------------------------------------------------------
# Defines 
#-------------------------------------------------------------------------------------------
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
# add_definitions(-DEASTL_THREAD_SUPPORT_AVAILABLE=0) 
add_definitions(-DEASTL_OPENSOURCE)
add_definitions(-D_SILENCE_STDEXT_HASH_DEPRECATION_WARNINGS)  # silence std::hash_map deprecation warnings


#-------------------------------------------------------------------------------------------
# Executable definition
#-------------------------------------------------------------------------------------------
add_executable(EASTLBenchmarks ${EASTLBENCHMARK_SOURCES})

set(EASTLBenchmark_Libraries
    # EAAssert
    # EABase
    # EAMain
    EASTL
    # EAStdC
	# EAThread
    # EATest
    )
target_link_libraries(EASTLBenchmarks ${EASTLBenchmark_Libraries})

#-------------------------------------------------------------------------------------------
# Run Unit tests and verify the results. 
#-------------------------------------------------------------------------------------------
add_test(EASTLBenchmarkRuns EASTLBenchmarks)
set_tests_properties (EASTLBenchmarkRuns PROPERTIES PASS_REGULAR_EXPRESSION "RETURNCODE=0")

