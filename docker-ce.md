# Docker Community Edition (CE)

## CLI

### Dependencies

- [hadolint](/hadolint.md)

### Installation

#### Homebrew

```sh
brew install docker
```

#### Linux

```sh
curl https://download.docker.com/linux/static/stable/x86_64/docker-19.03.1.tgz | tar -xzC /usr/local/bin --strip-components 1 docker/docker
```

#### APT

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo apt-key add - && sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

```sh
sudo apt update
sudo apt -y install docker-ce-cli
```

#### YUM

```sh
yum check-update
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum -y install docker-ce-cli
```

#### Zypper

```sh
sudo zypper refresh
sudo zypper install -y docker-ce-cli
```

#### Chocolatey

```sh
choco install -y docker-cli
```

### Commands

```sh
docker --help
```

### Tips

#### Alter Running Container

```sh
docker container stop [name]

docker commit [name] [name]-alt

docker container rm [name]

docker run -d \
  [...]
```

#### Visual Studio Code

```sh
code --install-extension ms-azuretools.vscode-docker
```

```sh
# Darwin
osascript -e 'quit app "Visual Studio Code"'

code --disable-extension ms-azuretools.vscode-docker
```

#### Command-line completion

```sh
# Using Oh My Zsh
sed -ri 's/^plugins=\((.*)\)/plugins=\(\1 docker\)/g' ~/.zshrc

rm ~/.zcompdump*

source ~/.zshrc
```

### Issues

#### Endpoint Exists

```log
docker: Error response from daemon: endpoint with name [container] already exists in network [network].
```

```sh
docker network disconnect -f [network] [container]
```

#### Bad Credential

```log
Error response from daemon: Get https://127.0.0.1:5000/v2/: unauthorized: BAD_CREDENTIAL
```

TODO

#### Device or resource busy

TODO

<!--
http://blog.jonathanargentiero.com/docker-sed-cannot-rename-etcsedl8ysxl-device-or-resource-busy/
-->

## Daemon

### Installation

#### Homebrew

```sh
brew cask install docker
```

#### APT

##### Xenial Xerus 16.04 and newer

```sh
curl -fsSL 'https://download.docker.com/linux/ubuntu/gpg' | \
  sudo apt-key add - && sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

```sh
sudo apt update
sudo apt -y install docker-ce
```

#### YUM

```sh
yum check-update
sudo yum-config-manager --add-repo 'https://download.docker.com/linux/centos/docker-ce.repo'
sudo yum -y install docker-ce
```

#### Zypper

```sh
sudo zypper refresh
sudo zypper install -y docker-ce
```

### Service

#### Systemd

```sh
sudo systemctl enable --now docker
```

### Configuration

#### Linux

```sh
sudo groupadd docker
```

```sh
sudo usermod -aG docker "$USER" && \
  sudo su - "$USER"
```

### Uninstall

#### APT

```sh
sudo apt purge -y docker-ce
sudo apt autoremove
```

#### YUM

```sh
sudo yum remove -y docker-ce
```

#### Zypper

```sh
sudo zypper rm docker
```

### Tips

#### Deep Clean

```sh
sudo rm -r /etc/docker
sudo rm -r /var/lib/docker
sudo rm -r /srv/docker
sudo rm -r /etc/sysconfig/docker
sudo rm -r /run/docker
```

```sh
sudo groupdel docker
```
