cmake_minimum_required(VERSION 2.8.11)

if(CMAKE_BUILD_TYPE STREQUAL Release)
  set(CMAKE_BUILD_TYPE Release)
  set(CMAKE_CXX_FILES "-O2 -g -Wall")
else()
  set(CMAKE_BUILD_TYPE Debug)
  set(CMAKE_CXX_FILES "-O0 -g -Wall")
endif()

project(albert)

find_package(Qt5Widgets REQUIRED)
find_package(Qt5X11Extras REQUIRED)
find_library(muparser REQUIRED)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

include_directories(
  src/
  src/ui/
  src/services/
)

set(SRCS
  src/main.cpp
  src/albertengine.cpp
  src/globalhotkey.cpp
  src/services/fileindex/fileindex.cpp
  src/services/bookmarkindex/bookmarkindex.cpp
  src/services/calculator/calculator.cpp
  src/services/websearch/websearch.cpp
  src/services/fileindex/fileitem.cpp
  src/services/calculator/calculatoritem.cpp
  src/services/bookmarkindex/bookmarkitem.cpp
  src/services/websearch/websearchitem.cpp
  src/services/appindex/appindexwidget.cpp
  src/services/appindex/appindex.cpp
  src/services/appindex/appitem.cpp
  src/services/bookmarkindex/bookmarkindexwidget.cpp
  src/services/calculator/calculatorwidget.cpp
  src/services/fileindex/fileindexwidget.cpp
  src/services/websearch/websearchwidget.cpp
  src/services/indexservice.cpp
  src/ui/albert.cpp
  src/ui/proposallistdelegate.cpp
  src/ui/proposallistview.cpp
  src/ui/inputline.cpp
  src/ui/settingsbutton.cpp
  src/ui/settingsdialog.cpp
  src/ui/hotkeywidget.cpp
)

qt5_wrap_ui(UIS
  src/ui/settingsdialog.ui
  src/services/appindex/appindexwidget.ui
  src/services/bookmarkindex/bookmarkindexwidget.ui
  src/services/calculator/calculatorwidget.ui
  src/services/fileindex/fileindexwidget.ui
  src/services/websearch/websearchwidget.ui
)

#linux
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  set(SRCS ${SRCS} src/globalhotkey_x11.cpp)
  add_executable(albert ${SRCS} ${UIS})
  target_link_libraries(albert Qt5::Widgets Qt5::X11Extras muparser X11)
  install(TARGETS albert RUNTIME DESTINATION bin)
endif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

#win
if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  set(SRCS ${SRCS} src/globalhotkey_win.cpp)
endif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")