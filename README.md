# Microdisseny docker compose

Contains multiple configurations to be used with docker compose.

## Add as submodule (recommended)
```sh
# Add submodule
git submodule add -b master git@github.com:Microdisseny/microdisseny-docker-compose.git docker

# Initialize and pull all changes
git submodule update --init --recursive
```

## Using it as submodule
To avoid using `--recurse-submodules` often (almost always), set the config to do it by default:
```sh
git config --global submodule.recurse true
# NOTE: Remove --global to set it up only to theis project
```

Clonning the repo:
```sh
# Clone is the only exception to the submodule.recurse setting so we need to do it manually
git clone --recurse-submodules <url>

# If you forgot the --recurse-submodules option, use
git submodule update --init --recursive
```
