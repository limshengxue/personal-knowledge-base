2025-06-08 10:46

# Become a shell wizard in ~12 mins
## Basic
- Shell = terminal = console = command line , they are *almost* the same thing

## Working with Directories
- `ls <argument> <options>` - list files in directory
	- Common options `-latch` 
		- Long list, show all file, sorted by time, in reversed, with human-readable file size
- `cd` - change directory
- `pwd` - print working directory
- `touch` - create a file (if didn't existed), update the timestamp if existed
- `cp <target> <destination>` - copy
- `mv <target> <destination>` - move
- `rm`  - remove a file
	- `rm -r` - recursive, often used to remove directory
- `ln -s` - create a symlink

#### Locating a File
- `find <path_to_search> -name "filename"`
	- real-time searching
	- slower
- `locate filename`
	- very fast in-db search
	- database need to be updated using `sudo updatedb`
- `fd`
	- faster version of `find` 
	- written in rust and use parallelism
- `fzf` - run a fuzzy finder over the current directory tree

## Working with Text
- `echo <argument,text that you want to print>`  - print text in terminal
- `cat <arguments>` - concatenate, often also used to print the content of a file
- `less` - view text content in scrollable format (more useful than `cat` for larger file)
- `more` - view text content but only can go forward
- `grep <options> <pattern> <target>` - find a string pattern in file
- `sed` - stream editor, can use to find and replace text
	- `{{command}} | sed 's/apple/mango/g'` - replace "apple" with "mango"
- `sort` - sort text content
- `head` / `tail` - see the first/last lines of the file

## Setting up Alias
- Can setup in `.bashrc` 
	- Example like `alias d='ls -lt |grep "^d"'
	- Need to source the file using `. ~/.bashrc` to see the changes
	- Source means to execute the file 

## Reading Manual
- use the `--help` option
- use `man`
- use `tldr`

## Pipeline
- use `|` as the pipeline, turn the output of the previous command into the input of the next command
	- E.g. `echo "Hello World" | sed "s/World/Universe/"`
- `xargs` - split the output into chunk, often used to chunk the output of a commands and pass them separately into another command
	- E.g. `ls | xargs du -sh` - view the disk usage of each file

## Subshell
- Using the `$()` to execute a command as a subshell command and inject the result into the current command
- E.g. `echo "Hello $(pwd)"`

## Redirection
- Using `>` to write the output of a command to a file
	- E.g. `ls --help > ls-help.txt`
- Using `>>` append, instead of overwrite

## Interesting Pipeline
- `compgen -c | fzf| xargs man` - fuzzy find and view the manual of command
- `du -ah . | sort -hr | head -n 10`  - find the largest file

# References
