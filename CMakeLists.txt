project (lib_clock)

cmake_minimum_required(VERSION 2.6)

add_subdirectory(src)
######################################################################################
# Selector to choose architecture to build lib_CLOCK
######################################################################################
set(LIB_CLOCK_ARCHITECTURE ${LIB_CLOCK_ARCHITECTURE} CACHE STRING "Architecture to build lib_CLOCK on")
get_property(architectures GLOBAL PROPERTY LIB_CLOCK_ARCHITECTURES)
list(SORT architectures)
set_property(CACHE LIB_CLOCK_ARCHITECTURE PROPERTY STRINGS None ${architectures})

#######################################################################################
# Check LIB_CLOCK_ARCHITECTURE has a valid value
#######################################################################################
if("${LIB_CLOCK_ARCHITECTURE}" STREQUAL "None")
message(FATAL_ERROR "No valid architecture chosen! Please check LIB_CLOCK_ARCHITECTURE - ${LIB_CLOCK_ARCHITECTURE}.")
endif("${LIB_CLOCK_ARCHITECTURE}" STREQUAL "None")

#######################################################################################
# Get Architecture specific build paramters
#######################################################################################
GET_PROPERTY(LIB_CLOCK_SOURCE_C            GLOBAL PROPERTY LIB_CLOCK_ARCH_SOURCE_C)
GET_PROPERTY(LIB_CLOCK_PRIVATE_DEFINITION  GLOBAL PROPERTY LIB_CLOCK_ARCH_PRIVATE_DEFINITION)
GET_PROPERTY(LIB_CLOCK_DEPEND              GLOBAL PROPERTY LIB_CLOCK_ARCH_DEPEND)

SET(LIB_CLOCK_DEPEND ${LIB_CLOCK_DEPEND} lib_convention)
SET(LIB_CLOCK_PRIVATE_DEFINITION ${LIB_CLOCK_PRIVATE_DEFINITION} )

#######################################################################################
#Build target settings
#######################################################################################
add_library(${PROJECT_NAME} STATIC ${LIB_CLOCK_SOURCE_C})
target_link_libraries(${PROJECT_NAME} ${LIB_CLOCK_DEPEND})
target_include_directories(${PROJECT_NAME} PUBLIC ./include)
target_compile_definitions(${PROJECT_NAME} PRIVATE ${LIB_CLOCK_PRIVATE_DEFINITION})
