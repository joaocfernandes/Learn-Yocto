# Raspberry Pi base image

Prerequisite you have completed the [Minimal](./MINIMAL.md) tutorial.
In this tutorial we will create a yocto base image with X11 and the Open SSH Server.

## Modify the local.conf

* Add the extra packages:
```
EXTRA_IMAGE_FEATURES ?= "debug-tweaks x11-base ssh-server-openssh"
```

* Add some extra free space to rootfs, you may want to create some files...
```
IMAGE_ROOTFS_EXTRA_SPACE = "524288"
```
## Build Your Image

- Initialize the build env  
```$ source oe-init-build-env```
- Modify your local.conf file  
    - Set your machine to ```MACHINE = "raspberrypi"``` according to your Raspberry Pi [model](https://github.com/agherzan/meta-raspberrypi/blob/sumo/docs/layer-contents.md#supported-machines).
    - (Optional) Set the package manager to deb ```PACKAGE_CLASSES ?= "package_deb"```
    - Set the SSTate Mirrors to save time in the builds.  
    
    ```
    SSTATE_MIRRORS = "\
        file://.* http://sstate.yoctoproject.org/dev/PATH;downloadfilename=PATH \n \
        file://.* http://sstate.yoctoproject.org/2.4/PATH;downloadfilename=PATH \n \
        file://.* http://sstate.yoctoproject.org/2.5/PATH;downloadfilename=PATH \n \"
    ```  
    
    - Modify the bbplayers.conf file and add all the listed dependencies listed on the [Readme.md](https://github.com/agherzan/meta-raspberrypi/blob/sumo/README.md#dependencies).  
    
    ```
    BBLAYERS ?= " \
        /home/joao/yocto/poky/meta \
        /home/joao/yocto/poky/meta-poky \
        /home/joao/yocto/poky/meta-yocto-bsp \
        /home/joao/yocto/poky/meta-openembedded/meta-oe \
        /home/joao/yocto/poky/meta-openembedded/meta-multimedia \
        /home/joao/yocto/poky/meta-openembedded/meta-networking \
        /home/joao/yocto/poky/meta-openembedded/meta-python \  
        /home/joao/yocto/poky/meta-raspberrypi \
        "
    ```  

    - Build the image  *minimal* ```$ bitbake -k core-image-minimal```.  
    Other available images are listed [here](https://github.com/agherzan/meta-raspberrypi/blob/sumo/docs/layer-contents.md#images).

## Transfer the image to the sd card

**Check what is your sd card device. Example uses dummy device /dev/XYZ. Be carefull this operation is not reversible.**

To check what is your sd card device use ```$ sudo lsblk -io KNAME,TYPE,SIZE,MODEL --nodep ```


```
$ sudo dd if=build/tmp/deploy/images/raspberrypi/core-image-minimal-raspberrypi-20180810110222.rootfs.rpi-sdimg of=/dev/XYZ status=progress
```


## Boot the image

Insert the sd card into your Raspberry Pi.  
If everything went smoothly you should be able to see a login prompt. Just use *root* without any password.