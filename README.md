# rofi 1password

Attempt of re-implementing an older version of rofi-1password with more recent `op` features.

Note: I think you probably just want to use `1password --quick-access` instead, `op` even with socket integration will only work with internet connection present.
1Password GUI + `1password --quick-access` also works offline.

## Features

* Storing 1Password session encrypted
* Login/Relogin to session via GUI only / `pinentry-rofi`
* List of all vaults with `Name/<Field>` syntax - which makes it feel more like `password-store`
* Sourcing of the script, to allow `op` authentication sharing session used by rofi
* Clearing and restoring clipboard (bug if you copy/paste too many instances at once it will ultimately restore from another secret)

## Requirements

* `wl-{copy,paste}` only (so swaywm)
* `libsecret`'s `secret-tool` for storing the 1Password session securely
* `pinentry-rofi`
* `libnotify`'s `notify-send`
* `jq`
* `sed`
* `rofi`
* `Bash`
* `1Password` for `notify-send` 1Password Icon.

## Assumptions

* You can't get 1Password socket with PAM to work
* You don't want 1Password daemon to run or depend on 1Password desktop alltogether


## `zshrc` example

You can use this script in your `.zshrc` to call `op` without having to store/login session key in your shell.

```zsh
# () instead of {} in order to avoid leaking global variables within the function
op() (
    # avoid/override op itself
    unset -f op
    # Make sure `rofi-op` is accessible in your path
    source "$(which rofi-op)"
    export_secrets
    op "${@}"
)
```
