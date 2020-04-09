---
author: Lucas Ramage
date: 2020-01-07
---

# Writing Tests for PRoot / CARE

Here are the steps required for adding a new test script.

## Test Types

There are more than one type of test:

- test programs written in C
- POSIX shell scripts

This post will highlight the second option, since there are more of them in the testsuite currently.

Note: if you take a peek at [`test/GNUmakefile`](https://github.com/proot-me/proot/blob/master/test/GNUmakefile), you will see the rules for compiling test programs; however, several of these tests names are hard-coded in this file.

## Running a Single Test

To run a single test, prepend `check-` to the filename of the test you wish to run.

```sh
make -C test check-test-python01.sh # run single test
```

## Example

```sh
#!/bin/sh
# Test description here
set -eu

# Check for test dependencies
for cmd in file; do
  if ! command -v "${cmd}" > /dev/null; then
      exit 125
  fi
done

# Check for PRoot binary
if [ ! -e "${PROOT}" ]; then
    exit 125
fi

# Explicitly define test case here
if [ 1 ]; then
    exit 1 # this test will fail
fi
```

Recently, [`test/test-python01.sh`](https://github.com/proot-me/proot/blob/master/test/test-python01.sh) was added
to test the new Python extension. It can be used as a reference for writing a new test script.

## Troubleshooting

The testsuite can be run with `V=1` for more verbose output.

## Test Requirements

Per the [release guide](https://github.com/proot-me/proot/blob/master/doc/howto-release.rst),

> All shell scripts must pass shellcheck.

### Reference

- [shellcheck](https://www.shellcheck.net)

### See Also

- [Meaningful names for tests](https://github.com/proot-me/proot/issues/164)
