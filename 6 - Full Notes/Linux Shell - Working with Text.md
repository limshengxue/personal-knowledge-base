2025-06-08 13:19

Tags: [[linux]] [[shell]]

# Linux Shell - Working with Text
- `echo <argument,text that you want to print>`  - print text in terminal
- `cat <arguments>` - concatenate, often also used to print the content of a file
- `less` - view text content in scrollable format (more useful than `cat` for larger file)
- `more` - view text content but only can go forward
- `grep <options> <pattern> <target>` - find a string pattern in file
- `sed` - stream editor, can use to find and replace text
	- `{{command}} | sed 's/apple/mango/g'` - replace "apple" with "mango"
- `sort` - sort text content
- `head` / `tail` - see the first/last lines of the file


# References
[[Become a shell wizard in ~12 mins]]