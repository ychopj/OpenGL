cmake_minimum_required (VERSION 3.27)
project (one)

set (OPENGL_LIBRARIES ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})

file (GLOB_RECURSE ProjectSources *.c*)
file (GLOB_RECURSE ProjectHeaders *.h*)
add_definitions(-DSHADER_DIR="${CAD_ROOT_DIR}/resource/shader/")
add_definitions(-DTEXTURE_DIR="${CAD_ROOT_DIR}/resource/texture/")

foreach(source_file ${ProjectSources})
    # 获取源文件的文件名（不包含路径和扩展名）
    get_filename_component(file_name ${source_file} NAME)

    # 如果文件名以lesson_开头，则将其后的部分作为可执行文件名
    string(REGEX MATCH "lesson_([^.]*)" match ${file_name})
    if(match)
        set(executable_name ${CMAKE_MATCH_1})
        add_executable(${executable_name} ${source_file}
                ${CAD_ROOT_DIR}/External/glad/src/glad.c)
        target_link_libraries (${executable_name} PRIVATE  glfw3 OpenGL32 imgui  stb_image Rendering)
    endif()
endforeach()

