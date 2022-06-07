# Makefile - Tutorial and Tips & Tricks

![Banner](./banner.svg)

> Tutorial and collection of tips and tricks for how to use makefile

- [Install the make executable](#install-the-make-executable)
- [How to write a target](#how-to-write-a-target)
  - [Tab not space](#tab-not-space)
  - [Don't echo the command](#dont-echo-the-command)
- [Configure vscode](#configure-vscode)
  - [Add the makefile extension](#add-the-makefile-extension)
  - [Add the .editorconfig file](#add-the-editorconfig-file)
- [Some tips](#some-tips)
  - [Define variable like getting the current directory](#define-variable-like-getting-the-current-directory)
  - [Getting informations from the .env file](#getting-informations-from-the-env-file)
  - [Set the default target](#set-the-default-target)
  - [Running dependant targets](#running-dependant-targets)
  - [Stop the job if a target fails](#stop-the-job-if-a-target-fails)
  - [Using parameters](#using-parameters)
- [Some functions](#some-functions)
  - [Open a web browser](#open-a-web-browser)
  - [Git - Work in progress](#git---work-in-progress)
  - [Clean folders](#clean-folders)
- [Tutorials](#tutorials)

## Install the make executable

```bash
sudo apt-get update && sudo apt-get -y install make
```

## How to write a target

### Tab not space

The indentation to use when creating a `makefile` is the tabulation; not spaces. Using spaces will broke the file. 

```makefile
helloWorld:
	echo "Hello world"
```

### Don't echo the command 

Running `make helloWorld` like below will output two lines on the console.

```makefile
helloWorld:
	echo "Hello world"
```

```text
echo "Hello world"
Hello world
```

To avoid the first one i.e. the output of the fired instruction, just prefix it with an arobase.

```makefile
helloWorld:
	@echo "Hello world"
```

Now, only the output will be echoed; no more the instruction itself.


## Configure vscode

### Add the makefile extension

[https://marketplace.visualstudio.com/items?itemName=ms-vscode.makefile-tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.makefile-tools)

### Add the .editorconfig file

Make sure your `Makefile` file is correctly formatted; add a file called `.editorconfig` in your root directory.

```text
[*]
end_of_line = lf
insert_final_newline = true

[Makefile]
indent_style = tab
indent_size = 4
```

## Some tips

### Define variable like getting the current directory

```makefile
PWD:=$(shell pwd)

current_dir:
	@echo "The current directory is ${PWD}"
```

### Getting informations from the .env file

Getting a value from a `.env` file is easy, just include it then use variables:

```env
DOCKER_IMAGE=cavo789/my_image
```

```makefile
include .env

migrateinit:
	@echo "The name of the image is ${DOCKER_IMAGE}"
```

This `include` tip will works with any file defining a variable and his value

We can perfectly have a file called `Make.config`, not `.env`

### Set the default target

By default, the first target defined in the file will be the default one i.e. the one fired when the user will just fire `make` on the command line.

```makefile
default: bonjour

bonjour:
	@echo "Bonjour"
```

### Running dependant targets

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

### Using parameters

Running a target with a parameter should be done using named parameters like this:

```bash
make bonjour firstname="Christophe"
```

This will create a variable called `firstname`, we then can use it:

```makefile
bonjour:
	@echo "Bonjour ${firstname}"
```

## Some functions

### Open a web browser

```makefile
# Opens the default browser
open-browser:
	@sensible-browser http://www.google.fr
	@echo "Le site a été démarré"
```

### Git - Work in progress

```makefile
wip:
    git add .
    git commit -m "wip
    git push
```

### Clean folders

```makefile
clean:
	rm -rf .cache .output node_modules/ storage/logs/ vendor/
```

We can also test for the existence of the folder first:

```makefile
clean:
	test ! -e vendor || rm -rf vendor
```



## Tutorials

* [https://makefiletutorial.com/](https://makefiletutorial.com/)
