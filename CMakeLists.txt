cmake_minimum_required(VERSION 3.10)

project(IFilterProject VERSION 0.1.0)

find_package(Boost REQUIRED COMPONENTS unit_test_framework)

add_library(ip_filter ip_filter.cpp)
add_executable(test_ip_filter test_ip_filter.cpp)

set_target_properties(ip_filter test_ip_filter PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(ip_filter PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")

target_include_directories(test_ip_filter PRIVATE ${Boost_INCLUDE_DIRS})
target_link_libraries(test_ip_filter PRIVATE ${Boost_LIBRARIES} ip_filter)

enable_testing()
add_test(NAME test_ip_filter COMMAND test_ip_filter)

# Копирование ресурсов (если необходимо)
set(RESOURCE_FILES 
    resources/ic.icns 
    # resources/info.plist  # Закомментируйте эту строку, если файл не нужен
)

foreach(RESOURCE ${RESOURCE_FILES})
    configure_file(${RESOURCE} ${CMAKE_BINARY_DIR}/${RESOURCE} COPYONLY)
endforeach()

set(CPACK_PACKAGE_NAME "ip_filter")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_GENERATOR "DEB")  # Или "Bundle" для macOS

message("Configured PROJECT_VERSION: ${PROJECT_VERSION}")
