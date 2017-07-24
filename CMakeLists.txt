cmake_minimum_required(VERSION 2.8.3)
project(rosbag_logger_test)

include(GNUInstallDirs)

set(Boost_INCLUDE_DIRS "/opt/crystax-ndk-10.3.2/sources/boost/1.59.0/include/")
set(Boost_LIBRARIES "-L/opt/crystax-ndk-10.3.2/sources/boost/1.59.0/libs/armeabi-v7a/gnu-4.9/ -lboost_date_time -lboost_filesystem -lboost_program_options -lboost_regex -lboost_system -lboost_thread")

find_package(console_bridge REQUIRED)

find_library(crystax_LIBRARIES NAMES crystax)
if (NOT crystax_LIBRARIES)
  message(FATAL_ERROR "crystax library not found")
endif()

find_library(roscpp_serialization_LIBRARIES NAMES roscpp_serialization)
if (NOT roscpp_serialization_LIBRARIES)
  message(FATAL_ERROR "roscpp_serialization library not found")
endif()

find_library(cpp_common_LIBRARIES NAMES cpp_common)
if (NOT cpp_common_LIBRARIES)
  message(FATAL_ERROR "cpp_common library not found")
endif()

find_library(rostime_LIBRARIES NAMES rostime)
if (NOT rostime_LIBRARIES)
  message(FATAL_ERROR "rostime library not found")
endif()

find_library(rosbag_storage_LIBRARIES NAMES rosbag_storage)
if (NOT rosbag_storage_LIBRARIES)
  message(FATAL_ERROR "rosbag_storage library not found")
endif()

find_library(roslz4_LIBRARIES NAMES roslz4)
if (NOT roslz4_LIBRARIES)
  message(FATAL_ERROR "roslz4 library not found")
endif()

find_library(lz4_LIBRARIES NAMES lz4)
if (NOT lz4_LIBRARIES)
  message(FATAL_ERROR "lz4 library not found")
endif()

find_package(BZip2 REQUIRED)
IF (BZIP2_FOUND)
   INCLUDE(CheckLibraryExists)
   CHECK_LIBRARY_EXISTS(${BZIP2_LIBRARIES} BZ2_bzCompressInit "" BZIP2_NEED_PREFIX)
ENDIF (BZIP2_FOUND)

message(STATUS ${crystax_LIBRARIES})

include_directories(
  include
  ${Boost_INCLUDE_DIRS}
)

add_executable(${PROJECT_NAME}_node src/main.cpp)

target_link_libraries(${PROJECT_NAME}_node
    "/opt/crystax-standalone-toolchain/arm-linux-androideabi/lib/libcrystax.a"
    ${Boost_LIBRARIES}
    ${cpp_common_LIBRARIES}
    ${roscpp_serialization_LIBRARIES}
    ${console_bridge_LIBRARIES}
    ${rostime_LIBRARIES}
    ${rosbag_storage_LIBRARIES}
    ${roslz4_LIBRARIES}
    ${lz4_LIBRARIES}
)
