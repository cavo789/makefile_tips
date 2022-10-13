# How to check if a folder exists or not

You can use the `ifeq` statement with a Linux shell command like below:

```makefile
ifeq ($(shell test -e vendor/a/b/c || echo -n no),no)
    @echo "The folder didn't exists"
endif
```

```makefile
ifeq ($(shell test -e vendor/a/b/c && echo -n yes),yes)
    @echo "The folder exists"
endif
```

Unlike the `test` statement, `ifeq` should be used without indentation. Directly at position column zero.
