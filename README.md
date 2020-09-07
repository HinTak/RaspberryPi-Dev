# Rapid Linux Kernel Driver Development

## Unload / Reload without rebooting

Apparently, this useful tips is not well-known : that it is possible to unload / reload linux kernel modules by using `modprobe`.
You can repeatedly do `modprobe -r` to unload, modify, `modprobe -v` (`-v` for verbose) to reload indefinitely, to test new code changes
without rebooting.

Using [respeaker](https://github.com/respeaker/seeed-voicecard) as an example. Firstly, you need to make sure that
no applications or background processes (like `pulseaudio` ...) are using the devices. Then you remove the devicetree
overlay, before you unload the individual drivers. The steps need to be done according to dependency orders.

What depends on what, can be found by `lsmod`; and `lsof` if an application is keeping a device opened.

```
$ sudo /opt/vc/bin/dtoverlay -d /boot/firmware/ -R seeed-8mic-voicecard
$ sudo modprobe -r snd_soc_ac108
$ sudo modprobe -r snd_soc_seeed_voicecard
$ sudo modprobe -r snd_soc_simple_card_utils
```

Then you modify and build your kernel modules (in [respeaker](https://github.com/respeaker/seeed-voicecard), by just doing `make`).
`uname -r` shows `5.4.0-1016-raspi`, my currently running kernel. You copy the newly built kernel modules into the place where
they would be found, updating the kernel dependencies with `depmod -a`, and re-load them:

```
$ sudo cp *.ko /lib/modules/5.4.0-1016-raspi/updates/dkms/
$ sudo depmod -a
$ sudo /usr/bin/seeed-voicecard
```

`/usr/bin/seeed-voicecard` more or less just reloads the device tree overlay, which also has the side-effect of probing for all the individual drivers.

## Tracing kernel driver execution

Many simple facilities are **not** available within the kernel. You cannot do `assert()`, and you cannot do `printf()`. Also, by its own nature,
many kernel routines are executed **asynchronously**, triggered by external events happening somehwhat simultaneously, processed by different CPU cores, too.

- `dump_stack()` does what it says: dump the call stack to the kernel ring buffer (i.e. to what `dmesg` shows). Useful when you cannot work out what calls what.

- `pr_info()` works like `printf()`, but like `dump_stack()`, writes to the kernel ring buffer. `dev_info()` is a variant which prepends device name.
Other variants of `pr_*()` and `dev_*()` exist. See `man 2 syslog` for details.

- There are more advanced technique with dynamic debugging, like `kernelshark` / `trace-cmd` (Ftrace), and `dtrace` (Systemtap) .