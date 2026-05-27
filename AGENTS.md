# AGENTS.md

This project uses the [ddev-drupal-contrib-lite](https://github.com/mandclu/ddev-drupal-contrib-lite) DDEV add-on. All commands below run inside the DDEV web container via `ddev <command>`.

## Target argument

All linting and analysis commands accept an optional path as their **first argument**:

| Invocation | Behavior |
|---|---|
| `ddev <command>` | Runs against all projects in `$DRUPAL_PROJECTS_PATH` (default: `web/modules/custom`) |
| `ddev <command> .` | Runs against the current working directory |
| `ddev <command> web/modules/custom/mymodule` | Runs against the specified path |

Flags and additional arguments always follow the target, e.g. `ddev phpcs web/modules/custom/mymodule -n`.

## PHP commands

Require `ddev composer install` before first use.

- `ddev phpcs [target]` — Check coding standards via PHP_CodeSniffer.
- `ddev phpcbf [target]` — Auto-fix coding standard violations.
- `ddev phpunit [target]` — Run PHPUnit tests. Pass a file or directory as the target, or use `--filter testMethodName` to run a specific test.
- `ddev phpstan [target]` — Run static analysis via PHPStan.

## JavaScript/CSS commands

Require `ddev exec "cd web/core && yarn install"` before first use.

- `ddev eslint [target]` — Lint JavaScript files.
- `ddev stylelint [target]` — Lint CSS files.
- `ddev nightwatch [target]` — Run Nightwatch end-to-end tests. Also requires the [ddev-selenium-standalone-chrome](https://github.com/ddev/ddev-selenium-standalone-chrome) add-on.

## Setup commands

- `ddev poser` — Install Drupal core and development dependencies (runs `composer install` with contrib-appropriate configuration). Run this once after `ddev start` and whenever `DRUPAL_CORE` changes.
- `ddev core-version <constraint>` — Switch the Drupal core version, e.g. `ddev core-version ^10`. Runs `ddev restart` and `ddev poser` automatically.

## Environment variables

| Variable | Default | Purpose |
|---|---|---|
| `DRUPAL_PROJECTS_PATH` | `modules/custom` | Path relative to the webroot where contrib projects live. Override in `.ddev/config.local.yaml`. |
| `DRUPAL_CORE` | `^11` | Drupal core version constraint used by `ddev poser`. Override via `ddev core-version` or `.ddev/.env.web`. |

## Typical workflow

```shell
ddev start
ddev poser
ddev phpcs web/modules/custom/mymodule
ddev phpcbf web/modules/custom/mymodule
ddev phpstan web/modules/custom/mymodule
ddev phpunit web/modules/custom/mymodule
```
