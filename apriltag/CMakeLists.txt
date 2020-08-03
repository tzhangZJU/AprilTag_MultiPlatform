cmake_minimum_required(VERSION 3.1)
project(Apriltag_Lib)

include_directories(common)
file(GLOB lib_srcs *.c common/*.c)
file(GLOB lib_hdrs *.h common/*.h)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")
if(CMAKE_COMPILER_IS_GNUCXX AND UNIX)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
endif()

add_library(${PROJECT_NAME} ${lib_srcs} ${lib_hdrs})

if(WIN32 AND MSVC)
  target_link_libraries (${PROJECT_NAME} PTHREADS_LIBRARY)
else()
  find_package(Threads REQUIRED)
  target_link_libraries(${PROJECT_NAME} PUBLIC Threads::Threads m)
endif()


  set(CMAKE_INSTALL_LIBDIR ${CMAKE_SOURCE_DIR}/install/lib)
  target_include_directories(${PROJECT_NAME} PUBLIC
      "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/>"
      "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/common>"
      "$<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${CMAKE_INSTALL_INCLUDEDIR}>/apriltag")

  set_target_properties(${PROJECT_NAME} PROPERTIES
      ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_INSTALL_LIBDIR}
      LIBRARY_OUTPUT_DIRECTORY ${CMAKE_INSTALL_LIBDIR}
  )

# install library
  install(TARGETS ${PROJECT_NAME} EXPORT apriltagTargets
      LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
      ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )

  # install header file hierarchy
  file(GLOB_RECURSE HEADER_FILES RELATIVE ${PROJECT_SOURCE_DIR} *.h)
  foreach(HEADER ${HEADER_FILES})
      string(REGEX MATCH "(.*)[/\\]" DIR ${HEADER})
      install(FILES ${HEADER} DESTINATION ${CMAKE_SOURCE_DIR}/install/include)
  endforeach()

  # rarely used, so comment these
  # if(UNIX)
  # # export library
  # install(EXPORT apriltagTargets
  #     FILE apriltagTargets.cmake
  #     NAMESPACE apriltag::
  #     DESTINATION ${CMAKE_INSTALL_LIBDIR}
  # )
  # install(FILES CMake/apriltagConfig.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR})
  # export(TARGETS ${PROJECT_NAME}
  #     NAMESPACE apriltag::
  #     FILE apriltagTargets.cmake)
  #endif

  # FILE(READ apriltag.pc.in PKGC)
  # STRING(REGEX REPLACE "^prefix=" "prefix=${CMAKE_INSTALL_PREFIX}" PKGC_CONF "${PKGC}" )
  # FILE(WRITE ${PROJECT_BINARY_DIR}/apriltag.pc ${PKGC_CONF})
  # install(FILES "${PROJECT_BINARY_DIR}/apriltag.pc" DESTINATION ${CMAKE_INSTALL_LIBDIR})

#   # Python wrapper
#   SET(Python_ADDITIONAL_VERSIONS 3)
#   find_package(PythonLibs)
#   execute_process(COMMAND which python3 OUTPUT_QUIET RESULT_VARIABLE Python3_NOT_FOUND)
#   execute_process(COMMAND python3 -c "import numpy" RESULT_VARIABLE Numpy_NOT_FOUND)
#   if (NOT Python3_NOT_FOUND AND NOT Numpy_NOT_FOUND AND PYTHONLIBS_FOUND)
#   # TODO deal with both python2/3
#   execute_process(COMMAND python3 ${PROJECT_SOURCE_DIR}/python_build_flags.py OUTPUT_VARIABLE PY_OUT)
#   set(PY_VARS CFLAGS LDFLAGS LINKER EXT_SUFFIX)
#   cmake_parse_arguments(PY "" "${PY_VARS}" "" ${PY_OUT})
#   separate_arguments(PY_CFLAGS)
#   separate_arguments(PY_LDFLAGS)
#   set(LIB_APRILTAG_PATH ${PROJECT_BINARY_DIR}/lib)

#   foreach(X detect py_type)
#   add_custom_command(OUTPUT ${PROJECT_BINARY_DIR}/apriltag_${X}.docstring.h
#       COMMAND < ${PROJECT_SOURCE_DIR}/apriltag_${X}.docstring sed 's/\"/\\\\\"/g\; s/^/\"/\; s/$$/\\\\n\"/\;' > apriltag_${X}.docstring.h
#       WORKING_DIRECTORY ${PROJECT_BINARY_DIR})
#   endforeach()

#   add_custom_command(OUTPUT apriltag_pywrap.o
#       COMMAND ${CMAKE_C_COMPILER} ${CMAKE_C_FLAGS} ${PY_CFLAGS} -I${PROJECT_BINARY_DIR} -c -o apriltag_pywrap.o ${PROJECT_SOURCE_DIR}/apriltag_pywrap.c
#       DEPENDS ${PROJECT_SOURCE_DIR}/apriltag_pywrap.c ${PROJECT_BINARY_DIR}/apriltag_detect.docstring.h ${PROJECT_BINARY_DIR}/apriltag_py_type.docstring.h)
#   add_custom_command(OUTPUT apriltag${PY_EXT_SUFFIX}
#       COMMAND ${PY_LINKER} ${PY_LDFLAGS} -Wl,-rpath=lib apriltag_pywrap.o ${LIB_APRILTAG_PATH}/libapriltag.so -o apriltag${PY_EXT_SUFFIX}
#       DEPENDS ${PROJECT_NAME} apriltag_pywrap.o)
#   add_custom_target(apriltag_python ALL
#       DEPENDS apriltag${PY_EXT_SUFFIX})

#   execute_process(COMMAND python3 -m site --user-site OUTPUT_VARIABLE PY_DEST)
#   string(STRIP ${PY_DEST} PY_DEST)
#   install(CODE "execute_process(COMMAND cp ${PROJECT_BINARY_DIR}/apriltag${PY_EXT_SUFFIX} ${PY_DEST})")
#   endif (NOT Python3_NOT_FOUND AND NOT Numpy_NOT_FOUND AND PYTHONLIBS_FOUND)


#   # Examples
#   # apriltag_demo
#   add_executable(apriltag_demo example/apriltag_demo.c)
#   target_link_libraries(apriltag_demo apriltag)

#   # opencv_demo
#   find_package(OpenCV)
#   if(OpenCV_FOUND)
#       add_executable(opencv_demo example/opencv_demo.cc)
#       target_link_libraries(opencv_demo apriltag ${OpenCV_LIBRARIES})
#       set_target_properties(opencv_demo PROPERTIES CXX_STANDARD 11)
#       install(TARGETS opencv_demo RUNTIME DESTINATION bin)
#   endif(OpenCV_FOUND)

#   # install example programs
#   install(TARGETS apriltag_demo RUNTIME DESTINATION bin)



