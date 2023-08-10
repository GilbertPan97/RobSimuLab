cmake_minimum_required(VERSION 3.5)

# config project version
set(ROBSIMULAB_VERSION_MAJOR 0)
set(ROBSIMULAB_VERSION_MINOR 4)
set(ROBSIMULAB_VERSION_PATCH 0)
set(BULLET_VERSION "${ROBSIMULAB_VERSION_MAJOR}.${ROBSIMULAB_VERSION_MINOR}.${ROBSIMULAB_VERSION_PATCH}")
message(STATUS "ROBSIMULAB version is: ${BULLET_VERSION}")

# local dependent package
set(PROJECT_ROOT_DIR "${CMAKE_CURRENT_SOURCE_DIR}")
set(THIRD_PARTY "${PROJECT_ROOT_DIR}/third_party")
project(RobSimuLab VERSION ${BULLET_VERSION})

option(BUILD_EXAMPLES "build the examples." ON)
option(ENABLE_VHACD "Use VHACD in BulletRobotics and pybullet" ON)
option(BUILD_SHARED_LIBS "Use shared libraries" OFF)

set(CMAKE_DEBUG_POSTFIX "d" CACHE STRING "add a postfix, usually d on windows")
set(CMAKE_RELEASE_POSTFIX "" CACHE STRING "add a postfix, usually empty on windows")
set(CMAKE_RELWITHDEBINFO_POSTFIX "rd" CACHE STRING "add a postfix, usually empty on windows")
set(CMAKE_MINSIZEREL_POSTFIX "s" CACHE STRING "add a postfix, usually empty on windows")

# platform check
if("${CMAKE_SIZEOF_VOID_P}" STREQUAL "4")
    set(SFRCALIB_PLATFORM "86")
else()
    set(SFRCALIB_PLATFORM "64")
endif()

find_package(OpenGL)
if (OPENGL_FOUND)
	message("OPENGL FOUND")
	message(${OPENGL_LIBRARIES})
else (OPENGL_FOUND)
	message("OPENGL NOT FOUND")
	set(OPENGL_gl_LIBRARY opengl32)
	set(OPENGL_glu_LIBRARY glu32)
endif (OPENGL_FOUND)

if (APPLE)
    find_package(COCOA_LIBRARY Cocoa)
endif()

# add window platform resource config macro definition
include(cmake/WinResource.cmake)

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
    # set(CMAKE_BUILD_TYPE "Release")
endif (NOT CMAKE_BUILD_TYPE)


option(BUILD_ENET "Set when you want to build apps with enet UDP networking support" ON)
option(BUILD_CLSOCKET "Set when you want to build apps with enet TCP networking support" ON)

if(NOT WIN32 AND NOT APPLE)
    option(BUILD_EGL "Build OpenGL/EGL" ON)
    if(BUILD_EGL)
        add_definitions(-DBT_USE_EGL)
    endif(BUILD_EGL)
endif()

option(BUILD_EXTRAS "Set when you want to build the extras" ON)

if(BUILD_EXTRAS)
    subdirs(Extras)
endif(BUILD_EXTRAS)

subdirs(src)
add_subdirectory(cmake)

if(BUILD_EXAMPLES)
    subdirs(examples)
endif()