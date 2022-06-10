# Getting information's from the .env file

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

## Define variable based on .env environment variables

Let's imagine you've a variable called `APP_ENV` in your `.env` file.

That variable can be set to `local`, `test` or whatever you want. Will be set to `production` when the application is running in the production environment.

So, based on that variable, we can define a variable like this:

```makefile
ifeq ($(APP_ENV), production)
	CMD:=
else
	CMD:=docker compose exec my_docker_image
endif
```

This means: if we're not running in production, every command will be fired inside our Docker container. If running in production, the command will be executed directly.

Here is an example:

```makefile
composer-update:
	${CMD} composer update --no-interaction
```

## Use a default value if the variable is not defined

If the OS environment variable `PHP_VERSION` is not defined, set its default value to `8.1`

```makefile
PHP_VERSION  :=$(or $(PHP_VERSION),8.1)
```

This will allow things like below i.e. target a custom version of a Docker image based on the selected PHP version.

```makefile
DOCKER_PHPQA:=jakzal/phpqa:php${PHP_VERSION}-alpine
```

### Override a variable

Even if the variable is still defined, you can override it by passing it on the command line:

```bash
make yamllint PHP_VERSION=8.1
```

This will start the `yamllint` target with `PHP_VERSION` set to `8.1` even if the variable is already defined and f.i. set to `7.4`.
