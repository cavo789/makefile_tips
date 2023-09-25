# Working with Docker

In a makefile we can exit the command if we need a given Docker container up and running.

The `if` statement below will make sure the `sonarqube` container is running; if not because not yet created or in a exit mode f.i., an error statement will be executed and the script will be stopped.

```makefile
sonar-scan:
ifeq ($(shell docker ps -a -q -f name=sonarqube -f status=running),)
    # The sonarqube container didn't exists yet or not running (exit mode f.i.)
	@printf $(_RED) "ERROR - Please first run \"make sonar-server\""
	exit 1
endif
```

The `if` below will check if the container exists and if not, will create it.
The `else` statement knows thus that the container exists but will make sure it's running.

```makefile
ifeq ($(shell docker ps -a -q -f name=sonarqube),)
    # The sonarqube container didn't exists yet, create it.
	docker run -d --name sonarqube sonarqube:latest
else
    # Make sure the container is running
	-@docker container start sonarqube > /dev/null 2>&1
endif
```
