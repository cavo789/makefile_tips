# Using parameters

Running a target with a parameter should be done using named parameters like this:

```bash
make bonjour firstname="Christophe"
```

This will create a variable called `firstname`, we then can use it:

```makefile
bonjour:
	@echo "Bonjour ${firstname}"
```

## Make sure the parameters is set

Imagine we want to run `make runsql SQL='SELECT * FROM users LIMIT 10'` i.e. the `SQL`argument should be defined otherwise we'll have a problem.

```makefile
COLOR_RED:=31
COLOR_YELLOW:=33

runsql:
	@[ "${SQL}" ] && printf "[1;${COLOR_YELLOW}m\n%s[0m\n\n" "Running ${SQL}" || ( printf "[1;${COLOR_RED}m\n%s[0m\n\n" "ERROR - Please set the SQL to execute; consult the help if needed"; exit 1 )
```
