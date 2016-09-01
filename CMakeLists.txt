
if(WIN32)
cmake_minimum_required (VERSION 3.6)
else()
cmake_minimum_required (VERSION 3.0)
endif()

project(opengl-drawing-a-triangle)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set (opengl-drawing-a-triangle_VERSION_MAJOR 1)
set (opengl-drawing-a-triangle_VERSION_MINOR 0)
set (opengl-drawing-a-triangle_VERSION_BUILD 0)

FILE(GLOB SOURCE_FILES
  src/main.cpp
)
source_group("Source Files" FILES ${SOURCE_FILES})

add_executable(opengl-drawing-a-triangle ${SOURCE_FILES})

set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT opengl-drawing-a-triangle)

if (MSVC)
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xfz ${CMAKE_SOURCE_DIR}/lib/glew-1.13.0-win32.zip
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xfz ${CMAKE_SOURCE_DIR}/lib/glfw-3.1.2.bin.WIN32.zip
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xfz ${CMAKE_SOURCE_DIR}/lib/glm-0.9.7.1.zip
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    )

    target_link_libraries(opengl-drawing-a-triangle
      ${CMAKE_BINARY_DIR}/glew-1.13.0/lib/Release/Win32/glew32.lib
      ${CMAKE_BINARY_DIR}/glfw-3.1.2.bin.WIN32/lib-vc2015/glfw3.lib
      opengl32.lib
    )

    include_directories(
        ${CMAKE_BINARY_DIR}/glew-1.13.0/include
        ${CMAKE_BINARY_DIR}/glfw-3.1.2.bin.WIN32/include
        ${CMAKE_BINARY_DIR}/glm
    )

    add_custom_command(TARGET opengl-drawing-a-triangle POST_BUILD        # Adds a post-build event to MyTest
        COMMAND ${CMAKE_COMMAND} -E copy_if_different  # which executes "cmake - E copy_if_different..."
            "${CMAKE_BINARY_DIR}/glew-1.13.0/bin/Release/Win32/glew32.dll"      # <--this is in-file
            $<TARGET_FILE_DIR:opengl-drawing-a-triangle>)

endif (MSVC)
# vim: ts=4 sw=4 et