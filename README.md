# Reproduce $GITHUB_PATH bug

This repository demonstrate a bug in the GitHub Actions tools.

***SOLVED***

The docs say (emphasis mine):
> Prepends a directory to the system PATH variable and automatically **makes it available to all subsequent actions in the current job**; **the currently running action cannot access the updated path variable**...

This is how updating the path usually works anyway, you have to create a new shell to get the new $PATH automatically. I was confused that Ubuntu seemed to be working without waiting for the next step, so I went and looked. Turns out Ubuntu runners already have the special path I was adding (`$HOME/.local/bin`) and thus the `... >> $GITHUB_PATH` was having no effect.

Original README content follows...

According to [this documentation page](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#adding-a-system-path), we should be able to modify our `$PATH` by doing something like this:
```
$ echo "$HOME/my/path" >> $GITHUB_PATH
```

However, this does not appear to work on Windows or macOS runners, as seen on [this Actions run](https://github.com/lorennorman/mac-actions-bug/actions/runs/7263620867).

[The action yml file](https://github.com/lorennorman/mac-actions-bug/blob/main/.github/workflows/demonstrate_bug.yml) is cleanly written and documented to demonstrate the bug. It does use the `shell: bash` property for all steps.

In the actions output, I've also echo'd the `$PATH` before and after the `>> $GITHUB_PATH` invocation for easy inspection. The path is clearly modified on Ubuntu, and not on macOS or Windows.

Thank you for taking a look!
