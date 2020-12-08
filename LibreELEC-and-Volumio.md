# Other Raspberry Pi distributions:

## LibreELEC, Volumio and Berryboot

http://releases.libreelec.tv/LibreELEC-RPi4.arm-9.2.6.img.gz

```
zcat LibreELEC-RPi4.arm-9.2.6.img.gz > LibreELEC-RPi4.arm-9.2.6.img
```

http://updates.volumio.org/pi/volumio/2.853/volumio-2.853-2020-11-20-pi.img.zip

```
-rw-rw-r--. 1 Hin-Tak Hin-Tak 575668224 Dec  3 09:19 LibreELEC-RPi4.arm-9.2.6.img
-rw-rw-r--. 1 Hin-Tak Hin-Tak 135795494 Oct 24 22:05 LibreELEC-RPi4.arm-9.2.6.img.gz
```

```
-rw-rw-r--. 1 Hin-Tak Hin-Tak  411582057 Nov 20 19:19 volumio-2.853-2020-11-20-pi.img.zip
-rw-r--r--. 1 Hin-Tak Hin-Tak 2936012800 Nov 20 19:18 volumio-2.853-2020-11-20-pi.img
```

Output of `disktype LibreELEC-RPi4.arm-9.2.6.img`:

```
--- LibreELEC-RPi4.arm-9.2.6.img
Regular file, size 549 MiB (575668224 bytes)
DOS/MBR partition map
Partition 1: 512 MiB (536870912 bytes, 1048576 sectors from 8192, bootable)
  Type 0x0C (Win95 FAT32 (LBA))
  FAT16 file system (hints score 5 of 5)
    Volume size 511.7 MiB (536584192 bytes, 65501 clusters of 8 KiB)
    Volume name "LIBREELEC"
Partition 2: 32 MiB (33554432 bytes, 65536 sectors from 1056768)
  Type 0x83 (Linux)
  Ext4 file system
    Volume name "STORAGE"
    UUID 09814ADF-0B3E-462B-AD3F-ABD8815B9B92 (DCE, v4)
    Volume size 32 MiB (33554432 bytes, 32768 blocks of 1 KiB)
```

Output of `disktype volumio-2.853-2020-11-20-pi.img`:

```
--- volumio-2.853-2020-11-20-pi.img
Regular file, size 2.734 GiB (2936012800 bytes)
DOS/MBR partition map
Partition 1: 61.04 MiB (64000000 bytes, 125000 sectors from 1, bootable)
  Type 0x0C (Win95 FAT32 (LBA))
  FAT32 file system (hints score 5 of 5)
    Volume size 60.08 MiB (62998528 bytes, 123044 clusters of 512 bytes)
    Volume name "boot"
Partition 2: 2.269 GiB (2435999744 bytes, 4757812 sectors from 125001)
  Type 0x83 (Linux)
  Ext4 file system
    Volume name "volumio"
    UUID 9A7F6125-409F-4594-A44A-94E1A9AAF1D6 (DCE, v4)
    Last mounted at "/mnt/volumio/images"
    Volume size 2.269 GiB (2435997696 bytes, 594726 blocks of 4 KiB)
Partition 3: 286.1 MiB (300000256 bytes, 585938 sectors from 4882813)
  Type 0x83 (Linux)
  Ext4 file system
    Volume name "volumio_data"
    UUID 22C04200-F995-44D3-B5BA-F2A9A977B0A6 (DCE, v4)
    Volume size 286.1 MiB (299999232 bytes, 73242 blocks of 4 KiB)
```

Berryboot:

http://www.berryboot.com/ (interestingly, `https://` goes to a different site!)

[berryboot-20201103-pi4.zip](https://downloads.sourceforge.net/project/berryboot/berryboot-20201103-pi4.zip)

[//]: # (https://netix.dl.sourceforge.net/project/berryboot/berryboot-20201103-pi4.zip)

```
sha1sum: 36996289cf9f8b37a1ee46d96484b9d0658d5de3
sha256sum: 2eae31aed11d2ff28c5ea629f3bbc1e317ff98cdcde187665046e5252a7341f9
-rw-rw-r--. 1 Hin-Tak Hin-Tak 47723837 Nov  3 15:29 berryboot-20201103-pi4.zip
```

### LibreELEC

The offsets in bytes are sector offset `x512` (or divide by 2 and with `KiB` suffix):

```
mount -o loop,ro,offset=541065216 LibreELEC-RPi4.arm-9.2.6.img -t ext4 /mnt
mount -o loop,ro,offset=4194304 LibreELEC-RPi4.arm-9.2.6.img -t vfat /mnt
```

Volume "STORAGE" is mostly empty and not interesting.

In "LIBREELEC", `cat /mnt/SYSTEM.md5`:

```
e4d2bc69b48775d43a97c920200e9e3b  target/SYSTEM
```

Interesting entries from `unsquashfs -ll /mnt/SYSTEM` :

```
lrwxrwxrwx root/root                27 2020-10-24 21:59 squashfs-root/etc/modprobe.d -> /storage/.config/modprobe.d
lrwxrwxrwx root/root                31 2020-10-24 21:59 squashfs-root/etc/modules-load.d -> /storage/.config/modules-load.d
drwxr-xr-x root/root                 3 2020-10-24 21:57 squashfs-root/flash
drwxr-xr-x root/root                 3 2020-10-24 21:57 squashfs-root/storage
-rwxr-xr-x root/root             51460 2020-10-24 21:59 squashfs-root/usr/bin/aplay
-rwxr-xr-x root/root              1310 2020-10-24 22:00 squashfs-root/usr/bin/apt-get
lrwxrwxrwx root/root                 7 2020-10-24 22:00 squashfs-root/usr/bin/bash -> busybox
-rwxr-xr-x root/root             30800 2020-10-24 22:02 squashfs-root/usr/bin/pacat
-rwxr-xr-x root/root              9936 2020-10-24 22:02 squashfs-root/usr/bin/pacmd
-rwxr-xr-x root/root             43140 2020-10-24 22:02 squashfs-root/usr/bin/pactl
lrwxrwxrwx root/root                 5 2020-10-24 22:02 squashfs-root/usr/bin/pamon -> pacat
lrwxrwxrwx root/root                 5 2020-10-24 22:02 squashfs-root/usr/bin/paplay -> pacat
lrwxrwxrwx root/root                 5 2020-10-24 22:02 squashfs-root/usr/bin/parec -> pacat
lrwxrwxrwx root/root                 5 2020-10-24 22:02 squashfs-root/usr/bin/parecord -> pacat
drwxr-xr-x root/root               411 2020-10-24 22:03 squashfs-root/usr/lib/kernel-overlays/base/lib/modules/4.19.127
drwxr-xr-x root/root                30 2020-10-24 21:59 squashfs-root/usr/lib/kernel-overlays/driver.dvb.crazycat/lib/modules/4.19.127
drwxr-xr-x root/root                30 2020-10-24 21:59 squashfs-root/usr/lib/kernel-overlays/driver.dvb.dvb-latest/lib/modules/4.19.127
lrwxrwxrwx root/root                16 2020-10-24 22:04 squashfs-root/usr/lib/modules -> /var/lib/modules
-rw-r--r-- root/root                30 2020-10-24 21:59 squashfs-root/usr/lib/systemd/system/flash.mount.d/dependencies.conf
-rw-r--r-- root/root                30 2020-10-24 21:59 squashfs-root/usr/lib/systemd/system/storage.mount.d/dependencies.conf
```

No `arecord`, and most of the dev tools. Also, `busybox` is not quite `bash`. `pacmd list-sources` is perhap a substitute for `arecord -L`.
On Raspbian, `arecord` is a sym-link to `aplay`. It presumably behaves differently, depends on what names it sees itself as.

TODO: where should `asound.conf`, `asound.state` be (possibly `/var/media/root/var/lib/alsa/asound.state`)? And, `alctl` misbehaves due to `/sys` not there.

## Volumio

```
mount -o loop,ro,offset=512 volumio-2.853-2020-11-20-pi.img /mnt
mount -o loop,ro,offset=64000512 volumio-2.853-2020-11-20-pi.img /mnt
mount -o loop,ro,offset=2500000256 volumio-2.853-2020-11-20-pi.img /mnt
```

Volume "volumio_data" is largely empty.

Volume "boot":

`userconfig.txt` is included by `config.txt`.

Volume "volumio":

Interesting entries from `unsquashfs -ll /mnt/volumio_current.sqsh`:

```
-rwxr-xr-x root/root            863400 2017-01-14 23:14 squashfs-root/bin/bash
-rwxr-xr-x root/root            683064 2018-08-04 00:17 squashfs-root/bin/busybox
lrwxrwxrwx root/root                 4 2020-11-20 18:57 squashfs-root/bin/sh -> dash
lrwxrwxrwx root/root                 4 2014-01-21 05:27 squashfs-root/bin/sh.distrib -> dash
drwxr-xr-x root/root                74 2020-11-20 19:18 squashfs-root/boot
-rwxr-xr-x root/root                41 2020-11-20 19:11 squashfs-root/boot/.firmware_revision
-rwxr-xr-x root/root                41 2020-11-20 19:11 squashfs-root/boot/.firmware_revision_kernel
drwxr-xr-x Hin-Tak/Hin-Tak          31 2020-11-20 19:05 squashfs-root/data
drwxrwxrwx root/root                 3 2020-11-20 19:05 squashfs-root/data/INTERNAL
-rw-r--r-- root/root                82 2009-02-02 23:06 squashfs-root/etc/dpkg/origins/debian
lrwxrwxrwx root/root                 8 2020-11-20 18:58 squashfs-root/etc/dpkg/origins/default -> raspbian
-rw-r--r-- root/root               120 2015-01-07 02:04 squashfs-root/etc/dpkg/origins/raspbian
drwxr-xr-x Hin-Tak/Hin-Tak         100 2020-11-20 19:14 squashfs-root/home/volumio
drwxrwxr-x root/root               298 2020-11-20 19:18 squashfs-root/lib/modules/4.19.118+
drwxrwxr-x root/root               298 2020-11-20 19:18 squashfs-root/lib/modules/4.19.118-v7+
drwxrwxr-x root/root               298 2020-11-20 19:18 squashfs-root/lib/modules/4.19.118-v7l+
lrwxrwxrwx root/root                14 2020-11-20 19:05 squashfs-root/mnt/INTERNAL -> /data/INTERNAL
drwxr-xr-x root/root                53 2020-11-20 19:09 squashfs-root/myvolumio
drwxr-xr-x root/root                46 2020-11-20 19:09 squashfs-root/myvolumio/app
-rwxr-xr-x root/root             59436 2014-07-17 06:27 squashfs-root/usr/bin/aplay
lrwxrwxrwx root/root                 5 2014-07-17 06:27 squashfs-root/usr/bin/arecord -> aplay
```

The presence of `dash` (and `dpkg/origins`) suggests it is just a Raspbian variant.

Indeed, extracting `unsquashfs /mnt/volumio_current.sqsh etc usr/lib/os-release`, Output of `cat squashfs-root/etc/*release*`:

```
PRETTY_NAME="Raspbian GNU/Linux 8 (jessie)"
NAME="Raspbian GNU/Linux"
VERSION_ID="8"
VERSION="8 (jessie)"
ID=raspbian
ID_LIKE=debian
HOME_URL="http://www.raspbian.org/"
SUPPORT_URL="http://www.raspbian.org/RaspbianForums"
BUG_REPORT_URL="http://www.raspbian.org/RaspbianBugs"
VOLUMIO_BUILD_VERSION="2c20ceacdd8365d3bd2fdff070d8c78700ff2903"
VOLUMIO_FE_VERSION="fa55af43e5eb642e968aa6de2f8a985962941d63"
VOLUMIO_BE_VERSION="c9e4849495e810bd6ebb6833232daba25d86dd4c"
VOLUMIO_ARCH="arm"
VOLUMIO_VARIANT="volumio"
VOLUMIO_TEST="FALSE"
VOLUMIO_BUILD_DATE="Fri Nov 20 20:05:14 CET 2020"

VOLUMIO_VERSION="2.853"
VOLUMIO_HARDWARE="pi"
VOLUMIO_HASH="f7f9725bb5ce7d6da77135f468c28964"
```

`unsquashfs /mnt/volumio_current.sqsh var/log`,
`squashfs-root/var/log/apt/history.log` and `squashfs-root/var/log/dpkg.log` are the interesing files.

The kernel on Volumio is:

```
Linux version 4.19.118-v7l+ (dom@buildbot) (gcc version 4.9.3 (crosstool-NG crosstool-ng-1.22.0-88-g8460611)) #1311 SMP Mon Apr 27 14:26:42 BST 2020
```

This appears to be `1.20200512-2` or `1.20200601-1` (See [Raspbian Kernel Versions](Raspbian-Kernel-Releases.md)).

If your `apt list -a raspberrypi-kernel` on Volumio looks like this:

```
Listing... Done
raspberrypi-kernel/testing 1.20201022-1 armhf [upgradable from: 1.20200512-2]
raspberrypi-kernel/now 1.20200512-2 armhf [installed,upgradable to: 1.20201022-1]
```

You should be able to download the matching header,

http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel-headers_1.20200512-2_armhf.deb

`dpkg -i` the downloaded file, then do `apt-mark hold raspberrypi-kernel raspberrypi-kernel-headers` and verify with `apt-mark showholds` that they are
marked to skip from auto-upgrade.

### Berryboot

Berryboot contains more or less the same content as Raspbian's `/boot` or Ubuntu's `/boot/firmware`.

Six files have no equivalents in Ubuntu's `/boot/firmware`:

```
-rw-r--r--  6.3 unx   171994 bx defN 20-Nov-03 16:12 bbloader.img
-rw-r--r--  6.3 unx    47661 bx defN 20-Nov-03 16:12 bcm2711-rpi-cm4.dtb
-rw-r--r--  6.3 unx 10726301 bx defN 20-Nov-03 16:12 berryboot.img
-rw-r--r--  6.3 unx 16847360 bx defN 20-Nov-03 16:12 kernel_rpi64.img
-rw-rw-r--  6.3 unx    32926 bx defN 19-Oct-05 02:15 LICENSE.berryboot
-rw-r--r--  6.3 unx 22577152 bx defN 20-Nov-03 16:12 shared.img
```

`bcm2711-rpi-cm4.dtb` is for the Compute Module 4, which is newer than Ubuntu 20.04.1 LTS:

```
	compatible = "raspberrypi,4-compute-module\0brcm,bcm2711";
	model = "Raspberry Pi Compute Module 4";
```

The other 4 image files are:

```
bbloader.img:     lzop compressed data - version 1.030, LZO1X-999, os: Unix
berryboot.img:    lzop compressed data - version 1.030, LZO1X-999, os: Unix
kernel_rpi64.img: Linux kernel ARM64 boot executable Image, little-endian, 4K pages
shared.img:       Squashfs filesystem, little endian, version 4.0, zlib compressed, 22574751 bytes, 2016 inodes, blocksize: 131072 bytes, created: Tue Nov  3 15:12:24 2020
```

`unsquashfs -ll shared.img` shows that it basically driver firmwares and kernel modules:

```
drwxr-xr-x root/root                26 2019-10-05 02:15 squashfs-root
drwxr-xr-x root/root                58 2020-10-30 15:40 squashfs-root/lib
drwxr-xr-x root/root               359 2020-11-03 15:10 squashfs-root/lib/firmware
...
drwxrwxr-x root/root                32 2020-11-03 15:11 squashfs-root/lib/modules
drwxr-xr-x root/root               327 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64
drwxr-xr-x root/root               149 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/kernel
...
-rw-r--r-- root/root            544762 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.alias
-rw-r--r-- root/root            570240 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.alias.bin
-rw-r--r-- root/root             14566 2020-11-03 15:11 squashfs-root/lib/modules/5.4.73v64/modules.builtin
-rw-r--r-- root/root             16383 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.builtin.bin
-rw-r--r-- root/root             65925 2020-11-03 15:11 squashfs-root/lib/modules/5.4.73v64/modules.builtin.modinfo
-rw-r--r-- root/root            189748 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.dep
-rw-r--r-- root/root            261937 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.dep.bin
-rw-r--r-- root/root               324 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.devname
-rw-r--r-- root/root             62341 2020-11-03 15:11 squashfs-root/lib/modules/5.4.73v64/modules.order
-rw-r--r-- root/root               329 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.softdep
-rw-r--r-- root/root            256144 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.symbols
-rw-r--r-- root/root            315006 2020-11-03 15:12 squashfs-root/lib/modules/5.4.73v64/modules.symbols.bin
```

The two lzop compressed images are cpio archives. Their contents can be examined with:

```
lzop -dc bbloader.img | cpio -tv
lzop -dc berryboot.img | cpio -tv
```

`bbloader.img` contains a busybox binary with a small init script; `berryboot.img` contains `BerrybootGUI` in a small OS with networking and
misc filesystem support.

`config.txt` contains a line `kernel=kernel_rpi64.img`, so `kernel_rpi64.img` functions like Raspbian's `kernel8.img`
or Ubuntu's `uboot_rpi_4.bin` (`kernel=uboot_rpi_4.bin`).
