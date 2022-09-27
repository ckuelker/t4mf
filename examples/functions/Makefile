# +---------------------------------------------------------------------------+
# | Makefile                                                                  |
# |                                                                           |
# | Examples for test functions                                               |
# |                                                                           |
# | Version: 0.1.0 (change inline)                                            |
# |                                                                           |
# | Copyright: (C) 2022 Christian Külker <c@c8i.org>                          |
# |                                                                           |
# | Changes:                                                                  |
# |                                                                           |
# | 0.1.0 2022-09-24 Christian Külker <c@c8i.org>                             |
# |     - initial release                                                     |
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
EXAMPLE_LC:=my cat
EXAMPLE_UC:=MY CAT
EXAMPLE_MC:=My-Cat is a doG
sucfirst=$(shell bash -c 'export VAR="$1";echo "$${VAR^}"')
sucall=$(shell bash -c 'export VAR="$1";echo "$${VAR^^}"')
slcfirst=$(shell bash -c 'export VAR="$1";echo "$${VAR,}"')
slcall=$(shell bash -c 'export VAR="$1";echo "$${VAR,,}"')
ucfirst=$(shell perl -e 'print ucfirst "$1"')
lcfirst=$(shell perl -e 'print lcfirst "$1"')
ucall=$(shell perl -e 'print uc "$1"')
lcall=$(shell perl -e 'print lc "$1"')
pcc=$(shell perl -e '$$_=lc"$1";s/(^|[-\s]+)(\w+)/\u$$2/g;print')

.PHONY: usage info test
usage:
	@echo "$(L)"
	@echo "USAGE:"
	@echo "$(L)"
	@echo "make usage     : this information"
	@echo "make info      : print more info"
	@echo "make test      : will run some tests"
info:
	@echo "VERSION: [$(VERSION)]"
test:
	@echo "VERSION: [$(VERSION)]"
	@echo "Shell sucfirst: $(EXAMPLE_LC) = [$(call sucfirst,$(EXAMPLE_LC))]"
	@echo "Shell sucall:   $(EXAMPLE_LC) = [$(call sucall,$(EXAMPLE_LC))]"
	@echo "Shell slcfirst  $(EXAMPLE_UC) = [$(call slcfirst,$(EXAMPLE_UC))]"
	@echo "Shell slcall:   $(EXAMPLE_UC) = [$(call slcall,$(EXAMPLE_UC))]"
	@echo "Perl ucfirst:   $(EXAMPLE_LC) = [$(call ucfirst,$(EXAMPLE_LC))]"
	@echo "Perl ucall:     $(EXAMPLE_LC) = [$(call ucall,$(EXAMPLE_LC))]"
	@echo "Perl lcfirst    $(EXAMPLE_UC) = [$(call lcfirst,$(EXAMPLE_UC))]"
	@echo "Perl lcall:     $(EXAMPLE_UC) = [$(call lcall,$(EXAMPLE_UC))]"
	@echo "Perl Camel Case pcc: $(EXAMPLE_LC)          = [$(call pcc,$(EXAMPLE_LC))]"
	@echo "Perl Camel Case pcc: $(EXAMPLE_UC)          = [$(call pcc,$(EXAMPLE_UC))]"
	@echo "Perl Camel Case pcc: $(EXAMPLE_MC) = [$(call pcc,$(EXAMPLE_MC))]"
