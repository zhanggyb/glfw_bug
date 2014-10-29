# CMake file for BlendInt project
#

file(GLOB sourcefiles "*.c")
list(APPEND glfw_bug_SRC ${sourcefiles})

add_executable(glfw_bug ${glfw_bug_SRC})

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
target_link_libraries(glfw_bug ${LIBS})

