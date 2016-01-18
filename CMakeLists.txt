cmake_minimum_required(VERSION 3.3)
project(port)


set(SOURCE_FILES source/main.c)

include_directories(${DEVKITPRO}/libctru/include)
link_directories(${DEVKITPRO}/libctru/lib)

set(COMMON_FLAGS "-march=armv6k -mtune=mpcore -mtp=soft -mfloat-abi=hard")

set(CMAKE_C_FLAGS ${COMMON_FLAGS})

add_executable(port.elf ${SOURCE_FILES})
target_link_libraries(port.elf ctru m)
set_target_properties(port.elf
        PROPERTIES
        LINK_FLAGS "-specs=3dsx.specs -g ${COMMON_FLAGS}"
        COMPILER_FLAGS "-g -Wall -O2 -mword-relocations -fomit-frame-pointer -ffunction-sections ${COMMON_FLAGS}")

add_custom_command(
        OUTPUT "port.3dsx"
        COMMAND "$ENV{DEVKITARM}/bin/3dsxtool"
        ARGS port.elf port.3dsx
        DEPENDS port.elf)
add_custom_target(port ALL
        DEPENDS port.3dsx)