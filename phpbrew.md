# PHPBrew

## CLI

### References

- [CentOS Repo's](/centos.md#repositories)

### Dependencies

- [GD Library](/gd.md)
- [OpenLDAP](/openldap.md)

#### Homebrew

```sh
brew install \
  wget \
  libxml2 \
  bzip2 \
  # libzip
  # mhash \
  # mcrypt \
  # pcre \
  # openssl@1.1 \

```

<!-- #### YUM

```sh
yum check-update
sudo yum -y install autoconf gcc libxml2-devel openssl-devel bzip2-devel libcurl-devel readline-devel libxslt-devel libzip-devel which lbzip2

# Repo: Webtatic
sudo yum -y install libmcrypt-devel
``` -->

### Installation

#### Binary

```sh
# Darwin
curl -L 'https://github.com/phpbrew/phpbrew/raw/master/phpbrew' -o /usr/local/bin/phpbrew && \
  chmod +x /usr/local/bin/phpbrew

# Linux
sudo curl -L 'https://github.com/phpbrew/phpbrew/raw/master/phpbrew' -o /usr/local/bin/phpbrew && \
  sudo chmod +x /usr/local/bin/phpbrew
```

### Configuration

```sh
phpbrew init

# Homebrew
# phpbrew lookup-prefix homebrew
```

### Environment

For Bash or Zsh, put something like this in your `$HOME/.bashrc` or `$HOME/.zshrc`:

```sh
# PHPBrew
source ~/.phpbrew/bashrc
```

```sh
sudo su - "$USER"
```

### Command

```sh
phpbrew help
```

### Downloader

- php_curl
- php_stream
- wget
- curl

More [downloader options](https://github.com/phpbrew/phpbrew/tree/master/src/PhpBrew/Downloader).

> Make sure the command is installed on the machine (`curl`, `wget`, etc.).

### Usage

```sh
# Versions List
phpbrew known \
  -u \
  --downloader=wget

# Variants
phpbrew variants

# Install
VERSION=[version] && \
  phpbrew -d install \
    --name="$VERSION-fpm-dev" \
    --downloader=wget \
    --stdout \
    "$VERSION" \
    +default +fpm +mysql

# Installed Versions
phpbrew list

# Switch Global
phpbrew switch [version]-dev

## Instance Only
phpbrew use [version]-dev

# Extensions
phpbrew extension

## Options
phpbrew -d ext \
  --show-options \
  --show-path

## Commands
phpbrew -d ext install [extension]
phpbrew -d ext show [extension]
phpbrew -d ext enable [extension]
phpbrew -d ext disable [extension]
phpbrew -d ext clean --purge [extension]

# Like
phpbrew install [version] as [version]-dev like [version]-dev

# Clean
phpbrew clean -a [version]-dev

# Purge
phpbrew switch-off
phpbrew purge [version]-dev
```

### Tips

#### Proxy

```sh
phpbrew -d ext install \
  --downloader=wget \
  --http-proxy="$http_proxy" \
  --http-proxy-auth="$http_proxy_auth" \
  [extension]
```

### Issues

####

```log
Warning: file_get_contents(http://pecl.php.net/channel.xml): failed to open stream: HTTP request failed! in phar:///usr/local/bin/phpbrew/vendor/corneltek/pearx/src/PEARX/Core.php on line 35
PHP Warning:  file_get_contents(): SSL: Connection reset by peer in phar:///usr/local/bin/phpbrew/vendor/corneltek/pearx/src/PEARX/Core.php on line 35
```

```sh
git clone https://github.com/phpbrew/PEARX
( cd PEARX && onion bundle && sudo pear install -f ./package.xml )
```

<!-- ```sh
# GNU Wget
sudo wget -P /etc/ssl/certs http://curl.haxx.se/ca/cacert.pem

#
sudo sed -i 's|;\(openssl\.cafile=\)|\1/etc/ssl/certs/cacert.pem|g' "$(php -i | grep -oE /.+/php.ini)"
sudo sed -i 's|;\(openssl\.capath=\)|\1/etc/ssl/certs|g' "$(php -i | grep -oE /.+/php.ini)"

#
php -m | grep openssl

php -i | grep '^openssl$' -A 9
``` -->

<!-- ####

```log
PHP Fatal error:  Uncaught PharException: bz2 extension is required for bzip2 compressed .phar file "/usr/local/bin/phpbrew" in /usr/local/bin/phpbrew:10
```

TODO -->

#### BZip2

```log
checking for BZip2 support... yes
checking for BZip2 in default path... not found
configure: error: Please reinstall the BZip2 distribution
Error: Configure failed: configure: error: Please reinstall the BZip2 distribution
```

```sh
phpbrew -d install \
  [...] \
  # Homebrew
  +bz2="$(brew --prefix bzip2)"
```

#### zlib

```log
checking for the location of zlib... configure: error: zip support requires ZLIB. Use --with-zlib-dir=<DIR> to specify prefix where ZLIB include and library are located
Error: Configure failed: checking for the location of zlib... configure: error: zip support requires ZLIB. Use --with-zlib-dir=<DIR> to specify prefix where ZLIB include and library are located
```

```sh
#
brew install zlib

#
phpbrew -d install \
  [...] \
  # Homebrew
  +zlib="$(brew --prefix zlib)"
```

#### OpenSSL

```log
make: *** [ext/openssl/openssl.lo] Error 1
Error: Make failed: make: *** [ext/openssl/openssl.lo] Error 1
```

```sh
# <= 5.5.x
phpbrew -d install \
  [...] \
  # Homebrew
  +openssl='/usr/local/opt/openssl'

# >= 5.6.x
phpbrew -d install \
  [...] \
  # Homebrew
  +openssl="$(brew --prefix openssl)"
  # Linux
  +openssl=shared

# export PKG_CONFIG_PATH="$(brew --cellar openssl)/$(brew info --json openssl | jq -r '.[0].installed[0].version')/lib/pkgconfig:$PKG_CONFIG_PATH"

# export PKG_CONFIG_PATH="/usr/local/Cellar/openssl/1.0.2t/lib/pkgconfig:$PKG_CONFIG_PATH"
```

#### GNOME XML library

```log
checking for libxml-2.0 >= 2.7.6... no
configure: error: in `/Users/brunowego/.phpbrew/build/7.4.3-fpm-dev':
configure: error: The pkg-config script could not be found or is too old.  Make sure it
is in your PATH or set the PKG_CONFIG environment variable to the full
path to pkg-config.
```

```sh
export PKG_CONFIG_PATH="$(brew --cellar libxml2)/$(brew info --json libxml2 | jq -r '.[0].installed[0].version')/lib/pkgconfig:$PKG_CONFIG_PATH"

export LDFLAGS="-L/usr/local/opt/libxml2/lib:$LDFLAGS"
```

#### cURL

```log
checking for cURL in default path... not found
configure: error: Please reinstall the libcurl distribution -
    easy.h should be in <curl-dir>/include/curl/
Error: Configure failed:     easy.h should be in <curl-dir>/include/curl/
```

```sh
phpbrew -d install \
  [...] \
  # Homebrew
  +curl="$(brew --prefix curl)"
```
