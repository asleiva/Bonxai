
add_library(bonxai_core INTERFACE)
set_target_properties(bonxai_core PROPERTIES LINKER_LANGUAGE CXX)


target_compile_features(bonxai_core INTERFACE cxx_std_17)
set(_HDRS
    include/bonxai/bonxai.hpp
    include/bonxai/serialization.hpp
)
set(_INCLUDE_DIR include)
target_sources(bonxai_core
    PRIVATE ${_HDRS} 
    PUBLIC FILE_SET HEADERS 
    BASE_DIRS ${_INCLUDE_DIR}
    FILES ${_HDRS})

# Add add entry for this project into CMake's package registry, so that this project can be found by other CMake projects
set(CMAKE_EXPORT_PACKAGE_REGISTRY FALSE)  # This flag is required to actually export the package for CMake versions >= 3.15
export(PACKAGE bonxai_core)
# Create a configuration file for this project, so that it can be imported by other CMake projects
export(TARGETS bonxai_core FILE bonxai_coreConfig.cmake)
 
install(TARGETS bonxai_core FILE_SET HEADERS )

if(benchmark_FOUND)
    add_subdirectory(benchmark)
endif()


