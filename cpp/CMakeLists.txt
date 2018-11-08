SET(execName head_pose_estimation)
CMAKE_MINIMUM_REQUIRED(VERSION 2.4)
PROJECT(${execName})

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 OLD)
	cmake_policy(SET CMP0015 OLD)
endif(COMMAND cmake_policy)

SET(EXECUTABLE_OUTPUT_PATH ".")

find_package(OpenCV REQUIRED)
find_package(CUDA ${ZED_CUDA_VERSION} EXACT REQUIRED)
find_package(dlib REQUIRED)

IF(NOT WIN32)
    add_definitions(-Wno-format-extra-args)
    SET(SPECIAL_OS_LIBS "pthread" "X11")
ENDIF()

include_directories(${CUDA_INCLUDE_DIRS})
include_directories(${OPENCV_INCLUDE_DIRS})

link_directories(${OpenCV_LIBRARY_DIRS})
link_directories(${CUDA_LIBRARY_DIRS})

ADD_EXECUTABLE(${execName} ${HDR_FILES} video_test_shape.cpp)
add_definitions(-std=c++11 -g -O3)

TARGET_LINK_LIBRARIES(${execName}
                        ${SPECIAL_OS_LIBS}
                        ${OpenCV_LIBRARIES}
                        ${dlib_LIBRARIES}
                        ${CUDA_CUDA_LIBRARY} ${CUDA_CUDART_LIBRARY})
