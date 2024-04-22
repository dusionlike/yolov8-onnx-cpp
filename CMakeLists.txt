CMAKE_MINIMUM_REQUIRED(VERSION 3.26)
project(YOLOv8CPP)

if(MSVC)
  add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/source-charset:utf-8>")
  add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/execution-charset:utf-8>")
endif()

# 设置输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

link_directories(${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

if(MSVC)
  SET (ONNXRUNTIME_DIR third_party/onnxruntime-win-x86-1.11.1)
  add_definitions(-DONNXRUNTIME_1_11_1)
endif()

# 添加到include_directories
include_directories(${ONNXRUNTIME_DIR}/include)
# 添加到link_directories
link_directories(${ONNXRUNTIME_DIR}/lib)

if(MSVC)
  set(OpenCV_DIR D:/opencv454/build)
endif()

FIND_PACKAGE(OpenCV REQUIRED)

include_directories(include)

file(GLOB_RECURSE CURR_SOURCES src/*.cpp)

# Create the executable
add_executable(YOLOv8CPP ${CURR_SOURCES})

SET(CMAKE_CXX_STANDARD 17)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

target_compile_features(YOLOv8CPP PRIVATE cxx_std_17)

TARGET_LINK_LIBRARIES(YOLOv8CPP ${OpenCV_LIBS})
TARGET_LINK_LIBRARIES(YOLOv8CPP onnxruntime)
