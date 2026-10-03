2025-06-08 13:22

Tags: [[linux]] [[shell]] [[pipeline]]

# Linux Shell - Building More Complex Command
## Pipeline
- use `|` as the pipeline, turn the output of the previous command into the input of the next command
	- E.g. `echo "Hello World" | sed "s/World/Universe/"`
- `xargs` converts input into command arguments and may invoke the command in batches.
	- For filenames, avoid parsing `ls`: whitespace and newlines can corrupt arguments. GNU tools support NUL-delimited input, e.g. `find . -maxdepth 1 -type f -print0 | xargs -0 -r du -sh`.
- Combining [[Linux Shell - Bash Profile#Setting up Alias]] is powerful way to reduce repetition

### Interesting Pipeline
- `compgen -c | fzf | xargs -r man` - select a command and open its manual using GNU xargs; requires fzf, and not every command has a man page.
- `du -ah . | sort -hr | head -n 10`  - find the largest file


## Subshell
- Using the `$()` to execute a command as a subshell command and inject the result into the current command
- E.g. `echo "Hello $(pwd)"`

## Redirection
- Using `>` to write the output of a command to a file (will overwrite the file content)
	- E.g. `ls --help > ls-help.txt`
- Using `>>` to append, instead of overwrite


# References
[[Become a shell wizard in ~12 mins]]
[GNU find and safe filename handling](https://www.gnu.org/software/findutils/manual/html_node/Safe-File-Name-Handling.html)
