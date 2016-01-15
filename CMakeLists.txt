cmake_minimum_required(VERSION 3.3)
project(port)


set(SOURCE_FILES source/main.c)

include_directories(${DEVKITPRO}/libctru/include)
link_directories(${DEVKITPRO}/libctru/lib)

set(COMMON_FLAGS "-march=armv6k -mtune=mpcore -mtp=soft -mfloat-abi=hard")

set(CMAKE_C_FLAGS ${COMMON_FLAGS})

add_executable(port ${SOURCE_FILES})
target_link_libraries(port ctru m)
set_target_properties(port
        PROPERTIES
        LINK_FLAGS "-specs=3dsx.specs -g ${COMMON_FLAGS}"
        COMPILER_FLAGS "-g -Wall -O2 -mword-relocations -fomit-frame-pointer -ffunction-sections ${COMMON_FLAGS}")