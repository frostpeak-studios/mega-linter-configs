# MegaLinter Configs

Shared repo containing various configs for use with [MegaLinter](https://megalinter.io/latest/).

## Table of Contents

- [Getting Started](#getting-started)
- [Flavors](#flavors)
- [Linter Configs](#linter-configs)

## Getting Started

The simplest way to use these configurations is to inherit from them in your `.mega-linter.yml` config by using
the `EXTENDS` property.

```yaml
# Use the base config
# This is language-agnostic and covers only formats such as JSON, YAML, etc.
# as well as repo checks, spelling and other tooling.
EXTENDS: https://raw.githubusercontent.com/frostpeak-studios/mega-linter-configs/refs/heads/main/.mega-linter.yml
```

```yaml
# Use a flavor-based config
# Note: These inherit from the base config, which may cause downstream issues when overriding/appending
# config values. It's recommended to copy flavor configs into your project directly rather than inheriting from
# them to avoid unexpected behavior or configuration.
EXTENDS: https://raw.githubusercontent.com/frostpeak-studios/mega-linter-configs/refs/heads/main/flavors/dotnet.yml
```

You can then overwrite whatever variables you need to suit your project's needs.

The following variables are set to be
appended to, rather than overwritten, when included in an inherited config:

```yaml
# Use to append properties instead of replacing them when using EXTENDS.
CONFIG_PROPERTIES_TO_APPEND:
- ENABLE
- DISABLE_LINTERS
- PLUGINS
- PRE_COMMANDS
- POST_COMMANDS
```

## Flavors

There are different configurations under the `flavors/` directory that are designed to work with different
[flavors](https://megalinter.io/latest/flavors/) of MegaLinter. These enable commonly used linters for these flavors
in addition to the base set of linters.

## Linter Configs

The configuration defines the `LINTER_RULES_PATH` and file names for our custom linter configs, which
exist in the [linter-configs repo](https://github.com/frostpeak-studios/linter-configs). You can override these or
remove them from the configuration to use your own configs.

The most reliable method of including the linter configs in your pipeline is to add the `linter-configs` repo
as a submodule. This allows you to easily update the configs or pin to a specific version, if you wish. Alternatively,
you can point `LINTER_RULES_PATH` to the repo and it will download them at runtime into the project's root, however
this method does not support downloading the CSpell `dictionaries` or `valestyles` subdirectories, which causes both of
these linters to fail.

```sh
git submodule add https://github.com/frostpeak-studios/linter-configs .config/linters
git submodule init
```

```yaml
# .mega-linter.yml
LINTER_RULES_PATH: .config/linters/resources
```
