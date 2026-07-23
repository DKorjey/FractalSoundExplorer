cmake_minimum_required(VERSION 3.20)

project(MandelbrotMusic)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(SFML_STATIC_LIBRARIES TRUE)

add_library(OpenAL UNKNOWN IMPORTED GLOBAL)
add_library(OpenAL::OpenAL ALIAS OpenAL)

set_target_properties(OpenAL PROPERTIES
    IMPORTED_CONFIGURATIONS "Release;Debug"
    IMPORTED_LOCATION_RELEASE "C:/Program Files/SFML/lib/openal32.lib"
    IMPORTED_LOCATION_DEBUG   "C:/Program Files/SFML/lib/openal32.lib"
)

find_package(SFML 2.6 COMPONENTS graphics window system audio REQUIRED)

add_executable(MandelbrotMusic
    src/main.cpp
)

if(WIN32)
    set_target_properties(MandelbrotMusic PROPERTIES
        WIN32_EXECUTABLE TRUE
    )
endif()

target_link_libraries(MandelbrotMusic PRIVATE
    sfml-graphics
    sfml-window
    sfml-system
    sfml-audio
)

add_custom_command(TARGET MandelbrotMusic POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
    ${CMAKE_SOURCE_DIR}/src/vert.glsl
    $<TARGET_FILE_DIR:MandelbrotMusic>/vert.glsl
)

add_custom_command(TARGET MandelbrotMusic POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
    ${CMAKE_SOURCE_DIR}/src/frag.glsl
    $<TARGET_FILE_DIR:MandelbrotMusic>/frag.glsl
)

add_custom_command(TARGET MandelbrotMusic POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
    ${CMAKE_SOURCE_DIR}/src/RobotoMono-Medium.ttf
    $<TARGET_FILE_DIR:MandelbrotMusic>/RobotoMono-Medium.ttf
)
