## ds-generic-react repo notes

### How theming works

- We define SCSS variables as the base of our theme in `styles/themes/_variables.scss`. We add `!default` to SCSS variables so that we are allowing the value to be set anywhere higher up the load order and if it does not get set then we apply the default value.

- We define different themes in `styles/themes/themeName`. We define CSS variables in these themes and use SCSS variables as their values.

```scss
// Import SCSS variables so we can use them as values in the CSS variables below
@import "../../variables";

/* CSS VARIABLES */
:root {
  // --color--primary--main is the CSS variable we define
  // #{$color--primary--main} is the syntax of how we use a SCSS variable as the value of the CSS variable
  --color--primary--main: #{$color--primary--main};
}

/ *CSS IN JS */
:export {
  // We also export CSS variable in a way that JavaScript can use
  colorPrimaryMain: $color--primary--main;
}
```

- In the project that use design system, we import a theme and then use the CSS variables defined in the theme.

```scss
// Alias have been set up in craco.config.js

// Import style from design system
@import "ds-generic-react-styles";
// Import a design system theme
@import "ds-generic-react-theme-light";

// Overwrite Primary color
$color--primary--main: #3dd459;

.text-xl {
  // use a CSS variable defined in the theme
  padding: var(--padding-button-xl-text);
  font-size: 16px;
}
```

### How styleguidist works

```jsx
import { makeStyles } from '@material-ui/core/styles';

// makeStyles() returns a hook, normally we call it `useStyles`, which is used to get the CSS classes we define
// We use function signature here because we want to get access to the default 'theme' provided by Material-UI
// https://material-ui.com/styles/api/#makestyles-styles-options-hook
// https://material-ui.com/customization/theming/
const useStyles = makeStyles((theme) => ({
  root: {
    '& > *': {
      margin: theme.spacing(1),
    },
  },
}));

Note the above `'& > *'` is a SASS selector. The `&` always refers to the parent selector when nesting. `>` is the child selector. So `& > *` means to select all the elements of the parent selector.

// Call the userStyles() hook to get CSS classes we defined in the style object passed to makeStyles().
const classes = useStyles();

// Use the CSS class in our code
<div className={classes.root}>
  ...code
</div>
```
