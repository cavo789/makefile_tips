# Tab not space

The indentation to use when creating a `makefile` is the tabulation; not spaces. Using spaces will broke the file. 

```makefile
helloWorld:
	echo "Hello world"
```

however it is possible to adapt this behavior using `.RECIPEPREFIX`:

```makefile
.RECIPEPREFIX = >

changeExt:
# This is a comment
> @printf "%s\n" "It works"
```

(source: [https://tech.davis-hansson.com/p/make/#dont-use-tabs](https://tech.davis-hansson.com/p/make/#dont-use-tabs))

