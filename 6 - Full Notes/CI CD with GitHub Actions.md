2025-06-08 19:15

Tags:  [[github actions]] [[ci cd]]

# CI CD with GitHub Actions
## GitHub Actions
- Fully integrated with GitHub
- Respond to GitHub event (push, merge, new issue ...)
- Community-powered workflows (obtain automation from the community)
- Any platform, any language, any cloud
- Easy to write and share (YAML)

### Useful Features
- Matrix execution (run the workflow with multiple variables)
- Logging
- Built-in secret store


## CI Workflow Sample
![[Attachments/Pasted image 20250520110905.png]]
The suggested actions for CI
- checkout (download the content of the repo to the runner machine)
- super-linter (to validate the code)

## CD Workflow Sample
![[Attachments/Pasted image 20250520111413.png]]



# References
[[Introduction to GitHub Actions]]
[[GitHub Actions CI CD]]
