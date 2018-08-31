# Yocto Hello World Recipe

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
    └── hello
        ├── files
        │   └── helloworld.c
        └──hello_0.1.bb
```

The helloworld.c contains the code that outputs "Hello World".

Focusing on the recipe hello_0.1.bb it contains the following:

```
SUMMARY = "Simple Hello World application"
SECTION = "examples"
LICENSE = "MIT"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/MIT;md5=0835ade698e0bcf8506ecda2f7b4f302"

SRC_URI = "file://helloworld.c"

TARGET_CC_ARCH += "${LDFLAGS}" 

S = "${WORKDIR}"

do_compile() {
${CC} helloworld.c -o helloworld
}

do_install() {
install -d ${D}${bindir}
install -m 0755 helloworld ${D}${bindir}
}
```

Now that we have the recipe hello created, let's try to build it using bitbake and verify if the output packages are generated.

```console
joao@joao-ThinkPad-T470p:~/yocto/poky$ bitbake hello
Loading cache: 100% |###############################################################################################################################################################################| Time: 0:00:00
Loaded 3081 entries from dependency cache.
NOTE: Resolving any missing task queue dependencies

Build Configuration:
BB_VERSION           = "1.38.0"
BUILD_SYS            = "x86_64-linux"
NATIVELSBSTRING      = "universal"
TARGET_SYS           = "arm-poky-linux-gnueabi"
MACHINE              = "raspberrypi"
DISTRO               = "poky"
DISTRO_VERSION       = "2.5.1"
TUNE_FEATURES        = "arm armv6 vfp arm1176jzfs callconvention-hard"
TARGET_FPU           = "hard"
(output ommited ...)
joao@joao-ThinkPad-T470p:~/yocto/poky$ cd build/tmp/deploy/deb/arm1176jzfshf-vfp/
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp$ ls -l hello*
-rw-r--r-- 2 joao joao  1940 ago 31 12:00 hello_0.1-r0_armhf.deb
-rw-r--r-- 2 joao joao  5412 ago 31 12:00 hello-dbg_0.1-r0_armhf.deb
-rw-r--r-- 2 joao joao   768 ago 31 12:00 hello-dev_0.1-r0_armhf.deb
```

The packages are created.  
If they are to be installed into a new built image they need to be added through the [IMAGE_INSTALL](https://www.yoctoproject.org/docs/latest/ref-manual/ref-manual.html#var-IMAGE_INSTALL). For a simple example add ```IMAGE_INSTALL_append = " hello"``` to local.conf. This will affect all images. A better solution is to modify a specific image recipe.

## Checking package contents

Just out of curiosity we can check if the binary is contained within the package and validate the target architecture.

```console
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp$ mkdir hellotmp
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp$ dpkg -x hello_0.1-r0_armhf.deb hellotmp/
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp$ cd hellotmp/
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp/hellotmp$ ls
usr
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp/hellotmp$ cd usr/bin/
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp/hellotmp/usr/bin$ ls -al
total 16
drwxr-xr-x 2 joao joao 4096 ago 31 12:00 .
drwxr-xr-x 3 joao joao 4096 ago 31 12:00 ..
-rwxr-xr-x 1 joao joao 5532 ago 31 12:00 helloworld
joao@joao-ThinkPad-T470p:~/yocto/poky/build/tmp/deploy/deb/arm1176jzfshf-vfp/hellotmp/usr/bin$ file helloworld 
helloworld: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.0, BuildID[sha1]=b93c751569ec6a184bf5ae5c9a43981ec7b190d9, stripped

```

The helloworld executable is correctly built targetting the ARM ISA. As a reminder our MACHINE is set to raspberrypi.