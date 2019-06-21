## Vim (VI Improved)

#### Command mode

- `u` to undo the last change
- `ctrl + r` redo the last change 

- `ctrl + f` move forward a page, `ctrl + d` to move forward half a page
- `ctrl + b` move backward a page, `ctrl + u` to move backward half a page

- `j` move down the file
- `k` move up the file
- `h` move to the left character by character
- `l` move to the right character by character
- `w` move to the right word by word
- `b` move to the left word by word
- `^` move to the beginning of the line
- `$` move to the end of the line

- `x` delete a character
- `dw` delete a word
- `dd` delete one line
- `D` delete from the current position

- `r` replace one character
- `cw` change a word
- `cc` change an entire line

- `~` toggle uppercase / lowercase

- `yy` copy a line
- `p` paste a line

__delete all lines__

- `gg` to move the cursor to the first line of the file, then `dG` to delete all the lines

#### Search

- `/textToSearch` search in the file
- `/\c` search case sensitive, `/\C` to search case insensitive
- `n` move to the next search result (forward)
- `shift + n` move to the next search result (backward)

#### Insert mode

- `i` insert at the cursor position
- `I` insert at the beginning of the line
- `a` append after the cursor position
- `A` append at the end of the line

#### Last-line mode

- `:` get into last-line mode
- `:set nu` display line number
- `:10` go to line 10

#### Visual mode

- `v` get into visual mode
- `shift + v` get into visual line mode

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
