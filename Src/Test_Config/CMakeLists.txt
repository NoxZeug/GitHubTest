cmake_minimum_required( VERSION 3.22 )
get_filename_component( CURR_FILE_NAME ${CMAKE_CURRENT_LIST_FILE} NAME )
get_filename_component( CURR_DIR_NAME  ${CMAKE_CURRENT_LIST_DIR} NAME )
include_guard(GLOBAL)
message(        "================================")
message( STATUS "CMAKE_CURRENT_LIST_DIR         : ${CMAKE_CURRENT_LIST_DIR}")
message( STATUS "CURRENT DIR / FILE             : ${CURR_DIR_NAME}/${CURR_FILE_NAME}")

#===============================================================================
#===============================================================================
#===============================================================================

if( COMMAND TARGET_LIST_CLEAR )
  TARGET_LIST_CLEAR()
endif()

# create a list of all subfolders
file(GLOB tmp_dirlist RELATIVE ${CMAKE_CURRENT_LIST_DIR} ${CMAKE_CURRENT_LIST_DIR}/* )
list(SORT tmp_dirlist COMPARE FILE_BASENAME CASE INSENSITIVE
  ORDER
    ASCENDING
  # DESCENDING
  )
set(dirlist "")
foreach(item ${tmp_dirlist})
  if(IS_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/${item}")
    list( APPEND dirlist ${item})
  endif()
endforeach()

# search for 'CMakeLists.txt' in all subfolders in 'dirlist'
# and go into the subfolder if the file exists.
foreach(subdir ${dirlist})
  set( NAME_PREFIX ${CURR_NAME_PREFIX}_${item_name} )
  if( EXISTS "${CMAKE_CURRENT_LIST_DIR}/${subdir}/CMakeLists.txt" )
    message( STATUS "\n>>> go into \"${subdir}\" in ${CMAKE_CURRENT_LIST_DIR}")
    add_subdirectory( ${subdir} )
    message( STATUS "<<< come from \"${subdir}\" in ${CMAKE_CURRENT_LIST_DIR}")
    if( COMMAND TARGET_LIST_ADDFOLDER )
      TARGET_LIST_ADDFOLDER(${CURR_DIR_NAME})
    endif()
  endif()
endforeach()

if( COMMAND TARGET_LIST_TO_PARENT )
  TARGET_LIST_TO_PARENT()
endif()

# vim: nu:nowrap:tw=0