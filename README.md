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
  - [Getting information's from the .env file](#getting-informations-from-the-env-file)
  - [Set the default target](#set-the-default-target)
  - [Running dependant targets](#running-dependant-targets)
  - [Stop the job if a target fails](#stop-the-job-if-a-target-fails)
  - [Using parameters](#using-parameters)
  - [Working with git](#working-with-git)
    - [Git - Work in progress](#git---work-in-progress)
- [Some functions](#some-functions)
  - [Self documenting makefile](#self-documenting-makefile)
  - [Open a web browser](#open-a-web-browser)
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

To avoid the first one i.e. the output of the fired instruction, just prefix it with an at sign (`@`).

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

### Getting information's from the .env file

Getting a value from a `.env` file is easy, just include it then use variables:

```env
DOCKER_IMAGE=cavo789/my_image
```

```makefile
include .env

helloDocker:
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


### Working with git

Retrieve some important variables from the shell:

```makefile
# Return the current branch like `master` or `dev` or ...
GIT_CURRENT_BRANCH:=$(shell git rev-parse --abbrev-ref HEAD)

# The project root directory (f.i. `~/repositories/my-project)
GIT_ROOT_DIR:=$(shell git rev-parse --show-toplevel)

# Get the URL of the project and remove the .git final extension
# Get something like `https://xxxxx/my-project`
GIT_URL_REPO:=$(shell git config --get remote.origin.url | sed -r 's:git@([^/]+)\:(.*\.git):https\://\1/\2:g' | grep -Po '.*(?=\.)')
```

When initialized, we can do things like this:

```makefile
git_open_repo: ## Start a browser and open the repository
	@sensible-browser ${GIT_URL_REPO}

git_open_pipeline: ## Start a browser and open the repository pipelines
	@sensible-browser ${GIT_URL_REPO}/pipelines

git_open_wiki: ## Start a browser and open the WIKI page of the repository
	@sensible-browser ${GIT_URL_REPO}/-/wikis/home
```
#### Git - Work in progress

```makefile
git_wip:
	git add .
	git commit -m "wip --no-verify
	git push
```

## Some functions

### Self documenting makefile

You can use a tip for creating a target like `help`. The idea is to scan the current `makefile` and extract the list of all verbs and their description.

![Self documenting makefile](./images/help.png)

There are a few different ways to achieve this. One working solution is to use [https://gist.github.com/klmr/575726c7e05d8780505a#file-show-help-minified-make](https://gist.github.com/klmr/575726c7e05d8780505a#file-show-help-minified-make).

The complexity comes when you're adding some files using `include` (like the `.env` file) and where the structure can be a bit different that your `makefile`.

The solution give here above is working for me.

### Open a web browser

```makefile
# Opens the default browser
open-browser:
	@sensible-browser http://www.google.fr
	@echo "The site has been opened in your browser"
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
