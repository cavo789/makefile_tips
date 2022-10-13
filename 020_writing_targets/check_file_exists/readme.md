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
