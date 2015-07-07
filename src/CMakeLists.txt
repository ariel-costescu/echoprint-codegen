# Create the library
FILE(GLOB codegen_SRCS
  "${CMAKE_CURRENT_SOURCE_DIR}/AudioBufferInput.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/AudioStreamInput.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Base64.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Codegen.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/echoprint.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Fingerprint.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/functions.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/MatrixUtility.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Metadata.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/SubbandAnalysis.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Whitening.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/adler32.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/compress.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/crc32.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/deflate.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzclose.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzlib.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzread.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzwrite.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/infback.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/inffast.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/inflate.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/inftrees.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/trees.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/uncompr.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/zutil.c"
  )
FILE(GLOB codegen_HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/*.h")

SET(BOOST_ROOT "C:\\boost_1_46_1")
SET(JNI_ROOT "C:\\Program Files\\Java\\jdk1.8.0_45\\include")
SET(JNI_ROOT_W32 "C:\\Program Files\\Java\\jdk1.8.0_45\\include\\win32")

# Find package taglib.
FIND_PATH(TagLib_INCLUDE_DIRS fileref.h
  PATH_SUFFIXES taglib
  DOC "Directory where TagLib headers are located"
  )
FIND_LIBRARY(TagLib_LIBRARY tag
  DOC "The TagLib library"
  )

FIND_PACKAGE(Threads REQUIRED)

INCLUDE_DIRECTORIES(SYSTEM
  ${TagLib_INCLUDE_DIRS}
  ${BOOST_ROOT}
  ${JNI_ROOT}
  ${JNI_ROOT_W32}
  )
ADD_LIBRARY(codegen SHARED ${codegen_SRCS})
TARGET_LINK_LIBRARIES(codegen
                      ${CMAKE_THREAD_LIBS_INIT}
                      ${TagLib_LIBRARY}
                      )
SET_TARGET_PROPERTIES(codegen PROPERTIES
  PUBLIC_HEADER "${codegen_HEADERS}"
  VERSION 4.1.2
  SOVERSION 4
  )
INSTALL(TARGETS codegen
  LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib # .so
  RUNTIME  DESTINATION "${INSTALL_LIB_DIR}" COMPONENT shlib # .dll
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}" COMPONENT dev
  )

# The main file
ADD_EXECUTABLE(echoprint-codegen
  "${CMAKE_CURRENT_SOURCE_DIR}/main.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/AudioBufferInput.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/AudioStreamInput.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Base64.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Codegen.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/echoprint.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Fingerprint.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/functions.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/MatrixUtility.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Metadata.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/SubbandAnalysis.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/Whitening.cxx"
  "${CMAKE_CURRENT_SOURCE_DIR}/adler32.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/compress.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/crc32.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/deflate.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzclose.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzlib.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzread.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/gzwrite.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/infback.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/inffast.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/inflate.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/inftrees.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/trees.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/uncompr.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/zutil.c"
  )
TARGET_LINK_LIBRARIES(echoprint-codegen codegen)
INSTALL(TARGETS echoprint-codegen
  DESTINATION "${INSTALL_BIN_DIR}"
  )
