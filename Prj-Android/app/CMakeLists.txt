
cmake_minimum_required(VERSION 3.1)
project(FaceTracking-lib)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_C_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")


include_directories(/Users/smkk/ncnn-android-lib/include)
include_directories(/Users/smkk/OpenCV-android-sdk/sdk/native/jni/include)

include_directories(src/main/cpp/include)
set(distribution_DIR ${CMAKE_SOURCE_DIR}/../../../../src/main/jniLibs)

#### libyuv  start###
add_library( # Sets the name of the library.
        yuv

        # Sets the library as ashared library.
        SHARED

        # Provides a relative pathto your source file(s).
        IMPORTED )

set_target_properties(
        yuv
        PROPERTIES IMPORTED_LOCATION
        ../../../../src/main/jniLibs/${ANDROID_ABI}/libyuv.so
)



find_library( # Sets the name of the path variable.
              log-lib
              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

set(DETECTION_FILES src/main/cpp/mtcnn.cpp)
set(SOURCE_FILES src/main/cpp/tracking-lib.cpp)

add_library(libncnn STATIC IMPORTED )

set_target_properties(libncnn PROPERTIES IMPORTED_LOCATION /Users/smkk/ncnn-android-lib/${ANDROID_ABI}/libncnn.a)

add_library(Tracking-lib SHARED ${DETECTION_FILES} ${SOURCE_FILES})


set(OpenCV_DIR "/Users/smkk/OpenCV-android-sdk/sdk/native/jni")
find_package(OpenCV REQUIRED)

target_link_libraries(Tracking-lib libncnn ${OpenCV_LIBS} z jnigraphics)


