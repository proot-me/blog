---
author: Lucas Ramage
date: 2020-02-27
architecture: aarch64
distribution: alpine
distribution_version: 3.15.0
---

# Static binaries for AArch64 using Alpine Linux

Regarding [Static binaries for ARM using Slackware](slackware-arm.md),

> I have tried to build it using slackware, but it seems to be for arm32 instead of 64. Furthermore there are some errors with reg.c file - missing members of structures mainly. I have even tried to build it in termux, but this resolves in "No rule to make target ".check_process_vm.o". I got the idea of building it using android toolchains, but I guess that wont work either, since proot is detecting the architecture it is built on. What would you suggest?
> <cite>@MarekPetr</cite>

## Compiling

```sh
# Extract aarch64 rootfs
mkdir alpine
cd alpine
wget https://dl-cdn.alpinelinux.org/alpine/v3.15/releases/aarch64/alpine-minirootfs-3.15.0-aarch64.tar.gz
tar -xf alpine-minirootfs-3.15.0-aarch64.tar.gz
rm alpine-minirootfs-3.15.0-aarch64.tar.gz
cd ..

# Fetching dependencies
proot -q qemu-aarch64 \
      -S alpine/ /sbin/apk add bash \
                               bsd-compat-headers \
                               clang \
                               clang-analyzer \
                               coreutils \
                               gcc \
                               git \
                               grep \
                               libarchive-dev \
                               libarchive-static \
                               linux-headers \
                               lzo \
                               make \
                               mcookie \
                               musl-dev \
                               python2-dev \
                               swig \
                               talloc-dev \
                               talloc-static \
                               uthash-dev
```

**Note: these packages are listed in the [Dependencies](dependencies.md) article.**

```
# Clone proot repository to rootfs
mkdir -p alpine/usr/src/
git clone https://github.com/proot-me/proot.git alpine/usr/src/proot
```

**Note: this step was performed from the host, not from within the Alpine rootfs**

```sh
# Compile loader
proot -q qemu-aarch64 \
      -S alpine/ /bin/sh -c 'LDFLAGS="${LDFLAGS} -static -ltalloc" make -C /usr/src/proot/src loader.elf build.h'

# Compile static proot
proot -q qemu-aarch64 \
      -S alpine/ /bin/sh -c 'LDFLAGS="${LDFLAGS} -static -ltalloc" make -C /usr/src/proot/src proot'

cp alpine/usr/src/proot/src/proot .
file proot
```

### Reference

- <https://stackoverflow.com/a/21647591/8507637>
- <https://gitlab.alpinelinux.org/alpine/aports/issues/11269>
