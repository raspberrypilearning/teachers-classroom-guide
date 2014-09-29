# Guide to installing software on Raspberry Pi

Start at the command line, or open an LXTerminal window.

The command line is a prompt waiting for its user to give a command. The prompt is ready and waiting for a command when the dollar sign (`$`) is shown. Enter the command `ls` and you'll see the contents of your home folder, it will list the files and folders. `ls` is a built-in command.

Try entering the following command:

```bash
tree
```

`tree` is not installed in Raspbian by default. Let's install it.

In Linux, most software is installed using a package manager. This makes it easy to install and remove software and staying in control. The package manager Raspbian uses is called `apt` (this stands for 'Advanced Packaging Tool' but most people just call it `apt`. To install the package `tree`, use the `install` command in `apt`:

```bash
apt-get install tree
```

This shows an error. You need to be the root user to install software! To gain root previleges, prefix a command with `sudo` - this means "super user do":

```bash
sudo apt-get install tree
```

Now enter `tree` at the command line and you should see `tree`'s output. `tree` is a useful utility which shows you all the files and folders recursively in a tree, unlike `ls` which just shows the contents of the current folder.
