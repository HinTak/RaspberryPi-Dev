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

touch records

-rw-r--r-- 1 pi pi 0 Apr 20 13:06 73:e957204e732bc2916a241dc61dd7dd14e9a98350-okay
-rw-r--r-- 1 pi pi 0 Apr 20 11:57 75:353e16bf60458fae5927cf04ff668fc152fff465-okay
-rw-r--r-- 1 pi pi 0 Apr 20 10:45 71:25317997cbcd0497a9460f79c322e0fde0b5b1a9-broken
-rw-r--r-- 1 pi pi 0 Apr 20 09:23 61:a49e460f847f54600fb1fdae63765eb54ae659cc-broken
-rw-r--r-- 1 pi pi 0 Apr 20 07:47 80:753ace0a34fbd39ac2ec654c6859823db420f69e-okay
-rw-r--r-- 1 pi pi 0 Apr 20 06:32 44:b18768f56162964f70bbb9119dba59a947d7d577-broken

$ git log --format=oneline
e6394eace63b503cbd3c897a9d1fef040f4d0c38 (HEAD -> rpi-5.5.y) 72:4378f1fbe924054a09ff0d4e39e1a581b9245252
798fca4dfacbbd7275c7839a38fff20d0d93fc41 Revert "72:4378f1fbe924054a09ff0d4e39e1a581b9245252"
4707b74857c521d0207a9b15aac4bf911b326c1b 72:4378f1fbe924054a09ff0d4e39e1a581b9245252
51b70d20687987c0bfa067574af0b388d56cd313 73:e957204e732bc2916a241dc61dd7dd14e9a98350
ff3073ac69d3b5ffb3abf2e01a7ae78405a60901 75:353e16bf60458fae5927cf04ff668fc152fff465
37a16865ca501d9f3a2171ee34228f0777a685d4 71:25317997cbcd0497a9460f79c322e0fde0b5b1a9
7a6171b2ad11ec622eb24c1c7f9970d2129d7764 61:a49e460f847f54600fb1fdae63765eb54ae659cc
9348f1ba9ec9934c5307af9a3b60fe79a0127c80 80:753ace0a34fbd39ac2ec654c6859823db420f69e
74c2580ffc1d787b6d6fee2c5c919f94eeac1600 44:b18768f56162964f70bbb9119dba59a947d7d577
71be3a025714b3e6f19cd1b4c1af9dabf062a074 1:e5dbdcb31285a975d623d2bf2c9e7b2940489008
e35049b8ac349543b16a823e7f1e97f3afa1b9b7 82:1c93a9e00e40e3a084ebbcaa4a8f412b83f1fa0d
f2cfdb734e6bb20c5c05a1d6ebf1e304f95858e3 ASoC: core: only convert non DPCM link to DPCM link
8d9fdf132e9c72a6e1fe6ee7493743953f45dbc2 Revert "ASoC: soc-core: disable route checks for legacy devices"
6f5e2bae8dbd834bf7b33949074d32d73d144d28 ASoC: soc-component.h: remove GPL explanation from header
722a54a94082d7cee0935b58a19ea71cd7dfd3d9 ASoC: soc-core: disable route checks for legacy devices
f7c177d3e693ca275ba04bca5baf4910501bb3db ASoC: soc-core: move soc_free_pcm_runtime()
b7bd3bd049c0865d67268bc4cec29ff1412e0b3a ASoC: soc-core: move snd_soc_register_dai()
e8769060c1dd1878db88348e7be5274ea981022f ASoC: soc-core: move snd_soc_lookup_component()
0c0d357003d3e04a9ec2b1465c09927132104f71 Revert "try stock 5.5 again to test .non_legacy_dai_naming"
08ca61ba37e92a096e2023aaf8e604e9cc0eeb36 try stock 5.5 again to test .non_legacy_dai_naming
ea92632fd20b461fc54463e515fbe357a106bd2d This also breaks!
523d26ed685cdc55865a77cb22776a0954f17d53 This seems to break!
a0d2e40df1fab6c178d6fdf6f66c260cd1eb8972 files unlikely to matter
8a17fa70872277f8428d582470fb4d74d7576d9b ASoC: add control components management
abad56c6ed4d766f90e599a56e894da9aa07466b ASoC: soc-core: add snd_soc_pcm_lib_ioctl()
789aa255159d302280b95bbb95bce26617f820e4 ASoC: soc-core: remove for_each_rtdcom_safe()
402653a6913aaa2057488d613c92a78e7c72bd93 ASoC: soc-dpcm: tidyup for_each_dpcm_xx() macro
c2743a4f4f7d53eb3168137dc6602ac682be6faa ASoC: soc.h: dobj is used only when SND_SOC_TOPOLOGY
881b4da006c43b613938b8b46d472a6b3b7528db ASoC: core: add SND_SOC_BYTES_E
e48ca5f7275cfc1517dd0c55de9af586b2d0676a ASoC: soc-core: disable route checks for legacy devices
f4f403ae697e5c9b3f6e7e5c1028fcd6f6377564 include/sound/soc.h
6259663241c82ce45c8c2a7f02e0dd4f1ea479e8 include/sound/soc-* and sound/soc-*
0f7754f9fab726b4cd42d2dc3c90d8830f7e79ab Revert "include/sound/soc* and sound/soc*"
06efb053e9086d840c806461ffbf75cee5856df1 include/sound/soc* and sound/soc*
971b73fe4bfee128c7d2a60b86b5e41ff2aff48b Wrong message
368812e07da4c50835ab43aeab978aa3d599bbb7 include/sound/soc* and sound/soc*
96a3278497cc6540d32dc4a7dba9100da6df16f0 (grafted, origin/rpi-5.5.y) kbuild: Disable gcc plugins
