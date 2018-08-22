# Genivi Development Platform (GDP)

In this short tutorial we will create a GDP image from the master branch.

## Build a qemu x86_64 image

* Clone the repository:
```
$ mkdir genivi-dev
$ git clone https://github.com/GENIVI/genivi-dev-platform.git genivi-dev
```

* Build the image through bitbake:
```
$ cd genivi-dev
$ source ./init.sh qemux86-64
$ bitbake -k genivi-dev-platform
```

Oh yes! It will take time...