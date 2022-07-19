---
layout: post
title:  "SSH into GitHub Codespaces"
date:   2022-07-18 18:00:00 +0000
categories: github codespaces ssh
---
Connecting to a [GitHub's codespaces](https://github.com/features/codespaces) instance through `ssh` could be as simple as:

```bash
$ gh cs ssh
```
...if using [`GitHub's CLI tool`](https://cli.github.com/) [`ssh` command](https://cli.github.com/manual/gh_codespace_ssh).

But, chances are you would have no clue when requested for the user's password.

```bash
$ gh cs ssh
? Choose codespace: garodriguezlp/garodriguezlp.github.io: ssh-codespsaces*
vscode@localhost's password:
```

## Setting the Password

At this point, it would be worth knowing how GitHub has configured the [`ssh` daemon](https://linux.die.net/man/8/sshd) for its
`codespaces`, which we could find [in this `dev-containers` reference
documentation](https://github.com/microsoft/vscode-dev-containers/blob/main/script-library/docs/sshd.md).

So, for those of you that do no want to read the whole thing, the process would be as simple as this:

1. Connect to the `codespaces` instance using
[`vscode`](https://docs.github.com/en/codespaces/developing-in-codespaces/using-github-codespaces-in-visual-studio-code). We could
also use the [`CLI` `code` command](https://cli.github.com/manual/gh_codespace_code) for that.

    ```bash
    $ gh cs code
    ```

2. Setting the password for the user with

    ```bash
    $ sudo passwd $(whoami)
    ```

## Is there another way to do this?

Besides following the "default" way mentioned in [GitHub
documentation](https://docs.github.com/en/codespaces/developing-in-codespaces/using-github-codespaces-with-github-cli#ssh-into-a-codespace)
that deals with `ssh` keys, which I consider a bit cumbersome, it looks like you could tweak the `devcontainer` ssh installation
via the installation script arguments, but I haven't tried that out yet.

For those of you wanting to explore it, this could help:

- The `sshd` installation script docs: [link](https://github.com/microsoft/vscode-dev-containers/blob/main/script-library/docs/sshd.md#syntax)
- The `sshd` installation script source code: [link](https://github.com/microsoft/vscode-dev-containers/blob/main/script-library/sshd-debian.sh)
- I guess this approach will require you to learn about codespaces secrets management, so here the docs: [link](https://docs.github.com/en/codespaces/managing-your-codespaces/managing-encrypted-secrets-for-your-codespaces)

BTW, `sshd` installation is part of what Microsoft calls `Dev container features`, which at the time of this writing is a feature in preview mode.

You can read more about it [here](https://code.visualstudio.com/docs/remote/containers#_dev-container-features-preview) and [check
out the many features directly supported by Microsoft](https://github.com/microsoft/vscode-dev-containers/tree/main/script-library/docs), or even [try to create your own one](https://github.com/microsoft/dev-container-features-template).
