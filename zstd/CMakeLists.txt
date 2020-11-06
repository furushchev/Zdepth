cmake_minimum_required(VERSION 3.5)
project(zstd C)

set(ZSTD_INCLUDE_FILES
    include/fse.h
    include/huf.h
    include/xxhash.h
    include/zstd.h
)

file(GLOB ZSTD_SOURCE_FILES
    src/*.c
    src/*.h
)

include_directories(include src)


################################################################################
# Targets

if (WIN32 AND BUILD_SHARED_LIBS)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
    if (WIN32)
        set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
        include(GenerateExportHeader)
    endif ()
endif ()

add_library(zstd ${ZSTD_SOURCE_FILES})
set_target_properties(zstd PROPERTIES PUBLIC_HEADER include/zstd.h)
target_include_directories(zstd PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
    "$<INSTALL_INTERFACE:include>"
)

include(GNUInstallDirs)

install(TARGETS zstd
    EXPORT zstd-target
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
