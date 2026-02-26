cmake_minimum_required(VERSION 3.20)
project(AdminDashboard VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Options
option(BUILD_TESTS     "Build unit tests"    ON)
option(ENABLE_SSL      "Enable HTTPS"        OFF)
option(ENABLE_METRICS  "Enable metrics"      ON)

# Dependencies (via vcpkg)
find_package(nlohmann_json REQUIRED)
find_package(spdlog        REQUIRED)
find_package(Crow          REQUIRED)
find_package(OpenSSL       OPTIONAL_COMPONENTS)

# Source files
file(GLOB_RECURSE SOURCES "src/*.cpp")
file(GLOB_RECURSE HEADERS "src/*.h" "include/*.h")

add_executable(${PROJECT_NAME} ${SOURCES})

target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    nlohmann_json::nlohmann_json
    spdlog::spdlog
    Crow::Crow
)

if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()
