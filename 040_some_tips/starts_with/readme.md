# How to check if a variable starts with a given value?

The use case is: we have a variable called `PHP_VERSION` and we need to detect if we need to deal with PHP 7 code or PHP 8 or greater.

```makefile
IS_PHP_7 := $(shell [[ $(PHP_VERSION) =~ 7[0-9.]+$$ ]] && echo "yes")

doThing:
ifdef IS_PHP_7
	@echo "Hey, it's an old version of PHP, why not migrate to PHP 8?"
else
	@echo "Nice! You're using PHP 8 or greater"
endif
```

Since Makefile didn't support regexes, we rely on the `shell` for running the regex and to return an non empty string if the expression is matched. In that case, the `IS_PHP_7` variable is defined and the `ifdef` construction will be verified.
