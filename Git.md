## Git

#### Fundamentals

At its core, Git is like a key value store. The key is a SHA1 (40-digit hexadecimal number), the value is the compressed data called `blob`, which represents a single file.  For directory, it is stored in a `tree`. In Git, the same content would only be stored once, which means two identical `blobs` would have exactly the same SHA1 key. All the data about our repository is stored in the hidden `.git` directory.

```
// ask Git for the SHA1 of the contents
echo 'Hello' | git hash-object --stdin

// generate SHA1 of the contents with metadata
// blob indicates it's a blob, 14 is the file size, \0 is the delimiter
echo 'blob 14\0Hello' | openssl sha1
```

In Git, a commit is a code snapshot. Commits point to parent commits and trees.

```
// look at the type of the object using a plumbing command
git cat-file -t 980a0

// look at the content of the object using a plumbing command
git cat-file -p 980a0
```

#### Gitflow

GitFlow is a branching model that standardizes branching and merging policy. It uses two branches to record the history of the project. The `master` branch stores the offcial release history, and the `develop` branch serves as an integration branch for features. We can also tag all commits in the `master` branch with a version number.

How the Gitflow works:

- First we need to create `develop` branch

```
git flow init
```

- Each new feature should reside in its own branch and `feature` branch should use `develop` as their parent branch. When a feature is complete, it gets merged back into `develop`

```
// create a feature branch based off develop branch
git flow feature start feature_branch

/* work on the feture branch */

// merge the feature branch into develop branch
// if you do not have permission to merge into develop branch, you can use the 'pull request' feature to achieve that
git flow feature finish --squash feature_branch
```

- When you have finished all the features for a release cycle, you fork a `release` branch off of `develop`. At this point, no new features can be added to the `release` branch anymore, only bug fixes can go to the `release` branch. Once it's ready to ship, the `release` branch get merged into `master` with a version number. In addition, it should be merged back into `develop`, which may have progressed since the release was initiated. Finally, the release branch will be deleted.

```
// create a release branch
git flow release start 0.1.0

/* added bug fixed to the release branch */

// merge the release branch into master branch
git checkout master
git checkout merge release/0.1.0
// merge the release branch into develop branch
git flow release finish '0.1.0'
```

- `hotfix` branch is based on `master` branch in order to quicly patch production releases.

```
// create a hotfix branch
git flow hotfix start hotfix_branch

/* work on the hotfix branch */

// merge the hotfix branch into master and develop branches
git flow hotfix finish hotfix_branch
```

#### git status

Navigate into a subfolder and use `git status .` to get status of just the subfolder.

`git status --short` to display compact status.

#### git grep

`git grep serverRender` to print lines matching a pattern, contain the word 'serverRender' in this case.

#### git revert 

`git revert HEAD` to revert the last commit and add a new commit to undo its changes.

#### git tag

`git tag v1.0.0` to add a reference to the current commits.

`git tag -a v1.0.0` to add annotation to a tag.

`git tag -n5` to list all tags along with the first 5 lines of annotations for each tag.

`git push --tags` to push tags to remote repository.

Using a tag name like v1.0.0 is called semantic versioning. The first number represents a major release which would introduce breaking changes. The second number means a minor release while the last one is for patch release.

#### git blame

`git blame fileName` to see who made the last change to each line in the file.

#### git diff

`git diff` to compare the difference between the current working directory and the last commit.

`git diff --stat` to get an condensed view of all the difference.

`git diff --cached` to see the difference between working directory and staging area.

`git diff HEAD` to see the difference between working directory + staging area and the last commit.

`git diff origin/master getRandomNumber.js` to see the difference of a specific file between two branches.

#### git log

We can use `/` to initiate a search when viewing git logs (in ternimal pager less), and use `n` to find the next match and `N` to jump to the previous one.

`git log --oneline` to condense the log messages into one line.

`git log -p` to see all the detailed changes made to each commit.

`git log --stat` to show number of insertion and deletion of each commit.

`git log -3` to show the 3 most recent commits.

`git log --author="dennis"` to list commits by a specific author.

`git log --grep='fixed'` to list commits with log message that matches the specified pattern, contains the word 'fixed' in this case.

`git log -i -p -S"math"` to list all the commits that involve the word "math", ignoring case by using `-i` flag.

`git log script.js` to list all commits that involve the script.js file.

We can use multiple arguments such as `git log --stat --oneline`.

#### git stash

`git stash` can temporarily store your changes to revert back to HEAD commit.

`git stash list` can list all your stashes.

`git stash apply` to restore stashed temporary changes to working directory.

`git stash drop stashName` to delete a stash.

`git stash clear` to remove all stash entries.

#### git config

`git config --global user.name 'Dennis'` to set up git username.

`git config --global user.email 'dennis@gmail.com'` to set up git email.

`git config --global core.editor vim` to set up code editor for resolving conflicts.

`git config --global alias.graph 'log --graph --oneline'` to set up an alias git command.

`git config --list` to examine the content of `.gitconfig` file.

#### git merge

`git merge branchName` to merge a branch into the current branch (the one with *).

#### git cherry-pick

We can choose a commit from one branch and apply it onto another by using `git cherry-pick`.

Make sure you are on the branch you want to apply the commit to, then run `git cherry-pick <commit-hash>`.

#### git add

`git add .` and `git add -A` does the same thing since git version 2.x.

#### git pull

Running `git pull` is equal to run `git fetch` and `git merge branchName`.

#### git branch

`git branch newBranchName` to create a new branch.

`git branch -a` to see both local and remote branches.

`git branch -d branchName` to delete a branch.

#### git checkout

`git checkout branchName` to switch to a branch.

`git checkout -b branchName` to create and switch to the newly created branch.

`git checkout -b branchName SHA` to create a branch based on a commit.

#### .gitignore

We can create a global `.gitignore` file for all projects.

```
cd ~
touch .gitignore_global
vi .gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```
