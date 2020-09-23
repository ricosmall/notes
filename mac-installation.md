# mac installation

## [iTerm2](https://www.iterm2.com/)

### Installation

download and install.

## [oh-my-zsh](https://ohmyz.sh/)

### Installation

```sh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
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

- [autojump](https://github.com/wting/autojump)

```sh
# install autojump
brew install autojump

# change .zshrc
vim ~/.zshrc

# add autojump to plugins line, like below:
plugins=(git zsh-autosuggestions autojump)
```

## [Homebrew](https://brew.sh/)

### Installation

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## [ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG)

### Installation

download and install.

## [nvm](https://github.com/nvm-sh/nvm)

### Installation

```sh
# install
brew install nvm

# config: add some export script to `.zshrc` or `.bashrc`
```

## Java 

### Installation

```sh
brew cask install java
```

## [Node](https://nodejs.org/en/)

### Installation

```sh
nvm install stable
```

## [Flutter](https://flutterchina.club/)

### Installation

see [guide](https://flutterchina.club/get-started/install/)

## [Nginx](https://www.nginx.com/)

### Installation

```sh
brew install nginx
```

### Utility

The default port is 8080, change config file `/usr/local/etc/nginx/nginx.conf` if you want to change another port.

- Operation for background use

```sh
# start
brew services start nginx

# stop
brew services stop nginx

# restart
brew services restart nginx
```

- Just for using now

```sh
nginx
```

## [Mongodb](https://www.mongodb.com/)

### Installation

```sh
brew install mongodb
```

### Utility

```sh
# start
brew services start mongodb

# stop
brew services stop mongodb

# restart
brew service restart mongodb
```

## [Sourcetree](https://www.sourcetreeapp.com/)

### Installation

```sh
brew cask install sourcetree
```

## [spf13-vim](http://vim.spf13.com/)

### Installation

```sh
curl https://j.mp/spf13-vim3 -L -o - | sh
```

## [Powerline fonts](https://github.com/powerline/fonts)

### Installation

Install one font like below:

```sh
brew tap caskroom/cask-fonts

brew cask install font-meslo-for-powerline
```

## [Itsycal](https://github.com/sfsam/itsycal)

It is a tiny calendar.

### Installation

download [here](https://www.mowglii.com/itsycal/) and install.

## [cloc](https://github.com/AlDanial/cloc) 

Count lines of code. cloc counts blank lines, comment lines, and physical lines of source code in many programming languages.

### Installation

```sh
brew install cloc
```

