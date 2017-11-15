## Vim

#### Normal mode

- `A` to move to the end of a line and switch to editing mode (move to the end of the line using `$`)
- `I` to move to the beginning of a line and switch to insert mode (move to the beginning of the line using `^`)
- `j` to move down the file
- `k` to move up the file
- `h` to move to the left character by character
- `l` to move to the right character by character
- `w` to move to the right word by word
- `b` to move to the left word by word
- `dd` to delete one line

#### Insert mode

- `i` to get into insert mode
- `Esc` to exit insert mode

#### Command mode

- `shift + ;` to get into command mode

#### Visual mode

- `v` to get into visual mode
- `shift + v` to get into visual line mode

#### Save file

- In command mode, use `w fileName` to save a file

#### Copy & Paste

- In visual mode, highlight the text you want to copy. Press `y` to copy the text and then switch to Normal mode and press `p` to paste the text

#### Configure Vim

Create a `~/.vimrc` file in home directory and put your Vim config in it.

```
syntax on
set number
```

