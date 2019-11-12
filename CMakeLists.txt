
cmake_minimum_required( VERSION 3.2 ) 
set( CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "Supported configuration types" FORCE )
        
project( pugixml-adapter LANGUAGES CXX )
get_directory_property( is_subproject PARENT_DIRECTORY )
include( CMakeDependentOption REQUIRED )

if ( NOT GIT_EXECUTABLE )
    find_package( Git )
    if ( NOT GIT_FOUND )
        message( FATAL_ERROR "git installation was not found." )
    endif()
endif()

execute_process(
    COMMAND ${GIT_EXECUTABLE} rev-parse --abbrev-ref HEAD
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_BRANCH
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
execute_process(
    COMMAND ${GIT_EXECUTABLE} rev-parse HEAD
    WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
    OUTPUT_VARIABLE GIT_HASH
    OUTPUT_STRIP_TRAILING_WHITESPACE
) 

message( STATUS "" )
message( STATUS "-----------------------------------------------------------" )
message( STATUS "" )
message( STATUS "pugixml-adapter" )
message( STATUS "Git current branch: ${GIT_BRANCH}" )
message( STATUS "Git commit hash: ${GIT_HASH}" )
message( STATUS "" )
message( STATUS "-----------------------------------------------------------" )

include( CMakeDependentOption )

option( pugixml-header-only 
  "compile pugixml as header-only" OFF )
CMAKE_DEPENDENT_OPTION( pugixml_shared_library 
  "Use shared library"
  "${BUILD_SHARED_LIBRARIES}"
  "NOT pugixml-header-only" OFF )

if( pugixml-header-only )
  add_library( pugixml-adapter INTERFACE )
  target_sources( pugixml-adapter 
    INTERFACE 
      "${CMAKE_CURRENT_SOURCE_DIR}/src/src/pugiconfig.hpp"
      "${CMAKE_CURRENT_SOURCE_DIR}/src/src/pugixml.hpp"
  )
  target_include_directories( pugixml-adapter INTERFACE src/src )
else()
  if( pugixml_shared_library )
    add_library( pugixml-adapter SHARED "" )
  else()
    add_library( pugixml-adapter STATIC "" )
  endif()

  target_sources( pugixml-adapter 
    PUBLIC 
      "${CMAKE_CURRENT_SOURCE_DIR}/src/src/pugiconfig.hpp"
      "${CMAKE_CURRENT_SOURCE_DIR}/src/src/pugixml.hpp"
    PRIVATE
      "${CMAKE_CURRENT_SOURCE_DIR}/src/src/pugixml.cpp"
  )
  target_include_directories( pugixml-adapter PUBLIC src/src )
endif()

