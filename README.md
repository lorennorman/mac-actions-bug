# Reproduce $GITHUB_PATH bug

This repository demonstrate a bug in the GitHub Actions tools.

According to [this documentation page](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#adding-a-system-path), we should be able to modify our `$PATH` by doing something like this:
```
$ echo "$HOME/my/path" >> $GITHUB_PATH
```

However, this does not appear to work on Windows or macOS runners, as seen on [this Actions run](https://github.com/lorennorman/mac-actions-bug/actions/runs/7263620867).

[The action yml file](https://github.com/lorennorman/mac-actions-bug/blob/main/.github/workflows/demonstrate_bug.yml) is cleanly written and documented to demonstrate the bug. It does use the `shell: bash` property for all steps.

In the actions output, I've also echo'd the `$PATH` before and after the `>> $GITHUB_PATH` invocation for easy inspection. The path is clearly modified on Ubuntu, and not on macOS or Windows.

Thank you for taking a look!
