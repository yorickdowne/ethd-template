# Contributing

Contributions are welcome. All contributed code will be covered by the Apache License v2 of this project.

## Variable naming in `ethd`

Local variables are `variable_name`, declared with `local` at the top of the function.

Global variables are `__variable_name`. Declare every global at the top of `ethd`, and give each new one a comment
naming the function that sets it and the functions that read it. Never reuse a global for a different meaning; use a
local instead. Use a global only for process-wide context set at startup, accumulators such as `__warning_msg`, state
that the error trap reads, and arrays handed back from a function, such as `__volumes`.

A function that assigns to a variable named by its caller, with `printf -v`, names all of its own locals
`_variable_name`, with a single underscore. Bash scoping is dynamic: a local that shares a name with the caller's
variable would receive the value instead, and the caller would get nothing. Callers use `variable_name` and
`__variable_name`, which cannot clash. A caller declares that variable `local`. These functions also reject an output
name that matches one of their locals, and exit with code 70. Make that check with `case` rather than `=~`, because a
failed `=~` match empties the caller's `BASH_REMATCH`. See `__get_value_from_env`.

Assign `$?` to `exitstatus` before checking its value, unless you have a specific reason not to.

## Linting

The CI uses [pre-commit](https://pre-commit.com/) to lint all code within the repo. Add it to your local
copy with `apt install pre-commit` and `pre-commit install`.

This repo uses a squash-and-merge workflow to avoid extra merge commits. Create a branch for your feature or fix,
and work on this branch, then offer a PR from there.

If you end up working on `main`, you can create an `upstream` remote with
`git remote add upstream https://github.com/org/repo.git` (replacing org and repo), and create a git alias with
`git config --global alias.push-clean '!git fetch upstream main && git rebase upstream/main && git push -f'`. You can
then `git push-clean` to your fork before opening a PR.
