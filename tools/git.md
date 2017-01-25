# Git

Master branch = Timeline

HEAD = Last commit on current branch.

Commit messages should be in the present tense instead of past. They should tell what a commit does instead of what happened. Ex. Add new module vs Added new module.  

Git uses vi as the default editor.  

## Git Ignore

This is a list of files/folders to be ignored in commits. To use it, create a `.gitignore` file in the working directory, **NOT** inside `.git`.  

Each line in the file is a new ignore rule. Ex. To ignore `node_modules`, just add that as one line.  

Also, pattern matching can be used to ignore specific files in specific places. Ex. `*.txt` ignores all text files, while `routes/*.js` ignores all javascript files in that folder.  

Finally, use `#` to comment.

## Workflow  

it's generally considered good practice to avoid merges where possible.

A good workflow is to leave the `master` for production and do all the coding in a separate `develop` branch. The `develop` branch can expand into `feature` branches which would later be merged back. When a certain stable version is reached in `develop`, it can be merged with `master` along with a version tag.  

Additonal brances such as `release` and `hotfix` can be introduced between the `master` and `develop` ones.  

If a branch is not shown, it's due to fast-forwarding i.e. no changes were made in the branch being merged into, hence the simpler merge log.  

## Example Project  

```
*   e67ccc4 (HEAD -> master) Merge branch 'header'
|\  
| * ce37b16 (header) Fix original header
* |   707692c Merge branch 'footer'
|\ \  
| * | 2dc6e90 (footer) Expand footer
| * | fd61a63 Create footer.html
* | | 8083e87 Add fifth bla in CAPS
* | | 8d56117 Add fourth bla in index.html
* | | 86c50f0 Added third bla in index.html
|/ /  
| | * 26326bb (header2) Fix header2 version
| | * d83bebc Modify header into version 2
| |/  
| * c2d4091 Create header.html
|/  
* 9524ae9 Create index.html

```

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

## Tagging  

A reference to a specific commit, used mostly for release versioning.  

Create tag.   
`git tag -a v0.0.1 -m "Version 0.0.1"`

List tags.  
`git tag`  

Open a specific version.  
`git checkout v0.0.1`  

Push tags to remote repo.  
`git push --tags`

## See Changes

Commit history.  
`git log`

Show unstaged differences since last commit.    
`git diff`

Show differences after staging.  
`git diff --staged`

### Tree

A visual tree with branch names included.  
`git log --oneline --decorate --all --graph`  

Add the `tree` alias as a shortcut.  
`git config --global alias.tree "log --oneline --decorate --all --graph"`  

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

Check a remote.  
`git remote show origin`

Remove a remote.  
`git remote rm NAME`

## Push  

Define which local branch (usually master) to push to which repository (usually origin). It asks for user and pass.  

`git push -u REPO_NAME BRANCH_NAME`  

`-u` remember the repo and the branch, so that only `git push` can be used.  

## Pull  

Used to update the local repo with the latest changes. Should be done often.  
`git pull`  

Behind the scenes, this creates an origin/master branch which is automatically merged into the master one, unless there is a merge conflict.  

### Pull Requests

Once someone completes a feature, they don’t immediately merge it into master. Instead, they push the feature branch to the central server and file a pull request asking to merge their additions into master.  

This gives other developers an opportunity to review the changes before they become a part of the main codebase.  

You can think of pull requests as a discussion dedicated to a particular branch.  

For example, if a developer needs help with a particular feature, all they have to do is file a pull request. Interested parties will be notified automatically, and they’ll be able to see the question right next to the relevant commits.

Once a pull request is accepted, the actual act of publishing a feature is much the same as in the Centralized Workflow. First, you need to make sure your local master is synchronized with the upstream master. Then, you merge the feature branch into master and push the updated master back to the central repository.

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

Check remote branches.  
`git branch -r`

Move to a specific branch (Set HEAD from master to BRANCH_NAME). This is like switching timelines.  
`git checkout BRANCH_NAME`  

Create AND move to a branch.  
`git checkout -b BRANCH_NAME`  

Create a remote branch. Usually origin.  
`git push REPO_NAME BRANCH_NAME`

Delete a branch.  
`git branch -d BRANCH_NAME`  

## Merge  

Move (checkout) to the branch (master) you want to merge into, and use:  
`git merge BRANCH_NAME`  

Merging is very easy if the master branch is not modified. This is fast-forwarding.  

If both branches were modified, a commit is created to do the merge. (Vi editor opens for the message)  

## Merge Conflicts  



# Example Workflows  

## Work on a new admin feature, fix a bug in master, and merge the new admin feature.  

Create a new branch and add features. Each commit moves the HEAD.  

```
git checkout -b admin  

git add admin/dashboard.html  
git commit -m "Add dashboard"  

git add admin/users.html  
git commit -m "Add user admin"
```

Fix bugs on master. Move to the master branch, check that we are on it and pull the latest changes from the remote. Push changes to remote.  

```
git checkout master  

git branch  

git pull  

git add store.rb  
git commit -m "Fix store bug"  

git add product.rb  
git commit -m "Fix product"  

git push  
```

Move back to the admin feature to finish changes. When done, move to master branch and merge with admin.  

```
git checkout admin  

git checkout master  

git merge admin  
```  

What the timeline (master) looks like. * = commit.  
```
*    - Merge
|\
| *  - Admin feature
* |  - Bug fix
* |  - Bug fix
| *  - Admin feature
|/
*    - Last commit before branch
```
