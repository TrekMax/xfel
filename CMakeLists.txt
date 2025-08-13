cmake_minimum_required(VERSION 3.20)
project(xfel C)


find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBUSB REQUIRED libusb-1.0)

include_directories(
    ${LIBUSB_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/chips
)

file(GLOB XFEL_SRCS
    "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/chips/*.c"
)

add_executable(xfel ${XFEL_SRCS})

link_directories(${LIBUSB_LIBRARY_DIRS})
target_link_libraries(xfel ${LIBUSB_LIBRARIES})

install(TARGETS xfel DESTINATION bin)
install(FILES 99-xfel.rules DESTINATION /etc/udev/rules.d)
install(FILES LICENSE DESTINATION share/licenses/xfel)
