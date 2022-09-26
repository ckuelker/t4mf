# +---------------------------------------------------------------------------+
# | Makefile                                                                  |
# |                                                                           |
# | Example of concatenate a path with t4mf-fs-path-concat                    |
# |                                                                           |
# | Version: 0.1.0 (change inline)                                            |
# |                                                                           |
# | Copyright: (C) 2022 Christian Külker <c@c8i.org>                          |
# |                                                                           |
# | Changes:                                                                  |
# |                                                                           |
# | 0.1.0 2022-09-24 Christian Külker <c@c8i.org>                             |
# |     - Initial release                                                     |
# |                                                                           |
# +---------------------------------------------------------------------------+
#
VERSION=0.1.0
# -----------------------------------------------------------------------------
# NO CHANGES BEYOND THIS POINT
ifndef VERSION
$(error VERSION is NOT defined)
endif
L:=============================================================================
sep := /
empty:=
space := $(empty) $(empty)
list_ok := path to a file
list_ng := path  to a file
file_path01 := $(subst $(space), $(sep), $(list_ok))
file_path02 := $(subst $(space),$(sep),$(list_ok))
file_path03 := /$(subst $(space),$(sep),$(list_ok))
file_path04 := ./$(subst $(space),$(sep),$(list_ok))
file_path05 := $(abspath $(file_path04))
file_path06 := $(realpath $(file_path04))
P := ../../../../bin/t4mf-fs-path-concat
file_path07 := $(shell echo $(list_ok)|$(P))
file_path08 := $(shell $(P) $(list_ok))
file_path10 := $(subst $(space), $(sep), $(list_ng))
file_path20 := $(subst $(space),$(sep),$(list_ng))
file_path30 := /$(subst $(space),$(sep),$(list_ng))
file_path40 := ./$(subst $(space),$(sep),$(list_ng))
file_path50 := $(abspath $(file_path40))
file_path60 := $(realpath $(file_path40))
file_path70 := $(shell echo $(list_ng)|$(P))
file_path80 := $(shell $(P) $(list_ng))

.PHONY: usage info test
usage:
	@echo "$(L)"
	@echo "USAGE:"
	@echo "$(L)"
	@echo "make usage     : this information"
	@echo "make info      : print more info"
	@echo "make test      : path test"
info:
	@echo "VERSION: [$(VERSION)]"
test:
	@echo "01 NG make [/path/to/a/file]          != [$(file_path01)]"
	@echo "02 OK make [path/to/a/file]            = [$(file_path02)]"
	@echo "03 OK make [/path/to/a/file]           = [$(file_path03)]"
	@echo "04 OK make [./path/to/a/file]          = [$(file_path04)]"
	@echo "05 OK make [SOMETHING/path/to/a/file]  = [$(file_path05)]"
	@echo "06 OK make []                          = [$(file_path06)]"
	@echo "07 OK t4mf [path/to/a/file]            = [$(file_path07)]"
	@echo "08 OK t4mf [path/to/a/file]            = [$(file_path08)]"
	@echo "10 NG make [ /path /to /a /file]      != [$(file_path10)]"
	@echo "20 NG make [path/to/a/file]           != [$(file_path20)]"
	@echo "30 NG make [/path/to/a/file]          != [$(file_path30)]"
	@echo "40 NG make [./path/to/a/file]         != [$(file_path40)]"
	@echo "50 OK make [SOMETHING/path/to/a/file]  = [$(file_path50)]"
	@echo "60 OK make []                          = [$(file_path60)]"
	@echo "70 OK t4mf [path/to/a/file]            = [$(file_path70)]"
	@echo "80 OK t4mf [path/to/a/file]            = [$(file_path80)]"
	@echo
