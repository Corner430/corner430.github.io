---
title: Configure oh-my-zsh
date: 2023-04-03 17:56:08
tags:
    - oh-my-zsh
    - zsh
    - shell
    - bash
---
- Install ZSH<!--more-->
`sudo apt install zsh-autosuggestions zsh-syntax-highlighting zsh`

- Install [Oh my ZSH.](https://github.com/ohmyzsh/ohmyzsh)
`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

- Install plugins.
    - autosuggesions plugin
    `git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions`
    - zsh-syntax-highlighting plugin
    `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting`
    - zsh-fast-syntax-highlighting plugin
    `git clone https://github.com/zdharma-continuum/fast-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/fast-syntax-highlighting`
    - zsh-autocomplete plugin
    `git clone --depth 1 -- https://github.com/marlonrichert/zsh-autocomplete.git $ZSH_CUSTOM/plugins/zsh-autocomplete`

- Enable plugins by adding them to .zshrc.
    - Open .zshrc
    `vim ~/.zshrc`
    - Find the line which says `plugins=(git)`.
    - Replace that line with `plugins=(git zsh-autosuggestions zsh-syntax-highlighting fast-syntax-highlighting zsh-autocomplete)`

- References
[Oh my ZSH](https://github.com/ohmyzsh/ohmyzsh)
[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
[zsh-fast-syntax-highlighting](https://github.com/zdharma/fast-syntax-highlighting)
[zsh-autocomplete](https://github.com/marlonrichert/zsh-autocomplete)
[Oh my zsh.](https://gist.github.com/n1snt/454b879b8f0b7995740ae04c5fb5b7df)