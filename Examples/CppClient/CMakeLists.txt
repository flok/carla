cmake_minimum_required(VERSION 3.1.0)
project(cpp_client VERSION 0.1.0)

SET(INSTALLDIR "INSTALLDIR" CACHE STRING "INSTALLDIR")
SET(BINDIR "BINDIR" CACHE STRING "BINDIR")

set(CMAKE_BUILD_TYPE RELEASE)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED) # LINK AGAINST THREAD

#set(CMAKE_POSITION_INDEPENDENT_CODE ON) # USE -fPIC

set(CMAKE_CXX_FLAGS "-fPIC -O3 -DNDEBUG -Werror -Wall -Wextra")

if(MSVC)
add_definitions(/MP)
endif()



target_include_directories(cpp_client PUBLIC ${INSTALLDIR}\include )

add_executable(cpp_client main.cpp)
link_directories(${INSTALLDIR}\lib)
target_link_libraries(cpp_client PRIVATE Threads::Threads)
