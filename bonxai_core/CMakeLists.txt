
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

# Create a configuration file for this project, so that it can be imported by other CMake projects
export(TARGETS bonxai_core FILE bonxai_coreConfig.cmake)
install(TARGETS bonxai_core FILE_SET HEADERS )

if(benchmark_FOUND)
    add_subdirectory(benchmark)
endif()


