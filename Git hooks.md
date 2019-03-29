## Git Hooks

#### Pre-commit

We can use `pre-commit` git hook to beautify our code as well as run the tests.

- use `prettier` to lint the code
- use `husky` to create a git hook
- use `lint-staged` to automatically add the linted files to staging area

```
"scripts": {
    "test": "jest",
    "pretty": "prettier --write --tab-width 4 \"src/**/*.js\"",
    "precommit": "lint-staged && npm test"
},
"lint-staged": {
    "*.js": [
        "npm run pretty",
        "git add"
    ]
}
```
