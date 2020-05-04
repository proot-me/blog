---
author: Lucas Ramage
date: 2020-01-12
---

# Dependencies

## Distributions

| Distribution | Release | Kernel Version |
| ------------ | ------- | -------------- |
| Alpine       | 3.10    | 4.19.53        |
| CentOS       | 7       | 3.10.0-1062    |
| Debian       | 9.12    | 4.9.210-1      |

## Packages

| Alpine | CentOS | Debian |
|------- | ------ | ------ |
| bsd-compat-headers | * | * |
| bash | * | * |
| clang | clang | clang |
| clang-analyzer | | clang-tools-4.0 |
| coreutils | * | * |
| * | * | curl |
| gcc | * | gcc |
| | | gdb |
| git | git | git |
| grep | * | * |
| | | lcov |
| libarchive-dev | libarchive-devel | libarchive-dev |
| linux-headers | * | * |
| lzo | | |
| make | * | make |
| mcookie | | |
| musl-dev | ** | |
| python2-dev | python-devel | |
| | | sloccount |
| | strace | strace |
| swig | swig | swig |
| talloc-dev | libtalloc-devel | libtalloc-dev |
| uthash-dev | uthash-devel | uthash-dev |

[alpine-ref]: https://alpinelinux.org
[centos-ref]: https://centos.org
[debian-ref]: https://www.debian.org
