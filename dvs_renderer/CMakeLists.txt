cmake_minimum_required(VERSION 2.8.3)
project(dvs_renderer)

## C++11
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslib
  rospy
  std_msgs
  tf
  cv_bridge
  message_generation
  prophesee_event_msgs
  image_transport
)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")

find_package(OpenCV REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS message_runtime std_msgs 
)

include_directories(
	include
	${catkin_INCLUDE_DIRS}
	${OpenCV_INCLUDE_DIRS}
)

link_directories(
	include
  ${catkin_INCLUDE_DIRS}
	${OpenCV_LIBRARY_DIRS}
)
# make the executable
add_executable(dvs_renderer
  src/image_tracking.cpp
  src/renderer.cpp
  src/renderer_node.cpp
)

add_dependencies(dvs_renderer ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

# make the nodelet into a library
#add_library(dvs_renderer_nodelet
#  src/image_tracking.cpp
#  src/renderer_nodelet.cpp
#  src/renderer.cpp
#)

# link the executable to the necesarry libs
target_link_libraries(dvs_renderer
   ${catkin_LIBRARIES}
   ${OpenCV_LIBRARIES}
)

#target_link_libraries(dvs_renderer_nodelet
#   ${catkin_LIBRARIES}
#   ${OpenCV_LIBRARIES}
#)

#cs_install()

# Install other support files for installation
#install(FILES dvs_renderer_nodelet.xml nodelet_stereo.launch nodelet_mono.launch
#  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)
