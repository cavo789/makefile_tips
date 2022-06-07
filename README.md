# Makefile - Tutorial and Tips & Tricks

![Banner](./banner.svg)

> Tutorial and collection of tips and tricks for how to use makefile

- [Install the make executable](#install-the-make-executable)
- [Get the current directory using a shell function](#get-the-current-directory-using-a-shell-function)
- [Getting informations from the .env file](#getting-informations-from-the-env-file)
- [Set the default target](#set-the-default-target)
- [Running dependant targets](#running-dependant-targets)
  - [Stop the job if a target fails](#stop-the-job-if-a-target-fails)
- [Open a web browser](#open-a-web-browser)
- [Git - Work in progress](#git---work-in-progress)
- [Using parameters](#using-parameters)
- [Tutorials](#tutorials)

## Install the make executable

```bash
sudo apt-get update && sudo apt-get -y install make
```

## Get the current directory using a shell function

```makefile
PWD:=$(shell pwd)

current_dir:
	@echo "The current directory is ${PWD}"
```

## Getting informations from the .env file

Getting a value from a `.env` file is easy, just include it then use variables:

```env
DOCKER_IMAGE=cavo789/my_image
```

```makefile
include .env

migrateinit:
	@echo "The name of the image is ${DOCKER_IMAGE}"
```

## Set the default target

By default, the first target defined in the file will be the default one i.e. the one fired when the user will just fire `make` on the command line.

```makefile
default: bonjour

bonjour:
	@echo "Bonjour"
```

## Running dependant targets

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

### Stop the job if a target fails

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

## Open a web browser

```makefile
# Opens the default browser
open-browser:
	@sensible-browser http://www.google.fr
	@echo "Le site a été démarré"
```

## Git - Work in progress

```makefile
wip:
    git add .
    git commit -m "wip
    git push
```

## Using parameters

Running a target with a parameter should be done using named parameters like this:

```bash
make bonjour firstname="Christophe"
```

This will create a variable called `firstname`, we then can use it:

```makefile
bonjour:
	@echo "Bonjour ${firstname}"
```

## Tutorials

* [https://makefiletutorial.com/](https://makefiletutorial.com/)
