## Yocto Quick Start

This tutorial has the main objective of generating and runnning a yocto image for arm on qemu.

- Use Git to clone Poky repo using the apropriate [version](https://wiki.yoctoproject.org/wiki/Releases) branch)  
```$ git clone -b sumo git://git.yoctoproject.org/poky.git```
- Build Your Image
    - Initialize the build env  
    ```$ source oe-init-build-env```
    - Modify your build/conf/local.conf file  
        - Set your machine to ```MACHINE = "qemuarm"```
        - Set the package manager to deb ```PACKAGE_CLASSES ?= "package_deb"```
        - Set the SSTate Mirrors to save time in the builds.  
        
        ```
            SSTATE_MIRRORS = "\
            file://.* http://sstate.yoctoproject.org/dev/PATH;downloadfilename=PATH \n \
            file://.* http://sstate.yoctoproject.org/2.4/PATH;downloadfilename=PATH \n \
            file://.* http://sstate.yoctoproject.org/2.5/PATH;downloadfilename=PATH \n \"
        ```       
        - Build the image  *minimal* ```$ bitbake -k core-image-minimal```  or the graphical [*sato*](https://en.wikipedia.org/wiki/OpenedHand) with ```$ bitbake -k core-image-sato```

            Common targets are:
            - core-image-minimal (no X server)
            - core-image-sato (X server with SATO)
            - meta-toolchain (??)
            - meta-ide-support (package for ensuring the build directory contains all appropriate toolchain packages for using an IDE) check also [Tips and Tricks](https://wiki.yoctoproject.org/wiki/TipsAndTricks/RunningEclipseAgainstBuiltImage)
        
- Run the image on qemu.  
```$ runqemu qemuarm ```