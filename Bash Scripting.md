## Bash Scripting

#### Fundamentals

Bash scripting is basically a bunch of commands you put in a `.sh` file and run them together.

Normally when you create a bash file like `myscript.sh`, you can't run it on your machine due to permission issue. You would need to do `chmod 755 myscript.sh` in order to allow the owner to write or modify the script and for everyone to execute it.

The Shebang `#!/bin/bash` is usually in the first line of a script file which ditates the path to the interpreter that should be used to run the rest of the lines in the text file.

#### Variables

Variables preset by the system

- $1 to represent the first command line argument
- $2 to represent the second command line argument
- $# - How many arguments were passed to the Bash script

Define your own variables

```bash
myvariable=Hello

echo $myvariable

sampledir=/etc

ls $sampledir
```

Use single quote or double quote to group things together.

- Single quotes will treat every character literally
- Double quotes will allow you to do substitution

We can also save the result of an expression into a variable `myvar=$( ls /etc | wc -l )`.

`export var1` to make the variable var1 available to child processes.

#### Input

```bash
echo Hello, who am I talking to?

read varname

echo It\'s nice to meet you $varname
```

```bash
// -p to specify a prompt
read -p 'Username: ' uservar

// -s to make the input silent
read -sp 'Password: ' passvar
```
