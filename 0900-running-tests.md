---
title: Running tests
aliases: [ "12-running-tests", "09-running-tests" ]
---

Tipi can detect and run _tests_ and _examples_ as specified in [tipi build conventions](./01-conventions#tests-or-examples).

## Executing **all** tests

To have tipi run all tests after a successful build use the `--test all` command line parameter:

```bash
tipi . -t linux-cxx17 --test all
```

## Executing specific tests

_Example 1:_ Run only tests defined in the executable `mytest`

```bash
tipi . --test mytest
```

The parameter can take a regex pattern to fit more complex cases:

_Example 2:_ Run tests starting with `system` and ending in `smoke`

```bash
tipi . --test "^system.+smoke$"
```

_Note:_ You may use the `<project-root>/.tipi.force-tests` file to override `--test` - see [§Test control files](#test-control-files)


## Excluding specific tests

If you need to exclude a number of test executables matched by _`--test <all|pattern>`_ you can use the `--exclude-test` parameters. This parameter can take a regex pattern to fit more complex cases:

_Example:_ run all tests except those matching `mytest1|mytest2`

```bash
tipi . --test all --exclude-test "mytest1|mytest2"
```

## Passing parameters to the test executable

Use the `--test-args <value>` command line parameter of tipi if you need to pass command line arguments to the test executable:


_Example:_ disable the test "intro" output and use minimal logging in a [doc-test](https://github.com/doctest/doctest/blob/master/doc/markdown/commandline.md) based test:

```bash
tipi . -t linux-cxx17 --test test/mytest --test-args "--no-intro=true --minimal" 
```

_Note:_ You may use the `<project-root>/.tipi.test-args` file to override `--test-args` - see [§Test control files](#test-control-files)

## Test control files

In order to provide additional flexibility for _live build_ and _continuous integration_ setups, tipi provides file-based facilities to control the values of `--test` and `--test-args`:

- `<project-root>/.tipi.force-tests` overrides the value of `--test` 
  - enable tests even if no `--test` parameter was passed while staying in monitor mode
  - change the selected test executables in monitor mode
- `<project-root>/.tipi.test-args` overrides the value of `--test-args`
  - change the test executable parameters while staying in monitor mode
  - set default values / complex arguments used in testing


_Example:_ Filter for test executables matching `$system-` (e.g. starting with "system-") and set the test arguments to the same values as in the example above.

> In `<project-root>/.tipi.force-tests` :
> ```txt
> ^system-
> ```

> In `<project-root>/.tipi.test-args` :
> ```txt
> --no-intro=true
> --minimal
> ```

_Then the test:_

```bash
tipi .
```

_Note:_ any newline `\n` or `CR+LF` characters in the file content will be replaced by a _single white space_ from the file content, so you may specify the passed parameters on distinct lines.

## Test execution concurrency

By default tipi will execute the test executables using the same concurrency level as set by the `-j` / compile jobs parameter (which defaults to the number of hardware threads of the system CPU).

If you want to change this setting independently you can specify `--test-jobs <N>` with `<N>` the number of concurrently executed binaries.

_Example 1:_ linear (one at a time) test execution (while compiling with default concurrency):

```bash
tipi . --test all --test-jobs 1
```

_Example 2:_ linear (one at a time) test execution and compiling at `-j4`:

```bash
tipi . --test all --test-jobs 1 -j4
```

_Note:_ in a remote build setting `--test-jobs` **does not** change the machine size used to execute the tests.
