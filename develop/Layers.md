# Create your first general Layer

In this tutorial we treat the creation of general layers and exclude the creation of BSP layers. The main diference between a General layer and a BSP layer is the existence of a machine configuration file and optionally a kernel recipe (.bb) or a kernel append file (.bbappend).

Follow the [ Yocto dev manual](https://www.yoctoproject.org/docs/latest/dev-manual/dev-manual.html#creating-your-own-layer) instructions.

The layer created was called *first*, therefore the folder meta-frist was created.
By the end you should have created the following structure inside the meta-first folder.

```console
joao@joao-ThinkPad-T470p:~/yocto/poky$ tree meta-first/
meta-first/
├── conf
│   └── layer.conf
├── COPYING.MIT
└──  README
```

## Create a Layer.conf file

The layer.conf file can be copied from and existing layer configuration and modified as necessary.

Follows an example of the created configuration file.

```
# We have a conf and classes directory, add to BBPATH
BBPATH .= ":${LAYERDIR}"

# We have recipes-* directories, add to BBFILES
BBFILES += "${LAYERDIR}/recipes-*/*/*.bb \
            ${LAYERDIR}/recipes-*/*/*.bbappend"

BBFILE_COLLECTIONS += "meta-first"
BBFILE_PATTERN_meta-first = "^${LAYERDIR}/"
BBFILE_PRIORITY_meta-first = "6"

LAYERVERSION_meta-first = "4"
LAYERSERIES_COMPAT_meta-first = "sumo"
```

An detailed explanation of the configuration file options is present in the [ Yocto dev manual](https://www.yoctoproject.org/docs/latest/dev-manual/dev-manual.html#creating-your-own-layer).

## Create a Layer programmatically

Another way to add a layer is to use the *bitbake-layers* script together with the *create-layer* subcommand.

This facilitates the addition of a new layer comparing with doing it by hand and can also be used for scripting.

Follows a use example:

```console
joao@joao-ThinkPad-T470p:~/yocto/poky$ bitbake-layers create-layer meta-newlayer
NOTE: Starting bitbake server...
Add your new layer with 'bitbake-layers add-layer meta-newlayer'
```

As you can see by the output just creating the layer does not add it to the bblayers.conf file.

The created folder structure is:

```console
joao@joao-ThinkPad-T470p:~/yocto/poky$ tree meta-newlayer
meta-newlayer
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
└── recipes-example
    └── example
        └── example_0.1.bb
```


