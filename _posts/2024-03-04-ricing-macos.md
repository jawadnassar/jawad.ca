---
layout: post
title: "Ricing MacOS"
date: 2024-03-04
categories: [Lab]
---

## Step 1: Install Homebrew

[Homebrew](https://brew.sh){:target="_blank"} is a package manager for MacOS that simplifies the process of installing software on MacOS.


```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```


## Step 2: Install iTerm2

With Homebrew installed, you can easily install [iTerm2](https://iterm2.com/){:target="_blank"} by running:

```shell
brew install --cask iterm2
```


## Step 3: Download and Apply iTerm2 Color Scheme

1. Choose your desired iTerm2 color scheme from [iTerm2 Color Schemes](https://iterm2colorschemes.com/){:target="_blank"}.
2. Download the theme file and save it using the following extension: `.itermcolors`.
3. Open iTerm2.
4. Go to iTerm > Preferences > Profiles > Colors tab.
5. Click on Color Presets... at the bottom right > Import...
6. Select your downloaded `.itermcolors` file.
7. Once imported, select it from the list of Color Presets.

![](https://jawad.ca/images/ricemacos1.png)

## Step 4: Install Oh My Zsh

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```


## Step 5: Install Syntax Highlighting and Auto-Suggestions Plugins

Syntax Highlighting:


```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

Auto-Suggestions:


```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

After cloning, add `zsh-syntax-highlighting` and `zsh-autosuggestions` to the `plugins` section in `~/.zshrc`.


```sh
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
```


## Step 6: Install and Configure the Powerlevel10k Theme


Clone the Powerlevel10k repository:


```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```


Update the `ZSH_THEME` line in your `~/.zshrc` file to use Powerlevel10k:


```shell
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Apply the changes by sourcing `~/.zshrc`:


```shell
source ~/.zshrc
```


## Step 7: Install a Nerd Font

To fully enjoy Powerlevel10k, you'll need a Nerd Font. Choose and download your preferred Nerd Font from [Nerd Fonts](https://www.nerdfonts.com/font-downloads). Install the font on your system then set it from iTerm2 preferences.



## Step 8: Beautifying the `ls` Command with `colorls` Ruby gem

Install `colorls`:


```shell
sudo gem install colorls
```


To avoid typing `colorls` every time, we'll create an alias in the `.zshrc` file. This will allow us to use `ls` as a substitute command. Open `~/.zshrc` and add:


```shell
alias ls='colorls'
```


**Apply Changes**: For the alias to take effect, source your `.zshrc` file by running:


```shell
source ~/.zshrc
```

Et voilà!

![](https://jawad.ca/images/ricemacos2.png)



