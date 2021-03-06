cmake_minimum_required(VERSION 2.8)
project(image_transport_tutorial)

find_package(catkin REQUIRED cv_bridge genmsg image_transport sensor_msgs)

# add the resized image message
add_message_files(DIRECTORY msg
   FILES ResizedImage.msg
)
generate_messages(DEPENDENCIES sensor_msgs)

catkin_package()

find_package(OpenCV REQUIRED)
find_package(OpenCV REQUIRED)
find_library(NONFREE opencv_nonfree2_4
						 PATHS /usr/lib
									 /usr/local/lib)

MESSAGE( STATUS "OPENCV_NONFREE_PATH: " ${NONFREE} )

include_directories(include ${OpenCV_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

# add the publisher example
#add_executable(my_publisher src/my_publisher_picture.cpp)
add_executable(my_publisher src/my_publisher_video.cpp)
target_link_libraries(my_publisher ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})

# add the subscriber example
#add_executable(my_subscriber src/my_subscriber.cpp)
add_executable(my_subscriber src/feature_finder.cpp)
target_link_libraries(my_subscriber ${catkin_LIBRARIES} ${OpenCV_LIBRARIES}  ${NONFREE})

# add the plugin examples
add_library(resized_publisher src/manifest.cpp src/resized_publisher.cpp src/resized_subscriber.cpp)
target_link_libraries(resized_publisher ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})
