## WebStorm | PhpStorm | IntelliJ

#### Useful default shortcuts

- `control + tab` to switch between opened tabs

- `command + shift + o` to search a file

- `command + shift + f` to search in path

- `command + shift + a` to search all actions (such as split the screen vertically)

- `option + shift + mouse click` to edit multiple lines

- `command + l` to use go to line (tips: go to line 1 will go to the top of the file)

- `option + command + t` to use `Surround With` feature

NOTE: you can create your own shortcuts by changing the `Keymap`.

#### Quick HTML generation

WebStorm or PhpStorm integrates with `Emmet`, which allows you to generate HTML quickly by using short-cut syntax like `.row>a.label*2`, which would generate an HTML structure as follows.

```
<div class="row">
  <a class="label"></a>
  <a class="label"></a>
</div>
```

`#wrapper.container.bg-secondary.text-light` generates 

```html
<div id="wrapper" class="container bg-secondary text-light"></div>
```

`lorem10` generates 10 lorem words

`div.left+div.right+div.center` generates

```html
  <div class="left"></div>
  <div class="right"></div>
  <div class="center"></div>
```

#### Settings for all projects

For settings that should apply to all the projects, goes to `File -> Other Settings -> Preferences for New Projects`.
