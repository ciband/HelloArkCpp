cmake_minimum_required(VERSION 3.2.2)
set(CMAKE_TOOLCHAIN_FILE "${CMAKE_SOURCE_DIR}/cmake/toolchain.cmake")

project(HelloArkCpp)


set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
#set(PROJECT_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)

# clone submodules
execute_process(
	COMMAND git submodule update --init --recursive
	WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/include
)

# build cpp-client
execute_process(
	COMMAND cmake .
	WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/include/cpp-client/src
)
execute_process(
	COMMAND cmake --build .
	WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/include/cpp-client/src
)

#build cpp-crypto
execute_process(
	COMMAND cmake .
	WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/include/cpp-crypto/src
)
execute_process(
	COMMAND cmake --build .
	WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/include/cpp-crypto/src
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

include_directories(
	${PROJECT_SOURCE_DIR}
	${PROJECT_SOURCE_DIR}/include/cpp-client/src
	${PROJECT_SOURCE_DIR}/include/cpp-crypto/src
	#directories that you shouldn't have to include in the future
	${PROJECT_SOURCE_DIR}/include/cpp-crypto/include/
	${PROJECT_SOURCE_DIR}/include/cpp-crypto/include/uECC
	${PROJECT_SOURCE_DIR}/include/cpp-client/include/
)

add_executable(HelloArkCpp
	${PROJECT_SOURCE_DIR}/main.cpp
)

find_library(HelloArkCpp
	NAMES Ark-Cpp-Client-lib
	PATHS ${CMAKE_SOURCE_DIR}/include/ark-client/lib
)

find_library(HelloArkCpp
	NAMES Ark-Cpp-Crypto-lib
	PATHS ${CMAKE_SOURCE_DIR}/include/ark-crypto/lib
)

target_link_libraries(HelloArkCpp Ark-Cpp-Client-lib)
target_link_libraries(HelloArkCpp Ark-Cpp-Crypto-lib)
