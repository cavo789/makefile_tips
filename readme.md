<!-- This file has been generated by the concat.sh script. -->
<!-- Don't modify this file manually (you'll loose your changes) -->
<!-- but run the tool once more -->
<!-- Last refresh date: Friday, June 10, 2022, 09:02:17 -->

# Makefile - Tutorial and Tips & Tricks

![Banner](./banner.svg)

> Tutorial and collection of tips and tricks for how to use makefile

## Install the make executable

Just run the following commands to install the `Make` executable on your host machine:

```bash
sudo apt-get update && sudo apt-get -y install make
```

## Writing your first makefile

### Tab not space

The indentation to use when creating a `makefile` is the tabulation; not spaces. Using spaces will broke the file. 

```makefile
helloWorld:
	echo "Hello world"
```

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

#### Stop the job if a target fails

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

#### Make sure the parameters is set

Imagine we want to run `make runsql SQL='SELECT * FROM users LIMIT 10'` i.e. the `SQL`argument should be defined otherwise we'll have a problem.

```makefile
COLOR_RED:=31
COLOR_YELLOW:=33

runsql:
	@[ "${SQL}" ] && printf "[1;${COLOR_YELLOW}m\n%s[0m\n\n" "Running ${SQL}" || ( printf "[1;${COLOR_RED}m\n%s[0m\n\n" "ERROR - Please set the SQL to execute; consult the help if needed"; exit 1 )
    ## Ok, the argument is set, we can continue and run the SQL instruction...	
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

## Configure Visual Studio Code

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

### Getting the current directory into a variable

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

### Working with git

#### Retrieve some important information

Retrieve some important variables from the shell:

```makefile
### Return the current branch like `master` or `dev` or ...
GIT_CURRENT_BRANCH:=$(shell git rev-parse --abbrev-ref HEAD)

### The project root directory (f.i. `~/repositories/my-project)
GIT_ROOT_DIR:=$(shell git rev-parse --show-toplevel)

### Get the URL of the project and remove the .git final extension
### Get something like `https://xxxxx/my-project`
GIT_URL_REPO:=$(shell git config --get remote.origin.url | sed -r 's:git@([^/]+)\:(.*\.git):https\://\1/\2:g' | grep -Po '.*(?=\.)')
```

#### Some git targets

When variables have been initialized, we can do things like this:

```makefile
git_open_repo: #### Start a browser and open the repository
	@sensible-browser ${GIT_URL_REPO}

git_open_pipeline: #### Start a browser and open the repository pipelines
	@sensible-browser ${GIT_URL_REPO}/pipelines

git_open_wiki: #### Start a browser and open the WIKI page of the repository
	@sensible-browser ${GIT_URL_REPO}/-/wikis/home
```

#### Git - Work in progress

Run `make git_wip` to quickly push your changes to the remote repository and skip the local hooks:

```makefile
git_wip:
	git add .
	git commit -m "wip --no-verify
	git push
```

### Working with PHP project and vendors

#### Updating the vendor folder only when needed

Having a target like below (i.e. called `vendor`) will result in a check *Should the vendor be upgraded or not?*.  This is done by first running the `composer.lock` target. The idea is then to compare the date/time of that file and `composer.json`.  If there is a difference, the `Make` tool will run `composer update` and will generate a newer version of `composer.lock` and thus a newer version of `vendor` too.

If no changes have been made to the `composer.json` file, nothing has to be done since `vendor` is considered up-to-date.

Pretty easy.

```makefile
#! The name of the target should be `composer.lock` i.e. should be the name of the file.
#### Generate the composer.lock file as soon as the file is no more synchronized with composer.json
composer.lock: composer.json
	${CMD} composer update --no-interaction

### Will display "make: 'vendor' is up to date." when the `composer.lock` datetime is the same as the `vendor` folder
### i.e. when there is no need to update the `vendor` folder
#! The name of the target should be `vendor` i.e. should be the name of the folder.
#### Make / update the `vendor` folder. Remove `composer.lock` or `vendor` to force recreating the folder.
vendor: composer.lock
	${CMD} composer install
```

To run the scenario, just run `make vendor`.

## Some functions

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

### Open a web browser

```makefile
### Opens the default browser
open-browser:
	@sensible-browser http://www.google.fr
	@echo "The site has been opened in your browser"
```

This is pretty useful when you work with git repositories:

```makefile
### Get the URL of the project and remove the .git final extension
### Get something like `https://xxxxx/my-project`
GIT_URL_REPO:=$(shell git config --get remote.origin.url | sed -r 's:git@([^/]+)\:(.*\.git):https\://\1/\2:g' | grep -Po '.*(?=\.)')

#### Start a browser and open the repository
git_open_repo: 
	@sensible-browser ${GIT_URL_REPO}
```

### Self documenting makefile

You can use a tip for creating a target like `help`. The idea is to scan the current `makefile` and extract the list of all verbs and their description.

![Self documenting makefile](./050_some_functions/self_document/images/help.png)

There are a few different ways to achieve this. One working solution is to use [https://gist.github.com/klmr/575726c7e05d8780505a#file-show-help-minified-make](https://gist.github.com/klmr/575726c7e05d8780505a#file-show-help-minified-make).

The complexity comes when you're adding some files using `include` (like the `.env` file) and where the structure can be a bit different that your `makefile`.

The solution give here above is working for me.

## Tutorials

* [https://makefiletutorial.com/](https://makefiletutorial.com/)