
## There is an old bug in texidvi that makes it not swallow the ~
## marker used to denote whitespace. This is actually due to fixing
## another bug whereby you could not run texidvi on directory names
## containing a tilde (as we happen to do for Debian builds of R
## alpha/beta/rc releases). The 'tilde' bug will go away as it
## reportedly has been squashed upstream but I am still bitten by it
## on Ubuntu so for now Dirk will insist on pdflatex and this helps.
whoami=$(shell whoami)

all:
	@echo The 'make all' target is now empty as eg R-Forge builds fall over
	@echo So please use 'make pdfclean; make pdfall' to rebuild

pdfall: rotateFiles index.html Rcpp-unitTests.pdf Rcpp-introduction.pdf Rcpp-modules.pdf Rcpp-package.pdf Rcpp-FAQ.pdf Rcpp-extending.pdf Rcpp-sugar.pdf Rcpp-attributes.pdf Rcpp-quickref.pdf

pdfclean:
	rm -f Rcpp-extending.pdf  Rcpp-FAQ.pdf  Rcpp-introduction.pdf  Rcpp-modules.pdf  Rcpp-package.pdf  Rcpp-quickref.pdf  Rcpp-sugar.pdf  Rcpp-attributes.pdf  Rcpp-unitTests.pdf

rotateFiles:
	cp -f Rcpp-extending/Rcpp-extending-fake.Rnw Rcpp-extending.Rnw
	cp -f Rcpp-FAQ/Rcpp-FAQ-fake.Rnw Rcpp-FAQ.Rnw
	cp -f Rcpp-modules/Rcpp-modules-fake.Rnw Rcpp-modules.Rnw
	cp -f Rcpp-package/Rcpp-package-fake.Rnw Rcpp-package.Rnw
	cp -f Rcpp-quickref/Rcpp-quickref-fake.Rnw Rcpp-quickref.Rnw
	cp -f Rcpp-sugar/Rcpp-sugar-fake.Rnw Rcpp-sugar.Rnw
	cp -f Rcpp-attributes/Rcpp-attributes-fake.Rnw Rcpp-attributes.Rnw

clean:
	rm -f index.html
	rm -f *.tex *.bbl *.blg *.aux *.out *.log

setvars:
ifeq (${R_HOME},)
R_HOME=	$(shell R RHOME)
endif
RPROG=	$(R_HOME)/bin/R
RSCRIPT=$(R_HOME)/bin/Rscript

index.html: rcpp.index.html
	cp rcpp.index.html index.html

Rcpp-unitTests.pdf:
	rm -fr unitTests-results/*
	$(RSCRIPT) --vanilla unitTests/Rcpp-unitTests.R
	$(RPROG) CMD Sweave Rcpp-unitTests.Rnw
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-unitTests.tex', pdf = TRUE, clean = TRUE )"
	rm -fr Rcpp-unitTests.tex

Rcpp-api.pdf: Rcpp-api.Rnw
	$(RPROG) CMD Sweave Rcpp-api.Rnw
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-api.tex', pdf = TRUE, clean = TRUE ) "
	rm -fr Rcpp-api.tex

Rcpp-introduction.pdf: Rcpp-introduction.Rnw
	$(RPROG) CMD Sweave Rcpp-introduction.Rnw
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-introduction.tex
	bibtex Rcpp-introduction
	pdflatex Rcpp-introduction.tex
	pdflatex Rcpp-introduction.tex
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-introduction.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-introduction.tex Rcpp-introduction.bbl Rcpp-introduction.blg Rcpp-introduction.aux Rcpp-introduction.out Rcpp-introduction.log

Rcpp-modules.pdf: Rcpp-modules/Rcpp-modules.Rnw
	cp -f Rcpp-modules/Rcpp-modules.Rnw .
	$(RSCRIPT) --vanilla -e "require(highlight); driver <- HighlightWeaveLatex(boxes = TRUE, bg = 'white' ); Sweave( 'Rcpp-modules.Rnw', driver = driver ); "
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-modules.tex', pdf = TRUE, clean = FALSE )"
	bibtex Rcpp-modules
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-modules.tex
	pdflatex Rcpp-modules.tex
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-modules.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-modules.tex Rcpp-modules.bbl Rcpp-modules.blg Rcpp-modules.aux Rcpp-modules.out Rcpp-modules.log
	cp -f Rcpp-modules/Rcpp-modules-fake.Rnw Rcpp-modules.Rnw

Rcpp-package.pdf : Rcpp-package/Rcpp-package.Rnw
	cp -f Rcpp-package/Rcpp-package.Rnw .
	$(RSCRIPT) --vanilla -e "require(highlight); driver <- HighlightWeaveLatex(boxes = TRUE, bg = 'white' ); Sweave( 'Rcpp-package.Rnw', driver = driver ); "
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-package
else
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-package.tex', pdf = TRUE, clean = FALSE )"
endif
	bibtex Rcpp-package
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-package
	pdflatex Rcpp-package
else
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-package.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-package.tex Rcpp-package.bbl Rcpp-package.blg Rcpp-package.aux Rcpp-package.out Rcpp-package.log
	cp -f Rcpp-package/Rcpp-package-fake.Rnw Rcpp-package.Rnw

Rcpp-FAQ.pdf : Rcpp-FAQ/Rcpp-FAQ.Rnw
	cp -f Rcpp-FAQ/Rcpp-FAQ.Rnw .
	$(RSCRIPT) --vanilla -e "require(highlight); driver <- HighlightWeaveLatex(boxes = TRUE, bg = 'white' ); Sweave( 'Rcpp-FAQ.Rnw', driver = driver ); "
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-FAQ.tex', pdf = TRUE, clean = FALSE )"
	bibtex Rcpp-FAQ
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-FAQ
	pdflatex Rcpp-FAQ
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-FAQ.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-FAQ.tex Rcpp-FAQ.bbl Rcpp-FAQ.blg Rcpp-FAQ.aux Rcpp-FAQ.out Rcpp-FAQ.log
	cp -f Rcpp-FAQ/Rcpp-FAQ-fake.Rnw Rcpp-FAQ.Rnw

Rcpp-extending.pdf : Rcpp-extending/Rcpp-extending.Rnw
	cp -f Rcpp-extending/Rcpp-extending.Rnw .
	$(RSCRIPT) --vanilla -e "require(highlight); driver <- HighlightWeaveLatex(boxes = TRUE, bg = 'white' ); Sweave( 'Rcpp-extending.Rnw', driver = driver ); "
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-extending.tex', pdf = TRUE, clean = FALSE )"
	bibtex Rcpp-extending
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-extending
	pdflatex Rcpp-extending
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-extending.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-extending.tex Rcpp-extending.bbl Rcpp-extending.blg Rcpp-extending.aux Rcpp-extending.out Rcpp-extending.log
	cp -f Rcpp-extending/Rcpp-extending-fake.Rnw Rcpp-extending.Rnw

Rcpp-sugar.pdf : Rcpp-sugar/Rcpp-sugar.Rnw
	cp -f Rcpp-sugar/Rcpp-sugar.Rnw .
	$(RSCRIPT) --vanilla -e "require(highlight); driver <- HighlightWeaveLatex(boxes = TRUE, bg = 'white' ); Sweave( 'Rcpp-sugar.Rnw', driver = driver ); "
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-sugar.tex', pdf = TRUE, clean = FALSE )"
	bibtex Rcpp-sugar
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-sugar
	pdflatex Rcpp-sugar
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-sugar.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-sugar.tex Rcpp-sugar.bbl Rcpp-sugar.blg Rcpp-sugar.aux Rcpp-sugar.out Rcpp-sugar.log
	rm Rcpp-sugar.Rnw
	cp Rcpp-sugar/Rcpp-sugar-fake.Rnw Rcpp-sugar.Rnw

Rcpp-attributes.pdf : Rcpp-attributes/Rcpp-attributes.Rnw
	cp -f Rcpp-attributes/Rcpp-attributes.Rnw .
	$(RSCRIPT) --vanilla -e "Sweave( 'Rcpp-attributes.Rnw' );"
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-attributes.tex', pdf = TRUE, clean = FALSE )"
	bibtex Rcpp-attributes
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-attributes
	pdflatex Rcpp-attributes
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-attributes.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-attributes.tex Rcpp-attributes.bbl Rcpp-attributes.blg Rcpp-attributes.aux Rcpp-attributes.out Rcpp-attributes.log
	rm Rcpp-attributes.Rnw
	cp Rcpp-attributes/Rcpp-attributes-fake.Rnw Rcpp-attributes.Rnw

Rcpp-quickref.pdf : Rcpp-quickref/Rcpp-quickref.Rnw
	cp -f Rcpp-quickref/Rcpp-quickref.Rnw .
	$(RSCRIPT) --vanilla -e "require(highlight); driver <- HighlightWeaveLatex(boxes = TRUE, bg = 'white' ); Sweave( 'Rcpp-quickref.Rnw', driver = driver ); "
	$(RSCRIPT) --vanilla -e "tools::texi2dvi( 'Rcpp-quickref.tex', pdf = TRUE, clean = FALSE )"
ifneq (,$(findstring edd,$(whoami)))
	pdflatex Rcpp-quickref
	pdflatex Rcpp-quickref
else
	$(RSCRIPT) -e "tools::texi2dvi( 'Rcpp-quickref.tex', pdf = TRUE, clean = TRUE )"
endif
	rm -fr Rcpp-quickref.tex Rcpp-quickref.bbl Rcpp-quickref.blg Rcpp-quickref.aux Rcpp-quickref.out Rcpp-quickref.log Rcpp-quickref.Rnw
	cp -f Rcpp-quickref/Rcpp-quickref-fake.Rnw Rcpp-quickref.Rnw

