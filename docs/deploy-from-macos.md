# Deploy from macOS

While you can't turn a macOS system in an rAlgoVPN, you can install the rAlgo scripts on a macOS system and use them to deploy your rAlgoVPN to a cloud provider.

rAlgo uses [Ansible](https://www.ansible.com) which requires Python 3. macOS does not include a version of Python 3 that you can use with rAlgo. (It does include an obsolete version of Python 2 installed as `/usr/bin/python` which you should ignore.)

You'll need to install Python 3 before you can run rAlgo. Python 3 is available from several different packagers, three of which are listed below.

**IMPORTANT:** At this time you **cannot** use Python 3.8 or later with Ansible on macOS. Choose a recent version of Python 3.7 instead.

## macOS 10.15 Catalina

Catalina comes with `/usr/bin/python3` installed. This file, and certain others like `/usr/bin/git`, start out as stub files that prompt you to install the Developer Command Line Tools the first time you run them. Having `git` installed can be useful but whether or not you choose to install the Command Line Tools you **cannot** use this version of Python 3 with rAlgo at this time. Instead install one of the versions below.

## Ansible and SSL Validation

Ansible validates SSL network connections using OpenSSL but macOS includes LibreSSL which behaves differently. Therefore each version of Python below includes or depends on its own copy of OpenSSL.

OpenSSL needs access to a list of trusted CA certificates in order to validate SSL connections. Each packager handles initializing this certificate store differently. If you see the error `CERTIFICATE_VERIFY_FAILED` when running rAlgo make sure you've followed the packager-specific instructions correctly, and that you're not inadvertently running Catalina's `/usr/bin/python3`.

## Install Python 3

Choose one of the packagers below as your source for Python 3. Avoid installing versions from multiple packagers on the same Mac as you may encounter conflicts. In particular they might fight over creating symbolic links in `/usr/local/bin`.

### Option 1: Install using the Homebrew package manager

If you're comfortable using the command line in Terminal the [Homebrew](https://brew.sh) project is a great source of software for macOS.

First install Homebrew using the instructions on the [Homebrew](https://brew.sh) page.

The install command below takes care of initializing the CA certificate store.

#### Installation
```
brew install python3
```
After installation open a new tab or window in Terminal and verify that the command `which python3` returns `/usr/local/bin/python3`.

#### Removal
```
brew uninstall python3
```

### Option 2: Install a package from Python.org

If you don't want to install a package manager you can download a Python package for macOS from [python.org](https://www.python.org/downloads/mac-osx/).

#### Installation

Download the most recent version of Python 3.7 and install it like any other macOS package. Then initialize the CA certificate store from Finder by double-clicking on the file `Install Certificates.command` found in the `/Applications/Python 3.7` folder.

When you double-click on `Install Certificates.command` a new Terminal window will open. If the window remains blank then the command has not run correctly. This can happen if you've changed the default shell in Terminal Preferences. Try changing it back to the default and run `Install Certificates.command` again.

After installation open a new tab or window in Terminal and verify that the command `which python3` returns either `/usr/local/bin/python3` or  `/Library/Frameworks/Python.framework/Versions/3.7/bin/python3`.

#### Removal

Unfortunately the python.org package does not include an uninstaller and removing it requires several steps:

1. In Finder, delete the package folder found in `/Applications`.
2. In Finder, delete the *rest* of the package found under ` /Library/Frameworks/Python.framework/Versions`.
3. In Terminal, undo the changes to your `PATH` by running:
```mv ~/.bash_profile.pysave ~/.bash_profile```
4. In Terminal, remove the dozen or so symbolic links the package created in `/usr/local/bin`. Or just leave them because installing another version of Python will overwrite most of them.

### Option 3: Install using the Macports package manager

[Macports](https://www.macports.org) is another command line based package manager like Homebrew. Most users will find Macports far more complex than Homebrew, but developers might find Macports more flexible. If you search for "Macports vs. Homebrew" you will find many opinions.

First install Macports per the [instructions](https://www.macports.org/install.php).

In addition to installing Python you'll need to install the package containing the CA certificates.

#### Installation
```
sudo port install python37
sudo port install curl-ca-bundle
```
After installation open a new tab or window in Terminal and verify that the command `which python3` returns `/opt/local/bin/python3`.

#### Removal
```
sudo port uninstall python37
sudo port uninstall curl-ca-bundle
```
