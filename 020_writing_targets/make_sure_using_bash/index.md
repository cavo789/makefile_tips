# Make sure you're using Bash

By default, `make` is using `/bin/sh`. This can be upgraded by adding this assignation at the top of the `makefile`:

```makefile
SHELL:=bash
```

(source: [https://tech.davis-hansson.com/p/make/#always-use-a-recent-bash](https://tech.davis-hansson.com/p/make/#always-use-a-recent-bash))
