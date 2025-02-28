cmake_minimum_required(VERSION 3.19)
project(gm82core C)

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

if(${CMAKE_C_COMPILER_ID} STREQUAL "MSVC")
    set(CMAKE_C_FLAGS "/O2 /GS- /W2 /WX")
endif()

add_library(gm82core SHARED gm82core.c hrt.c lovey01.c terrible_gm8_hacking.c)

option(INSTALL_GEX "Install the generated .gex" OFF)

if (INSTALL_GEX)
	add_custom_command(
		TARGET gm82core
		POST_BUILD
		COMMAND py build_gex.py gm82core.gej
		WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
		COMMENT "Building .gex"
	)
else()
	add_custom_command(
		TARGET gm82core
		POST_BUILD
		COMMAND py build_gex.py gm82core.gej --noinstall
		WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
		COMMENT "Building .gex"
	)
endif()