cmake_minimum_required(VERSION 3.5)

# Requirements
# 1) Install Qt5
# 2) Compile VTK and set QtGroup ON
# 3) Compile PCL and set VTK dir

project(pcl_visualizer)

# init_qt: Let's do the CMake job for us
set(CMAKE_AUTOMOC ON) # For meta object compiler
set(CMAKE_AUTORCC ON) # Resource files
set(CMAKE_AUTOUIC ON) # UI files

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Find the QtWidgets library
find_package(Qt5 REQUIRED Widgets)



find_package(VTK 9.0 PATHS /home/marcus/Software/VTK-9.1.0.rc2/build/install  REQUIRED)
find_package(PCL 1.12 PATHS /home/marcus/Software/pcl/build/install/share/pcl-1.12 REQUIRED)

message(STATUS "PCL: ${PCL_VERSION}")
message(STATUS "VTK: ${VTK_VERSION}")
message(STATUS "QT: ${Qt5_VERSION}")

# Fix a compilation bug under ubuntu 16.04 (Xenial)
# list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")

include_directories(${PCL_INCLUDE_DIRS} src)
add_definitions(${PCL_DEFINITIONS})

set(project_SOURCES src/main.cpp src/pcd_view_qt.cpp)

add_executable(${PROJECT_NAME} ${project_SOURCES})

target_link_libraries(${PROJECT_NAME} ${PCL_LIBRARIES}  Qt5::Widgets )
