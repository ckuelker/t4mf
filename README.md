---
title: Tools For Makefiles README
linkTitle: T4MF
author: Christian Külker
date: 2022-06-25
version: 0.1.0

---

# Abstract

This document describes briefly the aim and content of the git repository
[t4mf.git]. __Tools For Makefiles__ is a set of scripts, functions and
receipts, that can be used by Makefiles. One aspect is to collect functions,
that are useful for Makefiles. An other aspect is to collect useful scripts
that make for example the configuration space of external configuration files
accessible to Makefiles.

# Introduction

The main goal of __Tools For Makefiles__, short __t4mf__, is to provide access
to complex functions for Makefiles, that are impossible, difficult, or
cumbersome to implement directly in a Makefile.

While in the past linear configuration in a definition style like `A=B` or even
in a bit more complex way like a `*.ini` file was presented to Makefiles.
Recent configuration might be in the form of `YAML` or other markup languages.
While usually, as a quick workaround, configuration data is copied from the
configuration to the Makefile, it is better to avoid it. When the application
and the Makefile uses the same configuration, mistakes are minimized if the
Makefile can access the configuration directly. See `t4mf-yaml-get`.

This problem was probably solved many times before and most likely in more
elegant ways, however I did not came across those implementations. This
repository aggregates useful simple tools which helped me and hopefully others.
Be aware that using tools written in high level languages like Python or Perl
come with an execution time penalty. Usually software is build once with make
and executed many times without make, therefore time penalties coming from
reading the configuration space (especially if done only at the beginning) can
often be neglected.

## Scripts

### YAML

- `t4mf-yaml-get`: fetch a single or nested value from `YAML` configuration.
   Single values are delivered as is. Python `lists` are concatenated together
   with space and the keys of Python `dics` are also concatenated together with
   space.

### File System

- `t4fm-fs-path-concat`: concatenate strings, with spaces in-between, to a
   Unix/Linux path and removes a trailing slash or trailing slashes  (if any).
   It also removes './' from the beginning and sanitize slashes in-between,
   aka make one slash from two.

### Versions

- Sometimes comparing a version is important. There are many solutions to it.
  Programming a specific version for a Makefile is possible, but this problem
  was solved by may more high level languages. `t4mf-version-compare` compares
  a version and if not a match (or higher) it bails out with an error. While
  `t4mf-version-compare-ok` will compare two versions an print 'OK' if it is
  match or higher.

## Functions

GNU make offers so called functions. Together with advanced features of shells,
like bash >4.x, this fact can be used to implement more complex text
manipulation routines.

### Text Functions

#### Case Related Functions

Changing the case of some or all characters or make sure that the cases of a
string has a certain quality function can come in handy.

__Bash:__

```make
# Change the first character to upper case
ucfirst=$(shell bash -c 'export VAR="$1";echo "$${VAR^}"')
# Change all characters to upper case
ucall=$(shell bash -c 'export VAR="$1";echo "$${VAR^^}"')
# Change the first character to lower case
lcfirst=$(shell bash -c 'export VAR="$1";echo "$${VAR,}"')
# Change all characters to lower case
lcall=$(shell bash -c 'export VAR="$1";echo "$${VAR,,}"')
```

This functions will perform like the functions in Perl `uc`, `lc`, `ucfirst`
and `lcfirst`, but do not require the start of a Perl interpreter. To use them
to change 'upper_case' to 'Upper_case' for example do:

```make
text:=upper_case
Text:=$(call ucfirst,$(text))
```

__Perl:__

Of course also Perl can be used in this way to defined functions.

```make
# Change the first character to upper case
ucfirst=$(shell perl -e 'print ucfirst "$1"')
# Change the first character to lower case
lcfirst=$(shell perl -e 'print lcfirst "$1"')
# Change all characters to upper case
ucall=$(shell perl -e 'print uc "$1"')
# Change all characters to lower case
lcall=$(shell perl -e 'print lc "$1"')
```

With Perl the power or regular expression and other powerful string
manipulations can be sourced into `make`. The first example 'cc1' stands for
camel case and will convert a string "the variable test" to "TheVariableTest"
without a complex regular expression.

```make
cc1=$(shell perl -e 'print join "", map {ucfirst} split " ", "$1"')
```

It can be made more robust for strings that contain one or more spaces by
using a regular expression. This will convert "my  cat" to "MyCat".

```make
cc2=$(shell perl -e 'print join "", map {ucfirst} split /\s+/, "$1"')
```

If we want also to make sure that a capitalized or partly capitalized
string gets formatted in camel case we can streamline Perl functions.
This will convert "My  DoG" to "MyDog".

```make
cc3=$(shell perl -e 'print join "", map {ucfirst lc $$_} split /\s+/, "$1"')
```

The same can be done with a shorter regular expression. However the above is
more readable.

```make
cc4=$(shell perl -e '$$_=lc"$1";s/\s+(\w)/\u$$1/g;print')
```

In case it is not clear what it does:

1. Fetch `$1` as a string `"$1"`
2. Convert `$1` to lower case: `lc "$1"` (one space was removed)
3. Assign the result to `$_` (we need 2 `$` to escape it for make)
4. Apply the regex `/\s+(\w)/` to `$_` (a search for the first character of a
   word after one ore more spaces)
5. Replace the result found (one character of a word) `$$1` (we need 2 `$` to
   escape for make) with it's upper case `/u$$1/` character
6. And since the regex `s/\s+(\w)/\u$$1/g` has a `g` we do it for every space
   separated word
7. Print the result from the regex `$_` (implicit)

Some people would write the GNU make function alternatively with an added plus
sign, as `cc=$(shell perl -e '$$_=lc"$1";s/\s+(\w+)/\u$$1/g;print')`, so adding
a `+` to `\w`. This gives the same result as the previous regular expression.
Unfortunately, non of the above solutions are very robust for all kind of
strings and would not handle hyphens well for example. The string "My-Cat is
a doG" would become "My-catIsADog". Of course the regular expression can be
improved. We could search not only for space separated word by also for one ore
more hyphens, one ore more spaces and the beginning of the string: `pcc=$(shell
perl -e '$$_=lc"$1";s/(^|[-\s]+)(\w+)/\u$$2/g;print')` The sky is the limit.

```make
pcc=$(shell perl -e '$$_=lc"$1";s/(^|[-\s]+)(\w+)/\u$$2/g;print')
```

The above changes "My-Cat is a doG" to "MyCatIsADog".

### Error Text Formatting

Usually GNU `make` gives the possibility to end the execution of a `Makefile`
via the `error` call, like: `$(error ERROR: TEXT is not defined. Use 'make
TEXT=OK all')`.

```bash
make all
Makefile:30: *** ERROR: TEXT is not defined. Use 'make TEXT=OK all'.  Stop
```

While this is nice and good and usually should not be changed, sometimes the
error message can be overseen, if the project is large or one is tired. A
little bigger, more prominent message is appreciated. The problem is that the
`error` call remove all line feeds and similar characters from the output.
Therefore simple shell `echo` constructs will not do it. However when construct
`define..endef` and a function is used, better outputs are possible.

```make
L:=============================================================================
l:=----------------------------------------------------------------------------
define n


endef
ERROR=$(n)$(L)$(n)ERROR:$(n)$(l)$(n)$1$(n)$(L)$(n)

ifndef TEXT
$(error $(call ERROR,TEXT is not defined. Use 'make TEXT=OK $(MAKECMDGOALS)'))
endif

all:
```

This will print a message like

```bash
make all
Makefile:30: ***
============================================================================
ERROR:
----------------------------------------------------------------------------
TEXT is not defined. Use 'make TEXT=OK all'
============================================================================
.  Stop.
```

## Examples

This project provides some example Makefiles.

### Fs Path Concatenation

Execute `make test` to see the some file concatenation exercises.

```bash
make info test
VERSION: [0.1.0]
01 NG make [/path/to/a/file]          != [ /path /to /a /file]
02 OK make [path/to/a/file]            = [path/to/a/file]
03 OK make [/path/to/a/file]           = [/path/to/a/file]
04 OK make [./path/to/a/file]          = [./path/to/a/file]
05 OK make [SOMETHING/path/to/a/file]  = [/srv/g/github.com/ckuelker/t4mf-dev/examples/fs/path/concat/path/to/a/file]
06 OK make []                          = []
07 OK t4mf [path/to/a/file]            = [path/to/a/file]
08 OK t4mf [path/to/a/file]            = [path/to/a/file]
10 NG make [ /path /to /a /file]      != [ /path / /to /a /file]
20 NG make [path/to/a/file]           != [path//to/a/file]
30 NG make [/path/to/a/file]          != [/path//to/a/file]
40 NG make [./path/to/a/file]         != [./path//to/a/file]
50 OK make [SOMETHING/path/to/a/file]  = [/srv/g/github.com/ckuelker/t4mf-dev/examples/fs/path/concat/path/to/a/file]
60 OK make []                          = []
70 OK t4mf [path/to/a/file]            = [path/to/a/file]
80 OK t4mf [path/to/a/file]            = [path/to/a/file]
```

### Functions

The provided `Makefile` has a test example. Execute `make test`

```bash
# Test the functions
make test
VERSION: [0.1.0]
Shell sucfirst: my cat = [My cat]
Shell sucall:   my cat = [MY CAT]
Shell slcfirst  MY CAT = [mY CAT]
Shell slcall:   MY CAT = [my cat]
Perl ucfirst:   my cat = [My cat]
Perl ucall:     my cat = [MY CAT]
Perl lcfirst    MY CAT = [mY CAT]
Perl lcall:     MY CAT = [my cat]
Perl Camel Case pcc: my cat          = [MyCat]
Perl Camel Case pcc: MY CAT          = [MyCat]
Perl Camel Case pcc: My-Cat is a doG = [MyCatIsADog]
```

# Test the error

This example displays a better readable error message. To __not__ invoke
the error execute one of the following commands:

- `make TEXT=OK usage`
- `make TEXT=OK info`
- `make TEXT=OK test`

To actually display the error message __leave out__ the `TEXT=OK`:

```bash
make test

Makefile:30: ***
============================================================================
ERROR:
----------------------------------------------------------------------------
TEXT is not defined. Use 'make TEXT=OK test'
============================================================================
.  Stop.
```

# History

| Version | Date       | Notes                                                |
| ------- | ---------- | ---------------------------------------------------- |
| 0.1.0   | 2022-09-26 | Initial release                                      |

[t4mf.git]: https://github.com/ckuelker/t4mf
