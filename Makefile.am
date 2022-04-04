###############################################################################
## Makefile for Norwegian--Ukrainian
###############################################################################

LANG1=nor
LANG1b=nob
LANG1n=nno
LANG2=ukr
PREFIX1=$(LANG1)-$(LANG2)
PREFIX1b=$(LANG1b)-$(LANG2)
PREFIX1n=$(LANG1n)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)
PREFIX2b=$(LANG2)-$(LANG1b)
PREFIX2n=$(LANG2)-$(LANG1n)

BASENAME=apertium-$(PREFIX1)

TARGETS_COMMON =		\
	$(PREFIX1b).automorf.bin	\
	$(PREFIX1b).automorf-untrimmed.bin	\
	$(PREFIX1n).automorf.bin	\
	$(PREFIX1n).automorf-untrimmed.bin	\
	$(PREFIX1b).prob		\
	$(PREFIX1n).prob		\
	$(PREFIX1n).rlx.bin	\
	$(PREFIX1b).rlx.bin	\
	$(PREFIX1).autobil.bin	\
	$(PREFIX1).autolex.bin	\
	$(PREFIX1).autogen.bin	\
	$(PREFIX1).autopgen.bin	\
	$(PREFIX1).rtx.bin	\
	$(PREFIX2).automorf.bin	\
	$(PREFIX2).automorf-untrimmed.bin	\
	$(PREFIX2).prob		\
	$(PREFIX2).rlx.bin	\
	$(PREFIX2).autobil.bin	\
	$(PREFIX2).autolex.bin	\
	$(PREFIX2n).autogen.bin	\
	$(PREFIX2b).autogen.bin	\
	$(PREFIX2).rtx.bin

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## Monolingual transducers
###############################################################################



# -------------------
# Norwegian analysis:
# -------------------

.deps/$(LANG1b).automorf.bin: $(AP_SRC1b)/$(LANG1b).automorf.bin .deps/.d
	cp $< $@

$(PREFIX1b).automorf-untrimmed.bin: .deps/$(LANG1b).automorf.bin
	cp -v $< $@

$(PREFIX1b).automorf.bin: .deps/$(LANG1b).automorf.bin $(PREFIX1).autobil.bin
	lt-trim $^ $@

.deps/$(LANG1n).automorf.bin: $(AP_SRC1n)/$(LANG1n).automorf.bin .deps/.d
	cp $< $@

$(PREFIX1n).automorf-untrimmed.bin: .deps/$(LANG1n).automorf.bin
	cp -v $< $@

$(PREFIX1n).automorf.bin: .deps/$(LANG1n).automorf.bin $(PREFIX1).autobil.bin
	lt-trim $^ $@

# -------------------
# Norwegian generation:
# ---------------------


$(PREFIX2b).autogen.bin: $(AP_SRC1b)/$(LANG1b).autogen.bin
	cp $< $@

$(PREFIX2n).autogen.bin: $(AP_SRC1n)/$(LANG1n).autogen.bin
	cp $< $@


# -------------------
# Ukrainian analysis:
# -------------------

.deps/$(LANG2).automorf.bin: $(AP_SRC2)/$(LANG2).automorf.bin .deps/.d
	cp $< $@

$(PREFIX2).automorf-untrimmed.bin: .deps/$(LANG2).automorf.bin
	cp -v $< $@

$(PREFIX2).automorf.bin: .deps/$(LANG2).automorf.bin $(PREFIX2).autobil.bin
	lt-trim $^ $@

# -------------------
# Ukrainian generation:
# ---------------------



$(PREFIX1).autogen.bin: $(AP_SRC2)/$(LANG2).autogen.bin
	cp $< $@

###############################################################################
## Tagger models
###############################################################################

$(PREFIX1b).prob: $(AP_SRC1b)/$(LANG1b).prob
	cp $< $@

$(PREFIX1n).prob: $(AP_SRC1n)/$(LANG1n).prob
	cp $< $@

$(PREFIX2).prob: $(AP_SRC2)/$(LANG2).prob
	cp $< $@

###############################################################################
## Disambiguation rules
###############################################################################


$(PREFIX1b).rlx.bin: $(AP_SRC1b)/apertium-$(LANG1b).$(LANG1b).rlx
	cg-comp $< $@

$(PREFIX1n).rlx.bin: $(AP_SRC1n)/apertium-$(LANG1n).$(LANG1n).rlx
	cg-comp $< $@


$(PREFIX2).rlx.bin: $(AP_SRC2)/apertium-$(LANG2).$(LANG2).rlx
	cg-comp $< $@

###############################################################################
## Bilingual dictionaries
###############################################################################

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@

$(PREFIX2).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp rl $< $@

$(PREFIX2b).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp --var-left=nob rl $< $@

$(PREFIX2n).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp --var-left=nno rl $< $@


###############################################################################
## Lexical selection rules
###############################################################################

$(PREFIX1b).autolex.bin: $(BASENAME).$(PREFIX1b).lrx
	lrx-comp $< $@

$(PREFIX1n).autolex.bin: $(BASENAME).$(PREFIX1n).lrx
	lrx-comp $< $@

$(PREFIX2b).autolex.bin: $(BASENAME).$(PREFIX2b).lrx
	lrx-comp $< $@

$(PREFIX2n).autolex.bin: $(BASENAME).$(PREFIX2n).lrx
	lrx-comp $< $@



###############################################################################
## Structural transfer rules
###############################################################################


$(PREFIX1).rtx.bin: $(BASENAME).$(PREFIX1).rtx
	rtx-comp $< $@

$(PREFIX2).rtx.bin: $(BASENAME).$(PREFIX2).rtx
	rtx-comp $< $@

###############################################################################
## Postgeneration dictionaries
###############################################################################

$(PREFIX1).autopgen.bin: $(AP_SRC2)/$(LANG2).autopgen.bin
	cp $< $@


###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=					\
						$(BASENAME).$(PREFIX1).lrx	\
		$(BASENAME).$(PREFIX1).dix	\
		$(BASENAME).$(PREFIX2).lrx	\
		$(BASENAME).$(PREFIX1).rtx	\
		$(BASENAME).$(PREFIX2).rtx	\
		modes.xml

###############################################################################
## Installation
###############################################################################

apertium_nor_ukrdir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_nor_ukr_DATA=	\
	$(TARGETS_COMMON)


# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode

install-data-local: install-modes
uninstall-local: uninstall-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	apertium-regtest test
