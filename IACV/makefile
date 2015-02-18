clean:
	latexmk -c

summary:
	pandoc \
		--from markdown+yaml_metadata_block+table_captions \
		--to latex \
		--variable documentclass=article \
    --variable classoption=a4paper \
    --variable classoption=10pt \
		--number-sections \
		--include-in-header ../summary-header.tex \
		--listings \
		--output IACV.tex \
		--standalone \
		IACV*.md
	latexmk -pdflatex="pdflatex --shell-escape %O %S" -pdf IACV.tex
