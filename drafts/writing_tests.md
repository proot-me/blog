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

## Example Script

Copy the following contents to the `test/test-example.sh` file:

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

## Running a Single Test

To run a single test, prepend `check-` to the filename of the test you wish to run.

```sh
make -C test check-test-example.sh # run single test
```

## Running Entire Testsuite

```sh
make -C test
```

## Exit Codes

| Code | Status  |
|------|---------|
| 0    | ok      |
| 125  | skipped |
| *    | FAILED  |

Note: these are defined in [`test/GNUmakefile`](https://github.com/proot-me/proot/blob/master/test/GNUmakefile#L91).

## Troubleshooting

The testsuite can be run with `V=1` for more verbose output.

```sh
make -C test check-test-example.sh V=1
```

## Test Requirements

Per the [release guide](https://github.com/proot-me/proot/blob/master/doc/howto-release.rst),

> All shell scripts must pass shellcheck.

```sh
shellcheck test/check-test-example.sh
```

### Reference

- [Appendix E. Exit Codes With Special Meanings](http://tldp.org/LDP/abs/html/exitcodes.html)
- [shellcheck](https://www.shellcheck.net)

### See Also

- [Meaningful names for tests](https://github.com/proot-me/proot/issues/164)
