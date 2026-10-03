2026-04-12 09:44

# 12 Checkpointing
- Provide snapshot that allow recovery
- Trigger during new prompt
- Save 2 states
	- Code status and conversation status
- Use `/rewind` to revert back to a checkpoint
	- We can select rewind code, conversation, or both

![[Attachments/Pasted image 20260412095105.png]]


## Boundaries of Checkpointing
- Does not capture the effect Bash command 
- Does not capture external edit, if the developer manual edit some file when prompting, it doesn't get tracked



# References
