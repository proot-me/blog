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
rm alpine-minirootfs-3.11.3-aarch64.tar.gz
cd ..

# Configure DNS resolution
cp /etc/resolv.conf alpine/etc/

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

**Note: these packages are listed in the [Dependencies](../posts/dependencies.md) article.**

```
# Clone proot repository to rootfs
mkdir -p alpine/usr/src/
git clone https://github.com/proot-me/proot.git alpine/usr/src/proot
```

**Note: this step was performed from the host, not from within the Alpine rootfs**

```sh
# Compile loader
proot -q qemu-aarch64 \
      -S alpine/ /bin/sh -c 'LDFLAGS="${LDFLAGS} -static" make -C /usr/src/proot/src loader.elf build.h'

# Compile static proot
proot -q qemu-aarch64 \
      -S alpine/ /bin/sh -c 'LDFLAGS="${LDFLAGS} -static" make -C /usr/src/proot/src proot'

cp alpine/usr/src/proot/src/proot .
file proot
```

## Troubleshooting

```sh
/usr/lib/gcc/aarch64-alpine-linux-musl/9.2.0/../../../../aarch64-alpine-linux-musl/bin/ld: cannot find -ltalloc
/usr/lib/gcc/aarch64-alpine-linux-musl/9.2.0/../../../../aarch64-alpine-linux-musl/bin/ld: cannot find -lpython2.7

ld -ltalloc --verbose
ld -lpython2.7 --verbose
```

### References

- https://stackoverflow.com/a/21647591/8507637
