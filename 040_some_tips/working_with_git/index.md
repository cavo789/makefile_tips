# Working with git

## Retrieve some important information

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

## Some git targets

When variables have been initialized, we can do things like this:

```makefile
git_open_repo: ## Start a browser and open the repository
	@sensible-browser ${GIT_URL_REPO}

git_open_pipeline: ## Start a browser and open the repository pipelines
	@sensible-browser ${GIT_URL_REPO}/pipelines

git_open_wiki: ## Start a browser and open the WIKI page of the repository
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
