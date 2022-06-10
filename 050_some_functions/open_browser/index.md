# Open a web browser

```makefile
# Opens the default browser
open-browser:
	@sensible-browser http://www.google.fr
	@echo "The site has been opened in your browser"
```

This is pretty useful when you work with git repositories:

```makefile
# Get the URL of the project and remove the .git final extension
# Get something like `https://xxxxx/my-project`
GIT_URL_REPO:=$(shell git config --get remote.origin.url | sed -r 's:git@([^/]+)\:(.*\.git):https\://\1/\2:g' | grep -Po '.*(?=\.)')

## Start a browser and open the repository
git_open_repo: 
	@sensible-browser ${GIT_URL_REPO}
```
