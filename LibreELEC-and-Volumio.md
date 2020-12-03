# Other Raspberry Pi distributions:

## LibreELEC and Volumio

http://releases.libreelec.tv/LibreELEC-RPi4.arm-9.2.6.img.gz

zcat LibreELEC-RPi4.arm-9.2.6.img.gz > LibreELEC-RPi4.arm-9.2.6.img

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

This appears to be `1.20200512-2` or `1.20200601-1` (See Raspbian-Kernel-Releases.md).

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
