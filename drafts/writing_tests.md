---
author: Lucas Ramage
date: 2020-01-07
---

# Writing Tests for PRoot / CARE

The testsuite needs lots of love, but for now, here are some steps to add a new test.

There are more than one type of test, compiled tests written in c, or shell scripts.

If you take a peek at [`test/GNUmakefile`](https://github.com/proot-me/proot/blob/master/test/GNUmakefile), you will see some tests are hard-coded in there for some reason.

I recently added [`test/test-python01.sh`](https://github.com/proot-me/proot/blob/master/test/test-python01.sh), so you can use that one as a proper example.

Per the [release guide](https://github.com/proot-me/proot/blob/master/doc/howto-release.rst),

> All shell scripts must pass shellcheck.

If you aren't familiar with [shellcheck](https://www.shellcheck.net), then I highly recommend you check it out.

You can run the testsuite with `V=1` for verbose. That really helps, but it is very verbose.

To run a single test, prepend `check-` to the filename of the test you wish to run.

```sh
make -C test check-test-python01.sh # run single test
```

### See Also

- [Meaningful names for tests](https://github.com/proot-me/proot/issues/164)
