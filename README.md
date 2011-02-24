# rvm.deb: a sane system-wide rvm installation for debian/ubuntu

- rvm.deb provides a working ruby environment in a single package (rubygems, irb, rake, bundler, dev headers and debug symbols)
- rvm.deb comes pre-installed with the latest 1.8.7, 1.9.2, REE and JRuby
- rvm.deb includes dev packages required for popular rubygems (like `libxslt-dev` and `libxml2-dev` for `nokogiri`)
- rvm.deb plays nice with debian's `ruby1.8` via `rvm use system`
- rvm.deb installs to `/usr/bin/ruby` so you don't need `rvmsudo`

## usage

    $ apt-get install rvm

by default, `/usr/bin/ruby` points to debian's `ruby1.8`. to use REE by default system-wide:

    $ sudo rvm --default ree

to use jruby without making it the system default:

    $ source /usr/lib/rvm/scripts/rvm    # or put this in your ~/.{profile,bashrc,bash_profile,bash_login,zshrc}
    $ rvm use jruby

to install and use rubinius:

    $ sudo rvm install rbx
    $ rvm use rbx

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

    $ cp /var/cache/pbuilder/result/rvm*.deb .
    $ sudo dpkg -i rvm*.deb   # this will fail due to missing deps
    $ sudo apt-get -f install # install those deps automatically
    $ sudo dpkg -i rvm*.deb   # install the package

