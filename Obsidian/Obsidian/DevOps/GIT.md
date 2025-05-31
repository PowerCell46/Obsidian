## Global Information Tracker
Source control system 
- SVN/Централизираност: Историята се съхранява само на сървъра; Ако този сървър приключи, губим всичко)

### `git log -a`: show all commits

### `git restore fileName|.`: Discard Changes VS code
not staged or commited

### `git restore --staged fileName|.`: Discard Changes VS code
only staged not commited

```shell

```

---
### Naming of a branch on a particular task
##### feature/idOfTheTask/DesciptionOfTheTask

---
- <span style="color:rgb(107, 255, 174)">git branch -d</span> will only delete a branch if it's fully merged into its upstream branch or HEAD. If the branch isn't fully merged, Git will refuse to delete it, protecting you from losing commit history.

- <span style="color:rgb(107, 255, 174)">git branch -D</span> forces the deletion of a branch, regardless of its merge status. Use this option with caution, as it can lead to lost commits and potential conflicts.

____
# <span style="color:rgb(107, 255, 174)">Clone a Repo</span>

## 1. Navigate to a (Parent Folder)
## 2. get the HTTPS link of the repo
## 3. git clone htts://github.com/....

__________________________________________________________________________
# <span style="color:rgb(107, 255, 174)">Initialize a Repo</span>
## 1. Navigate to the folder
cd path/to/your/folder
## 2. Initialize the git repository
git init
## 3. Add the remote repository
git remote add origin https://gitlab.com/....git
## 4. Fetch the data from the remote repository
git fetch origin
## 5. Checkout the existing branch from the remote (`main`)
git checkout main
## 6. Add all files to the staging area
git add .
## 7. Commit the changes with a message
git commit -m "Repo init"
# 8. Push the changes 
git push origin main

__________________________________________________________________________
## git status
## git log

## git reset --hard

## git diff

## git branch --all
List all local and remote branches

## git merge {branch-name}
Merge another branch in the active branch

## touch readme.md
create a file

__________________________________________________________________________
# Push Changes
## 0. Navigate to the local repo in the CLI
## 1. git add .
## 2. git commit -m "...message..."
## 3. git push origin main

__________________________________________________________________________
# Pull Changes
## 0. Navigate to the local repo in the CLI
## 1. git fetch - fetch the latest changes
## (1.5). git status
## 2. git pull - pull changes from the remote version of the repo

__________________________________________________________________________
# Create branch
## 1. git branch `branchname`
create new branch
## 2. git checkout `branchname`
switch to the new local branch
## 3. git add .
## 4. git commit -m "message"
## 5. git push --set-upstream origin `branchname`
pushing the local branch to the repository

__________________________________________________________________________
# Push to another branch
## 1. git checkout `branchname`
## 2. git add .
## 3. git commit -m "Message"
## 4. git push origin `branchname` 
### / git push --set-upstream origin ``branchname`
Creating the new branch in the foreign repo

__________________________________________________________________________
# Resolve Conflict

## 1. git status
## 2. git fetch
## 3. git status
## (git pull)
## 4. git add .
## 5. git commit -m "adding the local change"
## 6. git pull
## 7. Fixing the conflict locally (e.g. in notepad)
## 8. git add .
## 9. git commit -m 'fixing the merge conflict'
## 10. git push

__________________________________________________________________________
# Merge Branches

## 1. git merge add-title
## 2. git push 

__________________________________________________________________________
# Delete Branch
## 1. git branch -d add-title
## 2. git push origin -d add-title

__________________________________________________________________________
# Authentication

## git config  (-global) user.name "PowerCell46"
## git config (-global) user.email "email@gmail.com"

__________________________________________________________________________
# Return a File to the last commit

#### git checkout HEAD -- src/test/java/com/latona/latona_survey_back_end/....

#### git pull {fileName}

___
### Add more than one file

##### `git add file1.js file2.js file3.js`

___

# Remove changes from a file

##### `git restore .../.../file.ext`

