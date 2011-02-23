# rvm.deb: a sane system-wide rvm installation for debian/ubuntu

- rvm.deb explicitly conflicts with all known ruby debian packages
- rvm.deb comes pre-installed with the latest 1.8.7, 1.9.2, REE and JRuby
- rvm.deb provides dev packages required for popular rubygems (like `libxslt-dev` and `libxml2-dev` for `nokogiri`)
- rvm.deb installs `/usr/bin/ruby` so you can use `sudo ruby`
- rvm.deb provides a working ruby environment in a single package (rubygems, irb, rake, bundler, dev headers and debug symbols)

## usage

    $ wget ...
    $ sudo dpkg -i rvm*.deb   # this will fail due to missing deps
    $ sudo apt-get -f install # install those deps automatically
    $ sudo dpkg -i rvm*.deb   # install the package

## building

rvm does not provide a separate build vs install step, so the only way to package it is by doing an actual system-wide rvm installation in a chroot.

first, setup a chroot environment with pbuilder:

    $ sudo apt-get install pbuilder
    $ sudo pbuilder create
    $ sudo pbuilder --update

give rvm root access inside the chroot so it can do a system-wide install:

    $ echo "BUILDUSERID=''"    > .pbuilderrc
    $ echo "BUILDUSERNAME=''" >> .pbuilderrc

build the package:

    $ pdebuild

install the newly built .deb:

    $ sudo dpkg -i /var/cache/pbuilder/result/rvm*.deb

