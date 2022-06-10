# Self documenting makefile

You can use a tip for creating a target like `help`. The idea is to scan the current `makefile` and extract the list of all verbs and their description.

![Self documenting makefile](./images/help.png)

There are a few different ways to achieve this. One working solution is to use [https://gist.github.com/klmr/575726c7e05d8780505a#file-show-help-minified-make](https://gist.github.com/klmr/575726c7e05d8780505a#file-show-help-minified-make).

The complexity comes when you're adding some files using `include` (like the `.env` file) and where the structure can be a bit different that your `makefile`.

The solution give here above is working for me.
