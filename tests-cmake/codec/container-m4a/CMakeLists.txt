cmake_minimum_required(VERSION 3.20)

# set the project name
project(container-m4a)
set (CMAKE_CXX_STANDARD 11)
set (DCMAKE_CXX_FLAGS "-Werror")
# add_compile_options(-Wstack-usage=1024)
include(FetchContent)

# Build with arduino-audio-tools
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../../.. ${CMAKE_CURRENT_BINARY_DIR}/arduino-audio-tools )
endif()

# Build with libhelix
FetchContent_Declare(ma GIT_REPOSITORY "https://github.com/pschatzmann/arduino-libhelix.git" GIT_TAG main )
FetchContent_GetProperties(helix)
if(NOT helix_POPULATED)
    FetchContent_Populate(helix)
    add_subdirectory(${helix_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/helix)
endif()


# build sketch as executable
add_executable (container-m4a container-m4a.cpp)

# set preprocessor defines
target_compile_definitions(container-m4a PUBLIC -DIS_MIN_DESKTOP -DHELIX_PRINT)

# OS/X might need this setting for core audio
#target_compile_definitions(portaudio PUBLIC -DPA_USE_COREAUDIO=1)

# specify libraries
target_link_libraries(container-m4a arduino_helix arduino-audio-tools)

