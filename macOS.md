## macOS

#### Show all the hidden files in Finder

Solution 1:

Press `Command + Shift + .` to toggle hidden files.

Solution 2:

Execute the following commands in Terminal to show all the hidden files. If you want to switch it back just change the TRUE to FALSE.

```
defaults write com.apple.finder AppleShowAllFiles TRUE
killall Finder
```
