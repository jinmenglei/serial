cmake_minimum_required(VERSION 3.5)
project(serial)

# Find catkin
#find_package(catkin REQUIRED)
if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic")
endif()


find_package(rosidl_default_generators REQUIRED)
find_package(ament_cmake REQUIRED)
## Sources
set(serial_SRCS
    src/serial.cc
    include/serial/serial.h
    include/serial/v8stdint.h
)

# If unix
list(APPEND serial_SRCS src/impl/unix.cc)
list(APPEND serial_SRCS src/impl/list_ports/list_ports_linux.cc)


## Add serial library
add_library(${PROJECT_NAME} ${serial_SRCS})


target_link_libraries(${PROJECT_NAME} rt pthread)


## Include headers
include_directories(include)

## Install executable
install(TARGETS
        ${PROJECT_NAME}
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
)

ament_export_include_directories(include)

ament_export_libraries(${PROJECT_NAME})
#add_executable(${PROJECT_NAME} src/serial.cc)
## Install headers
install(
        DIRECTORY include/serial
        DESTINATION include)


## Tests
#if(BUILD_TESTING)
#    add_subdirectory(tests)
#endif()

ament_export_dependencies(rosidl_default_runtime)
ament_package()

