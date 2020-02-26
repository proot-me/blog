# PRoot/Care Development Dependencies

## Distributions

| Distribution | Release | Kernel Version |
| ------------ | ------- | -------------- |
| Alpine       | 3.10    | 4.19.53        |
| CentOS       | 7       | 3.10.0-1062    |
| Debian       | 9.12    | 4.9.210-1      |

## Packages

| Distribution |                    |      |       |                  |          |      |     |     |     |      |      |                  |               |     |      |         |          |              |           |        |      |                   |            |
| ------------ | ------------------ | ---- | ----- | ---------------- | -------- | ---- | --- | --- | --- | ---- | ---- | ---------------- | ------------- | --- | ---- | ------- | -------- | ------------ | --------- | ------ | ---- | --------------- | -------- |
| Alpine       | bsd-compat-headers | bash | clang | clang-analyzer  | coreutils | *    | gcc |     | git | grep |      | libarchive-dev   | linux-headers | lzo | make | mcookie | musl-dev | python2-dev  |           |        | swig | talloc-dev    | uthash-dev |
| CentOS       | *                  | *    | clang |                 | *         | *    | *   |     | git | *    |      | libarchive-devel | *             |     | *    |         | **       | python-devel |           | strace | swig | libtalloc-devel | uthash-devel |
| Debian       | *                  | *    | clang | clang-tools-4.0 | *         | curl | gcc | gdb | git | *    | lcov | libarchive-dev   | *             |     | make |         |          |              | sloccount | strace | swig | libtalloc-dev | uthash-dev |

[alpine-ref]: https://alpinelinux.org
[centos-ref]: https://centos.org
[debian-ref]: https://www.debian.org
