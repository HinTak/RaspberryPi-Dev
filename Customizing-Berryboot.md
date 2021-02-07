# Customizing Berryboot to include additional out-of-tree kernel driver modules

This document describes how the [berryboot with respeaker drivers included](https://github.com/HinTak/berryboot/releases) is done.
The information is correct for the two releases, `berryboot-20201109-pi0123+respeaker` and `berryboot-20201108-pi4+respeaker`. Both
of them are available as vannilla builds (without the respeaker drivers), for people look for marginally more recent, and 32-bit builds.

My `doc-fix` branch is now more than doc-fixes, but contains misc fixes for building berryboot on more recent linux systems and compilers
(Fedora 33, Nov 2020) than the main berryboot developer's Ubuntu 18.04 (April 2018), and is already scheduled as
[a pull](https://github.com/maxnet/berryboot/pull/674). My `berryboot2018-32bit` branch contains 32-bit incompatible changes in addition,
and is not meant to be merged as is.

If you like this work, please feel free to donate at the bottom of the page at https://hintak.github.io/ .

## Out-of-tree drivers for your currently running kernel

Let's review how to build and install additional drivers for your currently running kernel.

There are 3 steps:

```
make -C /lib/modules/`uname -r`/build M=`pwd` clean
make -C /lib/modules/`uname -r`/build M=`pwd` modules
make -C /lib/modules/`uname -r`/build M=`pwd` modules_install
```

The `clean`, `modules`, `modules_install` targets are defined in the Linux kernel tree's top-level `Makefile`.
The procedure is essentially the same for kernel modules included in the kernel source tree (without the `-C ...` for the location of the kernel source) too.

The steps are, look at the (partial)
pre-configured kernel source tree at `/lib/modules/--curent_kernel--/build`, clean, build and install the modules (`M=`) in the current directory.
The partial pre-configured kernel source tree is usually available as an additional `kernel-devel`, `kernel-headers` package for distribution-shipped
kernels.

The `modules_install` target itself consists of 3 steps also: copy the modules to the right place, sign them if the kernel is configured to prefer or
enforce cryptographically signed modules, and update the mutual dependencies between kernel modules.

## Capture logs while building Berryboot

Appending `2>&1 | tee berryboot-build-log` to the berryboot build command, to capture the output to a file while still displaying
it on screen:

```
./build-berryboot.sh device_pi0123 2>&1 | tee berryboot-build-log
./build-berryboot.sh device_pi64   2>&1 | tee berryboot-build-log
```

## Adding out-of-tree kernel driver modules to Berryboot

Berryboot builds a whole lots of tools (including a cross-compiler), then use those tools to cross-compile the linux kernel. We don't need
to worry about the details, as it is all neatly done by the nice berryboot and buildroot developers. We are only interested in building our out-of-tree kernel modules "the same way". To do this,
examine how the kernel is built, by `grep DEPMOD= berryboot-build-log` (this looks for the kernel module dependency tool
being re-defined for cross-compilation).

There are three nearly identical lines, differing by the endings:

```
... Image
... dtbs
... INSTALL_DTBS_PATH=/my-dev-area/berryboot/buildroot-2018.08/output/images dtbs_install
```

This build the default kernel plus modules, the devicetree blobs, and install the devicetree blobs.

For cross-compiling out-of-tree drivers like the Respeaker, we switch now to working inside the driver's source tree. Cut-and-paste the common part
above, and append a little, and run them inside the driver's source code directory:

```
# Go to driver source code:
cd /my-dev-area/seeed-voicecard
#
... M=`pwd` clean
... M=`pwd` modules
```

We cannot just do "M=-pwd- modules_install", as berryboot moves the other installed kernel modules to `...shared/` before packaging. Looking carefully at the whole cut-and-paste common part, it includes this:

```
... INSTALL_MOD_PATH=/my-dev-area/berryboot/buildroot-2018.08/output/target ...
```

We need to edit the cut-and-paste part slightly to adjust to where the other modules have been moved to, to update the module dependency correctly:

```
... INSTALL_MOD_PATH=/my-dev-area/berryboot/buildroot-2018.08/output/shared/ ... M=`pwd` modules_install
```

While you are in the driver directory, you should also copy the `*.dtbo` files too:

```
cp -p *.dtbo /my-dev-area/berryboot/output/overlays/
```

Then, we switch back to working inside the berryboot directory, regenerate `shared.img` and copy it to the correct place:

```
# back to Berryboot:
cd /my-dev-area/berryboot
#
cd buildroot-2018.08/

# Regenerate shared.img
mksquashfs output/shared output/images/shared.img -all-root -noappend

# Move the vanilla one aside, and copy the new shared.img into the right place 
mv ../output/shared.img ../shared.img.vannilla
cp output/images/shared.img ../output/shared.img
```

You should check with `unsquashfs -ll ...` on the new `shared.img`, compared to the old `shared.img.vannilla`. Besides the new modules,
many of the `.../lib/modules/--versions--/modules.*` module dependency files should be slightly larger, or at least the same size. Changes
in directory sizes are unimportant, and can go up or down depending on alignments inside squashfs, I think.

Then you can delete `shared.img.vannilla`.


Now your berryboot's `output` directory is ready to be zipped up into a "berryboot+respeaker" release.
`cd output && zip -r9X ../berryboot+respeaker.zip *` as appropriate.

### Quirks and misc notes

- The first time you run `./build-berryboot.sh ...`, it always fails with this message:

```
Using ../configs/berryboot_defconfig as base
Merging ../configs/device_pi0123
#
# merged configuration written to ./.config (needs make)
#
make[1]: *** No rule to make target '/my-dev-area/berryboot/buildroot-2018.08/output/.br-external.mk'.  Stop.
make: *** [Makefile:79: _all] Error 2
```

The second time is fine. This is probably a bug that should be reported.

- The build process tries to download many tools from `http://ftpmirror.gnu.org` . The ftp mirror redirection does not always pick a good mirror.
Sometimes the one it picks is unresponsive and one needs to interrupt the download and just run it again to hope it pick a different mirror.

- The first (64-bit) vanilla build took about 4 hours 40 minutes on my laptop. This includes looking up fixes for building old softwares to current systems.
All of these fixes are now in [a pull](https://github.com/maxnet/berryboot/pull/674) . The second (32-bit) vanilla build took just under 3 hours.
Both requires 15GB+ disk space.
