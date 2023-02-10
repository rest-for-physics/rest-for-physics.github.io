---
layout: default
title: Contributing
parent: REST Advanced
nav_order: 10
---

REST-for-Physics has many contributors so a few steps are necessary in order to maintain a good code quality and style. For a complete guide please have a look at our main framework [Contributing guide](https://github.com/rest-for-physics/framework/blob/master/CONTRIBUTING.md).

In the following we will introduce crucial aspects you should consider before pushing to the repository.

### Installing pre-commit hooks

The [pre-commit tool](https://pre-commit.com/) is used to check the code style and to automatically fix some of the issues.

It can be installed with pip:

```
pip install pre-commit
```

Then, in the REST-for-Physics framework root directory, run:

```
pre-commit install
```

This will install the pre-commit hook in the .git directory of the repository. This hook will be executed every time you commit a change to the repository. If the code style is not correct, the commit will be aborted and you will have to fix the issues before committing again.

If you would like to commit without checking the code style, you can use the `--no-verify` option (not recommended):

```
git commit --no-verify
```

You can also manually run the pre-commit hook on all files with the following command. By default the hook is only run on the files that have been modified.

```
pre-commit run --all-files
```

For additional information, please refer to the [pre-commit documentation](https://pre-commit.com/).
