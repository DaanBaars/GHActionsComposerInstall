# Setup Composer Action

A GitHub Action that installs [Composer](https://getcomposer.org/) using the official installer (with SHA-384 checksum verification) and optionally runs an install command.

PHP is installed automatically via `apt` if it isn't already present on the runner, so the action works out of the box on `ubuntu-latest` and on most Debian/Ubuntu-based containers.

## Usage

### Basic

```yaml
- uses: actions/checkout@v4
- uses: daan-baars/composer-setup-action@v1
```

### Custom install command

```yaml
- uses: daan-baars/composer-setup-action@v1
  with:
    install: 'composer install --no-dev --optimize-autoloader'
```

### Install Composer only (skip the install step)

```yaml
- uses: daan-baars/composer-setup-action@v1
  with:
    install: ''
```

### Run in a sub-directory

```yaml
- uses: daan-baars/composer-setup-action@v1
  with:
    working-directory: ./backend
    install: 'composer install'
```

## Inputs

| Name                | Required | Default             | Description                                                                  |
| ------------------- | -------- | ------------------- | ---------------------------------------------------------------------------- |
| `install`           | No       | `composer install`  | Command to run after Composer is installed. Set to an empty string to skip.  |
| `working-directory` | No       | `.`                 | Directory in which the install command is executed.                          |

## How it works

1. Installs `php-cli` via `apt` if PHP isn't already on the runner.
2. Downloads the installer signature from `composer.github.io/installer.sig`.
3. Downloads the installer from `getcomposer.org/installer`.
4. Verifies the SHA-384 checksum matches the official signature.
5. Installs `composer` globally to `/usr/local/bin/composer` (uses `sudo` automatically when not running as root).
6. Runs the configured install command unless it's an empty string.

## Publishing your own copy

1. Push the contents of this directory (`action.yml` + `README.md`) to the root of a new GitHub repo, e.g. `your-org/composer-setup-action`.
2. Tag a release: `git tag v1 && git push --tags`.
3. Reference it from workflows as `your-org/composer-setup-action@v1`.

## License

MIT
