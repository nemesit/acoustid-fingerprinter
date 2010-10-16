cmake_minimum_required(VERSION 2.6)

project(fpsubmit)

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules)

find_package(Qt4 COMPONENTS QtCore QtGui QtNetwork QtConcurrent REQUIRED)
find_package(FFmpeg REQUIRED)
find_package(Taglib REQUIRED)
find_package(Chromaprint REQUIRED)

include(${QT_USE_FILE})

set(fpsubmit_HEADERS
	checkabledirmodel.h
	progressdialog.h
	fingerprinter.h
	mainwindow.h
)
set(fpsubmit_SOURCES
	checkabledirmodel.cpp
	progressdialog.cpp
	fingerprinter.cpp
	fingerprintcalculator.cpp
	tagreader.cpp
	mainwindow.cpp
	decoder.cpp
	main.cpp
)
#set(fpsubmit_UIS fpsubmit.ui)
#set(fpsubmit_RESOURCES fpsubmit.qrc)

qt4_wrap_cpp(fpsubmit_MOC ${fpsubmit_HEADERS})
qt4_wrap_ui(fpsubmit_UIS_H ${fpsubmit_UIS})
qt4_add_resources(fpsubmit_RESOURCES_CPP ${fpsubmit_RESOURCES})

add_definitions(
	-D__STDC_LIMIT_MACROS
	-D__STDC_CONSTANT_MACROS
)

include_directories(
	${CMAKE_CURRENT_BINARY_DIR}
	${FFMPEG_LIBAVCODEC_INCLUDE_DIRS}
	${FFMPEG_LIBAVFORMAT_INCLUDE_DIRS}
	${FFMPEG_LIBAVUTIL_INCLUDE_DIRS}
	${TAGLIB_INCLUDES}
	${CHROMAPRINT_INCLUDE_DIR}
)

add_executable(fpsubmit
    ${fpsubmit_SOURCES}
    ${fpsubmit_MOC}
    ${fpsubmit_UIS_H}
    ${fpsubmit_RESOURCES_CPP}
)

target_link_libraries(fpsubmit
	${QT_LIBRARIES}
	${FFMPEG_LIBAVFORMAT_LIBRARIES}
	${FFMPEG_LIBAVCODEC_LIBRARIES}
	${FFMPEG_LIBAVUTIL_LIBRARIES}
	${TAGLIB_LIBRARIES}
	${CHROMAPRINT_LIBRARIES}
)
