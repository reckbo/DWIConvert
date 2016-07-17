
project(DWIConvert)

#-----------------------------------------------------------------------------
# Dependencies.
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

add_library(DWIConvertSupportLib STATIC ${DWIConvertSupportLib_SRCS})
target_link_libraries(DWIConvertSupportLib ${DWIConvert_ITK_LIBRARIES})
find_package(SlicerExecutionModel REQUIRED GenerateCLP)
include(${SlicerExecutionModel_USE_FILE})

SEMMacroBuildCLI(
  NAME "DWIConvert" EXECUTABLE_ONLY
  TARGET_LIBRARIES DWIConvertSupportLib
  INCLUDE_DIRECTORIES ${ITK_INSTALL_PREFIX}/include/dcmtk/dcmdata ${ITK_INSTALL_PREFIX}/include/dcmtk/dcmjpls ${ITK_INSTALL_PREFIX}/include/dcmtk/oflog ${ITK_INSTALL_PREFIX}/include/
  )

 # StandardBRAINSBuildMacro(NAME DWIConvert EXECUTABLE_ONLY
 #  TARGET_LIBRARIES DWIConvertSupportLib)
