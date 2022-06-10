# Running dependant targets

By running `make php-cs-fixer`, we'll first run `vendor` then `update-them`, finally `php-cs-fixer` i.e. we can define a list of dependant targets.

```makefile
php-cs-fixer: vendor update-them
	@echo "And finally run php-cs-fixer"

vendor:
	@echo "First get vendors"

update-them:
	@echo "Then update vendors"
```

Running `make php-cs-fixer` will output this:

```text
First get vendors
Then update vendors
And finally run php-cs-fixer
```

## Stop the job if a target fails

If one of them fails, the script will stop.  In the example below, `php-cs-fixer` will never print `And finally run php-cs-fixer`.

```makefile
php-cs-fixer: vendor update-them
	@echo "And finally run php-cs-fixer"

vendor:
	@echo "First get vendors"

update-them:
	@echo "Then update vendors"
	exit 2 && echo "Oups, an error has occured"
```

Running `make php-cs-fixer` will output this:

```text
First get vendors
Then update vendors
And finally run php-cs-fixer
```

