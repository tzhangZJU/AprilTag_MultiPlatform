cmake_minimum_required ( VERSION 3.1 )
project(AprilTag_MultiPlatform)
if(WIN32 AND MSVC)
  add_subdirectory(pthreads-win32)
endif()
add_subdirectory(apriltag)

