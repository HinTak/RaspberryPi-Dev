## Raspbian

For the purpose of downgrading from 5.10.17 (`raspberrypi-kernel_1.20210303-1`) back to the last of 5.4 kernel (`raspberrypi-kernel_1.20210108-1`), to
investigate driver breakage, I did:

```
wget -O kernel-headers_armhf.deb http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel-headers_1.20210108-1_armhf.deb
wget -O kernel_armhf.deb http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel_1.20210108-1_armhf.deb
sudo dpkg -i kernel-headers_armhf.deb kernel_armhf.deb
sudo apt-mark hold raspberrypi-kernel-headers raspberrypi-kernel
sudo apt-mark showhold 
```

The last step (`sudo apt-mark showhold`) is for verification. The `sudo apt-mark hold` step stops them from getting auto-upgraded.
For switching to other kernel versions, refer to the table in [Raspbian Kernel Releases](Raspbian-Kernel-Releases.md).

To unhold (again, repeating the first step of verifying current holds), and go back to the latest:

```
sudo apt-mark showhold 
sudo apt-mark unhold raspberrypi-kernel-headers raspberrypi-kernel
sudo apt-get install raspberrypi-kernel-headers raspberrypi-kernel
```

**Note (Another Tested Alternative):** To downgrade to the last of the 4.19 (`raspberrypi-kernel_1.20200601-1`), replace the first two steps with:

```
wget -O kernel-headers_armhf.deb http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel-headers_1.20200601-1_armhf.deb
wget -O kernel_armhf.deb http://archive.raspberrypi.org/debian/pool/main/r/raspberrypi-firmware/raspberrypi-kernel_1.20200601-1_armhf.deb
```

## Ubuntu Pi Server

Ubuntu Pi Server is different from both Raspbian and Ubuntu x86/x86_64 Desktop in its boot process. It allows multiple kernel packages to be
installed, and it does not automatically downgrade when one installs an earlier kernel package.

What happens is that it copies the highest version available from `/boot` into `/boot/firmware`. To downgrade, one simply copies and over-write the pair of
`/boot/initrd.img-x.y.z-NNNN-raspi` to `/boot/firmware/initrd.img`, and `/boot/vmlinuz-x.y.z-NNN-raspi` to /boot/firmware/vmlinuz` .

For example, here is my current listing of `/boot` . `5.4.0-1032-raspi` is the latest from Ubuntu, and it was copied into `/boot/firmware` when first installed.
I am able to switch between `5.4.0-1030` and `5.8.0-1013` (both built by me to work around [respeaker bug #251](https://github.com/respeaker/seeed-voicecard/issues/251)) by copying the corresponding pair into `/boot/firmware`.

```
ubuntu@ubuntu:~$ ls -l /boot
total 144851
-rw-------  1 root root  4265721 Mar 12 03:01 System.map-5.4.0-1030-raspi
-rw-------  1 root root  4167287 Mar 19 20:05 System.map-5.4.0-1032-raspi
-rw-------  1 root root  5021435 Feb 15 12:02 System.map-5.8.0-1013-raspi
-rw-r--r--  1 root root   220924 Mar 12 03:01 config-5.4.0-1030-raspi
-rw-r--r--  1 root root   219032 Mar 19 20:05 config-5.4.0-1032-raspi
-rw-r--r--  1 root root   230013 Feb 15 12:02 config-5.8.0-1013-raspi
lrwxrwxrwx  1 root root       43 Apr  7 21:26 dtb -> dtbs/5.8.0-1013-raspi/./bcm2711-rpi-4-b.dtb
lrwxrwxrwx  1 root root       43 Apr  7 20:15 dtb-5.4.0-1030-raspi -> dtbs/5.4.0-1030-raspi/./bcm2711-rpi-4-b.dtb
lrwxrwxrwx  1 root root       43 Apr  7 20:05 dtb-5.4.0-1032-raspi -> dtbs/5.4.0-1032-raspi/./bcm2711-rpi-4-b.dtb
lrwxrwxrwx  1 root root       43 Apr  7 21:26 dtb-5.8.0-1013-raspi -> dtbs/5.8.0-1013-raspi/./bcm2711-rpi-4-b.dtb
drwxr-xr-x 11 root root     4096 Apr  7 21:26 dtbs
drwxr-xr-x  3 root root     6656 Jan  1  1970 firmware
lrwxrwxrwx  1 root root       27 Apr  7 21:25 initrd.img -> initrd.img-5.8.0-1013-raspi
-rw-r--r--  1 root root 30253278 Apr  7 20:15 initrd.img-5.4.0-1030-raspi
-rw-r--r--  1 root root 29954806 Apr  7 20:05 initrd.img-5.4.0-1032-raspi
-rw-r--r--  1 root root 31926938 Apr  7 21:26 initrd.img-5.8.0-1013-raspi
lrwxrwxrwx  1 root root       27 Apr  7 21:25 initrd.img.old -> initrd.img-5.4.0-1030-raspi
lrwxrwxrwx  1 root root       24 Apr  7 21:25 vmlinuz -> vmlinuz-5.8.0-1013-raspi
-rw-------  1 root root  8567401 Mar 12 03:01 vmlinuz-5.4.0-1030-raspi
-rw-------  1 root root  8392836 Mar 19 20:05 vmlinuz-5.4.0-1032-raspi
-rw-------  1 root root 25047552 Feb 15 12:02 vmlinuz-5.8.0-1013-raspi
lrwxrwxrwx  1 root root       24 Apr  7 21:25 vmlinuz.old -> vmlinuz-5.4.0-1030-raspi
```

The backup `vmlinuz.bak` and `initrd.img.bak` in `/boot/firmware/` are `5.4.0-1030` when it was the highest version.

```
ubuntu@ubuntu:~$ ls -l /boot/firmware/vm*
-rw-r--r--. 1 root root 25047552 Apr  7 21:26 /boot/firmware/vmlinuz
-rw-r--r--. 1 root root  8567401 Mar 12 02:01 /boot/firmware/vmlinuz.bak
ubuntu@ubuntu:~$ ls -l /boot/firmware/init*
-rw-r--r--. 1 root root 31926938 Apr  7 21:26 /boot/firmware/initrd.img
-rw-r--r--. 1 root root 30253278 Apr  7 20:15 /boot/firmware/initrd.img.bak
```
