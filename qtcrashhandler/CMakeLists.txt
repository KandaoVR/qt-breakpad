cmake_minimum_required(VERSION 3.1.0)

project(QtCrashHandler)
set(VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

# configure file
set(APP_DISPLAY_NAME "Kandao QtBreakpad Demo")
configure_file (
    "${PROJECT_SOURCE_DIR}/../app/app_version.h.in"
    "${PROJECT_SOURCE_DIR}/../app/app_version.h"
)
include_directories ("${PROJECT_SOURCE_DIR}/../")

# qt5 required
find_package(Qt5Widgets CONFIG REQUIRED)
find_package(Qt5Network CONFIG REQUIRED)

# sources
file(GLOB SOURCES
    "${PROJECT_SOURCE_DIR}/*.cpp" 
    "${PROJECT_SOURCE_DIR}/*.ui"
    "${PROJECT_SOURCE_DIR}/*.qrc"
)

# translate
# file(GLOB TS_FILES
#     "./*.ts"
# )
# qt5_create_translation(QM_FILES QtCrashHandler ${SOURCES} ${TS_FILES})
# if(APPLE)
#     set_source_files_properties( 
#         ${QM_FILES} 
#         PROPERTIES 
#         MACOSX_PACKAGE_LOCATION "MacOS" 
#     )
# endif(APPLE)

# app information
set(COMPANY "Kandao")
set(IDENTIFIER "com.kandao.qtcrashhandler")

# target on APPLE
if(APPLE)
    set(MACOSX_BUNDLE_INFO_STRING "Kandao Crash Handler") 
    set(MACOSX_BUNDLE_BUNDLE_VERSION ${VERSION}) 
    set(MACOSX_BUNDLE_ICON_FILE "CrashHandler.icns")
    set(MACOSX_BUNDLE_GUI_IDENTIFIER "com.kandao.qtcrashhandler") 
    set(MACOSX_BUNDLE_BUNDLE_NAME "Kandao Crash Handler") 

    # # icns
    # set(ICON_FILE Resources/CrashHandler.icns)
    # set_source_files_properties(${ICON_FILE} 
    #     PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    # target
    add_executable(QtCrashHandler ${SOURCES} ${ICON_FILE} ${QM_FILES})
endif(APPLE)

# target on WINDOWS
if(WIN32)
    # set(WIN_RC CrashHandler.rc)
    add_executable(QtCrashHandler ${SOURCES} ${WIN_RC} ${QM_FILES})
endif(WIN32)

# target on LINUX
if(LINUX)
    # set(WIN_RC CrashHandler.rc)
    add_executable(QtCrashHandler ${SOURCES} ${WIN_RC} ${QM_FILES})
endif(LINUX)

target_link_libraries(QtCrashHandler Qt5::Widgets Qt5::Network ${FRAMEWORKS})

# configure install
install (TARGETS QtCrashHandler DESTINATION Demo)

# for vs debug
if (WIN32)
    add_custom_command(TARGET QtCrashHandler POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy $<TARGET_FILE:QtCrashHandler> ${PROJECT_BINARY_DIR}/../$(Configuration)
    )
endif(WIN32)
