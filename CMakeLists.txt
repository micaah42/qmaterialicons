cmake_minimum_required(VERSION 3.22)

project(qmaterialicons-qml LANGUAGES CXX VERSION 2.0)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror")

find_package(Qt6 REQUIRED COMPONENTS Core Quick)
qt_standard_project_setup(REQUIRES 6.8)

file(GLOB_RECURSE RESOURCES
    RELATIVE ${CMAKE_CURRENT_LIST_DIR}
    resources/*.ttf
)

file(GLOB_RECURSE QML_FILES
    RELATIVE ${CMAKE_CURRENT_LIST_DIR}
    *.qml
)

set(QML_SINGLETONS
    Icons.qml
)

set_source_files_properties(${QML_SINGLETONS}
    PROPERTIES QT_QML_SINGLETON_TYPE TRUE
)

add_library(${PROJECT_NAME} STATIC)
qt_add_qml_module(${PROJECT_NAME}
    QML_FILES ${QML_FILES}
    RESOURCES ${RESOURCES}
    URI qmaterialicons
)

set_target_properties(${PROJECT_NAME} PROPERTIES SOVERSION ${PROJECT_VERSION})
target_link_libraries(${PROJECT_NAME} PUBLIC
    Qt6::Quick
    Qt6::Core
)

include(GNUInstallDirs)
install(TARGETS ${PROJECT_NAME}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    BUNDLE DESTINATION .
)
