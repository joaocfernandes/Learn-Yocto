# Create your first general Layer

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