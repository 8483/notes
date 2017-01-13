# Git

Master branch = Timeline

HEAD = Last commit on current branch.

Commit messages should be in the present tense instead of past. They should tell what a commit does instead of what happened. Ex. Add new module vs Added new module.  

Git uses vi as the default editor.  

## Help

List all the commands.  
`git help`

Explain a specific command.  
`git help COMMAND`

## General

Setting up owner of changes.  
`git config --global user.name "NAME"`  
`git config --global user.email "EMAIL"`

Initialize an empty repo inside a .git hidden directory.  
`git init`  

What changed since last commit?  
`git status`

## Staging

Stage an untracked file for committing.  
`git add FILENAME`  

Multiple files.  
`git add FILENAME FILENAME`  

A folder.  
`git add FOLDER/`  

All specific file types.   
`git add *.js`

... in a folder.  
`git add FOLDER/*.js`  

... in whole project.  
`git add "*.js"`  

Track everything.  
`git add --all` or `git add .`

Unstage files.  
`git reset HEAD FILENAME`

## Committing

Each commit moves the HEAD further up the timeline.  

Commit changes.  
`git commit -m "MESSAGE"`

## See Changes

Commit history.  
`git log`

Show unstaged differences since last commit.    
`git diff`

Show differences after staging.  
`git diff --staged`

## Undo

DON'T DO THESE AFTER PUSHING!  

Revert a file to the last commit version.  
`git checkout -- FILENAME`  

UNDO commit and move everything back to staging. The carrot on the HEAD means move to the previous commit.  
`git reset --soft HEAD^`  

DELETE the last 2 commits.    
`git reset --HARD HEAD^^`

Change last commit with overriding message.  
`git commit --amend -m "MESSAGE"`  

## Remote

A project can have multiple remotes ex. origin, test, production...

Add a remote i.e. bookmark a repo i.e. This NAME = this URL. The name is usually "origin", but it can be anything.  
`git remote add NAME URL`  

List all remotes.  
`git remote -v`  

Remove a remote.  
`git remote rm NAME`

## Push  

Define which local branch (usually master) to push to which repository (usually origin). It asks for user and pass.  

`git push -u REPO_NAME BRANCH_NAME`  

`-u` remember the repo and the branch, so that only `git push` can be used.  

## Pull  

Used to update the local repo with the latest changes. Should be done often.  
`git pull`

# Collaboration  

## Clone  

Create a local repository from a remote one.  
`git clone URL`  

... with a different name.  
`git clone URL NEW_NAME`  

Cloning does:  
- Download entire repo into a new local one.  
- Add "origin" remote, pointing to the clone URL.  
- Check out initial branch. (Set head to master)  

## Branch  

Switching branches will only show the files in that branch.  

Create new branch. HEAD still on master (Use checkout to switch).  
`git branch NAME`  

Check which branch we are on.  
`git branch`  

Move to a specific branch (Set HEAD from master to BRANCH_NAME). This is like switching timelines.  
`git checkout BRANCH_NAME`

Delete a branch.  
`git branch -d BRANCH_NAME`  

## Merge  
