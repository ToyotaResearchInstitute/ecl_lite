##############################################################################
# Cmake
##############################################################################

cmake_minimum_required(VERSION 3.5)
project(ecl_io)

##############################################################################
# Sockets
##############################################################################

set(sockets_INCLUDE_DIRS "") # Just assume they're in the system dirs for now
if(MINGW)
  set(sockets_LIBRARIES rpcrt4 shell32 ws2_32 advapi32 kernel32 msvcrt)
elseif(WIN32)
  find_library(sockets_LIBRARIES ws2_32)
else()
    set(sockets_LIBRARIES "")
endif()

##############################################################################
# Find Packages
##############################################################################

find_package(ament_cmake_ros REQUIRED)
find_package(ecl_build REQUIRED)
find_package(ecl_config REQUIRED)
find_package(ecl_errors REQUIRED)

##############################################################################
# Project Configuration
##############################################################################

ament_package_xml()
ecl_enable_cxx14_compiler()

include_directories(include
  ${ecl_config_INCLUDE_DIRS}
  ${ecl_errors_INCLUDE_DIRS}
  )

##############################################################################
# Sources
##############################################################################

add_subdirectory(include)
add_subdirectory(src)

##############################################################################
# Exports
##############################################################################

ament_export_interfaces(${PROJECT_NAME})
ament_export_dependencies(
    ecl_config
    ecl_errors
)
ament_export_libraries(${PROJECT_NAME})
ament_package()
