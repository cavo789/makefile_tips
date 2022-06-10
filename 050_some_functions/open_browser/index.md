# Open a web browser

```makefile
open-browser:
	@sensible-browser http://www.google.fr
	@echo "The site has been opened in your browser"
```

This is pretty useful when you work with git repositories:

```makefile
GIT_URL_REPO:=$(shell git config --get remote.origin.url | sed -r 's:git@([^/]+)\:(.*\.git):https\://\1/\2:g' | grep -Po '.*(?=\.)')

git_open_repo: 
	@sensible-browser ${GIT_URL_REPO}
```
