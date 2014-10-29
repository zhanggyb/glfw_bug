# CMake file for BlendInt project
#

cmake_minimum_required(VERSION 2.8.11)

project (glfw_bug)

set(CMAKE_BUILD_TYPE_INIT "Debug")

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(OS_LINUX TRUE)
    add_definitions(-D__UNIX__)
    add_definitions(-D__LINUX__)
    add_definitions(-DGL_GLEXT_PROTOTYPES)
endif()

if(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    set(OS_WINDOWS TRUE)
    add_definitions(-D__WIN__)
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(OS_APPLE TRUE)
    add_definitions(-D__UNIX__)
    add_definitions(-D__APPLE__)
endif()

# start of options
# end of options

if(OS_APPLE)
    set(CMAKE_LIBRARY_PATH ${CMAKE_LIBRARY_PATH} /usr/local/lib)
endif()

find_package(OpenGL REQUIRED)
if(OPENGL_FOUND)
    include_directories(${OPENGL_INCLUDE_DIR})
    set(LIBS ${LIBS} ${OPENGL_LIBRARIES})
    if(OS_APPLE)
        FIND_PATH(GL_H_INCLUDE_DIR gl.h)
        include_directories(${GL_H_INCLUDE_DIR})
    endif()
endif()

if(OS_APPLE)
    set(LIBS ${LIBS} pthread)
else()
    # add rt library
    set(LIBS ${LIBS} rt)
endif()

if(CMAKE_BUILD_TYPE STREQUAL "Debug" OR
        CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    add_definitions(-DDEBUG)
endif()

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -p -pg")
    endif()
    if(WITH_GPERFTOOLS)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pg")
        set(LIBS ${LIBS} profiler)
    endif()
endif()

if(MSVC)
    # Force to always compile with W4
    if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
        string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    else()
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
    endif()
elseif(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
    # Update if necessary
    #set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-long-long -pedantic")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
endif()

# build glfw
add_subdirectory(extern/glfw)
include_directories(extern/glfw/include)

if(OS_LINUX)
#    set(LIBS ${LIBS} Xcursor)   # Xcursor missing in latest glfw3
endif()

add_subdirectory(src)

