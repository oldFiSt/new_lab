cmake_minimum_required(VERSION 3.10)

# Настройка версии с использованием переменной GITHUB_RUN_NUMBER
if(DEFINED ENV{GITHUB_RUN_NUMBER})
    set(PATCH_VERSION $ENV{GITHUB_RUN_NUMBER})
else()
    set(PATCH_VERSION "1")
endif()

set(PROJECT_VERSION "0.0.${PATCH_VERSION}")
project(PrintIPTests VERSION ${PROJECT_VERSION})

# Найти Boost для тестирования
find_package(Boost REQUIRED COMPONENTS unit_test_framework)

# Добавление исполняемых файлов и библиотек
add_executable(test_ip_filter test_ip_filter.cpp) # Исполняемый файл для тестов

# Настройки стандартов C++
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)


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
add_test(NAME test_ip_filter COMMAND test_ip_filter) # Добавляем тесты

message("Configured PATCH_VERSION: ${PATCH_VERSION}")
message("Configured PROJECT_VERSION: ${PROJECT_VERSION}")
