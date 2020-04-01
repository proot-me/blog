---
author: Lucas Ramage
date: 2020-04-01
---

# PRoot on iOS

Since running PRoot on macOS has been requested multiple times, (See: [#81](https://github.com/proot-me/proot/issues/81), [#155](https://github.com/proot-me/proot/issues/155));
and since using Termux to run PRoot on Android works so wonderfully,
I thought I would take a minute to document running it on iOS.

## Setup for Alpine Linux

```sh
apk add curl proot --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing
mkdir alpine
curl -o alpine/rootfs.tar.gz http://dl-cdn.alpinelinux.org/alpine/v3.11/releases/x86/alpine-minirootfs-3.11.3-x86.tar.gz
cd alpine
tar -xf rootfs.tar.gz
rm rootfs.tar.gz
```

**Note: these steps were adapted from the [Static binaries for AArch64 using Alpine Linux](../posts/alpine-aarch64.md#compiling) article.**

## Running PRoot

There is a special surprise hidden below.

```sh
proot -S alpine
apk add coreutils # contains base64
echo "QXByaWwgRm9vbHMh" | base64 -d ; echo
```

## Troubleshooting

```sh
proot -S alpine
proot info: vpid 1: terminated with signal 31
proot warning: can't readlink '/proc/self/cwd': No such file or directory
```

See: [proot info: pid 19967: terminated with signal 31](https://github.com/proot-me/proot/issues/134)

### Reference

- [Alpine Linux](https://alpinelinux.org)
- [iSH](https://ish.app)
- [PRoot - Termux Wiki](https://wiki.termux.com/wiki/PRoot)
