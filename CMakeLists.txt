cmake_minimum_required(VERSION 3.10)
set(CMAKE_CXX_STANDARD 17)


project(APKG_util)


file(GLOB_RECURSE SOURCES ${PROJECT_SOURCE_DIR}/src/*.cpp )

set(COMMON_INCLUDES
${PROJECT_SOURCE_DIR}/vendor/
${PROJECT_SOURCE_DIR}/vendor/zlib/
${PROJECT_SOURCE_DIR}/vendor/SDL/include/
)

add_executable(APKG_util ${SOURCES})

if(CMAKE_BUILD_TYPE STREQUAL "Release")
    if (WIN32)
        if (MSVC)
            target_link_options(APKG_util PRIVATE /SUBSYSTEM:WINDOWS /ENTRY:mainCRTStartup)
        else()
            target_link_options(APKG_util PRIVATE -mwindows)
        endif()
    endif()
endif()

add_subdirectory(vendor/libsodium-cmake)
add_subdirectory(vendor/zlib)
add_subdirectory(vendor/SDL)

target_include_directories(APKG_util PRIVATE ${COMMON_INCLUDES})

target_link_libraries(APKG_util PRIVATE SDL3-static sodium zlibstatic)
add_compile_options(-Wall -Wextra -Werror)