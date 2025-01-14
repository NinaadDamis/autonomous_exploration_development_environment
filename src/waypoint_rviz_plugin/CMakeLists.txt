# cmake_minimum_required(VERSION 2.8.3)
# project(waypoint_rviz_plugin)

# find_package(catkin REQUIRED COMPONENTS
#   roscpp
#   rospy
#   rviz
# )

# set(CMAKE_AUTOMOC ON)
# if(rviz_QT_VERSION VERSION_LESS "5")
#  message(STATUS "Using Qt4 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
#  find_package(Qt4 ${rviz_QT_VERSION} EXACT REQUIRED QtCore QtGui)
#  include(${QT_USE_FILE})
# else()
#  message(STATUS "Using Qt5 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
#  find_package(Qt5 ${rviz_QT_VERSION} EXACT REQUIRED Core Widgets)
#  set(QT_LIBRARIES Qt5::Widgets)
# endif()

# add_definitions(-DQT_NO_KEYWORDS)

# catkin_package(
#   LIBRARIES  ${PROJECT_NAME}
#   CATKIN_DEPENDS roscpp rviz
# )

# include_directories(
# include
#   ${QT_INCLUDE_DIRS}
#   ${catkin_INCLUDE_DIRS}
# )

# set(HEADER_FILES
#         include/waypoint_tool.h
# )
# set(SRC_FILES
#     src/waypoint_tool.cpp
# )

# add_library(${PROJECT_NAME}
#             ${SRC_FILES} ${HEADER_FILES}
# )

# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
# target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${QT_LIBRARIES} ${Boost_LIBRARIES} ${OGRE_OV_LIBRARIES_ABS})

# install(TARGETS ${PROJECT_NAME}
#         ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#         LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#         RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

# install(FILES plugin_description.xml
#         DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

cmake_minimum_required(VERSION 3.5)
project(waypoint_rviz_plugin)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# if(rviz_QT_VERSION VERSION_LESS "5")
#  message(STATUS "Using Qt4 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
#  find_package(Qt4 ${rviz_QT_VERSION} EXACT REQUIRED QtCore QtGui)
#  include(${QT_USE_FILE})
# else()        
#  message(STATUS "Using Qt5 based on the rviz_QT_VERSION: ${rviz_QT_VERSION}")
#  find_package(Qt5 ${rviz_QT_VERSION} EXACT REQUIRED Core Widgets)
#  set(QT_LIBRARIES Qt5::Widgets)
# endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(rviz2 REQUIRED)
find_package(rviz_common REQUIRED)
find_package(rviz_default_plugins REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
# find_package(Qt5Core REQUIRED)
# find_package(Qt5Quick REQUIRED)
# find_package(Qt5Widgets REQUIRED)
find_package(Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt5 REQUIRED COMPONENTS Core)
find_package(Qt5 REQUIRED COMPONENTS Quick)
find_package(rviz_ogre_vendor REQUIRED)
find_package(pluginlib REQUIRED)

include_directories(include)
include_directories(${QT_INCLUDE_DIRS})
# add_executable(waypoint_tool src/waypoint_tool.cpp)
# ament_target_dependencies(waypoint_tool rclcpp rviz2 rviz_common rviz_default_plugins std_msgs geometry_msgs sensor_msgs pluginlib)
# target_link_libraries(waypoint_tool Qt5::Widgets Qt5::Core Qt5::Quick rviz_ogre_vendor::OgreMain rviz_ogre_vendor::OgreOverlay)


# target_compile_definitions(rviz_default_plugins PRIVATE "RVIZ_DEFAULT_PLUGINS_BUILDING_LIBRARY")

# # prevent pluginlib from using boost
# target_compile_definitions(rviz_default_plugins PUBLIC "PLUGINLIB__DISABLE_BOOST_FUNCTIONS")

pluginlib_export_plugin_description_file(rviz_common plugin_description.xml)



add_library(waypoint_rviz_plugin src/waypoint_tool.cpp)
ament_target_dependencies(waypoint_rviz_plugin rclcpp rviz2 rviz_common rviz_default_plugins std_msgs geometry_msgs sensor_msgs pluginlib)
target_link_libraries(waypoint_rviz_plugin Qt5::Widgets Qt5::Core Qt5::Quick rviz_ogre_vendor::OgreMain rviz_ogre_vendor::OgreOverlay)

install(TARGETS waypoint_rviz_plugin 
  DESTINATION lib/${PROJECT_NAME})

install(
  DIRECTORY 
  include
  DESTINATION include/${PROJECT_NAME}
)

# install(
#   DIRECTORY
#   launch
#   DESTINATION share/${PROJECT_NAME}
# )

install(
  FILES 
  plugin_description.xml
  DESTINATION share/${PROJECT_NAME}
)

# install(
#   TARGETS rviz_default_plugins
#   EXPORT rviz_default_plugins
#   ARCHIVE DESTINATION lib
#   LIBRARY DESTINATION lib
#   RUNTIME DESTINATION bin
#   INCLUDES DESTINATION include
# )

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()
ament_export_include_directories(include)
ament_export_libraries(${LIBRARIES})
# ament_export_targets(waypoint_rviz_plugin HAS_LIBRARY_TARGET)
ament_package()
