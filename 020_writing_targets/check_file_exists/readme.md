# How to check if a file exists or not

You can use the `test -s` statement like below:

```makefile
    @test -s .config/phpunit.xml || echo "phpunit.xml didn't exists"
```

```makefile
    @test -s .config/phpunit.xml && echo "phpunit.xml exists"
```

Unlike the `ifeq` statement, `test` should be indented.

We can use the `{ ... }` notation if we need to run more than one command; f.i.:

```makefile
    @test -s .config/phpunit.xml || { echo "phpunit.xml file is missing, please take actions! Exiting..."; exit 1; }
```

We can also do this before, f.i., includes an external file:

```makefile
ifneq ("$(wildcard .env)","")
	include .env
endif
```

Of course, we can also keep it simple i.e. just use the `-` before the command to ignore errors so, below, if the file didn't exists, no error will be raised and the script will continue.

```makefile
-include .env
```
