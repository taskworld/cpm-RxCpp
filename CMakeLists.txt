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

set(PROJ_SOURCES
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-amb.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-buffer_count.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-buffer_time.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-buffer_time_count.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-combine_latest.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-concat.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-concat_map.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-connect_forever.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-distinct_until_changed.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-filter.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-finally.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-flat_map.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-group_by.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-lift.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-map.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-merge.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-multicast.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-observe_on.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-pairwise.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-publish.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-reduce.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-ref_count.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-repeat.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-replay.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-retry.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-scan.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-skip.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-skip_until.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-start_with.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-subscribe.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-subscribe_on.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-switch_on_next.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-take.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-take_until.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-window.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-window_time.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-window_time_count.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/operators/rx-zip.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-connectable_observable.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-coordination.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-grouped_observable.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-includes.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-notification.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-observable.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-observer.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-operators.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-predef.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-scheduler.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-sources.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-subjects.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-subscriber.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-subscription.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-test.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-trace.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx-util.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/rx.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-currentthread.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-eventloop.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-immediate.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-newthread.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-runloop.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-sameworker.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-test.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/schedulers/rx-virtualtime.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-create.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-defer.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-error.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-interval.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-iterate.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-never.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-range.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-scope.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/sources/rx-timer.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/subjects/rx-behavior.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/subjects/rx-replaysubject.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/subjects/rx-subject.hpp
   ${PROJ_DIR}/Rx/v2/src/rxcpp/subjects/rx-synchronize.hpp
)

target_link_libraries(${CPM_LIB_TARGET_NAME} ${PROJ_SOURCES})

CPM_ExportAdditionalIncludeDir("${PROJ_DIR}")
