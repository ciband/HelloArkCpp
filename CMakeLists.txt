cmake_minimum_required(VERSION 3.2.2)

project(HelloArkCpp)


# clone submodules
execute_process(
	COMMAND git submodule update --init --recursive
	WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/include
)



if (MSVC)
 	add_definitions(
 		-D_CRT_SECURE_NO_WARNINGS
		-D_SCL_SECURE_NO_WARNINGS
		-DNOMINMAX
	)
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /utf-8")
elseif (UNIX)
	set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

add_subdirectory(lib/cpp-crypto)
#add_subdirectory(lib/cpp-client)

add_executable(HelloArkCpp
	${PROJECT_SOURCE_DIR}/main.cpp
)

#target_link_libraries(HelloArkCpp Ark-Cpp-Client-lib)
target_link_libraries(HelloArkCpp Ark-Cpp-Crypto-lib)
