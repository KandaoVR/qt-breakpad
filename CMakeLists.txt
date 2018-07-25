cmake_minimum_required(VERSION 3.1.0)

project(RawPlusCMake)
set(VERSION 1.0.7.3)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

find_package(Qt5Widgets CONFIG REQUIRED)
find_package(Qt5Network CONFIG REQUIRED)



# sources
file(GLOB RawPlus_SRCS
    "RawPlus/*.cpp" 
    "RawPlus/*.ui"
    "RawPlus/*.qrc"
    "RawPlus/RawPlus/*.cpp"
    "source/hdrplus/*.cpp"
    "source/dng_sdk/*.cpp"
    "source/utils/*.cpp"
)

# translate
file(GLOB TS_FILES
    "RawPlus/*.ts"
)
qt5_create_translation(QM_FILES RawPlus ${RawPlus_SRCS} ${TS_FILES})

if(WIN32)
    set(WIN_RC RawPlus/RawPlus.rc)
endif(WIN32)

# information
set(COMPANY "Kandao")
set(IDENTIFIER "com.kandao.rawplus")
if(APPLE)
    set(MACOSX_BUNDLE_INFO_STRING "Kandao Raw+ - Version ${VERSION}") 
    set(MACOSX_BUNDLE_BUNDLE_VERSION ${VERSION}) 
    set(MACOSX_BUNDLE_ICON_FILE "RawPlus.icns")
    set(MACOSX_BUNDLE_GUI_IDENTIFIER "com.kandao.rawplus") 
    set(MACOSX_BUNDLE_BUNDLE_NAME "Kandao Raw+") 

    set_source_files_properties( 
        ${QM_FILES} 
        PROPERTIES 
        MACOSX_PACKAGE_LOCATION "MacOS" 
    )

    # icns
    set(ICON_FILE RawPlus/Resources/RawPlus.icns)
    set_source_files_properties(${ICON_FILE} 
        PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")
endif(APPLE)

# target
add_executable(RawPlus MACOSX_BUNDLE ${RawPlus_SRCS} ${WIN_RC} ${ICON_FILE} ${QM_FILES})
target_link_libraries(RawPlus Qt5::Widgets Qt5::Network
    ${Boost_LIBRARIES} ${BOOST_SYSTEM} ${BOOST_FILESYSTEM} ${OpenCV_LIBS}
    ${RawPlus_LIB} ${DNG_LIB} ${JPEG_LIB}
    ${XMP_CORE} ${XMP_FILE} ${HALIDE_LIB}
    ${CARBON_LIBRARY}
)



