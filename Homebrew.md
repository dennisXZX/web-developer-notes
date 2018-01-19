## Homebrew

#### update local packages (formulae)

- `brew update` to update Homebrew itself
- `brew outdated` to find out the outdated packages
- `brew upgrade` to upgrade all packages

#### Clean up uninstalled packages

```
// clean up all old packages
brew cleanup

// see what would be cleaned up
brew cleanup -n
```

#### See where the stuff get downloaded

```
brew --cache
```
