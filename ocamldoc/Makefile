#**************************************************************************
#*                                                                        *
#*                                 OCaml                                  *
#*                                                                        *
#*             Maxence Guesdon, projet Cristal, INRIA Rocquencourt        *
#*                                                                        *
#*   Copyright 2001 Institut National de Recherche en Informatique et     *
#*     en Automatique.                                                    *
#*                                                                        *
#*   All rights reserved.  This file is distributed under the terms of    *
#*   the GNU Lesser General Public License version 2.1, with the          *
#*   special exception on linking described in the file LICENSE.          *
#*                                                                        *
#**************************************************************************

ROOTDIR = ..

include $(ROOTDIR)/Makefile.config
include $(ROOTDIR)/Makefile.common

OCAMLRUN ?= $(ROOTDIR)/boot/ocamlrun
OCAMLYACC ?= $(ROOTDIR)/yacc/ocamlyacc

STDLIBFLAGS = -nostdlib -I $(ROOTDIR)/stdlib
OCAMLC    = $(OCAMLRUN) $(ROOTDIR)/ocamlc $(STDLIBFLAGS)
OCAMLOPT  = $(OCAMLRUN) $(ROOTDIR)/ocamlopt $(STDLIBFLAGS)
OCAMLDEP  = $(OCAMLRUN) $(ROOTDIR)/tools/ocamldep -slash
OCAMLLEX  = $(OCAMLRUN) $(ROOTDIR)/boot/ocamllex
# TODO: figure out whether the DEBUG lines the following preprocessor removes
# are actually useful.
# If they are not, then the preprocessor logic (including the
# remove_DEBUG script and the debug target) could be removed.
# If they are, it may be better to be able to enable them at run-time
# rather than compile-time, e.g. through a -debug command-line option.
# In the following line, "sh" is useful under Windows. Without it,
# the ./remove_DEBUG command would be executed by cmd.exe which would not
# know how to handle it.
OCAMLPP=-pp 'sh ./remove_DEBUG'

# For installation
##############

MKDIR=mkdir -p
CP=cp
OCAMLDOC=ocamldoc

# TODO: clarify whether the following really needs to be that complicated
ifeq "$(UNIX_OR_WIN32)" "unix"
  ifeq "$(TARGET)" "$(HOST)"
    ifeq "$(SUPPORTS_SHARED_LIBRARIES)" "true"
      OCAMLDOC_RUN=$(OCAMLRUN) -I $(ROOTDIR)/otherlibs/$(UNIXLIB) -I $(ROOTDIR)/otherlibs/str ./$(OCAMLDOC)
    else
      OCAMLDOC_RUN=./$(OCAMLDOC)
    endif
  else
    OCAMLDOC_RUN=$(OCAMLRUN) ./$(OCAMLDOC)
  endif
else # Windows
  OCAMLDOC_RUN = \
    CAML_LD_LIBRARY_PATH="$(ROOTDIR)/otherlibs/win32unix;$(ROOTDIR)/otherlibs/str" $(OCAMLRUN) ./$(OCAMLDOC)
endif

OCAMLDOC_OPT=$(OCAMLDOC).opt
OCAMLDOC_LIBCMA=odoc_info.cma
OCAMLDOC_LIBCMI=odoc_info.cmi
OCAMLDOC_LIBCMXA=odoc_info.cmxa
OCAMLDOC_LIBA=odoc_info.$(A)

OCAMLDOC_LIBMLIS=odoc_info.mli
OCAMLDOC_LIBCMIS=$(OCAMLDOC_LIBMLIS:.mli=.cmi)
OCAMLDOC_LIBCMTS=$(OCAMLDOC_LIBMLIS:.mli=.cmt) $(OCAMLDOC_LIBMLIS:.mli=.cmti)

ODOC_TEST=odoc_test.cmo
GENERATORS_CMOS= \
        generators/odoc_todo.cmo \
        generators/odoc_literate.cmo
ifeq "$(NATDYNLINK)" "true"
GENERATORS_CMXS = $(GENERATORS_CMOS:.cmo=.cmxs)
else
GENERATORS_CMXS =
endif

# Compilation
#############

INCLUDES_DEP=\
  -I $(ROOTDIR)/utils \
  -I $(ROOTDIR)/parsing \
  -I $(ROOTDIR)/typing \
  -I $(ROOTDIR)/driver \
  -I $(ROOTDIR)/bytecomp \
  -I $(ROOTDIR)/toplevel

INCLUDES_NODEP=\
  -I $(ROOTDIR)/stdlib \
  -I $(ROOTDIR)/compilerlibs \
  -I $(ROOTDIR)/otherlibs/str \
  -I $(ROOTDIR)/otherlibs/dynlink \
  -I $(ROOTDIR)/otherlibs/$(UNIXLIB) \
  -I $(ROOTDIR)/otherlibs/$(GRAPHLIB)

INCLUDES=$(INCLUDES_DEP) $(INCLUDES_NODEP)

COMPFLAGS=$(INCLUDES) -absname -w +a-4-9-41-42-44-45-48 -warn-error A -safe-string -strict-sequence -strict-formats -bin-annot
LINKFLAGS=$(INCLUDES) -nostdlib

CMOFILES=\
  odoc_config.cmo \
  odoc_messages.cmo \
  odoc_global.cmo \
  odoc_types.cmo \
  odoc_misc.cmo \
  odoc_text_parser.cmo \
  odoc_text_lexer.cmo \
  odoc_text.cmo \
  odoc_name.cmo \
  odoc_parameter.cmo \
  odoc_value.cmo \
  odoc_type.cmo \
  odoc_extension.cmo \
  odoc_exception.cmo \
  odoc_class.cmo \
  odoc_module.cmo \
  odoc_print.cmo \
  odoc_str.cmo \
  odoc_comments_global.cmo \
  odoc_parser.cmo \
  odoc_lexer.cmo \
  odoc_see_lexer.cmo \
  odoc_env.cmo \
  odoc_merge.cmo \
  odoc_sig.cmo \
  odoc_ast.cmo \
  odoc_control.cmo \
  odoc_inherit.cmo \
  odoc_search.cmo \
  odoc_scan.cmo \
  odoc_cross.cmo \
  odoc_comments.cmo \
  odoc_dep.cmo \
  odoc_analyse.cmo \
  odoc_info.cmo

CMXFILES = $(CMOFILES:.cmo=.cmx)
CMIFILES = $(CMOFILES:.cmo=.cmi)

EXECMOFILES=\
  $(CMOFILES) \
  odoc_dag2html.cmo \
  odoc_to_text.cmo \
  odoc_ocamlhtml.cmo \
  odoc_html.cmo \
  odoc_man.cmo \
  odoc_latex_style.cmo \
  odoc_latex.cmo \
  odoc_texi.cmo \
  odoc_dot.cmo \
  odoc_gen.cmo \
  odoc_args.cmo \
  odoc.cmo

EXECMXFILES = $(EXECMOFILES:.cmo=.cmx)
EXECMIFILES = $(EXECMOFILES:.cmo=.cmi)

LIBCMOFILES = $(CMOFILES)
LIBCMXFILES = $(LIBCMOFILES:.cmo=.cmx)
LIBCMIFILES = $(LIBCMOFILES:.cmo=.cmi)


.PHONY: all
all: lib exe generators manpages

manpages: generators

.PHONY: exe
exe: $(OCAMLDOC)

.PHONY: lib
lib: $(OCAMLDOC_LIBCMA) $(OCAMLDOC_LIBCMI) $(ODOC_TEST)

.PHONY: generators
generators: $(GENERATORS_CMOS)

.PHONY: opt.opt allopt # allopt and opt.opt are synonyms
opt.opt: exeopt libopt generatorsopt
allopt: opt.opt

.PHONY: exeopt
exeopt: $(OCAMLDOC_OPT)

.PHONY: libopt
libopt: $(OCAMLDOC_LIBCMXA) $(OCAMLDOC_LIBCMI)

.PHONY: generatorsopt
generatorsopt: $(GENERATORS_CMXS)

# TODO: the following debug target could be replaced by a DEBUG variable
.PHONY: debug
debug:
	$(MAKE) OCAMLPP=""

OCAMLDOC_LIBRARIES = unix str dynlink ocamlcommon

OCAMLDOC_BCLIBRARIES = $(OCAMLDOC_LIBRARIES:%=%.cma)
OCAMLDOC_NCLIBRARIES = $(OCAMLDOC_LIBRARIES:%=%.cmxa)

$(OCAMLDOC): $(EXECMOFILES)
	$(OCAMLC) -o $@ -linkall $(LINKFLAGS) $(OCAMLDOC_BCLIBRARIES) $^

$(OCAMLDOC_OPT): $(EXECMXFILES)
	$(OCAMLOPT_CMD) -o $@ -linkall $(LINKFLAGS) $(OCAMLDOC_NCLIBRARIES) $^

$(OCAMLDOC_LIBCMA): $(LIBCMOFILES)
	$(OCAMLC) -a -o $@ $(LINKFLAGS) $^

$(OCAMLDOC_LIBCMXA): $(LIBCMXFILES)
	$(OCAMLOPT) -a -o $@ $(LINKFLAGS) $^

.PHONY: manpages
manpages: stdlib_man/Stdlib.3o

.PHONY: html_doc
html_doc: stdlib_html/Stdlib.html

.PHONY: dot
dot: ocamldoc.dot

ocamldoc.dot: $(EXECMOFILES)
	$(OCAMLDOC_RUN) -dot -dot-reduce -o $@ $(INCLUDES) odoc*.ml

# Parsers and lexers dependencies :
###################################
odoc_text_parser.ml: odoc_text_parser.mly
odoc_text_parser.mli: odoc_text_parser.mly

odoc_parser.ml:	odoc_parser.mly
odoc_parser.mli:odoc_parser.mly

odoc_text_lexer.ml: odoc_text_lexer.mll

odoc_lexer.ml:odoc_lexer.mll

odoc_ocamlhtml.ml: odoc_ocamlhtml.mll

odoc_see_lexer.ml: odoc_see_lexer.mll


# generic rules :
#################

.SUFFIXES: .mll .mly .ml .mli .cmo .cmi .cmx .cmxs

.ml.cmo:
	$(OCAMLC) $(OCAMLPP) $(COMPFLAGS) -c $<

.mli.cmi:
	$(OCAMLC) $(OCAMLPP) $(COMPFLAGS) -c $<

.ml.cmx:
	$(OCAMLOPT) $(OCAMLPP) $(COMPFLAGS) -c $<

.ml.cmxs:
	$(OCAMLOPT_CMD) -shared -o $@ $(OCAMLPP) $(COMPFLAGS) $<

.mll.ml:
	$(OCAMLLEX) $<

.mly.ml:
	$(OCAMLYACC) --strict -v $<

.mly.mli:
	$(OCAMLYACC) --strict -v $<

# Installation targets
######################

# TODO: it may be good to split the following rule in several ones, e.g.
# install-programs, install-doc, install-libs

INSTALL_MANODIR=$(INSTALL_MANDIR)/man3

.PHONY: install
install:
	$(MKDIR) "$(INSTALL_BINDIR)"
	$(MKDIR) "$(INSTALL_LIBDIR)/ocamldoc"
	$(MKDIR) "$(INSTALL_MANODIR)"
	$(INSTALL_PROG) $(OCAMLDOC) "$(INSTALL_BINDIR)/$(OCAMLDOC)$(EXE)"
	$(INSTALL_DATA) \
	  ocamldoc.hva *.cmi $(OCAMLDOC_LIBCMA) \
	  "$(INSTALL_LIBDIR)/ocamldoc"
	$(INSTALL_DATA) \
	  $(OCAMLDOC_LIBCMIS) \
	  "$(INSTALL_LIBDIR)/ocamldoc"
ifeq "$(INSTALL_SOURCE_ARTIFACTS)" "true"
	$(INSTALL_DATA) \
	  $(OCAMLDOC_LIBMLIS) $(OCAMLDOC_LIBCMTS) \
	  "$(INSTALL_LIBDIR)/ocamldoc"
endif
	if test -d stdlib_man; then \
	  $(INSTALL_DATA) stdlib_man/* "$(INSTALL_MANODIR)"; \
	else : ; fi

# Note: at the moment, $(INSTALL_MANODIR) is created even if the doc has
# not been built. This is not clean and should be changed.

.PHONY: installopt
installopt:
	if test -f $(OCAMLDOC_OPT); then $(MAKE) installopt_really ; fi

.PHONY: installopt_really
installopt_really:
	$(MKDIR) "$(INSTALL_BINDIR)"
	$(MKDIR) "$(INSTALL_LIBDIR)/ocamldoc"
	$(INSTALL_PROG) \
	   $(OCAMLDOC_OPT) "$(INSTALL_BINDIR)/$(OCAMLDOC_OPT)$(EXE)"
	$(INSTALL_DATA) \
	  $(OCAMLDOC_LIBCMIS) \
	  "$(INSTALL_LIBDIR)/ocamldoc"
ifeq "$(INSTALL_SOURCE_ARTIFACTS)" "true"
	$(INSTALL_DATA) \
	  $(OCAMLDOC_LIBMLIS) $(OCAMLDOC_LIBCMTS) \
	  "$(INSTALL_LIBDIR)/ocamldoc"
endif
	$(INSTALL_DATA) \
	  ocamldoc.hva *.cmx $(OCAMLDOC_LIBA) $(OCAMLDOC_LIBCMXA) \
	  "$(INSTALL_LIBDIR)/ocamldoc"

# TODO: also split into several rules

# Testing :
###########

.PHONY: test
test:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -html -colorize-code -sort -d $@ $(INCLUDES) -dump $@/ocamldoc.odoc odoc*.ml odoc*.mli -v
	$(MKDIR) $@-custom
	$(OCAMLDOC_RUN) -colorize-code -sort -d $@-custom $(INCLUDES) \
	-g generators/odoc_literate.cmo -g generators/odoc_todo.cmo \
	-load $@/ocamldoc.odoc -v

.PHONY: test_stdlib
test_stdlib:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -html -colorize-code -sort -d $@ $(INCLUDES) -dump $@/stdlib.odoc -keep-code \
	$(ROOTDIR)/stdlib/*.mli \
	$(ROOTDIR)/otherlibs/$(UNIXLIB)/unix.mli \
	$(ROOTDIR)/otherlibs/str/str.mli

.PHONY: test_stdlib_code
test_stdlib_code:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -html -colorize-code -sort -d $@ $(INCLUDES) -dump $@/stdlib.odoc -keep-code \
	`ls $(ROOTDIR)/stdlib/*.ml | grep -v Labels` \
	$(ROOTDIR)/otherlibs/$(UNIXLIB)/unix.ml \
	$(ROOTDIR)/otherlibs/str/str.ml

.PHONY: test_framed
test_framed:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -g odoc_fhtml.cmo -sort -colorize-code -d $@ $(INCLUDES) odoc*.ml odoc*.mli

.PHONY: test_latex
test_latex:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -latex -sort -o $@/test.tex -d $@ $(INCLUDES) odoc*.ml \
	        odoc*.mli test2.txt $(ROOTDIR)/stdlib/*.mli $(ROOTDIR)/otherlibs/unix/unix.mli

.PHONY: test_latex_simple
test_latex_simple:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -latex -sort -o $@/test.tex -d $@ $(INCLUDES) \
	-latextitle 6,subsection -latextitle 7,subsubection \
	$(ROOTDIR)/stdlib/hashtbl.mli \
	$(ROOTDIR)/stdlib/arg.mli \
	$(ROOTDIR)/otherlibs/$(UNIXLIB)/unix.mli \
	$(ROOTDIR)/stdlib/map.mli

.PHONY: test_man
test_man:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -man -sort -d $@ $(INCLUDES) odoc*.ml odoc*.mli

.PHONY: test_texi
test_texi:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -texi -sort -d $@ $(INCLUDES) odoc*.ml odoc*.mli

# stdlib non-prefixed :
#######################
SRC=$(ROOTDIR)

# Documented modules: stdlib + otherlib + utils(?) + parsing(for compiler-libs)

include Makefile.docfiles

stdlib_man/Stdlib.3o: $(OCAMLDOC) $(DOC_ALL_MLIS)
	$(MKDIR) stdlib_man
	$(OCAMLDOC_RUN) -man -d stdlib_man -nostdlib \
	-hide Stdlib -lib Stdlib $(DOC_ALL_INCLUDES) \
	-pp "$(AWK) -v ocamldoc=true -f $(SRC)/stdlib/expand_module_aliases.awk" \
	-t "OCaml library" -man-mini \
	-text $(SRC)/manual/manual/library/compiler_libs.mld \
	$(DOC_ALL_MLIS)

stdlib_html/Stdlib.html: $(OCAMLDOC) $(DOC_ALL_MLIS)
	$(MKDIR) stdlib_html
	$(OCAMLDOC_RUN) -html -d stdlib_html -nostdlib \
	-hide Stdlib -lib Stdlib $(DOC_ALL_INCLUDES) \
	-pp "$(AWK) -v ocamldoc=true -f $(SRC)/stdlib/expand_module_aliases.awk" \
	-t "OCaml library" \
	-text $(SRC)/manual/manual/library/compiler_libs.mld \
	$(DOC_ALL_MLIS)

.PHONY: autotest_stdlib
autotest_stdlib:
	$(MKDIR) $@
	$(OCAMLDOC_RUN) -g autotest/odoc_test.cmo\
	$(INCLUDES) -keep-code \
	$(ROOTDIR)/stdlib/*.mli \
	$(ROOTDIR)/otherlibs/$(UNIXLIB)/unix.mli \
	$(ROOTDIR)/otherlibs/str/str.mli


# odoc rules :
##############

.PHONY: odoc
odoc:
	rm -rf odoc
	$(MKDIR) odoc
	# .cmti --> .odoc
	for fn in $(ROOTDIR)/stdlib/stdlib*.cmti; do \
	  odoc compile $(INCLUDES) --package stdlib $(ROOTDIR)/stdlib/$$fn; \
	done
	for lib in str bigarray; do \
	  odoc compile $(INCLUDES) --package $$lib $(ROOTDIR)/otherlibs/$$lib/$$lib.cmti; \
	done
	odoc compile $(INCLUDES) --package unix $(ROOTDIR)/otherlibs/$(UNIXLIB)/unix.cmti
	for fn in $(ROOTDIR)/parsing/*.cmti; do \
	  odoc compile $(INCLUDES) --package parsing $(ROOTDIR)/parsing/$$fn; \
	done
	# .odoc --> .html
	odoc html $(INCLUDES) --output-dir odoc $(ROOTDIR)/stdlib/stdlib.odoc
	for lib in str bigarray $(UNIXLIB); do \
	  odoc html $(INCLUDES) --output-dir odoc $(ROOTDIR)/otherlibs/$$lib/$$lib.odoc; \
	done
	for fn in $(ROOTDIR)/parsing/*.odoc; do \
	  odoc html $(INCLUDES) --output-dir odoc $$fn; \
	done
	for d in odoc/*; do \
	  lib=`basename $$d`; \
	  cd $$d; \
	  echo -e The $$lib 'library.\n\nModules\n:{!modules:' * '}' > ../../index.mld; \
	  cd ../..; \
	  odoc html $(INCLUDES) --output-dir odoc --index-for=$$lib index.mld; \
	  rm -f index.mld; \
	done
	cp odoc_index.html odoc/index.html
	odoc css -o odoc

# backup, clean and depend :
############################

.PHONY: clean
clean:
	rm -f *~ \#*\#
	rm -f $(OCAMLDOC) $(OCAMLDOC_OPT) *.cma *.cmxa *.cmo *.cmi *.cmx *.cmt *.cmti *.$(A) *.$(O)
	rm -f odoc_parser.output odoc_text_parser.output
	rm -f odoc_lexer.ml odoc_text_lexer.ml odoc_see_lexer.ml odoc_ocamlhtml.ml
	rm -f odoc_parser.ml odoc_parser.mli odoc_text_parser.ml odoc_text_parser.mli
	rm -rf stdlib_man stdlib_html
	rm -f generators/*.cm[taiox] generators/*.$(A) generators/*.$(O) generators/*.cmx[as]

.PHONY: depend
depend:
	$(OCAMLYACC) odoc_text_parser.mly
	$(OCAMLYACC) odoc_parser.mly
	$(OCAMLLEX) odoc_text_lexer.mll
	$(OCAMLLEX) odoc_lexer.mll
	$(OCAMLLEX) odoc_ocamlhtml.mll
	$(OCAMLLEX) odoc_see_lexer.mll
	$(OCAMLDEP) $(INCLUDES_DEP) *.mll *.mly *.ml *.mli > .depend
	$(OCAMLDEP) $(INCLUDES_DEP) -shared generators/*.ml >> .depend

include .depend
