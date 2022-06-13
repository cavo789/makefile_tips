# Ignore error

Don't stop in case of error

```makefile
phplint:
	-php -l index.php
	echo "Yes, this script will continue..."
```

In case of linting error in `index.php` don't stop the execution of the script and process the next command.
