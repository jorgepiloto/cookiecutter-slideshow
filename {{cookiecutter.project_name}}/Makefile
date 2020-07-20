#
# Original author: Jake Zimmerman <jake@zimmerman.io>
# Modified by: Jorge Martize <jorge@on-orbit.dev>
# ============================================================================

TARGET={{cookiecutter.slides_name}}
DEP=$(wildcard *.sty *.jpg *.png)
THEME=Custom 


PANDOC_FLAGS =\
	-f markdown+tex_math_single_backslash \
	-st beamer \
	-V theme:$(THEME) \

LATEX_FLAGS = \

PDF_ENGINE = xelatex
PANDOCVERSIONGTEQ2 := $(shell expr `pandoc --version | grep ^pandoc | sed 's/^.* //g' | cut -f1 -d.` \>= 2)
ifeq "$(PANDOCVERSIONGTEQ2)" "1"
    LATEX_FLAGS += --pdf-engine=$(PDF_ENGINE)
else
    LATEX_FLAGS += --latex-engine=$(PDF_ENGINE)
endif

all: $(patsubst %.md,%.pdf,$(wildcard *.md))

# Generalized rule: how to build a .pdf from each .md
%.pdf: %.md $(DEP)
	pandoc $(PANDOC_FLAGS) $(LATEX_FLAGS) -o $@ $<

# Generalized rule: how to build a .tex from each .md
%.tex: %.md
	pandoc --standalone $(PANDOC_FLAGS) -o $@ $<

touch:
	touch *.md

again: touch all

clean:
	rm -f *.aux *.log *.nav *.out *.snm *.toc *.vrb || true

veryclean: clean
	rm -f *.pdf

view: $(TARGET).pdf
	if [ "Darwin" = "$(shell uname)" ]; then open $(TARGET).pdf ; else xdg-open $(TARGET).pdf ; fi

print: $(TARGET).pdf
	lpr $(TARGET).pdf

.PHONY: all again touch clean veryclean view print
