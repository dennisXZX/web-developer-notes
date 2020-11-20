## Visual Studio Code

### Shortcuts

- Organise imports - `option + shift + o`
- Open command palette - `shift + cmd + p`
- Toggle terminal - `ctrl + backtick`
- Toogle sidebar - `cmd + b`
- Switch between tabs - `ctrl + tab`
- Get back previously closed tabs - `cmd + shift + t`
- Quick file open - `cmd + p` (use `cmd + enter` to open file on the side)
- Open file on the side from side bar - `ctrl + enter`

- Move line up & down - `option + up/down`
- Copy line up & down - `shift + option + up/down`
- Add block comment - `shift + option + a`
- Add cursor - `option + click`
- Search an entity (function or variable) - `cmd + shift + o`

### Extensions

- One Dark Pro - VSCode theme
- GitLens
- Code Spell Checker
- Debugger for Chrome
- ES7 React/Redux/GraphQL/React-Native snippets
- Auto Rename Tag
- Bracket Pair Colorizer
- Prettier
- Eslint
- Path Intellisense

### Custom configs

#### Make VSCode recognise path alias

Go to `path-intellisense` settings and add the following to `Mappings for paths`

```
 "path-intellisense.mappings": {
    "@": "${workspaceRoot}/src"
  }
 ```
 
 Then reload your VSCode.

#### Enable screencast mode

- select `Toggle Screencast mode`

#### Add `code .` command to Mac terminal

- select `Shell Command: Install 'code' command in PATH`
