# dotfiles repo

## download dot files

``` bash
# change dir to homedir
cd ~

# clone repo
git clone https://github.com/jackykornet/dotfiles.git
```

## Run with Chezmoi for auto configuration
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply https://github.com/jackykornet/dotfiles.git

1. Installs chezmoi
2. Clones the dotfiles directory
3. Applies dot_* files to $HOME
4. Processes .chezmoiexternal.toml
    a. Downloads mise binary → ~/.local/bin/mise
5. Runs run_once_install-packages.sh
6. Runs run_onchange_after_install_packages.sh.tmpl
    a.  mise trust ~/.config/mise/config.toml
         i. mise install (installs tools in config.toml)

## Use in DevPod

Let the devcontainer run the `script/setup` script through the devcontainer.json file

```json devcontainer.json
{
  "build": {
    "context": "..",
    "dockerfile": "Dockerfile"
  },
   "postCreateCommand": "scripts/setup"
}
```

``` bash scripts/setup
#!/bin/bash

$HOME/.local/bin/mise trust $HOME/.config/mise/config.toml && $HOME/.local/bin/mise install
```


devpod up . --ide none --dotfiles git@github.com:you/dotfiles-devpod.git
