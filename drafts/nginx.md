---
author: Lucas Ramage
date: 2020-04-25
---

# Running NGINX in PRoot

## Setup for Alpine Linux

These instructions make use of the [LXC unprivileged configuration directory](https://github.com/lxc/lxc/blob/master/doc/lxc.system.conf#L11)

```sh
export PROOT_ROOTFS_DIR="~/.local/var/lib/lxc/alpine"
mkdir -p "${PROOT_ROOTFS_DIR}"
cd "${PROOT_ROOTFS_DIR}"
curl -o rootfs.tar.gz http://dl-cdn.alpinelinux.org/alpine/v3.11/releases/x86_64/alpine-minirootfs-3.11.6-x86_64.tar.gz
tar -xf rootfs.tar.gz
rm rootfs.tar.gz
```

**Note: these steps were adapted from the [Static binaries for AArch64 using Alpine Linux](alpine-aarch64.md#compiling) article.**

## Configuration

```sh
proot -S . /sbin/apk add git nginx shadow

useradd nginx

mkdir -p /run/nginx
touch /run/nginx/nginx.pid
```

Port 8080

## Troubleshooting

```sh
Cloning into '/var/www/localhost/htdocs'...
fatal: unable to access 'https://github.com/proot-me/proot-me.github.io.git/': error setting certificate verify locations:
  CAfile: /etc/ssl/certs/ca-certificates.crt
  CApath: none
```

```sh
Executing busybox-1.31.1-r9.trigger
ERROR: busybox-1.31.1-r9.trigger: script exited with error 127
Executing ca-certificates-20191127-r1.trigger
ERROR: ca-certificates-20191127-r1.trigger: script exited with error 127
2 errors; 26 MiB in 24 packages
```

See: [Cannot compile software inside a proot environment](https://github.com/proot-me/proot/issues/191)

### Reference

- [Configuring NGINX and NGINX Plus as a Web Server](https://docs.nginx.com/nginx/admin-guide/web-server/web-server)

- [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html#variables)

- [lxc.system.conf](https://linuxcontainers.org/lxc/manpages//man5/lxc.system.conf.5.html)
