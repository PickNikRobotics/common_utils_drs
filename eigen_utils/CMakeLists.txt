cmake_minimum_required(VERSION 3.5)
project(eigen_utils)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra)
endif()


# this is just to make the "catkin_package" macro available
find_package(ament_cmake REQUIRED)
find_package(Eigen3 REQUIRED)

set(THIS_PACKAGE_INCLUDE_DEPENDS
  Eigen3
)

include_directories(
  include
  ${EIGEN3_INCLUDE_DIR}
)

add_library(${PROJECT_NAME} SHARED src/eigen_numerical.cpp
                                   src/eigen_rand.cpp
                                   src/eigen_rigidbody.cpp
                                   src/eigen_utils_common.cpp
                                   src/eigen_rotations.cpp)

ament_target_dependencies(${PROJECT_NAME} ${THIS_PACKAGE_INCLUDE_DEPENDS})
target_include_directories(${PROJECT_NAME} PUBLIC include)

# TODO: Enable tests
# if(CATKIN_ENABLE_TESTING)
#   catkin_add_gtest(eigen_utils_tests test/test_transforms.cpp
#                                      test/test_unwrap.cpp)
#   target_link_libraries(eigen_utils_tests ${PROJECT_NAME})
# endif()

install(
  TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY include/
  DESTINATION include)

ament_export_dependencies(
  ${THIS_PACKAGE_INCLUDE_DEPENDS}
)
ament_export_include_directories(include)
ament_export_libraries(
  ${PROJECT_NAME}
)

ament_package()

