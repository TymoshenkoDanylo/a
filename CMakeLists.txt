cmake_minimum_required(VERSION 3.10.2)
project(echobot)

# Установка стандарта C++
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
set(Boost_USE_MULTITHREADED ON)


# Подключение библиотеки TgBot-cpp
add_subdirectory(libs/tgbot-cpp)

# Подключение библиотеки SQLiteCpp
set(SQLITECPP_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/libs/sqlite/include")
set(SQLITECPP_LIB_DIR "${CMAKE_SOURCE_DIR}/libs/SQLiteCpp/build")

# Поиск необходимых библиотек
find_package(Threads REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(Boost COMPONENTS system REQUIRED)
find_package(CURL)
find_package(SQLite3 REQUIRED)  # Используем системную версию SQLite

# Указать пути к Boost, OpenSSL и SQLiteCpp
include_directories(${OPENSSL_INCLUDE_DIR} ${Boost_INCLUDE_DIR} ${SQLITECPP_INCLUDE_DIR})

# Проверка на наличие CURL
if (CURL_FOUND)
    include_directories(${CURL_INCLUDE_DIRS})
    add_definitions(-DHAVE_CURL)
endif()

# Добавить исполняемый файл
add_executable(echobot src/main.cpp)

# Линковка с библиотеками
target_link_libraries(echobot
    TgBot              # TgBot-cpp библиотека
    ${SQLITECPP_LIB_DIR}/libSQLiteCpp.a  # SQLiteCpp библиотека
    sqlite3            # Используем системную SQLite библиотеку
    ${CMAKE_THREAD_LIBS_INIT} 
    ${OPENSSL_LIBRARIES} 
    ${Boost_LIBRARIES} 
    ${CURL_LIBRARIES}
)


