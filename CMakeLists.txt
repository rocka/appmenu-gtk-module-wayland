cmake_minimum_required(VERSION 3.31)

project(appmenu-gtk-module-wayland)

find_package(PkgConfig REQUIRED)
pkg_check_modules(GTK3 REQUIRED IMPORTED_TARGET gtk+-3.0)

add_subdirectory(libdbusmenu)

set(CMAKE_C_STANDARD 11)

set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(GENERATED_DIR "${CMAKE_CURRENT_SOURCE_DIR}/wayland/generated")

add_library(appmenu-gtk-module-wayland  SHARED
    "${SRC_DIR}/appmenu-gtk-module.c"
    "${SRC_DIR}/datastructs.c"
    "${SRC_DIR}/hijack.c"
    "${SRC_DIR}/support.c"
    "${SRC_DIR}/blacklist.c"
    "${SRC_DIR}/platform.c"
    "${GENERATED_DIR}/appmenu.c"
)

target_include_directories(appmenu-gtk-module-wayland PRIVATE "${GENERATED_DIR}")
target_link_libraries(appmenu-gtk-module-wayland PkgConfig::GTK3 dbusmenu-gtk)

set(TEST_DIR "${CMAKE_CURRENT_SOURCE_DIR}/tests")

add_executable(unity-gtk-menu-tester "${TEST_DIR}/demos/unity-gtk-menu-tester.c")
target_link_libraries(unity-gtk-menu-tester PkgConfig::GTK3)
