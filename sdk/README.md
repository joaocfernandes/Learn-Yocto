## [Yocto Project Application Development and the Extensible Software Development Kit](https://www.yoctoproject.org/docs/2.5/sdk-manual/sdk-manual.htm)


- Install the eSDK download from [here](http://downloads.yoctoproject.org/releases/yocto/yocto-2.5/toolchain/)
- Install eclipse from the Eclipse site. Current supported versions are Oxygen and Neon. Do not install Eclipse from your distribution's package repository. 
- Configure Eclipse according to the [sdk-manual](https://www.yoctoproject.org/docs/2.5/sdk-manual/sdk-manual.html#oxygen-setting-up-the-eclipse-ide).

Modify your image to be built with support for eclipse debugging, use the steps mentioned in the [wiki](https://wiki.yoctoproject.org/wiki/TipsAndTricks/RunningEclipseAgainstBuiltImage).

- Build your Image ```$ bitbake -k core-image-sato -c populate_sdk```
- Build the meta-ide-support image ```$ bitbake -k meta-ide-support```