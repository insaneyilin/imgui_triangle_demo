project( imgui_triangle_demo )
cmake_minimum_required( VERSION 3.0 )

set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11" )

find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS})

find_package(glfw3 REQUIRED)
include_directories(${GLFW_INCLUDE_DIRS})
link_libraries(${GLFW_LIBRARY_DIRS})

find_package(GLEW REQUIRED STATIC)
include_directories(${GLEW_INCLUDE_DIRS})

# imgui include dir.
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/third_party/imgui)

if (APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(OpenGL_LIBRARY OpenGL)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(COREVIDEO_LIBRARY CoreVideo)
    set(EXTRA_LIBS ${COCOA_LIBRARY} ${OpenGL_LIBRARY} ${IOKIT_LIBRARY} ${COREVIDEO_LIBRARY})
endif (APPLE)

add_executable( ${PROJECT_NAME}
    main.cc
    file_manager.cc
    file_manager.h
    opengl_shader.cc
    opengl_shader.h
    third_party/imgui/imgui.cpp 
    third_party/imgui/imgui_demo.cpp 
    third_party/imgui/imgui_draw.cpp 
    third_party/imgui/imgui_tables.cpp 
    third_party/imgui/imgui_widgets.cpp
    third_party/imgui/backends/imgui_impl_opengl3.cpp
    third_party/imgui/backends/imgui_impl_opengl3.h
    third_party/imgui/backends/imgui_impl_glfw.cpp
    third_party/imgui/backends/imgui_impl_glfw.h
    assets/simple-shader.fs
    assets/simple-shader.vs )

target_link_libraries( ${PROJECT_NAME}
    ${OPENGL_LIBRARIES}
    glfw
    GLEW::glew  # OS X: check `/usr/local/lib/cmake/glew/glew-targets.cmake` to find the glew target name
    ${EXTRA_LIBS} )

add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory              
        ${CMAKE_CURRENT_SOURCE_DIR}/assets $<TARGET_FILE_DIR:${PROJECT_NAME}>/shaders
    COMMENT "Copying shaders" VERBATIM )
