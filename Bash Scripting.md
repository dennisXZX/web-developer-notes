## Bash Scripting

#### Fundamentals

Bash scripting is basically a bunch of commands you put in a `.sh` file and run them together.

Normally when you create a bash file like `myscript.sh`, you can't run it on your machine due to permission issue. You would need to do `chmod 775 myscript.sh` in order to allow the owner to write or modify the script and for everyone to execute it.

The Shebang `#!/bin/bash` is usually in the first line of a script file which ditates the path to the interpreter that should be used to run the rest of the lines in the text file.
