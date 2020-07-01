## macOS

#### Search previous command in bash

`Ctl + R` searches previous CLI commands

#### Useful shortcuts

In Finder, `Command + Shift + G` opens `go to the folder` function

`Command + ,` opens the application’s preferences

`Command + l` gets focus on browser address bar

`Command + ~` switches between different windows for the same app

`Command + Shift + .` toggles hidden files

`Control + Command + Spacebar` brings up the emoji panel

`Command + M` minimises the app window

`Command + Shift + T` opens your last closed tab

`Command + Shift + [` navigates between tabs in a browser

`Command + ctrl + shift + 3` takes a screenshot for the whole screen, `4` takes screenshot for partial screen

`Command + shift + 4` takes a screenshot then edit it

#### Environment variables

`printenv` to get a list of environment variables

`export PORT = 5000` to set an environment variable, which now can be accessed using `process.env.PORT`

`export EDITOR=vim` to set your editor to Vim

#### Hide a folder or file

In Terminal, type `chflags hidden fileName / folderName`, to unhide them, you can use `chflags nohidden` followed by the file or folder path. Alternatively, you can use `mv fileName .fileName`. Because by default, folders with period at the beginning of their names are hidden in OS X.

#### Add alias to terminal

- Create a `.bash_profile` in user profile folder if it doesn't already exist
- Place your alias in the file

```
alias g="cd ~/Github/"
alias p="cd ~/Practices"
alias ..="cd .."
alias ...="cd ../../"
```

- Reload the bash file using `source .bash_profile`
