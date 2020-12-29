## ds-generic-react repo notes

```jsx
import { makeStyles } from '@material-ui/core/styles';

// makeStyles() returns a hook, normally we call it `useStyles`, which is used to 
// We use function signature here because we want to access to the default 'theme' provided by Material-UI
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
