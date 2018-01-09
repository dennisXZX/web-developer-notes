## Yarn

#### Daily usage

- `yarn install` to install project dependencies

- `yarn init` to initialize a project

- `yarn add [package-name]` to add a new dependency, add `--dev` flag to indicate dev dependencies

- `yarn add [package]@[version-or-tag]` to add a specific package version

- `yarn upgrade [package-name]` to ugrade a package

- `yarn upgrade-interactive` to interactively upgrade packages (press `space` to select a package and `a` to select all)

- `yarn remove [package-name]` to remove a package

#### Yarn lock file

After every install, upgrade or removal, yarn updates a `yarn.lock` file which keeps track of the exact package version installed in `node_modules` directory.
