
cmake_minimum_required(VERSION 3.3 FATAL_ERROR)

project(GaussianRandomNumber)

find_package(VTK COMPONENTS 
  vtkCommonCore QUIET)
if (NOT VTK_FOUND)
  message("Skipping GaussianRandomNumber: ${VTK_NOT_FOUND_MESSAGE}")
  return ()
endif()
message (STATUS "VTK_VERSION: ${VTK_VERSION}")
if (VTK_VERSION VERSION_LESS "8.90.0")
  # old system
  include(${VTK_USE_FILE})
  if(EMSCRIPTEN)
    web_add_executable(GaussianRandomNumber MACOSX_BUNDLE GaussianRandomNumber.cxx )
    web_target_link_libraries(GaussianRandomNumber PRIVATE ${VTK_LIBRARIES})
  else()
    add_executable(GaussianRandomNumber MACOSX_BUNDLE GaussianRandomNumber.cxx )
    target_link_libraries(GaussianRandomNumber PRIVATE ${VTK_LIBRARIES})
  endif()
else ()
  # include all components
  if(EMSCRIPTEN)
    web_add_executable(GaussianRandomNumber MACOSX_BUNDLE GaussianRandomNumber.cxx )
    web_target_link_libraries(GaussianRandomNumber PRIVATE ${VTK_LIBRARIES})
  else()
    add_executable(GaussianRandomNumber MACOSX_BUNDLE GaussianRandomNumber.cxx )
    target_link_libraries(GaussianRandomNumber PRIVATE ${VTK_LIBRARIES})
  endif()
  # vtk_module_autoinit is needed
  vtk_module_autoinit(
    TARGETS GaussianRandomNumber
    MODULES ${VTK_LIBRARIES}
    )
endif () 
