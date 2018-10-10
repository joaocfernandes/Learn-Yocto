# Qt official meta layer

This guide will show how to include Qt's meta layer on your Yocto project.
A simple calculator application layer will be added to demonstrate how the *meta-qt5* layer can be used to create packages for Qt based projects.

Prequisites:

* Basic knowledge about BitBake layers and recipes. Those concepts where explained in the [Create a Layer](./Layers.md) and [Create a Recipe](./Recipe-c.md) guides

### Adding the official qt5 meta layer

Navigate to Poky's root folder where the other layers are located (*meta*, *meta-yocto-bsp*, ...).
Use Git to clone *meta-qt5*'s [repository](https://github.com/meta-qt5/meta-qt5.git):

```
$ git clone -b sumo https://github.com/meta-qt5/meta-qt5.git
```

You should now have a new folder called *meta-qt5* on your project tree.

### Creating a new layer for the calculator application project

Create a new layer named *meta-qt-simple-calc* which will contain the recipes to build the calculator application package:

```
$ bitbake-layers create-layer meta-qt-simple-calc
```

BitBake should've created the following tree:

```
meta-qt-simple-calc/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
└── recipes-example
    └── example
        └── example.bb
```

BitBake automatically creates an example recipe in the layer folder. Remove it since you will be creating your own recipe:

```
$ cd meta-qt-simple-calc && rm -rf recipes-example
```

The layer configuration file will be incomplete. The compatibility variable ```LAYERSERIES_COMPAT``` for the layer should be set and BitBake also needs to be informed of the new package you want it to install by updating the ```IMAGE_INSTALL``` variable. Add this information to the *layer.conf* file:

```
# We have a conf and classes directory, add to BBPATH
BBPATH .= ":${LAYERDIR}"
LAYERSERIES_COMPAT_meta-qt-simple-calc = "sumo"

# We have recipes-* directories, add to BBFILES
BBFILES += "${LAYERDIR}/recipes-*/*/*.bb \
            ${LAYERDIR}/recipes-*/*/*.bbappend"

BBFILE_COLLECTIONS += "meta-qt-simple-calc"
BBFILE_PATTERN_meta-qt-simple-calc = "^${LAYERDIR}/"
BBFILE_PRIORITY_meta-qt-simple-calc = "6"

IMAGE_INSTALL += " qt-simple-calc"
```

### Adding a recipe to build the calculator application package

Add a new folder named *qt-simple-calc* to the layer, which will contain the application recipe files.

```
$ mkdir -p recipes-qt-simple-calc/qt-simple-calc && cd recipes-qt-simple-calc/qt-simple-calc
```

Add a new recipe file named *qt-simple-calc.bb*.
The recipe needs to download the calculator application source code, which is a Qt C++ project, from a git repository and build it using *qmake*.
The recipe will depend on the *meta-qt5* layer's *qtbase* recipe which builds the *QtCore* module and will inherit the *qmake5.bbclass*.
Finally the recipe will package the resulting binary file and place in the ```/opt/qt-simple-calc/``` folder.

```
SUMMARY = "Qt Simple Calculator Application Recipe"
DESCRIPTION = "This recipe builds a Qt project for a simple calculator application."
LICENSE = "MIT"
LIC_FILES_CHKSUM = "file://${COMMON_LICENSE_DIR}/MIT;md5=0835ade698e0bcf8506ecda2f7b4f302"

DEPENDS += "qtbase"

SRC_URI = "git://github.com/vpapadopou/qt-simple-calculator;branch=master"
SRCREV = "1af09d43f9a41ad3136a4fac9db63b9542692f91"

S = "${WORKDIR}/git"

do_install_append() {
    install -d ${D}/opt/qt-simple-calc/
    install -m 0755 simpleCalculator ${D}/opt/qt-simple-calc/
}

FILES_${PN} += "/opt/qt-simple-calc/simpleCalculator"

inherit qmake5
```

### Adding an append file to extend *qt5base*'s recipe

As of version 5.7, Qt does not deploy the fonts used in their projects. As a result the calculator application will not find any fonts on Qt's default font folder and will not display any text but Qt can look for system fonts using the *fontconfig* library if we tell it to.
In order to do this an additional parameter must be passed when building the *QtCore* module. Since the recipe responsible for building this module is layer *meta-qt5*'s *qtbase_git.bb* we will add an append file *qtbase_git.bbappend* to our recipe folder containing:

```
PACKAGECONFIG += "fontconfig"
```

### Building the system image

This far you've introduced two new layers to the project and you need to inform BitBake that you want to use them when building your system image.
Modify the *bblayers.conf* file and make sure you include the *meta-qt5*, *meta-openembedded/meta-oe* and *meta-qt-simple-calc* layers.

Build your system image:

```
$ bitbake -k core-image-base
```

When you boot your image, under ```/opt/qt-simple-calc/``` should be the simple calculator executable.
