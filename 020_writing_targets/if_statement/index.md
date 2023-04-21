# Using conditional statements

We can make some conditional statement like this but, be careful on the indentation:

```makefile
init:
ifeq ($(shell expr $(SET_PHP_VERSION) \>= 8), 1)
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Upgrade dependencies to PHP 8"
else
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Downgrade dependencies to PHP 7"
endif
```

`make init SET_PHP_VERSION=7.4` will display the downgrade message while `make init SET_PHP_VERSION=8.1` the upgrade one.

Another examples:

```makefile
ifneq ("$(DB_TYPE)","")
	echo "The DB_TYPE variable is not empty
endif

ifeq ("$(DB_TYPE)","")
	echo "The DB_TYPE variable is empty
endif
```
