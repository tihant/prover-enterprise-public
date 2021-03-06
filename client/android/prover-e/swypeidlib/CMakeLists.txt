# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Download OpenCV for android 3.3 here: https://opencv.org/releases.html
# set path to OpenCV

include_directories( ../../../videoanalytics-public/include )

MESSAGE( STATUS "CMAKE_SOURCE_DIR:         " ${CMAKE_SOURCE_DIR} )

# configure path toOpenCV here:
include(${CMAKE_SOURCE_DIR}/CMakeLists_includeOpenCV.txt)

#../../../videoanalytics-public/src/swype

add_library( native-lib SHARED
             src/main/cpp/native-lib.cpp
             ../../../videoanalytics-public/src/swype/swype_detect.cpp
             ../../../videoanalytics-public/src/swype/common.cpp
             ../../../videoanalytics-public/src/swype/Vector.cpp
             ../../../videoanalytics-public/src/swype/VectorExplained.cpp
             ../../../videoanalytics-public/src/swype/SwypeStepDetector.cpp
             ../../../videoanalytics-public/src/swype/CircleDetector.cpp
             ../../../videoanalytics-public/src/swype/BoundsChecker.cpp
             ../../../videoanalytics-public/src/swype/SwypeCodeDetectorDelta.cpp
             ../../../videoanalytics-public/src/swype/SwypeCodeDetector.cpp
             ../../../videoanalytics-public/src/swype/ShiftDetector.cpp
             ../../../videoanalytics-public/src/swype/Histogram.cpp
              )

add_library( lib_opencv SHARED IMPORTED )
set_target_properties(lib_opencv PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libopencv_java3.so)


# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       native-lib
                       lib_opencv

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
