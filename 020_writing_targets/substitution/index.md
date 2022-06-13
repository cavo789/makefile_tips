# Substitution

Consider `make convert INFILE=readme.md`

```makefile
outfile=$(INFILE:.md=.pdf)

COLOR_BLUE:=34

convert:
	@printf "[1;${COLOR_BLUE}m%s[0m\n\n" "INPUT FILE IS ${INFILE}; OUTPUT WILL BE ${outfile}"
```

The syntax `outfile=$(INFILE:.md=.pdf)` will replace `.md` by `.pdf` so we can, in this example, derive the output file based on the input file.
