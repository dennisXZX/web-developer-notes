## WebStorm | PhpStorm

#### Live reload for HTML/CSS

- install `LiveEdit` plugin, then launch the HTML page in debug mode.

#### Set ES6 as default language to all projects

For settings that should apply to all the projects, goes to `File -> Preferences for New Projects`.

#### Useful shortcut

- `command + 1` to toggle sidebar

- `alt + F12` to toggle Terminal

- `command + shift + o` to search a file

- `alt + command + o` to search for a method

- `command + shift + f` to search in path

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
