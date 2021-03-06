# PHP - Quality checks

The portion below can just be copied/pasted in your own `Makefile` to add quality controls features based on the [https://github.com/jakzal/phpqa](https://github.com/jakzal/phpqa) Docker image.

```makefile
COLOR_BLUE:=34
COLOR_CYAN:=36
COLOR_YELLOW:=33

# region - Implement `Quality Assurance` features; relying on the jakzal/phpqa Docker image)
# Define the Docker image to use for PHPQA (https://github.com/jakzal/phpqa)
#! PHP_VERSION should be defined in the `.env` file
PHP_VERSION:=$(or $(PHP_VERSION),8.1)

DOCKER_PHPQA:=jakzal/phpqa:php${PHP_VERSION}-alpine

# The full "docker run" command to be able to run a command in the jakzal/phpqa container
DOCKER_PHPQA_ARGS:=-it --rm -v ${PWD}:/project -w /project -v ${PWD}/.output/tmp-phpqa:/tmp

# Get the current user/group ID and define the docker "-u" argument
DOCKER_PHPQA_UID_GID:=-u $(shell id -u):$(shell id -g)

# Finally, the final command for running a command in the jakzal/phpqa container
DOCKER_PHPQA_COMMAND:=docker run ${DOCKER_PHPQA_ARGS} ${DOCKER_PHPQA_UID_GID} ${DOCKER_PHPQA}

## Run PHAN and report problems
phan:
	clear
    # This is how to define a local variable
	$(eval COMMAND := $(shell echo "phan --progress-bar --config-file .config/phan.php"))
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Report PHAN errors in your PHP files (no fix) using $(DOCKER_PHPQA)"
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Running \"${COMMAND}\""
	@printf "[1;${COLOR_BLUE}m%s[0m\n\n" "Note: make sure your /vendor folder is populated..."
	@docker run -it ${DOCKER_PHPQA_ARGS} ${DOCKER_PHPQA} phan --version
	@echo ""
	${DOCKER_PHPQA_COMMAND} ${COMMAND}

## Run PHP-CS-FIXER on the codebase IN A DRY-RUN MODE
php-cs-fixer-dry-run:
	clear
    # This is how to define a local variable
	$(eval COMMAND := $(shell echo "php-cs-fixer fix --verbose --config=/project/.config/.php-cs-fixer.php --using-cache=no --diff"))
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Report code formatting issues in your PHP files (no fix) using $(DOCKER_PHPQA)"
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Running \"${COMMAND} --dry-run\""
	${DOCKER_PHPQA_COMMAND} ${COMMAND} --dry-run

## Run PHP-CS-FIXER on the codebase and fix problems
php-cs-fixer-fix:
	clear
    # This is how to define a local variable
	$(eval COMMAND := $(shell echo "php-cs-fixer fix --verbose --config=/project/.config/.php-cs-fixer.php --using-cache=no --diff"))
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Automatically fix formatting issues in your PHP files using $(DOCKER_PHPQA)"
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Running \"${COMMAND}\""
	${DOCKER_PHPQA_COMMAND} ${COMMAND}

## PHP - Check the syntax of PHP files and report syntax errors if any
phplint:
	clear
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Check syntax validity of php files using $(DOCKER_PHPQA)"
    # https://github.com/overtrue/phplint
	${DOCKER_PHPQA_COMMAND} phplint --no-cache --exclude=vendor

## YAML - Check the syntax of yaml files in the directory root folder and report syntax errors if any
yamllint:
	clear
	@printf "[1;${COLOR_YELLOW}m%s[0m\n\n" "Check syntax validity of yaml files using $(DOCKER_PHPQA)"
    # https://github.com/j13k/yaml-lint
    # (it seems yaml-lint can't search in all the tree structure and *.yml won't get .gitlab-ci.yml; strange)
	${DOCKER_PHPQA_COMMAND} yaml-lint .*.yml *.yml .install/CI/*.yml
# endregion
```
