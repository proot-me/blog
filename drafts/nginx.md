---
author: Lucas Ramage
date: 2020-04-25
---

# Running NGINX in PRoot

## Rootfs

[previous post on Alpine](../posts/alpine-aarch64.md)

LXC unprivileged configuration directory

```sh
mkdir -p ~/.local/var/lib/lxc/alpine
```

Source: https://github.com/lxc/lxc/blob/master/doc/lxc.system.conf#L11

## Configuration

```sh
useradd nginx
```

Port 8080

## Troubleshooting

Git error with ca-certificates

### Reference

https://docs.nginx.com/nginx/admin-guide/web-server/web-server

https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html#variables

- [lxc.system.conf](https://linuxcontainers.org/lxc/manpages//man5/lxc.system.conf.5.html)

### See Also

https://github.com/proot-me/proot/issues/191
