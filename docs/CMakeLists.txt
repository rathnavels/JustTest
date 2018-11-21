find_package(Doxygen)
option(BUILD_DOCUMENTATION "Create and install the HTML based API documentation (requires Doxygen)" ${DOXYGEN_FOUND})

include_directories(${CMAKE_SOURCE_DIR}/Documentation)
include_directories(${CMAKE_SOURCE_DIR}/Documentation/Images)
#include_directories(${CMAKE_SOURCE_DIR}/Documentation/header)

if(BUILD_DOCUMENTATION)
  if(NOT DOXYGEN_FOUND)
    message("You must have doxygen installed to build the documentation!")
  else(NOT DOXYGEN_FOUND)
    set(doxyfile ${CMAKE_SOURCE_DIR}/Documentation/Doxyfile)
    MESSAGE("Doxyfile   :  " ${doxyfile})
    MESSAGE("Doxygen working directory : " ${CMAKE_CURRENT_BINARY_DIR})
    #file(COPY ${doxyfile} DESTINATION ${CMAKE_SOURCE_DIR}/)
    add_custom_target(Doxygen COMMAND ${DOXYGEN_EXECUTABLE} ${doxyfile}
                               WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
                               COMMENT "Generating API documentation with Doxygen"
                               VERBATIM)
  endif()
endif()

if(BUILD_DOCUMENTATION AND DOXYGEN_FOUND)
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/doxygen/html  DESTINATION ${ObjGL_INSTALL_TARGET}/Documentation)
  install(DIRECTORY ${CMAKE_SOURCE_DIR}/Documentation/Images DESTINATION ${ObjGL_INSTALL_TARGET}/Documentation)
  #install(DIRECTORY ${CMAKE_SOURCE_DIR}/Documentation/header DESTINATION ${ObjGL_INSTALL_TARGET}/Documentation)
endif()
