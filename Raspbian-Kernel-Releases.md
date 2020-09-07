# Raspbian Kernel Releases

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

**TL;DR** Below are three methods of getting at this information, for those who want to know.

# Method 1

The basic method is to read the beginning of the top-level `Makefile`.

First, we get a list of tags we are interested in:

```
$ git tag | grep raspberrypi-kernel_1.2020
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
```

Then we read each of them like this:

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

# Method 3

`git branch -a --contains` is a lot slower, but the information is more direct:

```
$  git tag | grep raspberrypi-kernel_1.2020 | xargs -n 1 -t git branch -a --contains
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
```

Interestingly, this shows `raspberrypi-kernel_1.20200527-1` and `raspberrypi-kernel_1.20200601+arm64-1` are not in any branches.
