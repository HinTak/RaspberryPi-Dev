# Building largely-compatible Deb kernel packages for Ubuntu and Raspbian

## Ubuntu


## Raspbian

Main Reference: [raspberrypi/linux#3279 comment](https://github.com/raspberrypi/linux/issues/3279#issuecomment-773493039).

Ran `./debian/update.sh` in [RPi-Distro/firmware](https://github.com/RPi-Distro/firmware), aftering updating submodules and setting up remote 'upstream' to
point to [raspberrypi/firmware](https://github.com/raspberrypi/firmware.git). Then use `reprepro` to publish.

The script requires [git-buildpackage](https://github.com/agx/git-buildpackage) ([Homepage/Documentation](https://honk.sigxcpu.org/piki/projects/git-buildpackage/)).
This is unfortunately not available on Fedora.

**Untested**