cmake_minimum_required(VERSION 3.22)
project(NESCRUN VERSION 1.0.0 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# JUCE: usa una copia local si existe (JUCE_LOCAL), si no la descarga.
if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/JUCE_LOCAL/CMakeLists.txt")
    add_subdirectory(JUCE_LOCAL JUCE_build)
else()
    include(FetchContent)
    FetchContent_Declare(JUCE
        GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
        GIT_TAG 8.0.4
        GIT_SHALLOW ON)
    FetchContent_MakeAvailable(JUCE)
endif()

juce_add_plugin(NESCRUN
    COMPANY_NAME "Nescrun Audio"
    COMPANY_WEBSITE "https://github.com/nescroon-sys/nescrun"
    BUNDLE_ID com.nescrun.vocalsuite
    PLUGIN_MANUFACTURER_CODE Nscr
    PLUGIN_CODE Nsc1
    FORMATS VST3 Standalone
    PRODUCT_NAME "NESCRUN"
    IS_SYNTH FALSE
    NEEDS_MIDI_INPUT FALSE
    NEEDS_MIDI_OUTPUT FALSE
    IS_MIDI_EFFECT FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE
    COPY_PLUGIN_AFTER_BUILD FALSE
    VST3_CATEGORIES "Fx Dynamics EQ Pitch Shift Reverb"
    DESCRIPTION "All-in-one vocal chain: tuning, cleanup, dynamics, tone, FX, loudness")

target_sources(NESCRUN PRIVATE
    Source/PluginProcessor.cpp
    Source/PluginEditor.cpp)

target_compile_definitions(NESCRUN PUBLIC
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
    JUCE_DISPLAY_SPLASH_SCREEN=0)

target_link_libraries(NESCRUN
    PRIVATE
        juce::juce_audio_utils
        juce::juce_dsp
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags)

# Herramienta de prueba offline (no forma parte del plugin)
option(NESCRUN_BUILD_TESTS "Build offline DSP test" OFF)
if(NESCRUN_BUILD_TESTS)
    juce_add_console_app(NescrunTest PRODUCT_NAME "NescrunTest")
    target_sources(NescrunTest PRIVATE Tests/OfflineTest.cpp Source/PluginProcessor.cpp Source/PluginEditor.cpp)
    target_compile_definitions(NescrunTest PRIVATE JUCE_WEB_BROWSER=0 JUCE_USE_CURL=0 NESCRUN_TEST=1)
    target_link_libraries(NescrunTest PRIVATE juce::juce_audio_utils juce::juce_dsp juce::juce_audio_processors)
endif()
