# How to extend a target

You've an existing target, let's say `hello` in our example, and you wish to extend it and add extra actions.

`hello` can be defined in the same makefile or in an included one but let's illustrate this with a basic example: we wish to add the *Nice to meet you* output.

```makefile
hello:
	@echo "Hello world"

hello:
	@echo "Nice to meet you"
```

If we run that file, here is the output.

```bash
> make hello

makefile:198: warning: overriding recipe for target 'hello'
makefile:195: warning: ignoring old recipe for target 'hello'
Nice to meet you
```

The solution: use `::` and not a single `:` after the recipe; see the next sample:

```makefile
hello::
	@echo "Hello world"

hello::
	@echo "Nice to meet you"

hello::
	@echo "Did you any plans for this week-end?"
```

If we run that file, here is the output.

```bash
> make hello

Hello world
Nice to meet you
Did you any plans for this week-end?
```

Recipes are just extended, the second one is appended to the first and so on so the order is important.
