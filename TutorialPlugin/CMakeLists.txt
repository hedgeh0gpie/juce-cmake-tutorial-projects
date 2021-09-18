cmake_minimum_required (VERSION 3.15)

project (TutorialPlugin VERSION 0.0.1)

# Adding JUCE
add_subdirectory (Ext/JUCE)

set (FormatsToBuild AU VST3)

# If a path to the AAX SDK is passed to CMake, an AAX version will be built too
if (AAX_SDK_PATH)
    juce_set_aax_sdk_path (${AAX_SDK_PATH})
    list (APPEND FormatsToBuild AAX)
endif()

juce_add_plugin (TutorialPlugin
        COMPANY_NAME JSM                            # Specify the name of the plugin's author
        IS_SYNTH FALSE                              # Is this a synth or an effect?
        NEEDS_MIDI_INPUT TRUE                       # Does the plugin need midi input?
        NEEDS_MIDI_OUTPUT TRUE                      # Does the plugin need midi output?
        IS_MIDI_EFFECT TRUE                         # Is this plugin a MIDI effect?
        COPY_PLUGIN_AFTER_BUILD TRUE                # Should the plugin be installed to a default location after building?
        PLUGIN_MANUFACTURER_CODE JSM0           # A four-character manufacturer id with at least one upper-case character
        PLUGIN_CODE JSM0                            # A unique four-character plugin id with at least one upper-case character
        FORMATS ${FormatsToBuild}                   # The formats to build. Other valid formats are: AAX Unity VST AU AUv3
        PRODUCT_NAME "TutorialPlugin")              # The name of the final executable, which can differ from the target name

target_compile_features (TutorialPlugin PRIVATE cxx_std_14)

target_sources (TutorialPlugin PRIVATE
        Source/PluginEditor.cpp
        Source/PluginProcessor.cpp)

target_compile_definitions (TutorialPlugin
        PUBLIC
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_STRICT_REFCOUNTEDPTR=1
        JUCE_VST3_CAN_REPLACE_VST2=0)

target_link_libraries (TutorialPlugin PRIVATE
        # JUCE Modules
        juce::juce_audio_utils
        juce::juce_dsp
        juce::juce_gui_basics
        juce::juce_core
        juce::juce_audio_plugin_client
        juce::juce_audio_basics
        juce::juce_graphics

        # Custom modules

        # Recommended flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
        juce::juce_recommended_config_flags)

#############################
