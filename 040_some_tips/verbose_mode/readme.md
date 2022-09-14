# Verbose mode

The idea: don't show informative message when running some targets.

The code below will check the presence of the `--quiet` argument / value in the `$ARGS` standard variable.

```makefile
# Sample code to demonstrate how to enable/disable verbose mode
# in a makefile.
#
# Using the "--quiet" argument in ARGS.
#
# Verbose mode: run `make testme` on the command line
# Silent  mode: run `make testme ARGS="--quiet"` on the command line

QUIET=$(if $(findstring --quiet,${ARGS}),true,false)

testme:
ifeq ($(QUIET),false)
	@printf '\e[1;30m%s\n\e[m' "QUIET MODE NOT ENABLED - We'll show any informative text."
endif
	@printf '\e[1;32m%s\n\n\e[m' "This is an important message"
´´´

The code above define a global variable `QUIET` that will be set to `true` or `false` depending on the presence of the `--quiet` keyword in `ARGS`.

Then, use the `ifeq` conditional structure to show (or hide) informative message.

By running `make testme ARGS="--quiet"` only *This is an important message* will be displayed.

