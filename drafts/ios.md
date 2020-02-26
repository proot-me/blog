---
author: Lucas Ramage
date: 2020-04-01
---

# PRoot on iOS

```sh
$ apk add curl proot --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing
$ mkdir alpine
$ curl -o alpine/rootfs.tar.gz http://dl-cdn.alpinelinux.org/alpine/v3.11/releases/x86/alpine-minirootfs-3.11.3-x86.tar.gz
$ cd alpine
$ tar -xf rootfs.tar.gz
$ rm rootfs.tar.gz
$ cd ..
$ proot -S alpine
# apk add coreutils
# echo "QXByaWwgRm9vbHMh" | base64 -d ; echo
```

[alpine-dl]: https://alpinelinux.org/downloads
[ish]: https://ish.app
