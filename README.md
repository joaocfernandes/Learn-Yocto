# Yocto-Basics

This guide intends to summarize the basic knowledge and steps performed through the read of the [NEW TO THE YOCTO PROJECT® ?](https://www.yoctoproject.org/docs/) 4 introductory sections.

## [Yocto Project Quick Build](https://www.yoctoproject.org/docs/2.5/brief-yoctoprojectqs/brief-yoctoprojectqs.html)

The tutorial will be followed with some small modifications in order to test a diferent target architecture.

- Use Git to clone Poky repo using the apropriate [version](https://wiki.yoctoproject.org/wiki/Releases) branch)  
```$ git clone -b sumo git://git.yoctoproject.org/poky.git```
- Build Your Image
    - Initialize the build env  
    ```$ source oe-init-build-env```
    - Modify your local.conf file  
        - Set your machine to ```MACHINE = "qemuarm"```
        - Set the package manager to deb ```PACKAGE_CLASSES ?= "package_deb"```
        - Set the SSTate Mirrors to save time in the builds.  
        
        ```
            SSTATE_MIRRORS = "\
            file://.* http://sstate.yoctoproject.org/dev/PATH;downloadfilename=PATH \n \
            file://.* http://sstate.yoctoproject.org/2.4/PATH;downloadfilename=PATH \n \
            file://.* http://sstate.yoctoproject.org/2.5/PATH;downloadfilename=PATH \n \"
        ```       
        - Build the image  *minimal* ```$ bitbake -k core-image-minimal```  or the *sato* with ```$ bitbake -k core-image-sato```

            Common targets are:
            - core-image-minimal (no X server)
            - core-image-sato (X server with SATO)
            - meta-toolchain (??)
            - meta-ide-support (??)
        
- Run the image on qemu.  
```$ runqemu qemuarm ```

## [Yocto Project Application Development and the Extensible Software Development Kit](https://www.yoctoproject.org/docs/2.5/sdk-manual/sdk-manual.htm)


- Install the eSDK download from [here](http://downloads.yoctoproject.org/releases/yocto/yocto-2.5/toolchain/)
- Install eclipse from the Eclipse site. Current supported versions are Oxygen and Neon. Do not install Eclipse from your distribution's package repository. 
- Configure Eclipse according to the [sdk-manual](https://www.yoctoproject.org/docs/2.5/sdk-manual/sdk-manual.html#oxygen-setting-up-the-eclipse-ide).

Modify your image to be built with support for eclipse debugging, use the steps mentioned in the [wiki](https://wiki.yoctoproject.org/wiki/TipsAndTricks/RunningEclipseAgainstBuiltImage).

- Build your Image ```$ bitbake -k core-image-sato -c populate_sdk```
- Build the meta-ide-support image ```$ bitbake -k meta-ide-support```
