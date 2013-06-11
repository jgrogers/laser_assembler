cmake_minimum_required(VERSION 2.8.3)
project(laser_assembler)

set (PKG_DEPENDS
  roscpp
  std_msgs
  geometry_msgs
  laser_geometry
  laser_filters
  sensor_msgs
  tf
)
set (BUILD_DEPENDS
  message_generation
  ${PKG_DEPENDS})
set (RUN_DEPENDS
  message_runtime
  ${PKG_DEPENDS})

find_package(catkin REQUIRED COMPONENTS ${BUILD_DEPENDS})

find_package(Eigen REQUIRED)
find_package(Boost REQUIRED COMPONENTS system signals)
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)
add_service_files(
  DIRECTORY srv
  FILES
  AssembleScans.srv
  AssembleScans2.srv
)
generate_messages(
  DEPENDENCIES
  std_msgs
  sensor_msgs
)
add_executable(laser_scan_assembler_srv src/laser_scan_assembler_srv.cpp)
target_link_libraries(laser_scan_assembler_srv 
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES})

add_executable(point_cloud_assembler_srv src/point_cloud_assembler_srv.cpp)
target_link_libraries(point_cloud_assembler_srv
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES})

add_executable(laser_scan_assembler src/laser_scan_assembler.cpp)
target_link_libraries(laser_scan_assembler 
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES})

add_executable(point_cloud_assembler src/point_cloud_assembler.cpp)
target_link_libraries(point_cloud_assembler
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES})

#add_executable(../test/dummy_scan_producer test/dummy_scan_producer.cpp)
#target_link_libraries(../test/dummy_scan_producer 
#  ${catkin_LIBRARIES}
#  ${Boost_LIBRARIES})

add_executable(merge_clouds src/merge_clouds.cpp)
target_link_libraries(merge_clouds 
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES})

#add_executable(../test/test_assembler test/test_assembler.cpp)
#(../test/test_assembler)

#rosbuild_add_rostest(${CMAKE_CURRENT_SOURCE_DIR}/test/test_laser_assembler.launch)

add_executable(periodic_snapshotter examples/periodic_snapshotter.cpp)
target_link_libraries(periodic_snapshotter
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES})

set(EXECUTABLES
  merge_clouds
  laser_scan_assembler
  point_cloud_assembler
  point_cloud_assembler_srv
  laser_scan_assembler_srv
  periodic_snapshotter
  )
set(${PROJECT_NAME}_EXPORTED_TARGETS ${PROJECT_NAME}_gencpp)

install(TARGETS ${EXECUTABLES} 
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp"
)

