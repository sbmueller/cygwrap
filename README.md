# cygwrap

This tool aims to improve compatibility of CLI programs within Cygwin by
replacing Cygwin paths with Windows paths.

## Description

This tool serves as a wrapper around any CLI Windows tools and tries to convert
the Cygwin into Windows paths, using `cygpath`. It only works with commands that
follow the order: executable, arguments, path:
```
executable --arg1 --arg2 -a -b /path/to/file
```

In particular this is useful, where executables are called automatically (e.g.
automated static code analysis triggered by an IDE).

## Example

For instance, when using `clang-tidy` with absolute paths, a call might look
like this:

```
clang-tidy --fix-errors --format-style=google --quiet /cygdrive/c/file.cc
```

Since `clang-tidy` is not available for Cygwin but for Windows, it will try to
interpret the provided path as a Windows path, resulting in something like
`C:\cygdrive\c\file.cc`. Eventually, `clang-tidy` will terminate with an error
code since this path can't be found.

## Usage

Just prepend a command by `cygwrap`, for instance
```
cygwrap clang-tidy --fix-errors --format-style=google --quiet /cygdrive/c/file.cc
```
