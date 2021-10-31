> Git Cheatsheet
* Push specific comment [to origin/master]
```
git push origin commitId:master
```
* Open UI to check commits status
```
gitk
```
* Uncommit last one commit [It will not be revert but uncommit]
```
git reset --soft HEAD~1
```
* Undo git reset --soft HEAD~1
``` 
git reset 'HEAD@{1}'
```
* git checkout -b newBranchName master/branchName
    (will create newBranchName in locally from master/branchName)
* Fetch all metadata from all remotes
```
git fetch --all
```
* Reset current branch on top of provided branch (branchName)
```
git reset --hard branchName
```
* Rebase from specific remote/branch
``` 
git rebase upstream/branch
```
* Check diff from all javascript files (will load all js files and compare one by one)
```
git difftool *.js
```
* Push commits to specific remote branch (force push to remote origin/branch)
```
git push -f origin branch
```
    
* Add Remote
```
git remote add upstream master-url
```
* Disable push of upstream
```
git remote set-url --push upstream DISABLE
```
* Git featch and rebase togather
```
git pull --rebase
```
* Cheery pick specific commit [it will pick the commit and put on top current branch]
```
git cheery-pick commitId
```
* if there are some conflicts we need to resolve those either meld/intellij idea
```
git cheery-pick --continue
```
it will finish the cherry-pick process now are go to go for push changes and raise PR
