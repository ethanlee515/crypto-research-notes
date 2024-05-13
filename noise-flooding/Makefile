main.pdf: main.tex macros.tex references.bib
	latexmk -pdf main
	cp main.pdf ../noise-flooding.pdf

clean:
	git clean -d -x -f
