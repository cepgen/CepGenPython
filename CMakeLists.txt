cmake_minimum_required(VERSION 3.20)
project(CepGenPython)

set(CEPGEN_PATH ${CEPGEN_PATH} $ENV{CEPGEN_PATH})
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR} ${CMAKE_SOURCE_DIR}/cmake ${CEPGEN_PATH}/cmake)

find_package(CepGen REQUIRED)
find_package(Boost REQUIRED COMPONENTS system python)
find_package(Python REQUIRED COMPONENTS Interpreter Development)

file(GLOB sources src/*.cpp)
add_library(pycepgen SHARED ${sources})
target_link_libraries(pycepgen ${CepGen_LIBRARIES} ${Boost_LIBRARIES})
target_include_directories(pycepgen
    PUBLIC ${CMAKE_SOURCE_DIR} ${CepGen_INCLUDE_DIRS} ${Python_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})
set_target_properties(pycepgen PROPERTIES PREFIX "")
install(TARGETS pycepgen DESTINATION "${Python_SITEARCH}")
