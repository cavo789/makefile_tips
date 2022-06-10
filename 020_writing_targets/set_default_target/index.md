# Set the default target

By default, the first target defined in the file will be the default one i.e. the one fired when the user will just fire `make` on the command line.

```makefile
default: bonjour

bonjour:
	@echo "Bonjour"
```
