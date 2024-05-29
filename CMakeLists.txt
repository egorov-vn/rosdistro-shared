cmake_minimum_required(VERSION 3.8)
project(shared)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Wno-dev)
endif()

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

set(target shared)

add_compile_definitions(APPLICATION_NAME="ROS Demo")
add_compile_definitions(VERSION_PROJECT="0")
add_compile_definitions(VERSION_PROJECT_MAJOR=1)
add_compile_definitions(VERSION_PROJECT_MINOR=0)
add_compile_definitions(VERSION_PROJECT_PATCH=0)
add_compile_definitions(QDATASTREAM_VERSION=QDataStream::Qt_5_12)
add_compile_definitions(PPROTO_VERSION_LOW=0)
add_compile_definitions(PPROTO_VERSION_HIGH=0)
add_compile_definitions(PPROTO_JSON_SERIALIZE)
add_compile_definitions(PPROTO_QBINARY_SERIALIZE)
add_compile_definitions(PPROTO_UDP_SIGNATURE="RSDO")
add_compile_definitions(DEFAULT_PORT=54000)
add_compile_definitions(CONFIG_DIR="/etc/schema-service")
add_compile_definitions(VAROPT_DIR="/var/opt/schema-service")
add_compile_definitions(QT_CORE_LIB=1)
add_definitions(-DQT_CORE_LIB)

find_package(ament_cmake REQUIRED)
find_package(Qt5 COMPONENTS Core Gui Widgets Network REQUIRED)
find_package(yaml-cpp REQUIRED)

set(CMAKE_AUTOMOC ON)
set(AM_MOC_COMPILE_DEFINITIONS ";QT4;QT4;QT_NO_DEBUG;QT_GUI_LIB;QT_CORE_LIB")

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}"
    shared
)

add_library(shared SHARED
    shared/config/appl_conf.cpp
    shared/config/appl_conf.h
    shared/config/logger_conf.cpp
    shared/config/logger_conf.h
    shared/config/yaml_config.cpp
    shared/config/yaml_config.h
    shared/logger/config.cpp
    shared/logger/config.h
    shared/logger/format.h
    shared/logger/logger.cpp
    shared/logger/logger.h
    shared/network/logger_operators.h
    shared/network/logger_operators.cpp
    shared/qt/network/interfaces.cpp
    shared/qt/network/interfaces.h
    shared/qt/expand_string.h
    shared/qt/logger_operators.cpp
    shared/qt/logger_operators.h
    shared/qt/qthreadex.cpp
    shared/qt/qthreadex.h
    shared/qt/quuidex.h
    shared/qt/stream_init.h
    shared/qt/version_number.cpp
    shared/qt/version_number.h
    shared/thread/thread_base.cpp
    shared/thread/thread_base.h
    shared/thread/thread_pool.cpp
    shared/thread/thread_pool.h
    shared/thread/thread_utils.cpp
    shared/thread/thread_utils.h
    shared/break_point.h
    shared/clife_alloc.h
    shared/clife_base.h
    shared/clife_ptr.h
    shared/defmac.h
    shared/container_ptr.h
    shared/list.h
    shared/processing_time.h
    shared/prog_abort.cpp
    shared/prog_abort.h
    shared/safe_singleton.h
    shared/simple_ptr.h
    shared/simple_signal.h
    shared/simple_timer.h
    shared/spin_locker.h
    shared/steady_timer.h
    shared/type_name.h
    shared/utils.cpp
    shared/utils.h
)

ament_export_targets(shared-targets HAS_LIBRARY_TARGET)

ament_export_libraries(shared)

ament_export_include_directories(shared)

target_link_libraries(shared Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Network)

target_compile_features(shared PUBLIC cxx_std_17)

install(
  DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
  DESTINATION include
)

install(
  TARGETS shared
  EXPORT shared-targets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include/shared
                       include/shared/shared
)

ament_package()
