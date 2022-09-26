# +---------------------------------------------------------------------------+
# | Makefile                                                                  |
# |                                                                           |
# | Description.                                                              |
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
l:=----------------------------------------------------------------------------
define n


endef
ERROR=$(n)$(L)$(n)ERROR:$(n)$(l)$(n)$1$(n)$(L)$(n)
ifndef TEXT
$(error $(call ERROR,TEXT is not defined. Use 'make TEXT=OK $(MAKECMDGOALS)'))
$(error ERROR: TEXT is not defined. Use 'make TEXT=OK $(MAKECMDGOALS)')
endif

.PHONY: usage info test
usage:
	@echo "$(L)"
	@echo "USAGE:"
	@echo "$(L)"
	@echo "make TEXT=OK usage : this information"
	@echo "make TEXT=OK info  : print more info"
	@echo "make TEXT=OK test  : will run some tests"
info:
	@echo "VERSION: [$(VERSION)]"
test:
	@echo "TEXT: [$(TEXT)]"
