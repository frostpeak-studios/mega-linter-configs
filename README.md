# MegaLinter Configs

Shared repo containing various configs for use with [MegaLinter](https://megalinter.io/latest/).

## Table of Contents

- [Getting Started](#getting-started)
- [Flavors](#flavors)
- [Components](#components)
- [Notes](#notes)
    - [Linter Configs](#linter-configs)
    - [Design Decisions](#design-decisions)
        - [No auto fixes](#no-auto-fixes)
        - [Config file naming and formats](#config-file-naming-and-formats)

## Getting Started

The simplest way to use these configurations is to inherit from them in your `.mega-linter.yml` config by using
the `EXTENDS` property.

```yaml
# Use the base config
# This is language-agnostic and covers only formats such as JSON, YAML, etc.
# as well as repo checks, spelling and other tooling.
EXTENDS: https://raw.githubusercontent.com/frostpeak-studios/mega-linter-configs/refs/heads/main/base.yml
```

```yaml
# Use a flavor-based config
EXTENDS: https://raw.githubusercontent.com/frostpeak-studios/mega-linter-configs/refs/heads/main/flavors/dotnet.yml
```

You can then overwrite whatever variables you need to suit your project's needs.

The following variables are set to be
appended to, rather than overwritten, when included in an inherited config:

```yaml
# components/options.yml
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

## Components

The `base` configuration file is composed of different components, each representing a different category or scope of
options and settings. This is done to keep the config modular and clean, rather than having a single, massive file.

This also allows for easily swapping out components based on project needs or linter flavors.

## Notes

### Linter Configs

The `configs.yml` component defines the `LINTER_RULES_PATH` and file names for our custom linter configs, which
exist in the [linter-configs repo](https://github.com/frostpeak-studios/linter-configs). You can override these, or
remove the `configs` component from the configuration hierarchy to use your own configs.

By default, the `mega-linter-runner` will download all of these configs to the root of your project, which may not
be the desired behavior when running locally rather than in a CI/CD container. To avoid this, you can include the
`linter-configs` repo as a submodule in your project and point the `LINTER_RULES_PATH` to it.

```sh
git submodule add https://github.com/frostpeak-studios/linter-configs .config/linters
git submodule init
```

In `components/cli.yml` we assume the linter configs are included as a submodule under `.config/linters` so we point
the linters to look at the `.config/linters/resources/` path for `.ignore` files. Override these arguments or change
the `cli` component to fit your needs.

### Design Decisions

#### No auto fixes

We don't allow the linters to apply fixes automatically, by default. You can enable this in your project with minimal
changes, see the [apply fixes docs](https://megalinter.io/latest/config-apply-fixes/) for more information.

#### Config file naming and formats

If a linter gives the option of providing a config file in JSON or YAML, we choose YAML. This is solely because of the
native comments support, which is missing in JSON. We also prefix the files with `.` for consistency, which some
linters do not expect and would not be able to automatically find the config file.
