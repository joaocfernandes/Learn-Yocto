# Raspberry Pi Image

This guide intends to summarize the essencial steps performed to build a Raspberry Pi image using Yocto.

## Download software dependencies

To build the image we are going to use the [Poky](http://git.yoctoproject.org/cgit/cgit.cgi/poky) and the layers [meta-openembedded](http://git.openembedded.org/meta-openembedded) , [meta-raspberrypi](https://github.com/agherzan/meta-raspberrypi) .

The list of all meta-raspberrypi dependencies are contained in the [Readme.md](https://github.com/agherzan/meta-raspberrypi/blob/sumo/README.md#dependencies) the provided MACHINE's are  listed on the [layer-contentens.md](https://github.com/agherzan/meta-raspberrypi/blob/sumo/docs/layer-contents.md#supported-machines)


- Use Git to clone Poky repo using the apropriate [version](https://wiki.yoctoproject.org/wiki/Releases) branch  
```$ git clone -b sumo git://git.yoctoproject.org/poky.git```

- Enter into the Poky folder  
```$ cd poky```

- Use Git to clone the meta-rasperrypi layer repo using the apropriate [version]  
```$ git clone -b sumo git://git.yoctoproject.org/meta-raspberrypi```

- Use Git to clone the meta-openembedded layer repo using the apropriate [version]  
```$ git clone -b sumo git://git.openembedded.org/meta-openembedded```

## Build Your Image

- Initialize the build env  
```$ source oe-init-build-env```
- Modify your local.conf file  
    - Set your machine to ```MACHINE = "raspberrypi"``` according to Raspberry Pi model.
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

**Check what is your sd card device. Example used device /dev/XYZ. This operation is not reversible.**

To check what is your sd card device use ```$ sudo lsblk -io KNAME,TYPE,SIZE,MODEL --nodep ```


```
$ sudo dd if=build/tmp/deploy/images/raspberrypi/core-image-minimal-raspberrypi-20180810110222.rootfs.rpi-sdimg of=/dev/XYZ status=progress```
