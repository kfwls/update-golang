[![license](http://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/udhos/update-golang/blob/master/LICENSE)

[![asciicast](https://asciinema.org/a/3ctuczryntzoay8mr64zwnh4h.png)](https://asciinema.org/a/3ctuczryntzoay8mr64zwnh4h)

# update-golang
update-golang is a script to easily fetch and install new Golang releases with minimum system intrusion.

Table of Contents
=================

  * [How it works](#how-it-works)
  * [Usage](#usage)
  * [Caution](#caution)
  * [Example](#example)
  * [Customization](#customization)
  * [Per\-user Install](#per-user-install)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc.go)

How it works
============

This is the default behavior:

1\. The script uses local system OS and ARCH to download the correct binary release. It is not harmful to run the script multiple times. Downloaded releases are kept as cache under '/usr/local'. You can erase them manually.

2\. The release is installed at '/usr/local/go'.

3\. The path '/usr/local/go/bin' is added to PATH using '/etc/profile.d/golang_path.sh'.

4\. Only if needed, GOROOT is properly setup, also using '/etc/profile.d/golang_path.sh'.

You can customize the behavior by setting environment variables (see Customization below).

Usage
=====

    git clone https://github.com/udhos/update-golang
    cd update-golang
    sudo ./update-golang.sh

Caution
=======

Before running the script, make sure you have an untampered copy by verifying the SHA256 checksum.

    $ wget -qO hash.txt https://raw.githubusercontent.com/udhos/update-golang/master/update-golang.sh.sha256
    $ sha256sum -c hash.txt
    update-golang.sh: OK

Example
=======

    $ sudo ./update-golang.sh
    update-golang.sh: version 0.3
    SOURCE=https://storage.googleapis.com/golang
    DESTINATION=/usr/local
    RELEASE=1.8.1
    OS=linux
    ARCH=amd64
    PROFILED=/etc/profile.d/golang_path.sh
    CACHE=/usr/local
    update-golang.sh: will install golang go1.8.1.linux-amd64 as: /usr/local/go
    update-golang.sh: https://storage.googleapis.com/golang/go1.8.1.linux-amd64.tar.gz is remote
    --2017-04-19 17:47:45--  https://storage.googleapis.com/golang/go1.8.1.linux-amd64.tar.gz
    Resolving storage.googleapis.com (storage.googleapis.com)... 216.58.222.112
    Connecting to storage.googleapis.com (storage.googleapis.com)|216.58.222.112|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 91277742 (87M) [application/x-gzip]
    Saving to: /usr/local/go1.8.1.linux-amd64.tar.gz

    /usr/local/go1.8.1.linux-amd64.tar.gz       100%[==========================================================================================>]  87,05M  11,2MB/s    in 8,4s

    2017-04-19 17:47:54 (10,4 MB/s) - /usr/local/go1.8.1.linux-amd64.tar.gz saved [91277742/91277742]

    update-golang.sh: not found symlink for old install
    update-golang.sh: remove old link: /usr/local/go
    update-golang.sh: untar: rm -rf /usr/local/go1.8.1.linux-amd64
    update-golang.sh: untar: tar xf /usr/local/go1.8.1.linux-amd64.tar.gz
    update-golang.sh: path: removing old settings from: /etc/profile.d/golang_path.sh
    update-golang.sh: path: issuing new /usr/local/go/bin to /etc/profile.d/golang_path.sh
    update-golang.sh: golang go1.8.1.linux-amd64 installed at: /usr/local/go
    update-golang.sh: testing: /usr/local/go/bin/go version
    go version go1.8.1 linux/amd64
    update-golang.sh: SUCCESS
    $   

Customization
=============

These environment variables are available for customization:

    SOURCE=https://storage.googleapis.com/golang ;# download location
    DESTINATION=/usr/local                       ;# install destination
    RELEASE=1.8.1                                ;# golang release
    OS=linux                                     ;# os
    ARCH=amd64                                   ;# arch
    PROFILED=/etc/profile.d/golang_path.sh       ;# update PATH, optionally set GOROOT
    CACHE=/usr/local                             ;# cache downloads

Example:

    $ sudo RELEASE=1.8 ./update-golang.sh

Per-user Install
================

Default behavior is to install Golang globally for all system users.

However you can use the environment variables to point locations to your per-user home directory.

Example:

    This example will install Golang under ~/golang for current user only.
    
    $ mkdir ~/golang
    $ DESTINATION=~/golang PROFILED=~/.profile ./update-golang.sh
