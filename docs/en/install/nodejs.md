---
title: installation
lastChanged: 13.09.2018
translatedFrom: de
translatedWarning: If you want to edit this document please delete "translatedFrom" field, elsewise this document will be translated automatically again
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/en/install/nodejs.md
hash: eH2G1BUwlRFk+U0oP5JXsohHGLGgD8kk/4x1IemEtyg=
---
# Install Node.js and npm
?> ***This is a placeholder***.<br><br> Help ioBroker and expand this article. Please note the [ioBroker Style Guide](community/styleguidedoc) so that the changes can be adopted more easily.

ioBroker and adapters are mainly written in the programming language JavaScript. Since a computer cannot execute Javascript directly, it needs the Node.js runtime environment to do so.

The following information is an unofficial translation of the [Node.js Foundation](https://nodejs.org/en/download/package-manager/) installation instructions.

## Contents
* [Arch Linux](#arch)
* [Debian and Ubuntu based Linux distributions](#)
* [Enterprise Linux and Fedora](#debian)
* [FreeBSD](#)
* [Gentoo](#)
* [NetBSD](#)
* [nvm](#)
* [OpenBSD](#)
* [openSUSE and SLE](#)
* [macOS](#)
* [SmartOS and illumos](#)
* [Void Linux](#)
* [Solus](#)
* [Windows](#windows)

<h2 id="arch">ArchLinux</h2>

Ready Node.js and npm packages are available in the community repository.

```
pacman -S nodejs npm
```

<h2 id="debian">Debian and Ubuntu based Linux distributions</h2>

This also includes **Linux Mint, Linux Mint Debian Edition (LMDE), elementaryOS, bash on Windows** and more.

Node.js is provided via the NodeSource Debian and Ubuntu Binary Repository (formerly Chris Lea's Launchpad PPA).
Help and scripts for the reposity can be found on GitHub under [nodesource/distributions](https://github.com/nodesource/distributions).

**Note:** If you want to install Node.js on Ubuntu Precise or Debian Wheezy, it is recommended to read the article on using [Node.js >= 6.x on older distributions](https://github.com/nodesource/distributions/blob/master/OLDER_DISTROS.md).

```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

For Node.js 10 use the following command:

```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
```

***Optional:*** Installation of the build tools

To compile and install native extensions with npm, the build tools should also be installed.

```
sudo apt-get install -y build-essential
```

**Available architectures:**

* **i386** (32-bit)
* **amd64** (64-bit)
* **armhf** (ARM 32-bit hard-float, ARMv7 and newer: arm-linux-gnueabihf)

Supported Ubuntu versions:

* **Ubuntu 14.04 LTS** (Trusty Tahr)
* **Ubuntu 16.04 LTS** (Xenial Xerus)

Supported Debian versions:

* **Debian 8** (jessie, old stable)
* **Debian 9 / stable** (stretch)
* **Debian 9 testing** (buster to be released as next stable)
* **Debian unstable** (sid never-to-be-released, aka rolling)

A Node.js package is also available in the official repo for Debian Sid (unstable), Jessie (testing) and Wheezy (wheezy-backports) as "nodejs". It only installs a nodejs binary.

The nodejs-legacy package installs a node symlink that is needed by many modules to build and run correctly. The Node.js modules available in the distribution official repositories do not need it.

Supported Linux Mint versions:

* **Linux Mint 17 "Qiana"** (via Ubuntu 14.04 LTS)
* **Linux Mint 17.1 "Rebecca"** (via Ubuntu 14.04 LTS)
* **Linux Mint 17.2 "Rafaela"** (via Ubuntu 14.04 LTS)
* **Linux Mint Debian Edition (LMDE) 2 "Betsy"** (via Debian 8)

Supported elementary OS versions:

* **elementary OS Luna** (via Ubuntu 12.04 LTS)
* **elementary OS Freya** (via Ubuntu 14.04 LTS)
* **elementary OS Loki** (via Ubuntu 16.04 LTS)
* **elementary OS Juno** (via Ubuntu 18.04 LTS)

Supported Trisquel versions:

* **Trisquel 7 "Belenos"** (via Ubuntu 14.04 LTS)

Supported BOSS versions:

* **BOSS 5.0 "Anokha"** (via Debian 7)

Enterprise Linux and Fedora

Including Red Hat® Enterprise Linux® / RHEL, CentOS and Fedora.

Node.js is available from the NodeSource Enterprise Linux and Fedora binary distributions repository. Support for this repository, along with its scripts, can be found on GitHub at nodesource/distributions.

Note that the Node.js packages for EL 5 (RHEL5 and CentOS 5) depend on the EPEL repository being available. The setup script will check and provide instructions if it is not installed.

On RHEL, CentOS or Fedora, for Node.js v8 LTS:

```
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
```

Alternatively for Node.js 10:

```
curl --silent --location https://rpm.nodesource.com/setup_10.x | sudo bash -
```

Then install:

```
sudo yum -y install nodejs
```

Optional: install build tools

To compile and install native addons from npm you may also need to install build tools:

```
sudo yum install gcc-c++ make
# or: sudo yum groupinstall 'Development Tools'
```

Available architectures:

* **i386** (32-bit, not available for EL7)
* **x86_64** (64-bit)

Supported Red Hat® Enterprise Linux® versions:

* **RHEL 5** (32-bit and 64-bit)
* **RHEL 6** (32-bit and 64-bit)
* **RHEL 7** (64-bit)

Supported CentOS versions:

* **CentOS 5** (32-bit and 64-bit)
* **CentOS 6** (32-bit and 64-bit)
* **CentOS 7** (64-bit)

Supported CloudLinux versions:

* **CloudLinux 6** (32-bit and 64-bit)

Supported Fedora versions:

* **Fedora 21** (Twenty One) (32-bit and 64-bit)
* **Fedora 20** (Heisenbug) (32-bit and 64-bit)
* **Fedora 19** (Schrodinger's Cat) (32-bit and 64-bit)

Other distributions known to be supported:

* **Oracle Linux** (mirrors RHEL very closely)
* **Amazon Linux** (tested on 2016.03)

alternatives

Official Fedora Node.js and npm packages are available in Fedora 18 and later. Install with:

```
sudo dnf install nodejs
```

In a hurry for the latest updates? Grab them from updates testing.

Enterprise Linux (RHEL and CentOS) users may use the Node.js and npm packages from the EPEL repository.

Install the appropriate epel-release RPM for your version (found on the EPEL repository homepage), then run:

```
sudo yum install nodejs npm --enablerepo=epel
```

In a hurry for the latest updates? Grab them from epel-testing.

Available architectures:

* **i686** (32-bit, not available for EL7)
* **x86_64** (64-bit)
* **armv6hl** (Raspberry Pi, Pidora only)
* **armv7hl** (32-bit ARM hard-float, ARMv7 and up, Fedora only)

Supported Red Hat® Enterprise Linux® versions:

* **RHEL 6** (i686/x86_64)
* **RHEL 7** (aarch64/x86_64)

RHEL 6 is no longer supported through EPEL, however you can use Red Hat Software Collections.

Additionally, versions of CentOS and Scientific Linux corresponding to the above RHEL versions are also officially supported by all EPEL packages, including nodejs. Amazon Linux is not officially supported by EPEL due to significant incompatibilities previously reported to the epel-devel mailing list, however you might find that nodejs at least still works.

Supported Fedora versions:

* **Fedora Rawhide** (i686/x86_64/armv7hl/aarch64/ppc64/ppc64le/s390x)
* **Fedora 27** (i686/x86_64/armv7hl/aarch64/ppc64/ppc64le/s390x)
* **Fedora 26** (i686/x86_64/armv7hl/aarch64/ppc64/ppc64le)

FreeBSD

The most recent release of Node.js is available via the www/node port.

Install a binary package via pkg:

```
pkg install node
```

Or compile it on your own using ports:

```
cd /usr/ports/www/node && make install
```

Gentoo

Node.js is available in the portage tree.

```
emerge nodejs
```

NetBSD

Node.js is available in the pkgsrc tree:

```
cd /usr/pkgsrc/lang/nodejs && make install
```

Or install a binary package (if available for your platform) using pkgin:

```
pkgin -y install nodejs
```

nvm

Node Version Manager is a bash script used to manage multiple released Node.js versions. It allows you to perform operations like install, uninstall, switch version, etc. To install nvm, use this install script.

On Unix / OS X systems Node.js built from source can be installed using nvm by installing into the location that nvm expects:

```
$ env VERSION=`python tools/getnodeversion.py` make install DESTDIR=`nvm_version_path v$VERSION` PREFIX=""
```

After this you can use nvm to switch between released versions and versions built from source. For example, if the version of Node.js is v8.0.0-pre:

```
$ nvm use 8
```

Once the official release is out you will want to uninstall the version built from source:

```
$ nvm uninstall 8
```

OpenBSD

Node.js is available through the ports system.

```
/usr/ports/lang/node
```

Using pkg_add on OpenBSD:

```
pkg_add node
```

openSUSE and SLE

Node.js is available in the main repositories under the following packages:

* **openSUSE Leap 42.2:** nodejs4
* **openSUSE Leap 42.3:** nodejs4, nodejs6
* **openSUSE Tumbleweed:** nodejs4, nodejs6, nodejs8
* **SUSE Linux Enterprise Server (SLES) 12:** nodejs4, nodejs6

    (The "Web and Scripting Module" must be added before installing.)

For example, to install Node.js 4.x on openSUSE Leap 42.2, run the following as root:

```
zypper install nodejs4
```

macOS

Simply download the macOS Installer directly from the nodejs.org web site.

If you want to download the package with bash:

```
curl "https://nodejs.org/dist/latest/node-${VERSION:-$(wget -qO- https://nodejs.org/dist/latest/ | sed -nE 's|.*>node-(.*)\.pkg</a>.*|\1|p')}.pkg" > "$HOME/Downloads/node-latest.pkg" && sudo installer -store -pkg "$HOME/Downloads/node-latest.pkg" -target "/"
```

alternatives

Using Homebrew:

brew install node

Using MacPorts:

```
port install nodejs<major version>
```

#Example
port install nodejs7

Using pkgsrc:

Install the binary package:

```
pkgin -y install nodejs
```

Or build manually from pkgsrc:

```
cd pkgsrc/lang/nodejs && bmake install
```

SmartOS and illuminos

SmartOS images come with pkgsrc pre-installed. On other illumos distributions, first install pkgsrc, then you may install the binary package as normal:

```
pkgin -y install nodejs
```

Or build manually from pkgsrc:

```
cd pkgsrc/lang/nodejs && bmake install
```

VoidLinux

Void Linux ships node.js stable in the main repository.

```
xbps-install -Sy nodejs
```

solus

Solus provides node.js in its main repository.

```
sudo eopkg install nodejs
```

<h2 id="windows">Windows</h2>

?> It is recommended to download Node.js from the machine where it will be installed. The Node.js download website automatically takes into account whether Windows is available as a 32- or 64-bit variant.

1. From the website

[https://nodejs.org/en/download](https://nodejs.org/en/download/) Download the current 8.x LTS version of Node.js.

![Download Node.js](../../de/install/media/w01downloadnode.png "':size=550'") *Download from Node.js*

1. Start the downloaded Node.js installer with a double click.

?> The program is usually located in the `Downloads` folder and follows the `node-<Version>.msi` naming scheme.

1. If a message appears that the app is not a verified app from the Microsoft Store,

   Select `Trotzdem installieren`.

1. Install Node.js with the default settings. Do not change the installation path.

   Confirm any security prompts that appear.

![Installing Node.js](../../de/install/media/w03nodeinst.gif) *Node.js installation*