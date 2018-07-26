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

############################################################
# build crash Handler
add_subdirectory (qtcrashhandler)

# build external breakpad repo
set(BREAKPAD_SOURCE_DIR "${PROJECT_SOURCE_DIR}/external/breakpad")
include_directories ("${BREAKPAD_SOURCE_DIR}/src")
add_subdirectory (external)

############################################################
# qtbreakpad handler
include_directories ("${PROJECT_SOURCE_DIR}/qtbreakpad/")

# sources
file(GLOB SOURCES
    "${PROJECT_SOURCE_DIR}/testapp/main.cpp" 
    "${PROJECT_SOURCE_DIR}/qtbreakpad/*.cpp" 
)

############################################################
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

############################################################
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

# linking
target_link_libraries(QtBreakpadDemo Qt5::Widgets Qt5::Network ${FRAMEWORKS} libbreakpad)

# configure install
install (TARGETS QtBreakpadDemo DESTINATION Demo)
