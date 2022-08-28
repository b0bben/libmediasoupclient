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


# Compile definitions for libwebrtc.
target_compile_definitions(${PROJECT_NAME} PUBLIC
  $<$<NOT:$<PLATFORM_ID:Windows>>:WEBRTC_POSIX>
  $<$<PLATFORM_ID:Windows>:WEBRTC_WIN>
  $<$<PLATFORM_ID:Windows>:NOMINMAX>
  $<$<PLATFORM_ID:Windows>:WIN32_LEAN_AND_MEAN>
  $<$<PLATFORM_ID:Darwin>:WEBRTC_MAC>

  $<$<CONFIG:Debug>:DEBUG>
  $<$<CONFIG:Debug>:_DEBUG>
)

install(TARGETS sdptransform DESTINATION lib)
install(FILES ${HEADER_FILES} DESTINATION include/sdptransform)
