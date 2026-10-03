2025-06-08 13:22

Tags: [[linux]] [[shell]] [[pipeline]]

# Linux Shell - Building More Complex Command
## Pipeline
- use `|` as the pipeline, turn the output of the previous command into the input of the next command
	- E.g. `echo "Hello World" | sed "s/World/Universe/"`
- Useful command:`xargs` - split the output into chunk, often used to chunk the output of a commands and pass them separately into another command
	- E.g. `ls | xargs du -sh` - view the disk usage of each file
- Combining [[Linux Shell - Bash Profile#Setting up Alias]] is powerful way to reduce repetition

### Interesting Pipeline
- `compgen -c | fzf| xargs man` - fuzzy find and view the manual of command
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