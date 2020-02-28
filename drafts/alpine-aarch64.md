---
author: Lucas Ramage
date: 2020-02-27
---

# Static PRoot for AArch64 using Alpine Linux

```sh
# Extract aarch64 rootfs
mkdir alpine
cd alpine
wget http://dl-cdn.alpinelinux.org/alpine/v3.11/releases/aarch64/alpine-minirootfs-3.11.3-aarch64.tar.gz
tar -xf alpine-minirootfs-3.11.3-aarch64.tar.gz
cd ..

# Fetching dependencies
proot -q qemu-aarch64 -r alpine/ /bin/sh
proot warning: can't chdir("/home/lramage/tmp/./.") in the guest rootfs: No such file or directory
proot info: default working directory is now "/"
/ $ . /etc/profile
devuan:/$ apk add bash \
                  bsd-compat-headers \
                  clang \
                  clang-analyzer \
                  coreutils \
                  gcc \
                  git \
                  grep \
                  libarchive-dev \
                  linux-headers \
                  lzo \
                  make \
                  mcookie \
                  musl-dev \
                  python2-dev \
                  swig \
                  talloc-dev \
                  uthash-dev
```

**Note: these packages are listed in the Dependencies article.**
