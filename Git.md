## Git

### git merge

`git merge branchName` to merge a branch into the current branch (the one with *).

### git cherry-pick

We can choose a commit from one branch and apply it onto another by `git cherry-pick`.

Make sure you are on the branch you want to apply the commit to, then run `git cherry-pick <commit-hash>`.

### git add

`git add .` and `git add -A` does the same thing since git version 2.x.

### git pull

Running `git pull` is equal to run `git fetch` and `git merge branchName`.

### git branch

`git branch -a` to see both local and remote branches.

`git branch -b newBranchName` to create a new branch and checkout to it.

`git branch -d branchName` to delete a branch.

### git checkout

`git checkout -` to checkout the previous branch.
