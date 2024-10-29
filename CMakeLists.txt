cmake_minimum_required(VERSION 3.10)

project(hello_library)

############################################################
# Create a library
############################################################

find_package(fmt CONFIG REQUIRED)

add_library(hello_library STATIC 
    "src/helloworld.cpp"
)
add_library(hello::library ALIAS hello_library)

target_include_directories(hello_library
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/include
)

target_link_libraries(hello_library PRIVATE fmt::fmt)


############################################################
# Create an executable
############################################################

add_executable(HelloWorld "src/main.cpp")
target_link_libraries(HelloWorld PRIVATE hello::library)

############################################################
# Create an test executable
############################################################

find_package(Catch2 3 REQUIRED)
# These tests can use the Catch2-provided main
add_executable(tests "test/tests.cpp")
target_link_libraries(tests PRIVATE Catch2::Catch2WithMain)
