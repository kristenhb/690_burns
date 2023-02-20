# Markdown Documentation for Github

## The purpose of this assignment was to get an introduction to git using GitHub and Markdown

### Steps to complete the assignment

1. Setup new account on Github.com
2. Setup github on Linux
3. Create SSH key on Linux
4. Copy SSH key to Github account
5. Clone 690_burns repo using the command below:

	```
	git clone git@github.com:kristenhb/690_burns.git
	```
6. Verify the repo was cloned by checking the directory in Linux
7. Modify the README.md file in the repo using nano text editor
8. Sync changes in the README file back to the repo using the commands below:

	```
	git add README.md
	git commit -m "Updated README File"
	git push origin main
	```
9. Create new file containing notes for the current assignment
10. Add the new file to the 690_burns repo using the command below:

	```
	git add Markdown_Documentation_For_Github.md
	git commit -m "Adding Markdown Documentation for Gihub Assignment Notes"
	git push origin main
	```

Note:
Found nice Markdown Cheatsheet site to use for future notes. See the link below:
[Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet) 
