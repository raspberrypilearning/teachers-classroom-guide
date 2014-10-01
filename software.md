# Guide to installing software on Raspberry Pi

Follow this guide from the command line, or open an LXTerminal window.

## Installing software with apt

The easiest way to manage installing, upgrading, and removing software is using `apt` (Advanced Packaging Tool) which comes from Debian. If a piece of software is packaged in Debian and works on the Raspberry Pi's ARM architecture, it should also be available in Raspbian.

To install or remove packages you need root user permissions, so your user needs to be in `sudoers` or you must be logged in as `root`. Read more about [users](http://raspberrypi.org/documentation/linux/usage/users.md) and [root](http://raspberrypi.org/documentation/linux/usage/root.md).

To install new packages, or update existing ones, you will need an internet connection.

Note that installing software uses up disk space on your SD card, so you should keep an eye on disk usage and use an appropriately sized SD card.

Also note that a lock is performed while software is installing, so you cannot install multiple packages at the same time.

## Software sources

APT keeps a list of software sources on your Pi in a file at `/etc/apt/sources.list`. Before installing software, you should update your package list with `apt-get update`:

```bash
sudo apt-get update
```

## Installing a package with APT

```bash
sudo apt-get install tree
```

Typing this command should inform the user how much disk space the package will take up and asks for confirmation of the package installation. Entering `Y` (or just hitting `Enter`, as yes is the default action) will allow the installation to occur. This can be bypassed by adding the `-y` flag to the command:

```bash
sudo apt-get install tree -y
```

Installing this package makes `tree` available for the user.

## Using an installed package

`tree` is a command line tool which provides a visualisation of the directory structure of the current directory, and all it contains.

- Typing `tree` runs the tree command. For example:

```bash
tree
..
├── hello.py
├── games
│   ├── asteroids.py
│   ├── pacman.py
│   ├── README.txt
│   └── tetris.py

```

- Typing `man tree` gives the manual entry for the package `tree`
- Typing `whereis tree` shows where `tree` lives:

```bash
tree: /usr/bin/tree
```

## Uninstalling a package with APT

### Remove

You can uninstall a package with `apt-get remove`:

```bash
sudo apt-get remove tree
```

The user is prompted to confirm the removal. Again, the `-y` flag will auto-confirm.

### Purge

You can also choose to completely remove the package and its associated configuration files with `apt-get purge`:

```bash
sudo apt-get purge tree
```

## Upgrading existing software

If software updates are available, you can get the updates with `sudo apt-get update` and install the updates with `sudo apt-get upgrade`, which will upgrade all of your packages. To upgrade a specific package, without upgrading all the other out-of-date packages at the same time, you can use `sudo apt-get install somepackage` (which may be useful if you're low on disk space or you have limited download bandwidth).

## Searching for software

You can search the archives for a package with a given keyword with `apt-cache search`:

```bash
apt-cache search locomotive
sl - Correct you if you type `sl' by mistake
```

You can view more information about a package before installing it with `apt-cache show`:

```bash
apt-cache show sl
Package: sl
Version: 3.03-17
Architecture: armhf
Maintainer: Hiroyuki Yamamoto <yama1066@gmail.com>
Installed-Size: 114
Depends: libc6 (>= 2.4), libncurses5 (>= 5.5-5~), libtinfo5
Homepage: http://www.tkl.iis.u-tokyo.ac.jp/~toyoda/index_e.html
Priority: optional
Section: games
Filename: pool/main/s/sl/sl_3.03-17_armhf.deb
Size: 26246
SHA256: 42dea9d7c618af8fe9f3c810b3d551102832bf217a5bcdba310f119f62117dfb
SHA1: b08039acccecd721fc3e6faf264fe59e56118e74
MD5sum: 450b21cc998dc9026313f72b4bd9807b
Description: Correct you if you type `sl' by mistake
 Sl is a program that can display animations aimed to correct you
 if you type 'sl' by mistake.
 SL stands for Steam Locomotive.
```

## Installing Python packages

Some Python packages can be found in the Raspbian archives and can be installed using APT, for example:

```bash
sudo apt-get update
sudo apt-get install python3-picamera
```

This is a preferable method of installing software, as it means that the modules you install can be kept up to date easily with the usual `sudo apt-get update` and `sudo apt-get upgrade` commands.

Python packages in Raspbian which are compatible with Python 2.x will always have a `python-` prefix. So, the `picamera` package for Python 2.x is named `python-picamera` (as shown in the example above). Python 3 packages always have a `python3-` prefix. So, to install `rpi.gpio` for Python 3 you would use:

```bash
sudo apt-get install python3-rpi.gpio
```

### pip

Not all Python packages are available in the Raspbian archives, and those that are can sometimes be out of date. If you can't find a suitable version in the Raspbian archives you can install packages from the [Python Package Index](http://pypi.python.org/) (PyPI). To do so, use the `pip` tool.

First install `pip` with `apt`.

```bash
sudo apt-get install python3-pip
```

or the Python 2 version:

```bash
sudo apt-get install python-pip
```

`pip-3.3` installs modules for Python 3 and `pip` installs modules for Python 2.

For example, the folowing command installs the Pibrella library for Python 3:

```bash
pip-3.3 install pibrella
```

and the folowing command installs the Pibrella library for Python 2:

```bash
pip install pibrella
```

Uninstall Python modules with `pip-3.3 uninstall` or `pip uninstall`.

Upload your own Python modules to `pip` with the [guide at PyPI](https://wiki.python.org/moin/CheeseShopTutorial#Submitting_Packages_to_the_Package_Index).
