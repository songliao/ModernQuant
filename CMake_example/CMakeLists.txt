# cmaker verion, always on top of CMakeLists.txt
cmake_minimum_required(VERSION 3.20)

# define project, one project can contain multiple targets
project(cmake_tutorial)

# set language standards
set(CMAKE_CXX_STANDARD 20)
set(CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_EXTENSIONS True)

find_package(Boost)
find_package(Armadillo CONFIG REQUIRED)

#### project level operation #######
# affect all targtes in the project
# 往项目中增加源文件的文件夹的相对路径, 源文件中必须包含CMakeLists.txt
add_subdirectory(calculate)

# 往项目中增加一个外部库头文件, 这样的话，项目中所有的targets都能识别这个外部库头（h文件, 文本文件）（head only library）
include_directories(${Boost_INCLUDE_DIRS}$)

# 往项目中增加一个外部的链接库（dll 或 so， 二进制文件）
link_libraries(${Boost_LIBRARIES})


### target level ####

# a project can have multiple targets
# target type: executable, library 
# add an exutable to the project 
add_executable(main main.cpp)
# add_library(target_name STATIC target_source1.cpp target_source2.cpp ....)
# 往项目中增加编译目标：可执行文件executable, 动态链接库library(SHARED), 静态链接库library(STATIC)


# 目标中的所有源文件，能识别搜索对应的外部库文件， 可以用尖括号搜寻头文件了
target_include_directories(main PRIVATE ${Boost_INCLUDE_DIRS}$)
# 目标中需要用的外部动态库或静态库
target_link_libraries(main PRIVATE ${ARMADILLO_LIBRARIES})


# CMAKE 结合软件包管理工具，比如vcpkg， 可以方便的引入一些第三方库,
# 使用之前，要设置CMAKE 和 软件管理工具的结合
find_package(Boost)
find_package(Armadillo CONFIG REQUIRED)
find_package(Eigen3)