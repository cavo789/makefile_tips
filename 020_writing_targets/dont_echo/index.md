# Don't echo the command 

Running `make helloWorld` like below will output two lines on the console.

```makefile
helloWorld:
	echo "Hello world"
```

```text
echo "Hello world"
Hello world
```

To avoid the first one i.e. the output of the fired instruction, just prefix it with an at sign (`@`).

```makefile
helloWorld:
	@echo "Hello world"
```

Now, only the output will be echoed; no more the instruction itself.
