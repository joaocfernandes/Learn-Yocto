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