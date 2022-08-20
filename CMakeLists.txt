cmake_minimum_required(VERSION 3.23)
project(SdlTestApp)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED YES)
set(CMAKE_CXX_EXTENSIONS OFF)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -lmingw32")
set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++")

set(INCLUDE_DIR "${CMAKE_SOURCE_DIR}/buildtools")
set(SDL2_DIR "${INCLUDE_DIR}/SDL2/SDL2-2.0.22")

set(SDL2_INCLUDE_DIRS "${SDL2_DIR}/include")
set(CMAKE_BINARY_DIR "${CMAKE_SOURCE_DIR}/deploy")

if (${CMAKE_SIZEOF_VOID_P} MATCHES 8)
    set(SDL2_LIBRARIES "${SDL2_DIR}/lib/x64/SDL2main.lib;${SDL2_DIR}/lib/x64/SDL2.lib")
else ()
    set(SDL2_LIBRARIES "${SDL2_DIR}/lib/x86/SDL2main.lib;${SDL2_DIR}/lib/x86/SDL2.lib")
endif ()

# link dependencies
include_directories(${SDL2_INCLUDE_DIRS})
link_directories(${SDL2_LIBRARIES})

set(SOURCES
        src/main.cpp
        )

add_executable(${PROJECT_NAME} ${SOURCES})
target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES})

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        "${SDL2_DIR}/lib/x64"
        $<TARGET_FILE_DIR:${PROJECT_NAME}>)