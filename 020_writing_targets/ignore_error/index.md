# Ignore error

Don't stop in case of error: add a `-` before the line like :`-php --lint index.php`

```makefile
phplint:
	-php --lint index.php
	echo "Yes, this script will continue..."
```

In case of linting error in `index.php` don't stop the execution of the script and process the next command.


Note: makefile can perhaps well display a message like `make: [makefile:114: up] Error 1 (ignored)` to inform the user an error has occurred but has been skipped.  If you don't want it at all (silent output), this is how to do:

```makefile
[...]
	@-docker network create -d bridge my_network >/dev/null 2>&1 || true
[...]
```

This command will create a new Docker network called `my_network` and if case of error (the network already exists f.i.), we don't care and don't want to see any output.


