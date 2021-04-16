The `rpi-5.4.y` branch [first diverges](https://stackoverflow.com/questions/1527234/finding-a-branch-point-with-git) from `linux-5.4.y` branch at v5.4.51,
with incremental merge up to `v5.4.83`:

```
$ git describe $(git rev-parse $(git rev-list --first-parent raspberrypi/rpi-5.4.y ^stable/linux-5.4.y | tail -n1)^)
v5.4.51
$ git describe $(git merge-base raspberrypi/rpi-5.4.y stable/linux-5.4.y)
v5.4.83
```

The `rpi-5.5.y` branch is at the moment a simple descendant of `linux-5.5.y` at v5.5.19:

```
$ git describe $(git rev-parse $(git rev-list --first-parent raspberrypi/rpi-5.5.y ^stable/linux-5.5.y | tail -n1)^)
v5.5.19
$ git describe $(git merge-base raspberrypi/rpi-5.5.y stable/linux-5.5.y)
v5.5.19
```

