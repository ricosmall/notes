# Linux Skills

## Install Latest Version of Git

### Installation

```bash
# Install basic tools
sudo yum groupinstall "Development Tools"
sudo yum install gettext-devel openssl-devel perl-CPAN perl-devel zlib-devel

# Download source code of latest version
wget https://github.com/git/git/archive/v2.28.0.tar.gz -O git.tar.gz

# Unzip package
tar -zxf git.tar.gz

# Make configure
cd git-2.28.0
make configure
./configure --prefix=/usr/local

# Make install
sudo make install

# Check git version
git --version

# If version is wrong, make a soft link
ln -s /usr/local/bin/git /usr/bin/git
```

### References

- [How To Install Git on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-centos-7)
