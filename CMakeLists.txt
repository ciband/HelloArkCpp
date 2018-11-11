cmake_minimum_required(VERSION 3.2.2)
set(CMAKE_TOOLCHAIN_FILE "${CMAKE_SOURCE_DIR}/cmake/toolchain.cmake")

include("${CMAKE_SOURCE_DIR}/cmake/HunterGate.cmake")

HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.23.29.tar.gz"
    SHA1 "797be99a8a4f4666766257563fad8aa3aa032da2"
)

project(HelloArkCpp)


set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
set(PROJECT_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)

# clone submodules
execute_process(
	COMMAND git submodule update --init --recursive
	WORKING_DIRECTORY ${PROJECT_INCLUDE_DIR}
)

if (MSVC)
 	add_definitions(
 		-D_CRT_SECURE_NO_WARNINGS
		-D_SCL_SECURE_NO_WARNINGS
		-DNOMINMAX
	)
  set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /utf-8")
elseif (UNIX)
	set (CMAKE_CXX_FLAGS "-std=c++11")
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

add_subdirectory(include/cpp-client)
add_subdirectory(include/cpp-crypto)

include_directories(
	${PROJECT_SOURCE_DIR}
	${PROJECT_INCLUDE_DIR}/ark-client/src
	${PROJECT_INCLUDE_DIR}/ark-crypto/src
)

find_library(HelloArkCpp
	NAMES Ark-Cpp-Client-lib Ark-Cpp-Crypto-lib
	PATHS ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}
)

add_executable(HelloArkCpp
	${PROJECT_SOURCE_DIR}/main.cpp
)

target_link_libraries(HelloArkCpp Ark-Cpp-Client-lib Ark-Cpp_Crypto-lib)
