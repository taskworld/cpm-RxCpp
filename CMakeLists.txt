cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

#-----------------------------------------------------------------------
# CPM configuration
#-----------------------------------------------------------------------
set(CPM_MODULE_NAME RxCpp)
set(CPM_LIB_TARGET_NAME ${CPM_MODULE_NAME})

if ((DEFINED CPM_DIR) AND (DEFINED CPM_UNIQUE_ID) AND (DEFINED CPM_TARGET_NAME))
  set(CPM_LIB_TARGET_NAME ${CPM_TARGET_NAME})
  set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CPM_DIR})
  include(CPM)
else()
  set(CPM_DIR "${CMAKE_CURRENT_BINARY_DIR}/cpm-packages" CACHE TYPE STRING)
  find_package(Git)
  if(NOT GIT_FOUND)
    message(FATAL_ERROR "CPM requires Git.")
  endif()
  if (NOT EXISTS ${CPM_DIR}/CPM.cmake)
    message(STATUS "Cloning repo (https://github.com/iauns/cpm)")
    execute_process(
      COMMAND "${GIT_EXECUTABLE}" clone https://github.com/iauns/cpm ${CPM_DIR}
      RESULT_VARIABLE error_code
      OUTPUT_QUIET ERROR_QUIET)
    if(error_code)
      message(FATAL_ERROR "CPM failed to get the hash for HEAD")
    endif()
  endif()
  include(${CPM_DIR}/CPM.cmake)
endif()

# Include CPM modules or externals here (with CPM_AddModule).

CPM_InitModule(${CPM_MODULE_NAME})

#------------------------------------------------------------------------------
# RxCpp
#------------------------------------------------------------------------------
# RxCpp as a subdirectory so that it can capture your compiler flags
set(PROJ_DIR ${CMAKE_CURRENT_SOURCE_DIR}/3rdParty/RxCpp)
CPM_EnsureRepoIsCurrent(
  TARGET_DIR ${PROJ_DIR}
  GIT_REPOSITORY "https://github.com/Reactive-Extensions/RxCpp.git"
  GIT_TAG v2.2.0
  USE_CACHING TRUE
  )

#-----------------------------------------------------------------------
# Source
#-----------------------------------------------------------------------

# Globbing has some downsides, but the advantages outweigh the
# disadvantages.
file (GLOB Sources
  ${PROJ_DIR}/Rx/v2/src/rxcpp/*.hpp
  )

#-----------------------------------------------------------------------
# Library setup
#-----------------------------------------------------------------------

# Build the library.
add_library(${CPM_LIB_TARGET_NAME} SHARED ${Sources})
SET_TARGET_PROPERTIES(${CPM_LIB_TARGET_NAME} PROPERTIES LINKER_LANGUAGE CXX)
if (NOT EMSCRIPTEN AND CPM_LIBRARIES)
  target_link_libraries(${CPM_LIB_TARGET_NAME} ${CPM_LIBRARIES})
endif()
