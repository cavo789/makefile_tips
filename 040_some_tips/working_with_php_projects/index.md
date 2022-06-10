# Working with PHP project and vendors

## Updating the vendor folder only when needed

Having a target like below (i.e. called `vendor`) will result in a check *Should the vendor be upgraded or not?*.  This is done by first running the `composer.lock` target. The idea is then to compare the date/time of that file and `composer.json`.  If there is a difference, the `Make` tool will run `composer update` and will generate a newer version of `composer.lock` and thus a newer version of `vendor` too.

If no changes have been made to the `composer.json` file, nothing has to be done since `vendor` is considered up-to-date.

Pretty easy.

```makefile
#! The name of the target should be `composer.lock` i.e. should be the name of the file.
## Generate the composer.lock file as soon as the file is no more synchronized with composer.json
composer.lock: composer.json
	${CMD} composer update --no-interaction

# Will display "make: 'vendor' is up to date." when the `composer.lock` datetime is the same as the `vendor` folder
# i.e. when there is no need to update the `vendor` folder
#! The name of the target should be `vendor` i.e. should be the name of the folder.
## Make / update the `vendor` folder. Remove `composer.lock` or `vendor` to force recreating the folder.
vendor: composer.lock
	${CMD} composer install
```

To run the scenario, just run `make vendor`.
