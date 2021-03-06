# Simple Python Version Management (pyenv)

## CLI

### Dependencies

#### Homebrew

```sh
# macOS Mojave before
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.*.pkg -target /

brew install \
  autoconf \
  pkg-config \
  openssl \
  readline \
  zlib
```

#### Zypper

```sh
sudo zypper refresh
sudo zypper install -y git-core curl tar gzip make gcc-c++ zlib-devel libffi-devel libopenssl-devel libbz2-devel readline-devel sqlite3-devel
```

#### YUM

```sh
yum check-update
sudo yum -y install git curl tar gzip make gcc-c++ zlib-devel libffi-devel openssl-devel bzip2-devel readline-devel sqlite-devel
```

#### APT

```sh
sudo apt update
sudo apt -y install git curl tar gzip make g++ zlib1g-dev libffi-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev
```

### Installation

#### Unix-like

```sh
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

### Environment

For Bash or Zsh, put something like this in your `$HOME/.bashrc` or `$HOME/.zshrc`:

```sh
# pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
```

```sh
sudo su - "$USER"
```

### Configuration

```sh
# Git ignore
echo '.python-version' >> ~/.gitignore_global
```

### Commands

```sh
pyenv help
```

### Bootstrap

```sh
# Install
pyenv install --list

# Installation
pyenv install [version]

# Versions
pyenv versions

# Set
pyenv local [version]
pyenv global [version]

# Initialize
eval "$(pyenv init -)"
```

### Issues

####

```sh
# Darwin
PYTHON_CONFIGURE_OPTS="--with-system-expat" \
  CFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix readline)/include -I$(xcrun --show-sdk-path)/usr/include" \
  LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix readline)/lib" \
  pyenv install [version]
```

### Uninstall

```sh
rm -fR ~/.pyenv
```
