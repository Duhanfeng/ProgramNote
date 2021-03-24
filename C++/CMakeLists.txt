# cmake文件demo,包含opencv/boost/halcon/hik等常用库类的添加逻辑

####################################################################################################
# 工程设置
####################################################################################################

# 设置cmake最低版本
cmake_minimum_required(VERSION 3.5)

# 设置工程名
project(ShapeMatch)

# 设定C++版本
set(CMAKE_CXX_STANDARD 11)

# 设置为release模式,在vs编译器编译的情况下,debug模式和release模式的性能存在较大的差异
set(CMAKE_BUILD_TYPE "Release")

# 开启此选项,可以在release模式下进行debug调试
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /Zi")
set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS_RELEASE} /DEBUG /OPT:REF /OPT:ICF")

####################################################################################################
# 添加第三方库
# 1. 设置头文件目录 include_directories
# 2. 设置库文件目录 link_directories
# 3. 将库文件链接进工程中 link_libraries  需要在add_executable之前
# 3. 将库文件链接进工程中 target_link_libraries  需要在add_executable之后
####################################################################################################

# 添加opencv支持
set(OpenCV_DIR "C:/Program Files/CppLibrary/opencv/build")
find_package(OpenCV REQUIRED)
link_directories(${OpenCV_LIBRARY_DIRS})
#target_link_libraries(${PROJECT_NAME}  ${OpenCV_LIBS})  # 这个要放最后

# 添加boost/serialization支持
include_directories("C:/Program Files/CppLibrary/boost/include/boost-1_75")
link_directories("C:/Program Files/CppLibrary/boost/lib")
if(${CMAKE_BUILD_TYPE} STREQUAL Release)
    set(BOOST_SERIALIZATION_LIB "libboost_serialization-vc142-mt-x64-1_75")
else()
    set(BOOST_SERIALIZATION_LIB "libboost_serialization-vc142-mt-gd-x64-1_75")
endif()
link_libraries(${BOOST_SERIALIZATION})
#target_link_libraries(${PROJECT_NAME} ${BOOST_SERIALIZATION_LIB})

####################################################################################################
# 设置输出
####################################################################################################

# 设置头文件目录
include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/match
)

#设置参与编译的文件
file(GLOB SRC_LIST "*.cpp" "match/*.cpp")

# 将源代码添加到此项目的可执行文件
add_executable (${PROJECT_NAME} ${SRC_LIST})

####################################################################################################
# 绑定工程和第三方库
####################################################################################################

target_link_libraries(${PROJECT_NAME} 
  ${OpenCV_LIBS}
)
