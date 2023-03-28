# Get a list of files and initialize a variable

Let's take a real use case: scan a folder called `.docker` and retrieve the list of `docker-compose*.yml` files there.

The objective is to initialize an environment variable called `COMPOSE_FILE` (see [https://docs.docker.com/compose/environment-variables/envvars/#compose_file](https://docs.docker.com/compose/environment-variables/envvars/#compose_file)) so, when running `docker compose` we can use all files at once (i.e. by not adding the `--file file1.yml --file file2.yml` and on)

So, getting the list of `docker-compose*.yml` can be done like this:

```makefile
DOCKER_YAML_FILES := $(shell find ./.docker -name 'docker-compose*.yml')
DOCKER_YAML_FILES := $(shell echo "$(DOCKER_YAML_FILES)" | tr ' ' ':')
```

The first line will return f.i. `docker-compose.yml docker-compose.override.yml docker-compose.mysql.yml`.

The second instruction will replace the space by a colon (`:`).

We'll thus obtain `docker-compose.yml:docker-compose.override.yml:docker-compose.mysql.yml`.

Now, to run `docker compose config` for instance, we just need to do the following i.e. first declare the `COMPOSE_FILE` environment variable then run the desired action.

```makefile
.PHONY: config
config:
 COMPOSE_FILE=${DOCKER_YAML_FILES} docker compose config
```
