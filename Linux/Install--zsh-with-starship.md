---
title: Installing ZSH with Starship theme
description: 
published: true
date: 2020-08-27T07:19:01.300Z
tags: 
editor: markdown
---

# Install ZSH with Starship 🚀

Install ZSH at first

```bash
sudo apt update && sudo apt install zsh
```

Install Oh my ZSH

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## Install Starship theme 

Run following script

```bash
curl -fsSL https://starship.rs/install.sh | bash
```

add following at end of `.zshrc` file

```bash
eval "$(starship init zsh)"
```

Logout and login.