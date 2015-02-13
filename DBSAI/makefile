clean:
	latexmk -C

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
		--output DBSAI.tex \
		--standalone \
		DBSAI*.md
	latexmk -pdflatex="pdflatex --shell-escape %O %S" -pdf DBSAI.tex
