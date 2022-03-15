cmake_minimum_required( VERSION 3.22 )

project( Example_Basics
  VERSION     1.0.0.0
  LANGUAGES   C CXX  )

# find all sources
file( GLOB_RECURSE all_sources
      *.cc *.cpp *.cxx
      *.hh *.hpp *.hxx
  )

# create an executable CMake-Target
add_executable( ${PROJECT_NAME}
  ${all_sources}
  )
  
if( COMMAND TARGET_LIST_APPEND_WITH_FOLDER )
  TARGET_LIST_APPEND_WITH_FOLDER( ${PROJECT_NAME} )
endif()

target_include_directories( ${PROJECT_NAME}
  PRIVATE
    ${PROJECT_BINARY_DIR}
    ${PROJECT_BINARY_DIR}
  )

  
# vim: nu:nowrap:tw=0
