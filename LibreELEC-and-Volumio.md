# Other Raspberry Pi distributions:

## LibreELEC and Volumio

http://releases.libreelec.tv/LibreELEC-RPi4.arm-9.2.6.img.gz

zcat LibreELEC-RPi4.arm-9.2.6.img.gz > LibreELEC-RPi4.arm-9.2.6.img

http://updates.volumio.org/pi/volumio/2.853/volumio-2.853-2020-11-20-pi.img.zip

```
-rw-rw-r--. 1 Hin-Tak Hin-Tak 575668224 Dec  3 09:19 LibreELEC-RPi4.arm-9.2.6.img
-rw-rw-r--. 1 Hin-Tak Hin-Tak 135795494 Oct 24 22:05 LibreELEC-RPi4.arm-9.2.6.img.gz
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

### LibreELEC

```
mount -o loop,ro,offset=541065216 LibreELEC-RPi4.arm-9.2.6.img -t ext4 /mnt
mount -o loop,ro,offset=4194304 LibreELEC-RPi4.arm-9.2.6.img -t vfat /mnt
```

`cat /mnt/SYSTEM.md5`:

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

TODO: where should `asound.conf`, `asound.state` be? And, `alctl` misbehaves due to `/sys` not there.

