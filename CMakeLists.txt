cmake_minimum_required(VERSION 2.8.3)
project(roscontrol_sot)

find_package(PkgConfig REQUIRED)

set(bullet_FOUND 0)
pkg_check_modules(bullet REQUIRED bullet)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  dynamic_graph_bridge
  control_msgs
  sensor_msgs
  realtime_tools
  controller_interface
)

include_directories(include ${bullet_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

link_directories(${bullet_LIBRARY_DIRS})

catkin_package()

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories( 
  ${catkin_INCLUDE_DIRS}
  )

## Declare a C++ library
add_library(rcsot_controller 
src/roscontrol-sot-controller.cpp
src/log.cpp
)

## Add cmake target dependencies of the executable
## same as for the library above
# add_dependencies(roscontrol_sot_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Specify libraries to link a library or executable target against
target_link_libraries(rcsot_controller
  ${catkin_LIBRARIES}
  ${bullet_LIBRARIES}
  )


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

## Mark executables and/or libraries for installation
install(TARGETS rcsot_controller
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)


foreach(dir config launch)
  install(DIRECTORY ${dir}
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  )
endforeach()


#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_roscontrol_sot.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)