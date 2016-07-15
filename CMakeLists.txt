
project(DWIConvert)

#-----------------------------------------------------------------------------
# Dependencies.
#
#
# ITK
# SlicerExecutionModel

include(${CMAKE_CURRENT_SOURCE_DIR}/FindITKUtil.cmake)
FindITKUtil( DWIConvert_ITK
  ITKImageIntensity
  ITKImageStatistics
  ITKTestKernel
  ITKIODCMTK
  ITKIORAW
  ITKDCMTK
  ITKNrrdIO
)

#-----------------------------------------------------------------------------
# enable_testing()

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR} ${CMAKE_MODULE_PATH})

set(DWIConvertSupportLib_SRCS
  FSLToNrrd.cxx
  NrrdToFSL.cxx
  DWIMetaDataDictionaryValidator.cxx
  )

# set(DWIConvertTest_SRC)
# foreach(f ${DWIConvertSupportLib_SRCS})
#   list(APPEND DWIConvertTest_SRC
#     ${CMAKE_CURRENT_LIST_DIR}/${f})
# endforeach()

add_library(DWIConvertSupportLib STATIC ${DWIConvertSupportLib_SRCS})
#target_link_libraries(DWIConvertSupportLib ${DWIConvert_ITK_LIBRARIES} BRAINSCommonLib)
target_link_libraries(DWIConvertSupportLib ${DWIConvert_ITK_LIBRARIES})

# set(PROGS_LIST_1
#   Convert4DImageTo3DSeries
#   )
# foreach(prog ${PROGS_LIST_1})
#   add_executable(${prog} ${prog}.cxx)
#   #target_link_libraries(${prog} BRAINSCommonLib ${DWIConvert_ITK_LIBRARIES})
#   target_link_libraries(${prog} ${DWIConvert_ITK_LIBRARIES})
# endforeach()

find_package(SlicerExecutionModel REQUIRED GenerateCLP)
include(${CMAKE_CURRENT_SOURCE_DIR}/CMake/CMakeBRAINS3BuildMacros.cmake)

 StandardBRAINSBuildMacro(NAME DWIConvert EXECUTABLE_ONLY
   TARGET_LIBRARIES DWIConvertSupportLib)

# if(BUILD_TESTING AND NOT BRAINSTools_DISABLE_TESTING)
#   add_subdirectory(TestSuite)
# endif()
