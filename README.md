# Rapid Linux Kernel Driver Development

See also [Raspbian Kernel Versions](Raspbian-Kernel-Releases.md), and Information about [LibreELEC, Volumio and Berryboot](LibreELEC-and-Volumio.md).
and [Customizing Berryboot with additional drivers](Customizing-Berryboot.md).

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
they would be found, updating the kernel dependencies with `depmod -a`, clear the kernel ring buffer, and re-load them:

```
$ sudo cp *.ko /lib/modules/5.4.0-1016-raspi/updates/dkms/
$ sudo depmod -a
$ sudo dmesg -c > /dev/null
$ sudo /usr/bin/seeed-voicecard
```

`/usr/bin/seeed-voicecard` more or less just reloads the device tree overlay, which also has the side-effect of probing for all the individual drivers.

## Tracing kernel driver execution

Many simple facilities are **not** available within the kernel. You cannot do `assert()`, and you cannot do `printf()`. Also, by its own nature,
many kernel routines are executed **asynchronously**, triggered by external events happening somehwhat simultaneously, processed by different CPU cores, too.

- `dump_stack()` does what it says: dump the call stack to the kernel ring buffer (i.e. to what `dmesg` shows). Useful when you cannot work out what calls what.

- `pr_info()` works like `printf()`, but like `dump_stack()`, writes to the kernel ring buffer. `dev_info()` is a variant which prepends device name.
Other variants of `pr_*()` and `dev_*()` exist. See `man 2 syslog` for details.

- In kernel code, the equivalent of `assert()` in normal application code, is `BUG_ON()`. A full abort with `BUG_ON()`, immediately killing
the current kernel thread, is very seldomly used though; instead, `WARN_ON()` let you do a more controlled abort. Both run `dump_stack()`,
in addition to dumping registers' content.

- There are more advanced technique with dynamic debugging, like `kernelshark` / `trace-cmd` (Ftrace), and `dtrace` (Systemtap) .

- The idea of "attaching a debugger" to a debug-enabled kernel, requires a second machine connected by a serial (RS-232) connection, with [KGDB](http://kgdb.wiki.kernel.org).

[Linux Device Drivers, Third Edition](https://lwn.net/Kernel/LDD3/) is the authoritative though somewhat dated reference.
