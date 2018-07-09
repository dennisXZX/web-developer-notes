## Visual Studio Code

#### Add shortcut to focus on integrated terminal

Add the following code to `keybindings.jsom`. (Preference -> Keyboard Shortcuts)

```
{ "key": "ctrl+`",          "command": "workbench.action.focusActiveEditorGroup", "when": "terminalFocus" },
{ "key": "ctrl+`",          "command": "workbench.action.terminal.focus", "when": "!terminalFocus" }
```

#### Add `code .` command to Mac terminal

- press `fn + F1` and search `install` in the input field
- select `Shell Command: Install 'code' command in PATH`
