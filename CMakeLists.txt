cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}")

PROJECT(realsense_scanner)
# directive to print detail build log
SET(CMAKE_VERBOSE_MAKEFILE ON)

# set the output directory for bin during building(make)
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

# set the output directory for libs during building(make)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# 可以分离dll和lib
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)

link_directories(${PROJECT_SOURCE_DIR}/lib)

include_directories(C:\Qt\Qt5.8.0\5.8\msvc2015_64\include\QtANGLE)
include_directories(C:\Qt\Qt5.8.0\5.8\msvc2015_64\include)
link_directories(C:\Qt\Qt5.8.0\5.8\msvc2015_64\lib)

################################################################################

set(Qt5Widgets_DIR C:\Qt\Qt5.8.0\5.8\msvc2015_64\lib\cmake\Qt5Widgets)
find_package(Qt5Widgets)

# OpenGL
find_package(GLUT REQUIRED)
find_package(OpenGL REQUIRED)

#OpenNI
find_package(OpenNI2 REQUIRED)

find_package(PCL 1.6.0 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

################################################################################

set(INCS
    "include/pcl/apps/in_hand_scanner/boost.h"
    "include/pcl/apps/in_hand_scanner/common_types.h"
    "include/pcl/apps/in_hand_scanner/eigen.h"
    "include/pcl/apps/in_hand_scanner/icp.h"
    "include/pcl/apps/in_hand_scanner/input_data_processing.h"
    "include/pcl/apps/in_hand_scanner/integration.h"
    "include/pcl/apps/in_hand_scanner/mesh_processing.h"
    "include/pcl/apps/in_hand_scanner/utils.h"
    "include/pcl/apps/in_hand_scanner/visibility_confidence.h"
)

set(IMPL_INCS
    "include/pcl/apps/in_hand_scanner/impl/common_types.hpp"
)

set(SRCS
    src/help_window.cpp
    src/main.cpp
    src/main_window.cpp
    src/icp.cpp
    src/in_hand_scanner.cpp
    src/input_data_processing.cpp
    src/integration.cpp
    src/mesh_processing.cpp
    src/opengl_viewer.cpp
    src/visibility_confidence.cpp
)

# Qt
set(MOC_IN_HAND_SCANNER_INC     "include/pcl/apps/in_hand_scanner/in_hand_scanner.h")
set(MOC_OPENGL_VIEWER_INC       "include/pcl/apps/in_hand_scanner/opengl_viewer.h")
set(MOC_OFFLINE_INTEGRATION_INC "include/pcl/apps/in_hand_scanner/offline_integration.h")

set(MOC_MAIN_WINDOW_INC "include/pcl/apps/in_hand_scanner/main_window.h")
set(MOC_HELP_WINDOW_INC "include/pcl/apps/in_hand_scanner/help_window.h")
set(UI_MAIN_WINDOW      src/main_window.ui)
set(UI_HELP_WINDOW      src/help_window.ui)

# Offline integration
set(OI_INCS
    "include/pcl/apps/in_hand_scanner/integration.h"
    "include/pcl/apps/in_hand_scanner/visibility_confidence.h"
)

set(OI_SRCS
    src/main_offline_integration.cpp
    src/integration.cpp
    src/offline_integration.cpp
    src/opengl_viewer.cpp
    src/visibility_confidence.cpp
)
################################################################################
# Qt
# http://qtnode.net/wiki/Qt4_with_cmake
# http://qt-project.org/quarterly/view/using_cmake_to_build_qt_projects
set(QT_USE_QTOPENGL TRUE)
include("${QT_USE_FILE}")
qt5_wrap_cpp(MOC_IN_HAND_SCANNER_SRC     "${MOC_IN_HAND_SCANNER_INC}")
qt5_wrap_cpp(MOC_OPENGL_VIEWER_SRC       "${MOC_OPENGL_VIEWER_INC}" OPTIONS -DBOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION)
qt5_wrap_cpp(MOC_OFFLINE_INTEGRATION_SRC "${MOC_OFFLINE_INTEGRATION_INC}" OPTIONS -DBOOST_NO_TEMPLATE_PARTIAL_SPECIALIZATION)

qt5_wrap_cpp(MOC_MAIN_WINDOW_SRC "${MOC_MAIN_WINDOW_INC}")
qt5_wrap_cpp(MOC_HELP_WINDOW_SRC "${MOC_HELP_WINDOW_INC}")
qt5_wrap_ui(UI_MAIN_WINDOW_INC   "${UI_MAIN_WINDOW}")
qt5_wrap_ui(UI_HELP_WINDOW_INC   "${UI_HELP_WINDOW}")

list(APPEND ADDITIONAL_LIBS ${QT_LIBRARIES})
include_directories("${CMAKE_CURRENT_BINARY_DIR}" "${CMAKE_CURRENT_SOURCE_DIR}/include")

# In-hand scanner
list(APPEND INCS "${MOC_IN_HAND_SCANNER_INC}" "${MOC_OPENGL_VIEWER_INC}" "${MOC_MAIN_WINDOW_INC}" "${MOC_HELP_WINDOW_INC}" "${UI_MAIN_WINDOW_INC}" "${UI_HELP_WINDOW_INC}")
list(APPEND SRCS "${MOC_IN_HAND_SCANNER_SRC}" "${MOC_OPENGL_VIEWER_SRC}" "${MOC_MAIN_WINDOW_SRC}" "${MOC_HELP_WINDOW_SRC}")

set(EXE_NAME "pcl_in_hand_scanner")
ADD_EXECUTABLE(${EXE_NAME} ${SRCS} ${INCS} ${IMPL_INCS})
target_link_libraries("${EXE_NAME}" ${PCL_LIBRARIES} ${OPENGL_LIBRARIES} ${QT_LIBRARIES} 
RSGrabber.lib 
Qt53DCore.lib
Qt53DExtras.lib
Qt53DInput.lib
Qt53DLogic.lib
Qt53DQuick.lib
Qt53DQuickExtras.lib
Qt53DQuickInput.lib
Qt53DQuickRender.lib
Qt53DRender.lib
Qt5AccessibilitySupport.lib
Qt5AxBase.lib
Qt5AxContainer.lib
Qt5AxServer.lib
Qt5Bluetooth.lib
Qt5Bootstrap.lib
Qt5Charts.lib
Qt5CLucene.lib
Qt5Concurrent.lib
Qt5Core.lib
Qt5DataVisualization.lib
Qt5DBus.lib
Qt5Designer.lib
Qt5DesignerComponents.lib
Qt5DeviceDiscoverySupport.lib
Qt5EglSupport.lib
Qt5EventDispatcherSupport.lib
Qt5FbSupport.lib
Qt5FontDatabaseSupport.lib
Qt5Gui.lib
Qt5Help.lib
Qt5Location.lib
Qt5Multimedia.lib
Qt5MultimediaQuick_p.lib
Qt5MultimediaWidgets.lib
Qt5Network.lib
Qt5NetworkAuth.lib
Qt5Nfc.lib
Qt5OpenGL.lib
Qt5OpenGLExtensions.lib
Qt5PacketProtocol.lib
Qt5PlatformCompositorSupport.lib
Qt5Positioning.lib
Qt5PrintSupport.lib
Qt5Purchasing.lib
Qt5Qml.lib
Qt5QmlDebug.lib
Qt5QmlDevTools.lib
Qt5Quick.lib
Qt5QuickControls2.lib
Qt5QuickParticles.lib
Qt5QuickTemplates2.lib
Qt5QuickTest.lib
Qt5QuickWidgets.lib
Qt5Script.lib
Qt5ScriptTools.lib
Qt5Scxml.lib
Qt5Sensors.lib
Qt5SerialBus.lib
Qt5SerialPort.lib
Qt5Sql.lib
Qt5Svg.lib
Qt5Test.lib
Qt5TextToSpeech.lib
Qt5ThemeSupport.lib
Qt5UiTools.lib
Qt5WebChannel.lib
Qt5WebEngine.lib
Qt5WebEngineCore.lib
Qt5WebEngineWidgets.lib
Qt5WebSockets.lib
Qt5WebView.lib
Qt5Widgets.lib
Qt5WinExtras.lib
Qt5Xml.lib
Qt5XmlPatterns.lib
qtfreetype.lib
qtlibpng.lib
qtmain.lib)