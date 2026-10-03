2025-06-08 13:21

Tags: [[linux]] [[shell]]

# Linux Shell - Bash Profile
Bash reads different startup files depending on how the shell starts. Put interactive aliases in `~/.bashrc`, rather than assuming every shell reads `~/.bash_profile`.

## Startup Files
- An interactive non-login Bash shell reads `~/.bashrc`.
- A login shell reads `/etc/profile`, then the first readable file among `~/.bash_profile`, `~/.bash_login`, and `~/.profile`.
- A login profile often explicitly sources `~/.bashrc`; Bash does not automatically read both files.

## Setting up Alias
For a simple reusable shortcut, add this to `~/.bashrc`:

```bash
alias recent='ls -lt'
```

Reload it in the current shell with `source ~/.bashrc` or `. ~/.bashrc`. Sourcing executes the file in the current shell, so review unfamiliar content before doing so.

Use [[Linux Shell - Building More Complex Command]] for pipelines and command substitution. These instructions concern Bash, not PowerShell or every Unix shell.

# References
[[Become a shell wizard in ~12 mins]]
[Bash startup files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)
