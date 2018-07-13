## Homebrew

#### Fundamentals

- `brew install --verbose --debug <formula name>` to install a formula with debugging messages
- `brew list` to list all the installed formulae
- `brew search <search term>` will list the possible formulae that match the search term you can install
- `brew help` to list all the possible commands

#### Update local packages (formulae)

- `brew update` to update Homebrew itself
- `brew outdated` to find out the outdated formulae
- `brew upgrade <formula name>` to upgrade a specific formula or `brew upgrade` to upgrade all formulae

#### Clean up uninstalled packages

```
// clean up all old formulae
brew cleanup

// see what would be cleaned up
brew cleanup -n
```

#### Uninstall a formula completely

```
brew uninstall <formula name> --force
```

#### See where the stuff get downloaded

```
brew --cache
```
