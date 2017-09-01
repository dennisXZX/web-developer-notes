## Git

### git tag

`git tag v1.0.0` to add a reference to the current commits.

`git tag -a v1.0.0` to add annotation to a tag.

`git tag -n5` to list all tags along with the first 5 lines of annotations for each tag.

`git push --tags` to push tags to remote repository.

Using a tag name like v1.0.0 is called semantic versioning. The first number represents a major release which would introduce breaking changes. The second number means a minor release while the last one is for patch release.

### git blame

`git blame fileName` to see who made the last change to each line in the file.

### git diff

`git diff` to compare the difference between the current working directory and the last commit.

`git diff --stat` to get an condensed view of all the difference.

`git diff --cached` to see the difference between working directory and staging area.

`git diff HEAD` to see the difference between working directory + staging area and the last commit.

`git diff origin/master getRandomNumber.js` to see the difference of a specific file between two branches.

### git log

`git log --oneline` to condense the log messages into one line.

`git log -p` to see all the detailed changes made to each commit.

`git log --stat` to show number of insertion and deletion of each commit.

`git log -3` to show the 3 most recent commits.

`git log --author="dennis"` to list commits by a specific author.

`git log --grep='fixed'` to list commits with log message that matches the specified pattern.

`git log -i -p -S"math"` to list all the commits that involve the word "math", ignoring case by using `-i` flag.

`git log script.js` to list all commits that involve the script.js file.

We can use multiple arguments such as `git log --stat --oneline`.

### git config

`git config --global user.name 'Dennis'` to set up git username.

`git config --global user.email 'dennis@gmail.com'` to set up git email.

`git config --global core.editor vim` to set up code editor for resolving conflicts.

`git config --global alias.graph 'log --graph --oneline'` to set up an alias git command.

`git config --list` to examine the content of `.gitconfig` file.

### git merge

`git merge branchName` to merge a branch into the current branch (the one with *).

### git cherry-pick

We can choose a commit from one branch and apply it onto another by using `git cherry-pick`.

Make sure you are on the branch you want to apply the commit to, then run `git cherry-pick <commit-hash>`.

### git add

`git add .` and `git add -A` does the same thing since git version 2.x.

### git pull

Running `git pull` is equal to run `git fetch` and `git merge branchName`.

### git branch

`git branch -a` to see both local and remote branches.

`git branch -b newBranchName` to create a new branch based on the current branch and checkout to it.

`git branch -d branchName` to delete a branch.

### git checkout

`git checkout -` to checkout the previous branch.

### .gitignore

We can create a global `.gitignore` file for all projects.

```
cd ~
touch .gitignore_global
vi .gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```
