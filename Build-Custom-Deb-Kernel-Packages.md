# Building largely-compatible Deb kernel packages for Ubuntu and Raspbian

## Ubuntu

Took 140% CPU 3:10:45 Elapsed, 15% CPU, 52:39.72 Elapsed, 12% CPU, 1:15:39 Elapsed; 106% CPU 4:16:53 Elapsed second build.

Requires about 12.8GB of disk space, which consists of 1.8GB for the bare clone, 1.4GB for checkout'ed source code and debian/build/ eventually containing about 9.6GB.

#### Quirks 1:

`fakeroot debian/rules clean` on Fedora shows:
```
/bin/bash: line 2: kernel-wedge: command not found
```

This is obviously harmless. A new clone is as clean as it is...

#### Quirks 2 faked-tcp hangs

#### Quirks 3: Fixdep

Background: [problem with cross-built kernel packages](https://github.com/raspberrypi/linux/issues/3279)

In [linux-linaro-oneiric repo](https://git.linaro.org/ubuntu/linux-linaro-oneiric.git),
[LINARO: Use KBUILD_SCRIPTROOT to cross build scripts](http://git.linaro.org/ubuntu/linux-linaro-oneiric.git/commit/?id=f84e13f13d798d3fca6ed0f73e91363ad68c0e43)
[KBuild: Allow scripts/* to be cross compiled](http://git.linaro.org/ubuntu/linux-linaro-oneiric.git/commit/?id=c1db0c3282f471ef25f0cfcfcf2dc623360f44ab)
.

Somewhat more recently, in linux-linaro-raring repo,
[LINARO: Conditionally use KBUILD_SCRIPTROOT to crossbuild scripts](http://git.linaro.org/ubuntu/linux-linaro-raring.git/commit/?h=linaro-ubuntu-packaging-3.7-aarch64&id=1688776d0e03cb3f41e488440e9ee217c02aa399)
[KBuild: Allow scripts/* to be cross compiled](http://git.linaro.org/ubuntu/linux-linaro-raring.git/commit/?h=linaro-ubuntu-packaging-3.7-aarch64&id=86efc7ec96c2a4330102a33f2cc40ae318909c8b)
,
[LINARO: Conditionally use KBUILD_SCRIPTROOT to crossbuild scripts](http://git.linaro.org/ubuntu/linux-linaro-raring.git/commit/?id=58b03ed8e21bbf22de2191cb11bb89e3c6f1eb11)
[KBuild: Allow scripts/* to be cross compiled](http://git.linaro.org/ubuntu/linux-linaro-raring.git/commit/?id=3aa8ee7600ca594075f77a8c05eb39cae0fa758d)
.

## Raspbian

Main Reference: [raspberrypi/linux#3279 comment](https://github.com/raspberrypi/linux/issues/3279#issuecomment-773493039).

Ran `./debian/update.sh` in [RPi-Distro/firmware](https://github.com/RPi-Distro/firmware), aftering updating submodules and setting up remote 'upstream' to
point to [raspberrypi/firmware](https://github.com/raspberrypi/firmware.git). Then use `reprepro` to publish.

The script requires [git-buildpackage](https://github.com/agx/git-buildpackage) ([Homepage/Documentation](https://honk.sigxcpu.org/piki/projects/git-buildpackage/)).
This is unfortunately not available on Fedora.

**Untested**