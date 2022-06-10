# Clean folders

```makefile
COLOR_YELLOW:=33

clean:
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Remove node_modules and vendor folders"
	rm --recursive --force node_modules/ vendor/
	@printf "[1;${COLOR_YELLOW}m\n%s[0m\n\n" "Empty .cache, .output and storage/logs"
	rm --recursive --force .cache/* .output/* storage/logs/*
```

We can also test for the existence of the folder first:

```makefile
clean:
	test ! -e vendor || rm -rf vendor
```
