project(ppocrv5ncnn)

cmake_minimum_required(VERSION 3.10)

#set(OpenCV_DIR ${CMAKE_SOURCE_DIR}/opencv-mobile-4.12.0-android/sdk/native/jni)
if(DEFINED OpenCV_DIR_FROM_GRADLE)
    message(STATUS "Using OpenCV path from Gradle: ${OpenCV_DIR_FROM_GRADLE}")
    set(OpenCV_DIR ${OpenCV_DIR_FROM_GRADLE})
else()
    message(WARNING "OpenCV_DIR_FROM_GRADLE not set. Falling back to default path.")
    # 這個備用路徑指向我們在 Gradle 中設定的固定目錄名稱
    set(OpenCV_DIR ${CMAKE_SOURCE_DIR}/opencv-mobile-android/sdk/native/jni)
endif()
find_package(OpenCV REQUIRED core imgproc)

#set(ncnn_DIR ${CMAKE_SOURCE_DIR}/ncnn-20250916-android-vulkan/${ANDROID_ABI}/lib/cmake/ncnn)
if(DEFINED ncnn_DIR_FROM_GRADLE)
    message(STATUS "Using NCNN path from Gradle: ${ncnn_DIR}")
    #    set(ncnn_DIR ${ncnn_DIR_FROM_GRADLE}/${ANDROID_ABI}/lib/cmake/ncnn)
    set(ncnn_DIR ${CMAKE_SOURCE_DIR}/ncnn-android-vulkan/${ANDROID_ABI}/lib/cmake/ncnn)
else()
    message(WARNING "ncnn_DIR_FROM_GRADLE not set. Falling back to default path. Please sync project with Gradle.")
    set(ncnn_DIR ${CMAKE_SOURCE_DIR}/ncnn-android-vulkan/${ANDROID_ABI}/lib/cmake/ncnn)
endif()

find_package(ncnn REQUIRED)

add_library(ppocrv5ncnn SHARED ppocrv5ncnn.cpp ppocrv5.cpp ndkcamera.cpp)

target_link_libraries(ppocrv5ncnn ncnn ${OpenCV_LIBS} camera2ndk mediandk)
