# Working with PHP project and vendors

## Updating the vendor folder only when needed

Having a target like below (i.e. called `vendor`) will result in a check *Should the vendor be upgraded or not?*.  This is done by first running the `composer.lock` target. The idea is then to compare the date/time of that file and `composer.json`.  If there is a difference, the `Make` tool will run `composer update` and will generate a newer version of `composer.lock` and thus a newer version of `vendor` too.

If no changes have been made to the `composer.json` file, nothing has to be done since `vendor` is considered up-to-date.

Pretty easy.

```makefile
composer.lock: composer.json
	${CMD} composer update --no-interaction

vendor: composer.lock
	${CMD} composer install
```

To run the scenario, just run `make vendor`.
