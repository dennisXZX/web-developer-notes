## Bash Scripting

#### Productivity tips

__add alias__

Put the following aliases in `~/.bash_profile` (bash shell) or `~/.zshrc` (zsh shell)

```bash
alias ll="ls -alG"
```

__add executable files to PATH__

Add all scripts in my-scripts folder to `$PATH`

```bash
export PATH="$PATH:~/my-scripts"
```

__project initialisation__

```bash
echo 'Initialising JS project at $(pwd)'
git init
npm init -y # create package.json with all the defaults
mkdir src
touch src/index.js
code .
```

After creating this bash script, you can copy it to the `$PATH`, for example, `cp init-js.sh /usr/local/init-js`, so you can execute it anywhere you want.

__check website status__

```bash
check_status() {
  # curl a URL, -I gets the header info, -L follows redirection, -s indicates fail silently
  # pipe the curl result to head and extract the first line
  # split the result into two parts and take the second one
  local status=$(curl -ILs $1 | head -n 1 | cut -d ' ' -f 2)

  # failure if $status is less than 200 or greater than 299
  if [[ $status -lt 200 ]] || [[ $status -gt 299 ]]; then
     echo "$1 failed with a $status"
     return 1
  else
    echo "$1 succeeded with a $status"
  fi
}

# call the function with the argument passed from command line
check_status $1
```

#### Fundamentals

Bash scripting is basically a bunch of commands you put in a `.sh` file and run them together.

Normally when you create a bash file like `myscript.sh`, you can't run it on your machine due to permission issue. You would need to do `chmod 755 myscript.sh` pr `chmod u+x myscript.sh` in order to allow the owner to write or modify the script and for everyone to execute it.

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
