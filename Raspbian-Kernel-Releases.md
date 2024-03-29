# Raspbian Kernel Releases

| Tags | Linux Kernel Version |
| ---  | --- |
| raspberrypi-kernel_1.20200114-1       | 4.19.93 |
| raspberrypi-kernel_1.20200205-1       | 4.19.97 |
| raspberrypi-kernel_1.20200210-1       | 4.19.97 |
| raspberrypi-kernel_1.20200212-1       | 4.19.97 |
| raspberrypi-kernel_1.20200512-2       | 4.19.118 |
| raspberrypi-kernel_1.20200527-1       | 5.4.42 |
| raspberrypi-kernel_1.20200601+arm64-1 | 5.4.42 |
| raspberrypi-kernel_1.20200601-1       | 4.19.118 |
| raspberrypi-kernel_1.20200717-1       | 5.4.51 |
| raspberrypi-kernel_1.20200723-1       | 5.4.51 |
| raspberrypi-kernel_1.20200811-1       | 5.4.51 |
| raspberrypi-kernel_1.20200819-1       | 5.4.51 |
| raspberrypi-kernel_1.20200902-1       | 5.4.51 |
| raspberrypi-kernel_1.20200902-1       | 5.4.51 |
| raspberrypi-kernel_1.20201022-1       | 5.4.72 |
| raspberrypi-kernel_1.20201126-1       | 5.4.79 |
| raspberrypi-kernel_1.20201201-1       | 5.4.79 |
| raspberrypi-kernel_1.20210104-1       | 5.4.83 |
| raspberrypi-kernel_1.20210108-1       | 5.4.83 |
| raspberrypi-kernel_1.20210201-1       | 5.10.11 |
| raspberrypi-kernel_1.20210303-1       | 5.10.17 |
| raspberrypi-kernel_1.20210430-1       | 5.10.17 |
| raspberrypi-kernel_1.20210527-1       | 5.10.17 |
| 1.20210727                            | 5.10.52 |
| 1.20210805                            | 5.10.52 |
| 1.20210831                            | 5.10.60 |
| 1.20210928                            | 5.10.63 |
| 1.20211007                            | 5.10.63 |
| 1.20211029                            | 5.10.63 |
| 1.20230306                            | 5.15.84 |
| 1.20230317                            | 6.1.19  |
| 1.20230405                            | 6.1.21  |

`1.20200527-1` and `1.20200601+arm64-1` are 64-bit Aarch64-only releases, while the rest, up to `1.20200601-1`, are 32-bit only.
`1.20200717-1` was the first release available in both forms.

**TL;DR** Below are three methods of getting at this information, for those who want to know.

# Method 1

The basic method is to read the beginning of the top-level `Makefile`.

First, we get a list of tags we are interested in:

```
$ git tag | grep raspberrypi-kernel_1.202
raspberrypi-kernel_1.20200114-1
raspberrypi-kernel_1.20200205-1
raspberrypi-kernel_1.20200210-1
raspberrypi-kernel_1.20200212-1
raspberrypi-kernel_1.20200512-2
raspberrypi-kernel_1.20200527-1
raspberrypi-kernel_1.20200601+arm64-1
raspberrypi-kernel_1.20200601-1
raspberrypi-kernel_1.20200717-1
raspberrypi-kernel_1.20200723-1
raspberrypi-kernel_1.20200811-1
raspberrypi-kernel_1.20200819-1
raspberrypi-kernel_1.20200902-1
raspberrypi-kernel_1.20201022-1
raspberrypi-kernel_1.20201126-1
raspberrypi-kernel_1.20201201-1
raspberrypi-kernel_1.20210104-1
raspberrypi-kernel_1.20210107.master-1
raspberrypi-kernel_1.20210108-1
raspberrypi-kernel_1.20210108.master-1
raspberrypi-kernel_1.20210111.master-1
raspberrypi-kernel_1.20210201-1
raspberrypi-kernel_1.20210303-1
raspberrypi-kernel_1.20210430-1
raspberrypi-kernel_1.20210527-1
```

Then we read each of their actual kernel version like this:

```
$ git show raspberrypi-kernel_1.20200114-1:Makefile | head
# SPDX-License-Identifier: GPL-2.0
VERSION = 4
PATCHLEVEL = 19
SUBLEVEL = 93
EXTRAVERSION =
NAME = "People's Front"

# *DOCUMENTATION*
# To see a list of typical targets execute "make help"
# More info can be located in ./README
```

So `raspberrypi-kernel_1.20200114-1` corresponds to kernel `4.19.93`.

# Method 2

`git log x..y` is empty, if `x` includes `y` as ancestor. Thus it is possible to work out that some recent tags are **not** in the `rpi-4.19.y` branch:

```
$ git tag | grep raspberrypi-kernel_1.202 | xargs -n 1 -I{} git log -1 --decorate raspberrypi/rpi-4.19.y..{} | grep ^commit | uniq
commit 971a2bb14b459819db1bda8fcdf953e493242b42 (tag: raspberrypi-kernel_1.20200601+arm64-1, tag: raspberrypi-kernel_1.20200527-1)
commit 3e92327867b6b40fed95ed36bb20f56d6e0cc879 (tag: raspberrypi-kernel_1.20200717-1)
commit f0eeafb5d9109c2a1d512f045d7326a8a1d33e7e (tag: raspberrypi-kernel_1.20200723-1)
commit f2f7e4b23d8788e96f81a7522b2f703e51c53e70 (tag: raspberrypi-kernel_1.20200902-1, tag: raspberrypi-kernel_1.20200819-1, tag: raspberrypi-kernel_1.20200811-1)
commit ff93994fb3f92070d8521d709ad04675ecaa5817 (tag: raspberrypi-kernel_1.20201022-1)
commit 9797f1a4938c20139b00a25de93cc99efb5c291b (tag: raspberrypi-kernel_1.20201201-1, tag: raspberrypi-kernel_1.20201126-1)
commit 76c49e60e742d0bebd798be972d67dd3fd007691 (tag: raspberrypi-kernel_1.20210108-1, tag: raspberrypi-kernel_1.20210104-1)
```

Likewise, tags **not** in the `rpi-5.4.y` branch:

```
$ git tag | grep raspberrypi-kernel_1.202 | xargs -n 1 -I{} git log -1 --decorate raspberrypi/rpi-5.4.y..{} | grep ^commit | uniq
commit 767722747b98c2d57425f984da2a8192ae30cc69 (tag: raspberrypi-kernel_1.20200114-1)
commit f16e91dad2af9d57aef477cc1f522040353849f5 (tag: raspberrypi-kernel_1.20200212-1, tag: raspberrypi-kernel_1.20200210-1, tag: raspberrypi-kernel_1.20200205-1)
commit fe2c7bf4cad4641dfb6f12712755515ab15815ca (tag: raspberrypi-kernel_1.20200601-1, tag: raspberrypi-kernel_1.20200512-2)
commit 971a2bb14b459819db1bda8fcdf953e493242b42 (tag: raspberrypi-kernel_1.20200601+arm64-1, tag: raspberrypi-kernel_1.20200527-1)
commit fe2c7bf4cad4641dfb6f12712755515ab15815ca (tag: raspberrypi-kernel_1.20200601-1, tag: raspberrypi-kernel_1.20200512-2)
```

`commit 971a2bb14b459819db1bda8fcdf953e493242b42` is not in either of the branches.

This method has the advantage of showing which tags correspond to the same commit (just built differently). These take a minute or two.

# Method 3

`git branch -a --contains` is a lot slower (the below took over two hours!), but the information is more direct:

```
$  git tag | grep raspberrypi-kernel_1.202 | xargs -n 1 -t git branch -a --contains
git branch -a --contains raspberrypi-kernel_1.20200114-1 
  remotes/raspberrypi/rpi-4.19.y
git branch -a --contains raspberrypi-kernel_1.20200205-1 
  remotes/raspberrypi/rpi-4.19.y
git branch -a --contains raspberrypi-kernel_1.20200210-1 
  remotes/raspberrypi/rpi-4.19.y
git branch -a --contains raspberrypi-kernel_1.20200212-1 
  remotes/raspberrypi/rpi-4.19.y
git branch -a --contains raspberrypi-kernel_1.20200512-2 
  remotes/raspberrypi/rpi-4.19.y
git branch -a --contains raspberrypi-kernel_1.20200527-1 
git branch -a --contains raspberrypi-kernel_1.20200601+arm64-1 
git branch -a --contains raspberrypi-kernel_1.20200601-1 
  remotes/raspberrypi/rpi-4.19.y
git branch -a --contains raspberrypi-kernel_1.20200717-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20200723-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20200811-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20200819-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20200902-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20201022-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20201126-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20201201-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20210104-1 
  remotes/raspberrypi/rpi-5.4.y
git branch -a --contains raspberrypi-kernel_1.20210108-1 
  remotes/raspberrypi/rpi-5.4.y
```

Interestingly, this shows `raspberrypi-kernel_1.20200527-1` and `raspberrypi-kernel_1.20200601+arm64-1` are not in any branches.

# Method 4

The 14GB [firmware repository](git@github.com:RPi-Distro/firmware.git) has a `changelog` file. A shallow clone is sufficient for this purpose.
The `changelog` contains information about kernel versions back to Sept 2019, and March 2018:

```
$ grep -E '(raspberrypi-firmware|Linux version|Kernel| 4.)' debian/changelog
raspberrypi-firmware (1:1.20230405-1) bullseye; urgency=medium
  * Linux version 6.1.21
raspberrypi-firmware (1:1.20230317-1) bullseye; urgency=medium
  * firmware as of 4c6c5389d55d419e38f00116829d5a0f30c7bfbf
  * Linux version 6.1.19
raspberrypi-firmware (1:1.20230315-1) bullseye; urgency=medium
  * Linux version 6.1.19
raspberrypi-firmware (1:1.20230306-1) bullseye; urgency=medium
  * Linux version 5.15.84
raspberrypi-firmware (1:1.20230106-1) bullseye; urgency=medium
  * Linux version 5.15.84
raspberrypi-firmware (1:1.20221104-1) bullseye; urgency=medium
  * Linux version 5.15.76
raspberrypi-firmware (1:1.20221028-1) bullseye; urgency=medium
  * Linux version 5.15.74
raspberrypi-firmware (1:1.20220830-1) bullseye; urgency=medium
  * firmware as of 48cd70fe84432c5d050637b61e4b7b9c831c98bf
  * Linux version 5.15.61
raspberrypi-firmware (1:1.20220811-1) bullseye; urgency=medium
  * Linux version 5.15.56
raspberrypi-firmware (1:1.20220331-1) bullseye; urgency=medium
  * Linux version 5.15.32
raspberrypi-firmware (1:1.20220328-1) bullseye; urgency=medium
  * Linux version 5.15.30
raspberrypi-firmware (1:1.20220308-2) bullseye; urgency=medium
raspberrypi-firmware (1:1.20220308-1) bullseye; urgency=medium
  * Linux version 5.10.103
raspberrypi-firmware (1:1.20220120-2) bullseye; urgency=medium
raspberrypi-firmware (1:1.20220120-1) bullseye; urgency=medium
  * Linux version 5.10.92
raspberrypi-firmware (1:1.20220118-1) bullseye; urgency=medium
  * Linux version 5.10.92
raspberrypi-firmware (1:1.20220106-1) bullseye; urgency=medium
  * Linux version 5.10.89
raspberrypi-firmware (1:1.20211118-3) bullseye; urgency=medium
raspberrypi-firmware (1:1.20211118-2) bullseye; urgency=medium
raspberrypi-firmware (1:1.20211118-1) bullseye; urgency=medium
  * Linux version 5.10.63
raspberrypi-firmware (1:1.20211029-1) bullseye; urgency=medium
  * Linux version 5.10.63
raspberrypi-firmware (1:1.20211007-2) bullseye; urgency=medium
raspberrypi-firmware (1:1.20211007-1) bullseye; urgency=medium
  * Linux version 5.10.63
raspberrypi-firmware (1:1.20210928-1) bullseye; urgency=medium
  * Linux version 5.10.63
raspberrypi-firmware (1:1.20210831-3) bullseye; urgency=medium
raspberrypi-firmware (1:1.20210831-2) bullseye; urgency=medium
raspberrypi-firmware (1:1.20210831-1) buster; urgency=medium
  * firmware as of 46f356dcecc5204ca9a8cef7e6894e083b231bc7
  * Linux version 5.10.60
raspberrypi-firmware (1:1.20210805-1) buster; urgency=medium
  * Linux version 5.10.52
raspberrypi-firmware (1:1.20210727-1) buster; urgency=medium
  * Linux version 5.10.52
raspberrypi-firmware (1:1.20210527-1) buster; urgency=medium
  * Linux version 5.10.17
raspberrypi-firmware (1:1.20210430-2) buster; urgency=medium
  * Linux version 5.10.17
raspberrypi-firmware (1.20210430-1) buster; urgency=medium
  * Linux version 5.10.17
raspberrypi-firmware (1.20210303-1) buster; urgency=medium
  * Linux version 5.10.17
raspberrypi-firmware (1.20210201-1) buster; urgency=medium
  * Linux version 5.10.11
raspberrypi-firmware (1.20210108-1) buster; urgency=medium
  * Linux version 5.4.83
raspberrypi-firmware (1.20210104-1) buster; urgency=medium
  * Linux version 5.4.83
raspberrypi-firmware (1.20201201-1) buster; urgency=medium
  * Linux version 5.4.79
raspberrypi-firmware (1.20201126-1) buster; urgency=medium
  * Linux version 5.4.79
raspberrypi-firmware (1.20201022-1) buster; urgency=medium
  * Linux version 5.4.72
raspberrypi-firmware (1.20200902-2) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200902-1) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200819-1) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200811-1) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200805-1) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200723-1) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200717-1) buster; urgency=medium
  * Linux version 5.4.51
raspberrypi-firmware (1.20200601-1) buster; urgency=medium
  * Linux version 4.19.118
raspberrypi-firmware (1.20200512-2) buster; urgency=medium
raspberrypi-firmware (1.20200512-1) buster; urgency=medium
  * Linux version 4.19.118
raspberrypi-firmware (1.20200212-1) buster; urgency=medium
  * Linux version 4.19.97
raspberrypi-firmware (1.20200210-1) buster; urgency=medium
  * Linux version 4.19.97
raspberrypi-firmware (1.20200205-1) buster; urgency=medium
  * Linux version 4.19.97
raspberrypi-firmware (1.20200203-1) buster; urgency=medium
  * Linux version 4.19.97
raspberrypi-firmware (1.20200131-1) buster; urgency=medium
  * Linux version 4.19.93
raspberrypi-firmware (1.20200114-1) buster; urgency=medium
  * Linux version 4.19.93
raspberrypi-firmware (1.20191119-1) buster; urgency=medium
  * Linux version 4.19.83
raspberrypi-firmware (1.20190925+1-1) buster; urgency=medium
  * Linux version 4.19.75
raspberrypi-firmware (1.20190925-2) buster; urgency=medium
  * Linux version 4.19.75
raspberrypi-firmware (1.20190925-1) buster; urgency=medium
  * Linux version 4.19.75
raspberrypi-firmware (1.20190916-1) buster; urgency=medium
  * Linux version 4.19.71
...
raspberrypi-firmware (1.20180313-1) stretch; urgency=medium
  * Kernel 4.9.80
...
```

## Method 4a

A variant of the above, parsing the online version of that file, from https://github.com/RPi-Distro/firmware/ :

```
curl https://github.com/RPi-Distro/firmware/ | grep -i debian | cut -f 10 -d '"' \
| xargs -I{} curl https://github.com/{} | grep changelog | cut -f 10 -d '"' \
| xargs -I{} curl https://github.com/{} | grep raw-url | cut -f 4 -d '"' \
| xargs -I{} wget -O - https://github.com/{} | grep -E '(raspberrypi-firmware|Linux version|Kernel| 4.)'
```
