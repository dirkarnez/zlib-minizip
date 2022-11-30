cmake_minimum_required(VERSION 3.14)
project("libminizip")

add_library(zlib STATIC IMPORTED)

set_property(
  TARGET zlib APPEND PROPERTY IMPORTED_CONFIGURATIONS RELEASE
)

set_target_properties(zlib PROPERTIES
  IMPORTED_LINK_INTERFACE_LANGUAGES_RELEASE "CXX"
  IMPORTED_LOCATION_RELEASE "${ZLIB_ROOT}/lib/libzlibstatic.a"
)
  
set_target_properties(zlib PROPERTIES
  MAP_IMPORTED_CONFIG_MINSIZEREL Release
  MAP_IMPORTED_CONFIG_RELWITHDEBINFO Release
)

target_include_directories(zlib INTERFACE "${ZLIB_ROOT}/include")

SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static")

add_library(${PROJECT_NAME} STATIC
    "include/crypt.h"
    "include/ioapi.h"
    "include/iowin32.h"
    "include/mztools.h"
    "include/unzip.h"
    "include/zip.h"
    "src/ioapi.c"
    "src/iowin32.c"
    "src/mztools.c"
    "src/unzip.c"
    "src/zip.c"
)

target_include_directories(${PROJECT_NAME}
    PUBLIC
    "include")

target_link_libraries(${PROJECT_NAME} zlib)