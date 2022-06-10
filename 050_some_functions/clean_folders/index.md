# Clean folders

```makefile
clean:
	rm -rf .cache .output node_modules/ storage/logs/ vendor/
```

We can also test for the existence of the folder first:

```makefile
clean:
	test ! -e vendor || rm -rf vendor
```
