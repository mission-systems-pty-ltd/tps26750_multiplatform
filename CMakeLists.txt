cmake_minimum_required(VERSION 3.16)
project(tps26750
    VERSION 1.0.2
    LANGUAGES CXX
    DESCRIPTION "TPS26750 USB-PD controller driver, with a Linux i2c-dev backend"
)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(tps26750 STATIC
    src/tps26750.cpp
    src/tps26750_platform_i2c.cpp
)
add_library(tps26750::tps26750 ALIAS tps26750)

target_include_directories(tps26750 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/tps26750>
)
target_compile_features(tps26750 PUBLIC cxx_std_11)
target_compile_options(tps26750 PRIVATE -Wall -Wextra)

install(TARGETS tps26750 EXPORT tps26750Targets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
install(DIRECTORY include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/tps26750
)
install(EXPORT tps26750Targets
    FILE tps26750Config.cmake
    NAMESPACE tps26750::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/tps26750
)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/tps26750ConfigVersion.cmake
    COMPATIBILITY SameMajorVersion
)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/tps26750ConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/tps26750
)
