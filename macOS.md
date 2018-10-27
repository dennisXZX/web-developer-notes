## macOS

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

#### Environment variables

`printenv` to get a list of environment variables

`export PORT = 5000` to set an environment variable, which now can be accessed using `process.env.PORT`

#### Hide a folder or file

In Terminal, type `chflags hidden fileName / folderName`, to unhide them, you can use `chflags nohidden` followed by the file or folder path. Alternatively, you can use `mv fileName .fileName`. Because by default, folders with period at the beginning of their names are hidden in OS X.
