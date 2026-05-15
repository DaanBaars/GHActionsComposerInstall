# Setup Composer Action

A GitHub Action that installs [Composer](https://getcomposer.org/) using the official installer (with SHA-384 checksum verification) and runs `composer install`.

PHP is installed automatically via `apt` if it isn't already present on the runner, so the action works out of the box on `ubuntu-latest` and on most Debian/Ubuntu-based containers.

## Usage

### Basic

```yaml
- uses: DaanBaars/GHActionsComposerInstall@v1
```

This runs `composer install` in the repository root.

### With composer install options

```yaml
- uses: DaanBaars/GHActionsComposerInstall@v1
  with:
    composer-options: '--no-dev --optimize-autoloader'
```

Resulting command: `composer install --no-dev --optimize-autoloader`.

### Run in a sub-directory

```yaml
- uses: DaanBaars/GHActionsComposerInstall@v1
  with:
    working-directory: ./backend
    composer-options: '--no-progress --prefer-dist'
```

### Show the installed Composer version

```yaml
- uses: DaanBaars/GHActionsComposerInstall@v1
  with:
    expose-composer-version: 'true'
```

Adds a dedicated `Show Composer version` step to the workflow log.

## Inputs

| Name                      | Required | Default | Description                                                                |
| ------------------------- | -------- | ------- | -------------------------------------------------------------------------- |
| `composer-options`        | No       | `''`    | Extra options appended to `composer install` (e.g. `--no-dev`).            |
| `working-directory`       | No       | `.`     | Directory in which `composer install` is executed.                         |
| `expose-composer-version` | No       | `false` | When `'true'`, runs `composer --version` as a dedicated step at the end.   |

## How it works

1. Installs `php-cli` via `apt` if PHP isn't already on the runner.
2. Downloads the installer signature from `composer.github.io/installer.sig`.
3. Downloads the installer from `getcomposer.org/installer`.
4. Verifies the SHA-384 checksum matches the official signature.
5. Installs `composer` globally to `/usr/local/bin/composer` (uses `sudo` automatically when not running as root).
6. Runs `composer install` with the provided `composer-options` in `working-directory`.

## License

MIT