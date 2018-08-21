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

- Build the image  *base* ```$ bitbake -k core-image-base```.

## Transfer the image to the sd card

**Check what is your sd card device. Example uses dummy device /dev/XYZ. Be carefull this operation is not reversible.**

To check what is your sd card device use ```$ sudo lsblk -io KNAME,TYPE,SIZE,MODEL --nodep ```

```
$ sudo dd if=build/tmp/deploy/images/raspberrypi/core-image-base-raspberrypi-20180810110222.rootfs.rpi-sdimg of=/dev/XYZ status=progress
```

## Boot the image

Insert the sd card into your Raspberry Pi.  
If everything went smoothly you should be able to see a Terminal Window. You can make also use of your mouse.