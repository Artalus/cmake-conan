# cmake-conan

[![Build status](https://ci.appveyor.com/api/projects/status/0y2994lfwcpw9232/branch/master?svg=true)](https://ci.appveyor.com/project/ConanCIintegration/cmake-conan/branch/master)

[![Build Status](https://travis-ci.org/conan-io/cmake-conan.svg?branch=master)](https://travis-ci.org/conan-io/cmake-conan)

CMake wrapper for the Conan C and C++ package manager.

**Testing required**: This tools is still experimental, please try and contribute, specially with the extraction of settings from cmake.


This cmake module allows to launch ``conan install`` from cmake.

It can be used in this way:


```cmake

cmake_minimum_required(VERSION 2.8)
project(myproject CXX)

# Download automatically, you can also just copy the conan.cmake file
if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
   message(STATUS "Downloading conan.cmake from https://github.com/memsharded/cmake-conan")
   file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/master/conan.cmake"
                 "${CMAKE_BINARY_DIR}/conan.cmake")
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)

conan_cmake_run(REQUIRES Hello/0.1@memsharded/testing
                BASIC_SETUP 
                BUILD missing)

add_executable(main main.cpp)
target_link_libraries(main ${CONAN_LIBS})
```

If you want to use targets, you could do:

```cmake
include(conan.cmake)
conan_cmake_run(REQUIRES Hello/0.1@memsharded/testing
                BASIC_SETUP CMAKE_TARGETS
                BUILD missing)

add_executable(main main.cpp)
target_link_libraries(main CONAN_PKG::Hello)
```


If you want to use your own ``conanfile.txt`` or ``conanfile.py`` instead of generating a temporary one, you could do:

```cmake
include(conan.cmake)
conan_cmake_run(CONANFILE conanfile.txt  # or relative build/conanfile.txt
                BASIC_SETUP CMAKE_TARGETS
                BUILD missing)
```

The resolution of the path will be relative to the root ``CMakeLists.txt`` file.

To use the [cmake_multi](http://docs.conan.io/en/latest/integrations/cmake.html#cmake-multi-configuration-environments) generator you just need to make sure ``CMAKE_BUILD_TYPE`` is empty and use a CMake generator that supports multi-configuration.

Please check the source code for other options and arguments.

Development, contributors
-----------------------------

There are some tests, you can run in python, with nosetests, for example:

```bash
$ nosetests . --nocapture
```
