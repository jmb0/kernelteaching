OUT_DIR = out

VIEWER ?= evince $(VIEWER_OPTS)

LATEX_OPTS = --shell-escape
LATEX_OPTS += --interaction=batchmode

SVGTOLATEX ?= inkscape
SVGTOLATEX_OPTS ?= -z --export-latex
SVGTOLATEX_IN ?= --file=
SVGTOLATEX_OUT ?= --export-pdf=

VPATH = .:./$(OUT_DIR)

all: pdf

# Specific view targets goes here

view-intro: pdf
	$(VIEWER) $(OUT_DIR)/intro/intro.pdf

view-sample: pdf
	$(VIEWER) $(OUT_DIR)/sample/presentation_1.pdf

# ---

view: pdf
	$(VIEWER) $(wildcard $(OUT_DIR)/*/*.pdf)

svg: $(addsuffix .pdf_tex, $(basename $(wildcard */*.svg)))
pdf: svg $(addsuffix .pdf, $(basename $(wildcard */*.tex)))


%.pdf_tex: %.svg
	$(eval DIR=$(dir $?))
	$(eval TARGET=$(addsuffix .pdf, $(basename $@)))
	@echo "* Processing $?"
	@mkdir -p $(OUT_DIR)/$(DIR)
	@$(SVGTOLATEX) $(SVGTOLATEX_OPTS) $(SVGTOLATEX_IN)$< $(SVGTOLATEX_OUT)$(OUT_DIR)/$(TARGET) >/dev/null 2>&1

%.pdf: %.tex
	$(eval DIR=$(dir $?))
	$(eval FILE=$(patsubst $(DIR)%,%, $?))
	@echo "* Processing $?"
	@mkdir -p $(OUT_DIR)/$(DIR)
	@rubber --pdf --into $(OUT_DIR)/$(DIR) $?

clean:
	@test -d $(OUT_DIR) && rm -r $(OUT_DIR) || echo "Nothing to do"