cmake_minimum_required(VERSION 3.19)
 
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON) # We require C++ 17 or later

add_library(bonxai_map)

target_compile_definitions(bonxai_map PRIVATE)

set(_HDRS
    include/bonxai_map/probabilistic_map.hpp)
set(_SRCS
     src/probabilistic_map.cpp
)

target_sources(bonxai_map
    PRIVATE ${_HDRS} ${_SRCS}
    PUBLIC FILE_SET HEADERS 
    BASE_DIRS ${_INCLUDE_DIR}
    FILES ${_HDRS})

target_link_libraries(bonxai_map PUBLIC
    bonxai_core)

set_target_properties(bonxai_map PROPERTIES LINKER_LANGUAGE CXX)

# Add add entry for this project into CMake's package registry, so that this project can be found by other CMake projects
set(CMAKE_EXPORT_PACKAGE_REGISTRY FALSE)  
# This flag is required to actually export the package for CMake versions >= 3.15
export(PACKAGE bonxai_map)
# Create a configuration file for this project, so that it can be imported by other CMake projects
export(TARGETS bonxai_map FILE bonxai_mapConfig.cmake)
 
install(TARGETS bonxai_map FILE_SET HEADERS )

####################################################
if(${ENABLE_PCL})
    # PCL utils library
    add_library(pcl_utils)

    target_compile_definitions(pcl_utils PRIVATE
        bonxai_map
        ${PCL_DEFINITIONS})

    set(pcl_Utils_HDRS
	    include/bonxai_map/pcl_utils.hpp)
    set(pcl_Utils_SRCS
         src/pcl_utils.cpp)

    target_sources(pcl_utils
        PRIVATE ${pcl_Utils_HDRS} ${pcl_Utils_SRCS}
        PUBLIC FILE_SET HEADERS 
        BASE_DIRS ${_INCLUDE_DIR}
        FILES ${_HDRS})


    target_link_libraries(pcl_utils PUBLIC
        ${PCL_LIBRARIES})

    set_target_properties(pcl_utils PROPERTIES LINKER_LANGUAGE CXX)

    # Add add entry for this project into CMake's package registry, so that this project can be found by other CMake projects
    set(CMAKE_EXPORT_PACKAGE_REGISTRY FALSE)  
    # This flag is required to actually export the package for CMake versions >= 3.15
    export(PACKAGE pcl_utils)
    # Create a configuration file for this project, so that it can be imported by other CMake projects
    export(TARGETS pcl_utils FILE pcl_utilsConfig.cmake)
 
    install(TARGETS pcl_utils FILE_SET HEADERS )
endif()

if(benchmark_FOUND AND octomap_FOUND)
    add_subdirectory(benchmark)
endif()
