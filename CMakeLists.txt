cmake_minimum_required(VERSION 3.27)
project(OpenAl-1-24-3)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
include("C://codeLib//cmake//BasicFunc.cmake")
add_compile_options(-finput-charset=UTF-8 -fexec-charset=UTF-8)

include_directories(${FFmpegInclude} ${OpenAL_24_3_Include} ${SDL3Include} ${sndfileInclude} C:/codeLib/fmt/include)
link_directories(${FFmpeg} ${OpenAL_24_3} ${SDL3} ${sndfile} C:/codeLib/fmt/lib C:/codeLib/OpenAl-1-24-3/lib)

set(common
    examples/common/alhelpers.c
    examples/common/alhelpers.h
    examples/common/almalloc.h
    examples/common/alnumbers.h
    examples/common/alnumeric.h
)
set(dll SDL3 sndfile avcodec avformat swscale avdevice avutil swresample avfilter OpenAL32)

add_executable(alffplay examples/alffplay.cpp ${common})

target_link_libraries(alffplay PRIVATE ${dll} libfmt.a)

add_custom_command(TARGET alffplay POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
    ${OpenAL_24_3}/OpenAL32.dll
    $<TARGET_FILE_DIR:alffplay>
)