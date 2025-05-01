cmake_minimum_required(VERSION 3.22)

project(AudioPluginTemplate)

# Currently, JUCE supports only C++ 20 cross-platform.
set(CMAKE_CXX_STANDARD 20)

set(LIB_DIR ${CMAKE_CURRENT_SOURCE_DIR}/libs)

# Downloads CPM if not already downloaded. CPM is an easy-to-use package manager nicely integrated with CMake.
include(cmake/cpm.cmake)

# Downloads and configures JUCE. It sets up some variables, like JUCE_SOURCE_DIR.
cpmaddpackage(
  NAME
  JUCE
  GIT_TAG
  8.0.6
  VERSION
  8.0.6
  GITHUB_REPOSITORY
  juce-framework/JUCE
  SOURCE_DIR
  ${LIB_DIR}/juce
)

# Add googletest.
cpmaddpackage(
  NAME
  GOOGLETEST
  GITHUB_REPOSITORY
  google/googletest
  VERSION
  1.16.0
  SOURCE_DIR
  ${LIB_DIR}/googletest
  OPTIONS
  "INSTALL_GTEST OFF"
  "gtest_force_shared_crt ON"
)

# Add compiler warning utilities
include(cmake/CompilerWarnings.cmake)
include(cmake/Util.cmake)

# Adds all the targets configured in the "plugin" folder.
add_subdirectory(plugin)

# This command allows running tests from the "build" folder (the one where CMake generates the project to).
enable_testing()

# Adds all the targets configured in the "test" folder.
add_subdirectory(test)
