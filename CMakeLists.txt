cmake_minimum_required(VERSION 3.24)

if (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
elseif("${METAMODULE_SDK_DIR}" STREQUAL "")
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
endif()
include(${METAMODULE_SDK_DIR}/plugin.cmake)

project(SonusModular VERSION 1.0.0 DESCRIPTION "Sonus Modular Pack" LANGUAGES C CXX)

# Set C++17 standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Define source directories
set(SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/SonusModular)
set(DSP_DIR ${SOURCE_DIR}/dsp)

add_library(SonusModular STATIC)

target_compile_definitions(SonusModular PRIVATE 
    __V1x 
    __PLUGIN
)

# Add compile options for better control over symbols
target_compile_options(SonusModular PRIVATE
    -fdata-sections
    -ffunction-sections
    -fno-exceptions
    -fno-threadsafe-statics
    -fno-use-cxa-atexit
    $<$<CXX_COMPILER_ID:GNU>:-fvisibility=hidden>
    $<$<CXX_COMPILER_ID:GNU>:-fvisibility-inlines-hidden>
)

# Add linker flags for static linking
target_link_options(SonusModular PRIVATE
    -Wl,--gc-sections
    -static-libgcc
    -static-libstdc++
)

# Add source files
target_sources(SonusModular PRIVATE
    ${SOURCE_DIR}/src/sonusmodular.cpp
    ${SOURCE_DIR}/src/addiction.cpp
    ${SOURCE_DIR}/src/bitter.cpp
    ${SOURCE_DIR}/src/bymidside.cpp
    ${SOURCE_DIR}/src/campione.cpp
    ${SOURCE_DIR}/src/chainsaw.cpp
    ${SOURCE_DIR}/src/ctrl.cpp
    ${SOURCE_DIR}/src/deathcrush.cpp
    ${SOURCE_DIR}/src/fluidmix.cpp
    ${SOURCE_DIR}/src/fraction.cpp
    ${SOURCE_DIR}/src/harmony.cpp
    ${SOURCE_DIR}/src/ladrone.cpp
    ${SOURCE_DIR}/src/luppolo.cpp
    ${SOURCE_DIR}/src/luppolo3.cpp
    ${SOURCE_DIR}/src/micromacro.cpp
    ${SOURCE_DIR}/src/mrcheb.cpp
    ${SOURCE_DIR}/src/multimulti.cpp
    ${SOURCE_DIR}/src/neurosc.cpp
    ${SOURCE_DIR}/src/oktagon.cpp
    ${SOURCE_DIR}/src/osculum.cpp
    ${SOURCE_DIR}/src/paramath.cpp
    ${SOURCE_DIR}/src/piconoise.cpp
    ${SOURCE_DIR}/src/pith.cpp
    ${SOURCE_DIR}/src/pusher.cpp
    ${SOURCE_DIR}/src/ringo.cpp 
    ${SOURCE_DIR}/src/scramblase.cpp
    ${SOURCE_DIR}/src/tropicana.cpp
    ${SOURCE_DIR}/src/twoff.cpp
    ${SOURCE_DIR}/src/yabp.cpp
)

# Add include directories
target_include_directories(SonusModular PRIVATE 
    ${SOURCE_DIR}
    ${SOURCE_DIR}/src
    ${DSP_DIR}
)

# Create the plugin file
create_plugin(
    SOURCE_LIB      SonusModular
    PLUGIN_NAME     SonusModular
    PLUGIN_JSON     ${SOURCE_DIR}/plugin.json
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets
    DESTINATION     ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins
) 