# Hello World CMake Recipe

Prerequisites:

* You have created a new layer. (In the example I called it meta-first).
* The mentioned layer is appended to your build/conf/layers.bbconf.

Your folder structure for your new layer should look similar to this:

```console
joao@joao-ThinkPad-T470p:~/yocto/poky$ tree meta-first/
meta-first/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
└── recipes-hello
    ├── hello
    │   ├── files
    │   │   ├── helloworld-2.c
    │   │   └── helloworld.c
    │   ├── hello_0.1.bb
    │   └── hello_0.2.bb
    └── hellocmake
        ├── files
        │   ├── CMakeLists.txt
        │   └── hellocmake.cpp
        └── hellocmake.bb

```

The file hellocmake.cpp contains the code that outputs "Hello World".  
The CMakeLists contains the following
```
cmake_minimum_required(VERSION 1.9)
project (hellocmake)
add_executable(hellocmake hellocmake.cpp)
install(TARGETS hellocmake RUNTIME DESTINATION bin)

```

The Yocto build system contains classes to support building CMake packages. To use CMake in a recipe you need to inherit the CMake class.
Generally the CMake build system knows how to install the software so a overwrite over do_install is not necessary.

```
SUMMARY = "Simple Hello World Cmake application"
SECTION = "examples"
LICENSE = "MIT"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/MIT;md5=0835ade698e0bcf8506ecda2f7b4f302"

SRC_URI = "\
            file://CMakeLists.txt \
            file://hellocmake.cpp \
        "

S = "${WORKDIR}"

inherit cmake

EXTRA_OECMAKE = ""
```

