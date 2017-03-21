Pandoc Markdown-Latex Boilerplate
==========================

* Initial Author: David Caplan http://www.davecap.com/
* Extended Author: Brian Khuu http://www.briankhuu.com/

Use this to write a paper in Markdown and render it as PDF.

 *   Step 1: edit the template.tex to your liking
 *   Step 2: edit the example.md with your text, update config.txt and sections.txt
 *   Step 3: make pdf (`make pdf` in linux)(`winmake pdf` in windows)
 *   Step 4: enjoy pdf

**TL;DR: For windows users double-click on `winmake.bat`**

# Versions

* V1.0 - Now should be build-able in both linux and windows with consistant configeration system
* V1.1 - Just noticed that pandoc can now natively generate pdf files and have thus removed markdown2latex command. Added pdf safemode which omits template if anything goes wrong with it. Also updated the default template to https://github.com/jgm/pandoc-templates/blob/master/default.latex which is the latest one that pandoc supports. Lastly builname is now definable in config file. It was previously hardcoded in.
* V1.2 - Made the interface for winmake.bat more noobproof. Also arraged the pages to be more understandable and cleaner.
* V1.3 - Decided to move all the pages and images to specifically a hardcoded /source/ folder. Which would help keep everything more cleaner.
* V1.4 - Changed the makefile script to just focus on linux compatible code rather than windows as well (Since we can just use the .bat file).

------------------------------------------------------------------------

# Requirements

 Follow instruction on installing pandoc in this link http://pandoc.org/installing.html . For PDF output, you’ll also need to install LaTeX. We recommend MiKTeX.

 You also need to install the content of https://github.com/citation-style-language/styles into the csl if you want to fully ultilize csl.

# Building

## Configering the build system

in config text file `config.txt`, you may have these default entries

    SECTIONS_FILEPATH=_SECTIONS.txt
    BUILDNAME=example
    REFERENCES=references.bib
    TEMPLATE=template.tex
    # TEMPLATE=ut-thesis.tex
    CSL=../elsevier-with-titles

This is what it means

	SECTIONS_FILEPATH=
		Holds a link to a file
		that contains a list
		of pages to join

	BUILDNAME=
		Let's you define the name of the generated file in the /build/ folder.

	REFERENCES=
		Links to pandoc reference file
		for the biography

	TEMPLATE=
		Links to template file use by LaTeX

	CSL=
		Choose from a list of Citation Style Language
		files found in ./CSL/ e.g. IEEE style.

Note:
 * `#` are comments.
 * You cannot have spaces in your key=value like `TEMPLATE = ut-thesis.tex` if you want to use `winmake.bat`, it must be like `TEMPLATE=ut-thesis.tex`.
 * If you open the config.txt file to modify it (e.g. notepad.exe), you need to close the config file, otherwise MikTex will come up with an error stating that the files are already in use.
 *

### SECTIONS_FILEPATH

The file in SECTIONS_FILEPATH might look like this:

```
    example.md references.md
```

Where each file is entered in a single line delimitated by a single ` ` in sequence.

The reason is because the build file is automagically copying the content from `sections.txt` directly into `$(SECTIONS)` in for example the pandoc build html command line: `pandoc -S -5 ...etc... -t html --normalize $(SECTIONS)`. (e.g. `pandoc -S -5 ...etc... -t html --normalize example.md references.md`


## Experimental

This is only tested in windows, but hopefully should work on linux as well.

```
    example.md
    references.md
```


## CSL: Citation Style Language

The CSL files are located in the csl submodule.

## Linux

---

	make pre

Create build folder **This is always done before outputting a new build**

Then jumps to the source directory for the next stage of actually making the document.

---

	make post

@echo POST

---

	make clean

Remove build folder

---

	make pdf

Create pdf `markdown2pdf --toc -N --bibliography=../$(REFS) -o ../build/example.pdf --csl=../csl/$(CSL).csl --template=$../(TEMPLATE) $(SECTIONS)`

---

	make latex

Create latex `pandoc --toc -N --bibliography=../$(REFS) -o ../build/example.tex --csl=../csl/$(CSL).csl --template=../$(TEMPLATE) $(SECTIONS)`

---

	make html

Create html `pandoc -S -5 --mathjax="http://cdn.mathjax.org/mathjax/latest/MathJax.js" --section-divs -s --biblatex --toc -N --bibliography=../$(REFS) -o ../build/example.html -t html --normalize $(SECTIONS)`

---

	make embed

Create embedded html `pandoc -S --reference-links --mathjax="http://cdn.mathjax.org/mathjax/latest/MathJax.js" --section-divs -N --bibliography=../$(REFS) --csl=../csl/$(CSL).csl -o ../build/embed.html -t html --normalize $(SECTIONS)`

---

	make epub

Create epub format `pandoc -S -s --biblatex --toc -N --bibliography=../$(REFS) -o ../build/example.epub -t epub --normalize $(SECTIONS)`

---

	make

Falls back to pdf behaviour

------------------------------------------------------------------------

## Windows

**NOOB?: Just double click on `winmake.bat` and type pdf or pdf-safemode if pdf doesn't work. Else choose html.**

---

	winmake clean

Removes the build folder

---

	winmake pdf

Builds a pdf file to ./build/ folder. Requires LaTeX.

---

	winmake pdf-safemode

Builds a pdf file to ./build/ folder. Requires LaTeX. Ignores template and CSL settings.

---

	winmake epub

Builds a epub file to ./build/ folder

---

	winmake html

Builds a html file to ./build/ folder

---

	winmake

Opens up a prompt.

---


------------------------------------------------------------------------


## Tips and Tricks

### Using makefile in windows

You are not restricted to using winmake.bat in windows. If you install git for windows from http://www.git-scm.com/ in addition to the make program in http://gnuwin32.sourceforge.net/packages/make.htm (Also add the program to path). This is since a copy of git will include a Git Bash Shell, which will support unix commands like `cat` etc... .

For your convenience, a copy of make.exe (As of 25 November 2006) is included. [Being GPLed its source code is also in the website.](http://gnuwin32.sourceforge.net/packages/make.htm).

### Opening Windoww commandline in the right directly quickly

SHIFT+RIGHT_CLICK on your folder in windows will display an extra entry in the dropdown saying "Open Command Window Here". This will save you lots of time. Or you can make a batch file like `buildpdf.bat` that contains `winmake pdf`.


### editing .bib bibilography files

Some software to consider

* http://jabref.sourceforge.net/
