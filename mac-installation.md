# mac installation

## [iTerm2](https://www.iterm2.com/)

### Installation

download and install.

## [oh-my-zsh](https://ohmyz.sh/)

### Installation

```sh
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### Plugins

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

```sh
# clone the package
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions

# change .zshrc
vim ~/.zshrc

# add zsh-autosuggestions to plugins line, like below:
plugins=(git zsh-autosuggestions)
```

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

```sh
# clone the package
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

# add source config to .zshrc
echo "source ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ~/.zshrc

# make effective
source ~/.zshrc
```

## [Homebrew](https://brew.sh/)

### Installation

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## [ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG)

### Installation

download and install.
