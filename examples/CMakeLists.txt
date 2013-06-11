
add_executable(examples/periodic_snapshotter periodic_snapshotter.cpp)
target_link_libraries(examples/periodic_snapshotter
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES+})
