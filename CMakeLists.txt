cmake_minimum_required(VERSION 3.1.0)

project(QtBreakpadDemo)
set(VERSION 1.0.0)
ADD_DEFINITIONS(-DUNICODE)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# configure file
set(APP_DISPLAY_NAME "Kandao QtBreakpad Demo")
configure_file (
    "${PROJECT_SOURCE_DIR}/app/app_version.h.in"
    "${PROJECT_SOURCE_DIR}/app/app_version.h"
)

# install prefix
set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_BINARY_DIR}/install")

# qt5 required
find_package(Qt5Widgets CONFIG REQUIRED)
find_package(Qt5Network CONFIG REQUIRED)

# build crash Handler
add_subdirectory (qtcrashhandler)

# qtbreakpad handler
include_directories ("${PROJECT_SOURCE_DIR}/qtbreakpad/")

# external breakpad repo
set(BREAKPAD_SOURCE_DIR "${PROJECT_SOURCE_DIR}/external/breakpad")
include_directories ("${BREAKPAD_SOURCE_DIR}/src")

# source copied from qtbreakpad.pri
set(BREAKPAD_SRCS 
    "${BREAKPAD_SOURCE_DIR}/src/common/string_conversion.cc"
    "${BREAKPAD_SOURCE_DIR}/src/common/convert_UTF.c"
    "${BREAKPAD_SOURCE_DIR}/src/common/md5.cc"
)

if (WIN32)
    set(BREAKPAD_SRCS ${BREAKPAD_SRCS}
        "${BREAKPAD_SOURCE_DIR}/src/common/windows/guid_string.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/windows/handler/exception_handler.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/windows/crash_generation/minidump_generator.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/windows/crash_generation/client_info.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/windows/crash_generation/crash_generation_client.cc"
    )
endif(WIN32)

if (LINUX)
    set(BREAKPAD_SRCS ${BREAKPAD_SRCS}
        "${BREAKPAD_SOURCE_DIR}/src/client/minidump_file_writer.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/log/log.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/handler/exception_handler.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/handler/minidump_descriptor.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/linux/guid_creator.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/dump_writer_common/thread_info.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/dump_writer_common/ucontext_reader.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/minidump_writer/linux_dumper.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/minidump_writer/minidump_writer.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/minidump_writer/linux_ptrace_dumper.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/microdump_writer/microdump_writer.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/linux/file_id.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/linux/elfutils.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/linux/linux_libc_support.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/linux/memory_mapped_file.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/linux/safe_readlink.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/linux/crash_generation/crash_generation_client.cc"
    )
endif(LINUX)

if (APPLE)
    set(BREAKPAD_SRCS ${BREAKPAD_SRCS}
        "${BREAKPAD_SOURCE_DIR}/src/client/minidump_file_writer.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/mac/crash_generation/crash_generation_client.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/mac/handler/exception_handler.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/mac/handler/minidump_generator.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/mac/handler/breakpad_nlist_64.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/mac/handler/dynamic_images.cc"
        "${BREAKPAD_SOURCE_DIR}/src/client/mac/handler/protected_memory_allocator.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/bootstrap_compat.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/file_id.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/macho_id.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/macho_reader.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/macho_utilities.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/macho_walker.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/string_utilities.cc"
        "${BREAKPAD_SOURCE_DIR}/src/common/mac/MachIPC.mm"
    )
endif(APPLE)

# sources
file(GLOB SOURCES
    "${PROJECT_SOURCE_DIR}/testapp/main.cpp" 
    "${PROJECT_SOURCE_DIR}/qtbreakpad/*.cpp" 
)

# translate
# file(GLOB TS_FILES
#     "./*.ts"
# )
# qt5_create_translation(QM_FILES RawPlus ${SOURCES} ${TS_FILES})
# if(APPLE)
#     set_source_files_properties( 
#         ${QM_FILES} 
#         PROPERTIES 
#         MACOSX_PACKAGE_LOCATION "MacOS" 
#     )
# endif(APPLE)

# app information
set(COMPANY "Kandao")
set(IDENTIFIER "com.kandao.qtbreakpaddemo")

# target on APPLE
if(APPLE)
    set(MACOSX_BUNDLE_INFO_STRING "Kandao QtBreakpad Demo") 
    set(MACOSX_BUNDLE_BUNDLE_VERSION ${VERSION}) 
    set(MACOSX_BUNDLE_ICON_FILE "QtBreakpadDemo.icns")
    set(MACOSX_BUNDLE_GUI_IDENTIFIER "com.kandao.qtbreakpaddemo") 
    set(MACOSX_BUNDLE_BUNDLE_NAME "Kandao QtBreakpad Demo") 

    # # icns
    # set(ICON_FILE Resources/QtBreakpadDemo.icns)
    # set_source_files_properties(${ICON_FILE} 
    #     PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    # framework
    find_library(corefoundation_lib CoreFoundation)
    find_library(cfnetwork_lib CFNetwork)
    set(FRAMEWORKS
        ${cfnetwork_lib}
        ${corefoundation_lib})

    # target
    add_executable(QtBreakpadDemo ${SOURCES} ${BREAKPAD_SRCS} ${ICON_FILE} ${QM_FILES})
endif(APPLE)

# target on WINDOWS
if(WIN32)
    # set(WIN_RC QtBreakpadDemo.rc)
    add_executable(QtBreakpadDemo ${SOURCES} ${BREAKPAD_SRCS} ${WIN_RC} ${QM_FILES})
endif(WIN32)

# target on LINUX
if(LINUX)
    # set(WIN_RC QtBreakpadDemo.rc)
    add_executable(QtBreakpadDemo ${SOURCES} ${BREAKPAD_SRCS} ${WIN_RC} ${QM_FILES})
endif(LINUX)

target_link_libraries(QtBreakpadDemo Qt5::Widgets Qt5::Network ${FRAMEWORKS})

# configure install
install (TARGETS QtBreakpadDemo DESTINATION Demo)
