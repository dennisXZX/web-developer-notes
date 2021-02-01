## ds-generic-react repo notes

### How theming works

#### In design system codebase

- We define SCSS variables as the base of our theme in `styles/themes/_variables.scss`. We add `!default` to SCSS variables so that we are allowing the value to be set anywhere higher up the load order and if it does not get set then we apply the default value.

- We define different themes in `styles/themes/themeName`. We define CSS variables in these themes and use SCSS variables defined at `styles/themes/_variables.scss` as their values.

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

#### In projects that use design system

- We create a `src/assets/styles/variables.scss` in which we
  - Import deign system component style
  - Import design system theme style
  - Define SCSS mixins
  - Overwrite CSS variables exposed from design system to change theme

```scss
// Alias have been set up in craco.config.js

// Import deign system component style
@import "ds-generic-react-styles";
// Import design system theme style
@import "ds-generic-react-theme-light";

// Define some mixins
@mixin content-container {
  ...code
}

// Overwrite CSS variables exposed from design system to change theme
// Define a new SCSS variable
$color--primary--main: #3dd459;

:root {
  // Overwrite the CSS variable exposed from design system
  --color--primary--main: #{$color--primary--main};
}
```

- We create a `src/assets/styles/theme.jsx` to define a custom Material-UI theme

```js
// Import all the variables, which can be used in CSS-in-JS
import variables from './variables.scss';

// Define a custom Material-UI theme
const palette = {
  common: {
    black: variables.colorCommonBlack,
    white: variables.colorCommonWhite,
  },
}

export { palette };
```

- Inject the theme into `<ThemeProvider>` at `src/index.jsx`

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
