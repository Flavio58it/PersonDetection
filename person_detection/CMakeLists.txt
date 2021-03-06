cmake_minimum_required(VERSION 2.8.3)
project(person_detection)

# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
# project(ground_based_rgbd_people_detector)

# set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-fpermissive -Ofast -march=corei7")
# cmake -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ .
# set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-fpermissive")
# set(CMAKE_C_FLAGS ${CMAKE_C_FLAGS} "-std=c99")
# SET(CMAKE_CXX_FLAGS "-ggdb -O3 -fPIC -std=c++0x -march=native -mno-avx")
# SET(CMAKE_CXX_FLAGS "-ggdb -O3 -fPIC -std=c++0x -march=native -mno-avx")

# compiling the package requires the OS and processor architecture to be set
# for openni stuff.
# This worked with rosbuild, but on some systems catkin seems to "forget"
# the required settings. On Ubuntu 12.04/14.04, we are safe to set
# linux=1 and __x86_64__=1, which are the values that XnPlatform.h tests for.
# Note that just SET(linux 1) in CMakeLists.txt will not work.
# 

# check whether running on 32 or 64 bit system
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(ARCH "64bit")
  SET(CMAKE_CXX_FLAGS "-ggdb -O3 -fPIC -std=c++0x -Dlinux=1 -D__x86_64__=1 -march=native -mno-avx")
else(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(ARCH "32bit")
  SET(CMAKE_CXX_FLAGS "-ggdb -fPIC -std=c++0x -Dlinux=1 -Di386=1 -march=corei7 -mno-avx")
endif(CMAKE_SIZEOF_VOID_P EQUAL 8)




## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
	roscpp
	tf
	std_msgs
	sensor_msgs
	geometry_msgs
	visualization_msgs
	pcl_conversions
	person_detection_msgs

        cv_bridge
        face_detection_msgs
        image_transport
)


## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)
find_package(PCL 1.7 REQUIRED )
find_package( OpenCV REQUIRED )

catkin_package(
  # INCLUDE_DIRS include
  # LIBRARIES april_tags_lib
  CATKIN_DEPENDS roscpp tf std_msgs sensor_msgs geometry_msgs visualization_msgs pcl_conversions person_detection_msgs
  DEPENDS person_detection_msgs PCL OpenCV
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
#include_directories(
#  include/
#  include/${PROJECT_NAME}
# ${catkin_INCLUDE_DIRS}
# /usr/include/eigen3
#)


#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
# set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)


include_directories(
${PCL_INCLUDE_DIRS} 
${catkin_INCLUDE_DIRS}
${OpenCV_INCLUDE_DIRS}
)
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})


add_executable (ground_based_rgbd_people_detector src/main_ground_based_people_detection.cpp)
add_dependencies(ground_based_rgbd_people_detector person_detection_msgs_generate_messages_cpp )
target_link_libraries(ground_based_rgbd_people_detector  ${PCL_LIBRARIES} ${catkin_LIBRARIES} ${OpenCV_LIBRARIES} )
# target_link_libraries (ground_based_rgbd_people_detector ${PCL_LIBRARIES} roscpp rostime rosconsole roscpp_serialization)


#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

# Mark executables and/or libraries for installation
install(TARGETS ground_based_rgbd_people_detector
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

#install(DIRECTORY include/${PROJECT_NAME}/
#  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#  FILES_MATCHING PATTERN "*.h"
#  PATTERN ".svn" EXCLUDE
#)
## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )
