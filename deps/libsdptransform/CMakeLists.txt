cmake_minimum_required(VERSION 3.0)

project(sdptransform VERSION 1.2.9)

# For CMake >= 3.1.
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
# For CMake < 3.1.
add_compile_options(-std=c++11)

# Project options.
option(SDPTRANSFORM_BUILD_TESTS "Build unit tests and readme helpers" OFF)

if (${SDPTRANSFORM_BUILD_TESTS})
	subdirs(test readme-helper)
endif()


include_directories(${sdptransform_SOURCE_DIR}/include)

set(
	SOURCE_FILES
	src/grammar.cpp
	src/parser.cpp
	src/writer.cpp
)

set(
	HEADER_FILES
	include/sdptransform.hpp
	include/json.hpp
)

add_library(sdptransform STATIC ${SOURCE_FILES} ${HEADER_FILES})

# Need because webrtc always compiles statically and including CRT as MT on windows
if(WIN32 AND NOT MINGW)
  set_property(TARGET ${PROJECT_NAME} PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
  target_link_libraries(${PROJECT_NAME}	PRIVATE WS2_32 secur32.lib winmm.lib dmoguids.lib wmcodecdspuuid.lib msdmo.lib strmiids.lib)
endif()

install(TARGETS sdptransform DESTINATION lib)
install(FILES ${HEADER_FILES} DESTINATION include/sdptransform)
