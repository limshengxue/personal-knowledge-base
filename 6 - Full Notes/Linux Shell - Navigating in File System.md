2025-06-08 13:17

Tags: [[linux]] [[shell]]

# Linux Shell - Navigating in File System
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

# References
[[Become a shell wizard in ~12 mins]]