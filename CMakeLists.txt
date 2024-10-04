cmake_minimum_required(VERSION 3.10)

# Установка имени проекта и версии
project(PrintIPTests VERSION 0.1.0)

# Найти Boost для тестирования
find_package(Boost REQUIRED COMPONENTS unit_test_framework)

# Добавление исполняемых файлов и библиотек
add_executable(test_ip_filter test_ip_filter.cpp) # Исполняемый файл для тестов


# Настройки для тестов
target_include_directories(test_ip_filter
    PRIVATE ${Boost_INCLUDE_DIRS}
)
target_link_libraries(test_ip_filter
    PRIVATE ${Boost_LIBRARIES}
    helloworld
)


# Настройки компилятора в зависимости от платформы
if (MSVC)
    target_compile_options(test_ip_filter PRIVATE /W4)
else ()
    target_compile_options(test_ip_filter PRIVATE -Wall -Wextra -pedantic -Werror)
endif()

# Установка для macOS и Linux (если необходимо)
if (APPLE)
    set(CPACK_GENERATOR "Bundle")
elseif (UNIX)
    set(CPACK_GENERATOR "DEB")
endif()

set(CPACK_PACKAGE_CONTACT example@example.com)

include(CPack)

# Настройка тестов
enable_testing()
add_test(NAME test_version COMMAND test_ip_filter) # Добавляем тесты

message("Configured PROJECT_VERSION: ${PROJECT_VERSION}")
