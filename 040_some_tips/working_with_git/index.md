# Working with git

## Retrieve some important information

Retrieve some important variables from the shell:

```makefile
GIT_CURRENT_BRANCH:=$(shell git rev-parse --abbrev-ref HEAD)

GIT_ROOT_DIR:=$(shell git rev-parse --show-toplevel)

GIT_URL_REPO:=$(shell git config --get remote.origin.url | sed -r 's:git@([^/]+)\:(.*\.git):https\://\1/\2:g' | grep -Po '.*(?=\.)')
```

## Some git targets

When variables have been initialized, we can do things like this:

```makefile
git_open_repo:
	@sensible-browser ${GIT_URL_REPO}

git_open_pipeline:
	@sensible-browser ${GIT_URL_REPO}/pipelines

git_open_wiki:
	@sensible-browser ${GIT_URL_REPO}/-/wikis/home
```

## Git - Work in progress

Run `make git_wip` to quickly push your changes to the remote repository and skip the local hooks:

```makefile
git_wip:
	git add .
	git commit -m "wip --no-verify
	git push
```
