## SASS

#### Interpolation `#{}`

```scss
.col-1-of-2 {
  width: calc((100% - #{$gutter-horizontal}) / 2);
  background-color: #28b485;
}
```

#### Compile SASS

Install `node-sass` into your project as dev dependency and then set up a script in package.json.

```
"scripts": {
  "compile:sass": "node-sass ./sass/main.scss ./css/style.css -w"
}
```

#### &

`&` represents the selector at the current point

```scss
// declare a bunch of variables
$color-primary: #f9ed69;
$color-secondary: #f08a5d;

.nav {
  margin: 30px;
  background-color: $color-primary
    
  li {
    display: inline-block

    // & is equal to .nav li
    &:first-child {
      margin: 0;
    }
  }
}
```

#### Mixin

```scss
// declare mixins
@mixin clearfix {
  &::after {
    content: "";
      clear: both;
    display: table;
  }
}

@minxin style-link-text($color) {
  text-decoration: none;
  text-transform: uppercase;
  color: $color;
}

nav {
  margin: 30px;
  
  @include style-link-text($color-text-light)
  @include clearfix; // use a mixin
}
```

#### Build-in function

darken() / lighten()

```scss
// darken()
.btn-main {
  &:link {
    background-color: $color-secondary;
  }
  
  &:hover {
    background-color: darken($color-secondary, 5%);
  }
}
```
