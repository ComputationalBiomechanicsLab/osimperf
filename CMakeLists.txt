cmake_minimum_required(VERSION 3.25)
project(osimperf VERSION 0.0.1 LANGUAGES CXX C)

if(NOT (UNIX AND NOT APPLE))
    message(FATAL_ERROR "Only Linux builds are currently supported (check CMAKE_INSTALL_RPATH etc.)")
endif()

include(GNUInstallDirs)  # CMAKE_INSTALL_BINDIR

find_package(OpenSim REQUIRED)

set(CMAKE_INSTALL_RPATH "$ORIGIN/../lib")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH OFF)

add_executable(osimperf-runner osimperf-runner.cpp)
set_target_properties(osimperf-runner PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED YES CXX_EXTENSIONS OFF)
target_compile_features(osimperf-runner PRIVATE cxx_std_17)
target_link_libraries(osimperf-runner PRIVATE ${OpenSim_LIBRARIES})
target_include_directories(osimperf-runner PRIVATE ${OpenSim_INCLUDE_DIRS})

foreach(path IN LISTS CMAKE_PREFIX_PATH)
    target_link_directories(osimperf-runner PRIVATE "${path}/${CMAKE_INSTALL_LIBDIR}")
endforeach()

install(TARGETS osimperf-runner RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

