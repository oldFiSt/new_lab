cmake_minimum_required(VERSION 3.10)

# Установка имени проекта и версии
project(IFilterProject VERSION 0.1.0)

# Найти Boost для тестирования
find_package(Boost REQUIRED COMPONENTS unit_test_framework)

# Добавление исполняемых файлов и библиотек
add_library(ip_filter ip_filter.cpp)          # Библиотека с вашими функциями
add_executable(test_ip_filter test_ip_filter.cpp) # Исполняемый файл для тестов

# Настройки стандартов C++
set_target_properties(ip_filter test_ip_filter PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

# Установка директории для заголовочных файлов
target_include_directories(ip_filter
    PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}"
)

# Настройки для тестов
target_include_directories(test_ip_filter
    PRIVATE ${Boost_INCLUDE_DIRS}
)
target_link_libraries(test_ip_filter
    PRIVATE ${Boost_LIBRARIES}
    ip_filter
)

# Линковка исполняемого файла с библиотекой
target_link_libraries(test_ip_filter PRIVATE ip_filter)

# Установка ресурсов (иконки и plist)
set(RESOURCE_FILES 
    resources/ic.icns 
    resources/info.plist
)

# Копирование ресурсов в директорию сборки (если необходимо)
foreach(RESOURCE ${RESOURCE_FILES})
    configure_file(${RESOURCE} ${CMAKE_BINARY_DIR}/${RESOURCE} COPYONLY)
endforeach()

# Настройки компилятора в зависимости от платформы
if (MSVC)
    target_compile_options(ip_filter PRIVATE /W4)
    target_compile_options(test_ip_filter PRIVATE /W4)
else ()
    target_compile_options(ip_filter PRIVATE -Wall -Wextra -pedantic -Werror)
    target_compile_options(test_ip_filter PRIVATE -Wall -Wextra -pedantic -Werror)
endif()

# Настройка тестов
enable_testing()
add_test(NAME test_ip_filter COMMAND test_ip_filter) # Добавляем тесты

message("Configured PROJECT_VERSION: ${PROJECT_VERSION}")
