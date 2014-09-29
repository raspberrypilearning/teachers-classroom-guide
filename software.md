# Guide to installing software on Raspberry Pi

Follow this guide from the command line, or open an LXTerminal window.

## Installing software with apt

The command line is a prompt waiting for its user to give a command. The prompt is ready and waiting for a command when the dollar sign (`$`) is shown. Enter the command `ls` and you'll see the contents of your home folder, it will list the files and folders. `ls` is a built-in command.

Try entering the following command:

```bash
tree
```

`tree` is not installed in Raspbian by default. Let's install it.

In Linux, most software is installed using a package manager. This makes it easy to install and remove software and staying in control. The package manager Raspbian uses is called `apt` (this stands for 'Advanced Packaging Tool' but most people just call it `apt`. To install the package `tree`, enter:

```bash
apt-get install tree
```

This shows an error. You need to be the root user to install software! To gain root previleges, prefix a command with `sudo` - this means "super user do":

```bash
sudo apt-get install tree
```

Now enter `tree` at the command line and you should see `tree`'s output. `tree` is a useful utility which shows you all the files and folders recursively in a tree, unlike `ls` which just shows the contents of the current folder.

## Removing software with apt

To remove the package `tree`, simply type the command:

```bash
sudo apt-get remove tree
```

However this will leave behind any configuration files for the package. To completely remove the package and its configuration files, use `--purge`:

```bash
sudo apt-get remove tree --purge
```

Note: you can only purge a package you have installed, so you can't purge *after* removing, only instead of.

## Searching for packages in apt

To search the `apt` package list, enter:

```bash
apt-cache search picamera
```
